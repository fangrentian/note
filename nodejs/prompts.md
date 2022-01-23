# 介绍

prompts是轻量级美观且用户友好的交互式提示工具

# 特点

* 简单: prompts没有很多的依赖,也没有拆分成很多的模块.
* 友好用户: prompt使用布局和颜色创建漂亮的cli界面.
* Promise化: 使用promises 和 async/await处理异步操作.
* 灵活: 所有prompts都是独立的，可以单独使用.
* 可测试: 提供以编程方式提交答案的方法.
* 一致性: 所有prompts有一致的体验.

# 安装

```shell
npm install --save prompts
```

# 用法

```js
const prompts = require('prompts');

(async () => {
	const response = await prompts({
		type: 'number',
		name: 'value',
		message: 'How old are you?',
		validate: value => value < 18 ? `Nightclub is 18+ only` : true
	});

	console.log(response); // => { value: 24 }
})();
```

[查看更多配置选项](https://github.com/terkelg/prompts/blob/master/example.js)

## 单一 prompt

使用单个提示对象。返回一个带有响应的对象。

```js
const prompts = require('prompts');

(async () => {
	const response = await prompts({
		type: 'text',
		name: 'meaning',
		message: 'What is the meaning of life?'
	});

	console.log(response.meaning);
})();
```

## prompt链

使用提示对象列表。返回一个带有响应的对象。确保给每个prompt有一个唯一的name属性，以防止被覆盖。

```js
const prompts = require('prompts');

const questions = [
	{
		type: 'text',
		name: 'username',
		message: 'What is your GitHub username?'
	},
	{
		type: 'number',
		name: 'age',
		message: 'How old are you?'
	},
	{
		type: 'text',
		name: 'about',
		message: 'Tell something about yourself',
		initial: 'Why should I?'
	}
];

(async () => {
	const response = await prompts(questions);

	// => response => { username, age, about }
})();
```

## 动态提示

提示属性也可以是函数。类型设置为假值的提示对象将被跳过。

```js
const prompts = require('prompts');

const questions = [
	{
		type: 'text',
		name: 'dish',
		message: 'Do you like pizza?'
	},
	{
		type: prev => prev == 'pizza' ? 'text' : null,
		name: 'topping',
		message: 'Name a topping'
	}
];

(async () => {
	const response = await prompts(questions);
})();
```

# API prompts(prompts, options)

## 参数prompts

类型为数组或对象。是提示用户回答的问题。[查看提示类型列表](https://www.npmjs.com/package/prompts#-types)

提示可以被提交(return语句、回车)或取消(esc、abort、ctrl+c、ctrl+d)。当提示被取消时，返回的响应对象上不包含相关属性。

## options参数

### onSubmit

在每次提示提交后调用的回调。它接收三个参数(prompt、answer、answers)，其中prompt是当前的提示对象，answer是用户对当前问题的回答，answers是到目前为止用户的回答。支持异步函数。

返回true退出提示链并返回到目前为止收集的所有响应，否则继续迭代提示对象。

```js
(async () => {
	const questions = [{...}];
	const onSubmit = (prompt, answer) => console.log(`Thanks I got ${answer} from ${prompt.name}`);
	const response = await prompts(questions, {onSubmit});
})();
```

### onCancel

当用户取消/退出提示时调用的回调。它接收两个参数(prompt, answers)，其中prompt是当前的提示对象，answers是到目前为止用户的回答。支持异步函数。

返回true继续并防止提示循环中止。在取消时，返回到目前为止收集的响应。

```js
(async () => {
	const questions = [{...}];
	const onCancel = prompt => {
		console.log('Never stop prompting!');
		return true;
	}
	const response = await prompts(questions, {onCancel});
})();
```

### override

通过将一个带有答案的对象传递给prompt.override来预先回答问题。与process参数结合使用时功能强大。

```js
const prompts = require('prompts');
prompts.override(require('yargs').argv);

(async () => {
	const response = await prompts([
		{
			type: 'text',
			name: 'twitter',
			message: `What's your twitter handle?`
		},
		{
			type: 'multiselect',
			name: 'color',
			message: 'Pick colors',
			choices: [
				{title: 'Red', value: '#ff0000'},
				{title: 'Green', value: '#00ff00'},
				{title: 'Blue', value: '#0000ff'}
			],
		}
	]);

	console.log(response);
})();
```

### inject(values)

以编程方式注入响应对象。这使您能够提前准备响应对象。如果找到任何注入的值，则立即用注入的值解析。此特性仅用于测试。

#### 参数values

包含要注入的值的数组。解析的值将从内部注入数组中移除。每个值可以是一个值数组，以便为被多次询问的问题提供答案。如果一个值是Error的实例，它将模拟用户取消/退出行为。

```js
const prompts = require('prompts');

prompts.inject(['@terkelg', ['#ff0000', '#0000ff']]);

