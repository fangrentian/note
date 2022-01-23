# chalk.js node终端样式库

chalk.js可以为console.log()的输出信息配置不同的显示颜色和样式, 使用可组合API，您可以链接或嵌套所需样式。

# 基本用法

`chalk.<style>[.<style>...](string, [string...])`

链接调用，最后一个接收字符串参数。调用顺序无关紧要，样式冲突时后面的覆盖前面的。

```js
import chalk from 'chalk';
const log = console.log;
// Combine styled and normal strings
log(chalk.blue('Hello') + ' World' + chalk.red('!'));

// Compose multiple styles using the chainable API
log(chalk.blue.bgRed.bold('Hello world!'));

// Pass in multiple arguments
log(chalk.blue('Hello', 'World!', 'Foo', 'bar', 'biz', 'baz'));

// Nest styles
log(chalk.red('Hello', chalk.underline.bgBlue('world') + '!'));

// Nest styles of the same type even (color, underline, background)
log(chalk.green(
	'I am a green line ' +
	chalk.blue.underline.bold('with a blue substring') +
	' that becomes green again!'
));

// ES2015 template literal
log(`
CPU: ${chalk.red('90%')}
RAM: ${chalk.green('40%')}
DISK: ${chalk.yellow('70%')}
`);

// Use RGB colors in terminal emulators that support it.
log(chalk.rgb(123, 45, 67).underline('Underlined reddish color'));
log(chalk.hex('#DEADED').bold('Bold gray!'));
```

# 定制主题

```js
import chalk from 'chalk';

const error = chalk.bold.red;
const warning = chalk.hex('#FFA500'); // Orange color

console.log(error('Error!'));
console.log(warning('Warning!'));
```

# 使用console.log字符串替换

```js
import chalk from 'chalk';

const name = 'Sindre';
console.log(chalk.green('Hello %s'), name);
//=> 'Hello Sindre'
```

# chalk.level 指定颜色支持的级别

* 0:	禁用所有颜色
* 1:	基本颜色支持（16 种颜色）
* 2:	256 色支持
* 3:	真彩色支持（1600 万色）

颜色支持是自动检测的，但你可以通过设置 **level** 属性来覆盖它。

```js
import {Chalk} from 'chalk';

const customChalk = new Chalk({level: 0});
```

# 支持的修饰符

* reset: 重置当前样式。
* bold: 使文本加粗。
* dim: 使文本具有较低的不透明度。
* italic: 使文本变为斜体。（未得到广泛支持）
* underline: 在文本下方放置一条水平线。（未得到广泛支持）
* overline: 在文本上方放置一条水平线。（未得到广泛支持）
* inverse: 反转背景和前景色。
* hidden: 打印文本，但使其不可见。
* strikethrough: 在文本中心放置一条水平线。（未得到广泛支持）
* visible: 仅当粉笔的颜色级别高于零时才打印文本。对于纯粹是化妆品的东西很有用。

# 支持的颜色方法

* black
* red
* green
* yellow
* blue
* magenta
* cyan
* white
* blackBright（别名： ，graygrey)
* redBright
* greenBright
* yellowBright
* blueBright
* magentaBright
* cyanBright
* whiteBright

# 支持的背景颜色方法

* bgBlack
* bgRed
* bgGreen
* bgYellow
* bgBlue
* bgMagenta
* bgCyan
* bgWhite
* bgBlackBright（别名： ，bgGraybgGrey)
* bgRedBright
* bgGreenBright
* bgYellowBright
* bgBlueBright
* bgMagentaBright
* bgCyanBright
* bgWhiteBright

# 256 和真彩色支持

```js
chalk.rgb(255, 136, 0).bold('Orange!')
chalk.bgRgb(255, 136, 0).bold('Orange!')
chalk.hex('#FF8800').bold('Orange!')
chalk.bgHex('#FF8800').bold('Orange!')
chalk.ansi256(194)('Honeydew, more or less')
chalk.bgAnsi256(194)('Honeydew, more or less')
```
