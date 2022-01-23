# 简介

cheerio是jquery核心功能的一个快速灵活而又简洁的实现，主要是为了用在服务器端需要对DOM进行操作的地方.

```js
const cheerio = require('cheerio');
const $ = cheerio.load('<h2 class="title">Hello world</h2>');

$('h2.title').text('Hello there!');
$('h2').addClass('welcome');

$.html();
//=> <html><head></head><body><h2 class="title welcome">Hello there!</h2></body></html>
```

# 特点

* 熟悉的语法：cheerio实现了jQuery的一个子集，去掉了jQuery中所有与DOM不一致或者是用来填浏览器的坑的东西，重现了jQuery最美妙的API

* 快到没朋友：cheerio使用了及其简洁而又标准的DOM模型， 因此对文档的转换，操作，渲染都极其的高效。基本的端到端测试显示它的速度至少是JSDOM的8倍

* 极其灵活：cheerio使用了@FB55编写的非常兼容的htmlparser2，因此它可以解析几乎所有的HTML和XML

# 关于JSDOM

cheerio产生的原因是出于对JSDOM的失望，主要体现在以下三点：

* JSDOM的解析规则太过于严格：JSDOM的解析器无法处理现在许多的流行网站的内容

* JSDOM太慢了：解析大的网站甚至可以产生可察觉的延迟

* JSDOM太过于重量级：JSDOM的目标是提供与浏览器一样的DOM环境，但是我们往往不需要这样。我们需要的只是一种简单，熟悉的方式来操作我们的HTML

# 什么时候你应该用JSDOM

cheerio并非万能，当你需要一个浏览器一样的环境时，你最好还是用JSDOM，尤其是你需要进行自动化的功能测试时

**示例html模板**

```html
<ul id="fruits">
  <li class="apple">Apple</li>
  <li class="orange">Orange</li>
  <li class="pear">Pear</li>
</ul>
```

# 加载 

首先你需要先加载你的HTML。jQuery会自动完成这一步，因为jQuery操作的DOM是固定的。但是在使用cheerio时我们要手动加载我们的HTML文档

首选方法: 

```js
// ES6 or TypeScript:
import * as cheerio from 'cheerio';

// In other environments:
const cheerio = require('cheerio');

const $ = cheerio.load('<ul id="fruits">...</ul>');

$.html();
//=> <html><head></head><body><ul id="fruits">...</ul></body></html>
```

类似于web浏览器上下文, cheerio.load会包含`<html>`, `<head>`, 和 `<body>`元素,如果不需要, 可以设置cheerio.load的第三个参数为 `false`

```js
const $ = cheerio.load('<ul id="fruits">...</ul>', null, false);

$.html();
//=> '<ul id="fruits">...</ul>'
```

你也可以通过传递字符串作为上下文来加载HTML:

```js
$('ul', '<ul id="fruits">...</ul>');
```

或者把HTML字符串作为root

```js
$('li', 'ul', '<ul id="fruits">...</ul>');
```

如果你需要修改默认的解析选项你也可以传一个额外的配置对象

```js
const $ = cheerio.load('<ul id="fruits">...</ul>', {
  xml: {
    normalizeWhitespace: true,
  },
});

const $ = cheerio.load('<ul id="fruits">...</ul>', {
	normalizeWhitespace: true,
	xmlMode: true
});
```

这些解析选项直接取自htmlparser2,因此也可以在cheerio中使用任何在htmlparser2中有效的选项。更多配置可以查看 [domhandler](https://github.com/fb55/DomHandler) 和 [htmlparser2](https://github.com/fb55/htmlparser2/wiki/Parser-options)的配置项.

有些用户可能希望使用htmlparser2库解析标记，并使用Cheerio遍历/操作结果结构。这种情况可能发生在那些从1.0之前的Cheerio版本(依赖于htmlparser2)升级的人，那些处理无效标记的人(因为htmlparser2更容易)，或者那些在性能关键的情况下操作的人(因为htmlparser2在某些情况下可能更快)。请注意，“更宽容”意味着htmlparser2具有错误纠正机制，这些机制并不总是与web浏览器所观察到的标准相匹配。这种行为在解析非html内容时可能很有用。
为了支持这些情况，load也接受htmlparser2兼容的数据结构作为它的第一个参数。用户可以安装htmlparser2，使用它来解析输入，并将结果传递给cheerio.load:

```js
// Usage as of htmlparser2 version 6:
const htmlparser2 = require('htmlparser2');
const dom = htmlparser2.parseDocument(document, options);

const $ = cheerio.load(dom);
```

# 选择器 $( selector, [context], [root] )

cheerio的选择器几乎和jQuery一模一样，所以语法上十分相像

选择器在上下文范围内搜索，而上下文范围在根范围内搜索。选择器和上下文可以是字符串表达式、DOM元素、DOM元素数组或cheerio对象。root通常是HTML文档字符串。

这个选择器方法是遍历和操作文档的起点。与jQuery一样，它是在文档中选择元素的主要方法。

```js
$('.apple', '#fruits').text();
//=> Apple

$('ul .pear').attr('class');
//=> pear

$('li[class=orange]').html();
//=> Orange
```

# XML命名空间

您可以使用XML Namespaces进行选择，但由于CSS规范，需要转义冒号(:)才能使选择器有效:

```js
$('[xml\\:id="main"');
```

# 渲染

当你准备渲染文档时，你可以在“root”选项上调用html方法:

```js
$.root().html();
//=>  <html>
//      <head></head>
//      <body>
//        <ul id="fruits">
//          <li class="apple">Apple</li>
//          <li class="orange">Orange</li>
//          <li class="pear">Pear</li>
//        </ul>
//      </body>
//    </html>
```

如果你想呈现一个选择的**outerHTML**，你可以使用cheerio.html实用函数:

```js
cheerio.html($('.pear'));
//=> <li class="pear">Pear</li>
```

默认情况下，cheerio.html将保留一些标签处于打开状态。有时，您可能希望呈现有效的XML文档格式。例如，你可以解析以下XML片段:

```js
onst $ = cheerio.load(
  '<media:thumbnail url="http://www.foo.com/keyframe.jpg" width="75" height="50" time="12:05:01.123"/>'
);
```

稍后如果又想渲染XML格式文档,调用cheerio.xml实用函数:

```js
$.xml();
//=> <media:thumbnail url="http://www.foo.com/keyframe.jpg" width="75" height="50" time="12:05:01.123"/>
```

你也可以使用文本静态方法**cheerio.text**渲染Cheerio对象的文本内容:

```js
const $ = cheerio.load('This is <em>content</em>.');
cheerio.text($('body'));
//=> This is content.
```

# 插件

一旦你加载了一个文档，你可以用自定义插件方法扩展原型或等效的fn属性:

```js
const $ = cheerio.load('<html><body>Hello, <b>world</b>!</body></html>');
$.prototype.logHtml = function () {
    console.log(this.html());
};

$('body').logHtml(); // logs "Hello, <b>world</b>!" to the console
```

如果你使用的是TypeScript，你也应该为你的新方法添加一个类型定义:

```typescript
declare module 'cheerio' {
	interface Cheerio<T> {
		logHtml(this: Cheerio<T>): void;
	}
}
```

# DOM Node 对象

Cheerio集合由一些类似于基于浏览器的DOM节点的对象组成。你可以期望它们定义以下属性:

* tagName
* parentNode
* previousSibling
* nextSibling
* nodeValue
* firstChild
* childNodes
* lastChild