(async () => {
	const response = await prompts([
		{
			type: 'text',
			name: 'twitter',
			message: `What's your twitter handle?`
		},
		{
			type: 'multiselect',
			name: 'color',
			message: 'Pick colors',
			choices: [
				{title: 'Red', value: '#ff0000'},
				{title: 'Green', value: '#00ff00'},
				{title: 'Blue', value: '#0000ff'}
			],
		}
	]);

	// => { twitter: 'terkelg', color: [ '#ff0000', '#0000ff' ] }
})();
```

# Prompt对象

Prompts Objects是定义“问题”和提示类型的JavaScript对象.

* type: String | Function,
* name: String | Function,
* message: String | Function,
* initial: String | Function | Async Function
* format: Function | Async Function,
* onRender: Function
* onState: Function
* stdin: Readable
* stdout: Writeable

每个属性都可以是函数类型，并将在提示用户之前调用。 函数接收三个(prev, values, prompt)，其中prev是前一个提示符的值，values是到目前为止收集的所有值的响应对象，prompt是前一个提示符对象。

```js
const prompt = {
	type: prev => prev > 3 ? 'confirm' : null,
	name: 'confirm',
	message: (prev, values) => `Please confirm that you eat ${values.dish} times ${prev} a day?`
}
```

## type

定义要显示的提示符类型。有关有效值，请参阅[提示类型列表](https://www.npmjs.com/package/prompts#-types)如果type是一个假值，将跳过该问题。

## name

取名字段不要重复, 相同的字段, 后面的会覆盖前面的

## message

显示给用户的信息

## initial

可选的默认提示值。也支持异步函数

## format

接收用户输入并返回的格式化值。返回的值将被添加到响应对象中。 接收两个参数(val, values)，其中val是当前提示符的值，values是当前响应对象，以防您需要基于之前的响应格式化。

```js
const pompt = {
	type: 'number',
	name: 'price',
	message: 'Enter price',
	format: val => Intl.NumberFormat(undefined, {style: 'currency', currency: 'USD'}).format(val);
}
```

## onRender

当提示被渲染时的回调。函数接收[kleur](https://github.com/lukeed/kleur)作为它的第一个参数，它引用当前提示。

```js
const prompt = {
	type: 'number',
	message: 'This message will be overridden',
	onRender(kleur) {
		this.msg = kleur.cyan('Enter a number');
	}
} 
```

## onState 

当当前提示的状态改变时的回调。函接收一个参数(state)，其中state是一个带有当前状态快照的对象。状态对象有两个属性**value** 和 **abort**。例如{value: 'This is '， abort: false}

## stdin and stdout

默认情况下prompt使用process.stdin接收输入,使用process.stdout输出, 如果你想使用其它Stream方法,可以设置这两个参数.

# 类型

* text(message, [initial], [style])
* password(message, [initial])
* invisible(message, [initial])
* number(message, initial, [max], [min], [style])
* confirm(message, [initial])
* list(message, [initial])
* toggle(message, [initial], [active], [inactive])
* select(message, choices, [initial], [hint], [warn])
* multiselect(message, choices, [initial], [max], [hint], [warn])
* autocomplete(message, choices, [initial], [suggest], [limit], [style])
* date(message, [initial], [warn])

## 配置参数

* message: 提示信息
* initial: 默认值
* style: 渲染风格(默认，密码，隐形，表情)
* format: 接收用户输入, 返回格式化后数据加入到响应对象中
* validate: 接收用户的输入。如果该值有效，则返回true，否则返回错误消息String。如果返回false，则显示一个默认的错误消息
* onRender: 当提示被渲染时的回调
* onState: 状态改变的回调
* max: 最大值, 默认Infinity
* min: 最小值, 默认-Infinity
* float: 是否允许浮点类型数值。默认值为 `false`
* round: 整数浮点值为x个小数。默认为2
* increment: 使用方向键时增加步长。默认为1
* separator: 字符串分隔符。将修剪所有的空白从开始和结束的字符串。默认为”、“
* active: 活动状态的文本, 默认是 'on'
* inactive: 非活动状态的文本, 默认是 'off'
* hint: 提示显示给用户
* warn: 选择禁用选项时要显示的消息
* choices: 字符串或选择对象的数组[{title, description, value, disabled}，…]。如果未指定选项，则该选项在数组中的索引将用作其值。
* instructions: 显示Prompt介绍
* optionsPerPage: 每页显示的选项数(默认为10)
* suggest: 过滤功能。默认按标题属性排序。应该始终返回一个**promise**。过滤器默认使用标题
* clearFirst: 第一个ESCAPE键将清除输入
* fallback: 没有找到匹配时的回退消息。如果提供，则默认为初始值
* locales: 用于定义自定义区域设置
* mask: 日期的格式掩码,默认值:YYYY-MM-DD HH: mm: ss

**默认locales**

```
{
  months: [
    'January', 'February', 'March', 'April',
    'May', 'June', 'July', 'August',
    'September', 'October', 'November', 'December'
  ],
  monthsShort: [
    'Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
    'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'
  ],
  weekdays: [
    'Sunday', 'Monday', 'Tuesday', 'Wednesday',
    'Thursday', 'Friday', 'Saturday'
  ],
  weekdaysShort: [
    'Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'
  ]
}
```