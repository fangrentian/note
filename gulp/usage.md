[查看官网](https://www.gulpjs.com.cn/)

# 快速入门

## 安装 gulp 命令行工具

```shell
npm install --global gulp-cli
```

## 创建项目目录,初始化package.json,安装 gulp，作为开发时依赖项,检查 gulp 版本

```shell
PS F:\nodeProjects> mkdir gulpdemo


    目录: F:\nodeProjects


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        2022/1/24     10:29                gulpdemo


PS F:\nodeProjects> cd gulpdemo
PS F:\nodeProjects\gulpdemo> npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg>` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
package name: (gulpdemo)
version: (1.0.0)
description:
entry point: (index.js)
test command:
git repository:
author:
license: (ISC)
About to write to F:\nodeProjects\gulpdemo\package.json:

{
  "name": "gulpdemo",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC"
}


Is this OK? (yes)
PS F:\nodeProjects\gulpdemo> npm install --save-dev gulp
npm WARN deprecated chokidar@2.1.8: Chokidar 2 will break on node v14+. Upgrade to chokidar 3 with 15x less dependencies.
npm WARN deprecated source-map-resolve@0.5.3: See https://github.com/lydell/source-map-resolve#deprecated
npm WARN deprecated urix@0.1.0: Please see https://github.com/lydell/urix#deprecated
npm WARN deprecated source-map-url@0.4.1: See https://github.com/lydell/source-map-url#deprecated
npm WARN deprecated resolve-url@0.2.1: https://github.com/lydell/resolve-url#deprecated
npm WARN deprecated fsevents@1.2.13: fsevents 1 will break on node v14+ and could be using insecure binaries. Upgrade to fsevents 2.
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@^1.2.7 (node_modules\chokidar\node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.13: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})
npm WARN gulpdemo@1.0.0 No description
npm WARN gulpdemo@1.0.0 No repository field.

+ gulp@4.0.2
added 327 packages from 226 contributors in 40.832s
PS F:\nodeProjects\gulpdemo> gulp --version
CLI version: 2.3.0
Local version: 4.0.2
```

## 创建 gulpfile 文件

```js
function defaultTask(cb) {
	// place code for your default task here
	console.log('gulp 默认任务')
	cb();
}

exports.default = defaultTask
```

## 测试

在项目根目录下执行 gulp 命令：

```shell
PS F:\nodeProjects\gulpdemo> gulp
[10:39:26] Using gulpfile F:\nodeProjects\gulpdemo\gulpfile.js
[10:39:26] Starting 'default'...
gulp 默认任务
[10:39:26] Finished 'default' after 23 ms
```

如需运行多个任务（task），可以执行 `gulp <task> <othertask>`

# Gulpfile 详解

gulpfile 是项目目录下名为 **gulpfile.js** （或者首字母大写 **Gulpfile.js**，就像 Makefile 一样命名）的文件，在运行 `gulp`
命令时会被自动加载。在这个文件中，你经常会看到类似 `src()`、`dest()`、`series()` 或 `parallel()` 函数之类的 gulp API，除此之外，纯 JavaScript 代码或 Node
模块也会被使用。任何导出（export）的函数都将注册到 gulp 的任务（task）系统中。

# Gulpfile 转译

你可以使用需要转译的编程语言来书写 gulpfile 文件，例如 TypeScript 或 Babel，通过修改 gulpfile.js 文件的扩展名来表明所用的编程语言并安装对应的转译模块。

* 对于 TypeScript，重命名为 gulpfile.ts 并安装 [ts-node](https://www.npmjs.com/package/ts-node) 模块。
* 对于 Babel，重命名为 gulpfile.babel.js 并安装 [@babel/register](https://www.npmjs.com/package/@babel/register) 模块。

针对此功能的高级知识和已支持的扩展名的完整列表，请参考 [gulpfile 转译](https://www.gulpjs.com.cn/docs/documentation-missing) 文档。

# Gulpfile 分割

大部分用户起初是将所有业务逻辑都写到一个 gulpfile 文件中。随着文件的变大，可以将此文件重构为数个独立的文件。

每个任务（task）可以被分割为独立的文件，然后导入（import）到 gulpfile 文件中并组合。这不仅使事情变得井然有序，而且可以对每个任务（task）进行单独测试，或者根据条件改变组合。

Node 的模块解析功能允许你将 **gulpfile.js** 文件替换为同样命名为 **gulpfile.js** 的目录，该目录中包含了一个名为 **index.js** 的文件，该文件被当作 **gulpfile.js**
使用。并且，该目录中还可以包含各个独立的任务（task）模块。

# 创建任务（task）

每个 gulp 任务（task）都是一个异步的 JavaScript 函数，此函数是一个可以接收 callback 作为参数的函数，或者是一个返回 stream、promise、event emitter、child process 或
observable 类型值的函数。由于某些平台的限制而不支持异步任务，因此 gulp
还提供了一个漂亮 [替代品](https://www.gulpjs.com.cn/docs/getting-started/async-completion#using-async-await) .

## 导出任务

任务（tasks）可以是 **public（公开）** 或 **private（私有）** 类型的。

* 公开任务（Public tasks）: 从 gulpfile 中被导出（export），可以通过 `gulp` 命令直接调用。
* 私有任务（Private tasks）: 被设计为在内部使用，通常作为 `series()` 或 `parallel()` 组合的组成部分。

一个私有（private）类型的任务（task）在外观和行为上和其他任务（task）是一样的，但是不能够被用户直接调用。如需将一个任务（task）注册为公开（public）类型的，只需从 gulpfile 中导出（export）即可。

```js
const {series} = require('gulp');

// `clean` 函数并未被导出（export），因此被认为是私有任务（private task）。
// 它仍然可以被用在 `series()` 组合中。
function clean(cb) {
	// body omitted
	cb();
}

// `build` 函数被导出（export）了，因此它是一个公开任务（public task），并且可以被 `gulp` 命令直接调用。
// 它也仍然可以被用在 `series()` 组合中。
function build(cb) {
	// body omitted
	cb();
}

exports.build = build;
exports.default = series(clean, build);
```

_在以前的 gulp 版本中，`task()` 方法用来将函数注册为任务（task）。虽然这个 API 依旧是可以使用的，但是 导出（export）将会是主要的注册机制，除非遇到 export 不起作用的情况。_

## 组合任务

Gulp 提供了两个强大的组合方法： `series()` 和 `parallel()`，允许将多个独立的任务组合为一个更大的操作。这两个方法都可以接受任意数目的任务（task）函数或已经组合的操作。`series()`
和 `parallel()` 可以互相嵌套至任意深度。

如果需要让任务（task）按顺序执行，请使用 `series()` 方法。

```js
const {series} = require('gulp');

function transpile(cb) {
	// body omitted
	cb();
}

function bundle(cb) {
	// body omitted
	cb();
}

exports.build = series(transpile, bundle);
```

对于希望以最大并发来运行的任务（tasks），可以使用 `parallel()` 方法将它们组合起来。

```js
const {parallel} = require('gulp');

function javascript(cb) {
	// body omitted
	cb();
}

function css(cb) {
	// body omitted
	cb();
}

exports.build = parallel(javascript, css);
```

当 `series()` 或 `parallel()` 被调用时，任务（tasks）被立即组合在一起。这就允许在组合中进行改变，而不需要在单个任务（task）中进行条件判断。

```js
const {series} = require('gulp');

function minify(cb) {
	// body omitted
	cb();
}


function transpile(cb) {
	// body omitted
	cb();
}

function livereload(cb) {
	// body omitted
	cb();
}

if (process.env.NODE_ENV === 'production') {
	exports.build = series(transpile, minify);
} else {
	exports.build = series(transpile, livereload);
}
```

`series()` 和 `parallel()` 可以被嵌套到任意深度。

```js
const {series, parallel} = require('gulp');

function clean(cb) {
	// body omitted
	cb();
}

function cssTranspile(cb) {
	// body omitted
	cb();
}

function cssMinify(cb) {
	// body omitted
	cb();
}

function jsTranspile(cb) {
	// body omitted
	cb();
}

function jsBundle(cb) {
	// body omitted
	cb();
}

function jsMinify(cb) {
	// body omitted
	cb();
}

function publish(cb) {
	// body omitted
	cb();
}

exports.build = series(
	clean,
	parallel(
		cssTranspile,
		series(jsTranspile, jsBundle)
	),
	parallel(cssMinify, jsMinify),
	publish
);
```

当一个组合操作执行时，这个组合中的每一个任务每次被调用时都会被执行。例如，在两个不同的任务（task）之间调用的 `clean` 任务（task）将被执行两次，并且将导致不可预期的结果。因此，最好重构组合中的 `clean`
任务（task）。

如果你有如下代码：

```js
// This is INCORRECT
const {series, parallel} = require('gulp');

const clean = function (cb) {
	// body omitted
	cb();
};

const css = series(clean, function (cb) {
	// body omitted
	cb();
});

const javascript = series(clean, function (cb) {
	// body omitted
	cb();
});

exports.build = parallel(css, javascript);
```

重构为：

```js
const {series, parallel} = require('gulp');

function clean(cb) {
	// body omitted
	cb();
}

function css(cb) {
	// body omitted
	cb();
}

function javascript(cb) {
	// body omitted
	cb();
}

exports.build = series(clean, parallel(css, javascript));
```

# 异步执行

Node 库以多种方式处理异步功能。最常见的模式是 [error-first callbacks](https://nodejs.org/api/errors.html#errors_error_first_callbacks)
，但是你还可能会遇到 [streams](https://nodejs.org/api/stream.html#stream_stream)
、[promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
、[event emitters](https://nodejs.org/api/events.html#events_events)
、[child processes](https://nodejs.org/api/child_process.html#child_process_child_process) ,
或 [observables](https://github.com/tc39/proposal-observable/blob/master/README.md) 。gulp 任务（task）规范化了所有这些类型的异步功能。

# 任务（task）完成通知

当从任务（task）中返回 stream、promise、event emitter、child process 或 observable 时，成功或错误值将通知 gulp 是否继续执行或结束。如果任务（task）出错，gulp
将立即结束执行并显示该错误。

当使用 `series()` 组合多个任务（task）时，任何一个任务（task）的错误将导致整个任务组合结束，并且不会进一步执行其他任务。当使用 `parallel()`
组合多个任务（task）时，一个任务的错误将结束整个任务组合的结束，但是其他并行的任务（task）可能会执行完，也可能没有执行完。

### 返回 stream

```js
const {src, dest} = require('gulp');

function streamTask() {
	return src('*.js')
		.pipe(dest('output'));
}

exports.default = streamTask;
```

### 返回 promise

```js
function promiseTask() {
	return Promise.resolve('the value is ignored');
}

exports.default = promiseTask;
```

### 返回 event emitter

```js
const {EventEmitter} = require('events');

function eventEmitterTask() {
	const emitter = new EventEmitter();
	// Emit has to happen async otherwise gulp isn't listening yet
	setTimeout(() => emitter.emit('finish'), 250);
	return emitter;
}

exports.default = eventEmitterTask;
```

### 返回 child process

```js
const {exec} = require('child_process');

function childProcessTask() {
	return exec('date');
}

exports.default = childProcessTask;
```

### 返回 observable

```js
const {Observable} = require('rxjs');

function observableTask() {
	return Observable.of(1, 2, 3);
}

exports.default = observableTask;
```

### 使用 callback

如果任务（task）不返回任何内容，则必须使用 callback 来指示任务已完成。在如下示例中，callback 将作为唯一一个名为 `cb()` 的参数传递给你的任务（task）。

```js
function callbackTask(cb) {
	// `cb()` should be called by some async work
	cb();
}

exports.default = callbackTask;
```

如需通过 callback 把任务（task）中的错误告知 gulp，请将 `Error` 作为 callback 的唯一参数。

```js
function callbackError(cb) {
	// `cb()` should be called by some async work
	cb(new Error('kaboom'));
}

exports.default = callbackError;
```

然而，你通常会将此 callback 函数传递给另一个 API ，而不是自己调用它。

```js
const fs = require('fs');

function passingCallback(cb) {
	fs.access('gulpfile.js', cb);
}

exports.default = passingCallback;
```

## gulp 不再支持同步任务（Synchronous tasks）

gulp 不再支持同步任务（Synchronous tasks）了。因为同步任务常常会导致难以调试的细微错误，例如忘记从任务（task）中返回 stream。

当你看到 "Did you forget to signal async completion?" 警告时，说明你并未使用前面提到的返回方式。你需要使用 callback 或返回 stream、promise、event
emitter、child process、observable 来解决此问题。

## 使用 async/await

如果不使用前面提供到几种方式，你还可以将任务（task）定义为一个 `async` 函数，它将利用 promise 对你的任务（task）进行包装。这将允许你使用 `await` 处理 promise，并使用其他同步代码。

```js
const fs = require('fs');

async function asyncAwaitTask() {
	const {version} = fs.readFileSync('package.json');
	console.log(version);
	await Promise.resolve('some result');
}

exports.default = asyncAwaitTask;
```

# 处理文件

gulp 暴露了 `src()` 和 `dest()` 方法用于处理计算机上存放的文件。

`src()` 接受 glob 参数，并从文件系统中读取文件然后生成一个 Node 流（stream）。它将所有匹配的文件读取到内存中并通过流（stream）进行处理。

由 `src() `产生的流（stream）应当从任务（task）中返回并发出异步完成的信号，就如 创建任务（task） 文档中所述。

```js
const {src, dest} = require('gulp');

exports.default = function () {
	return src('src/*.js')
		.pipe(dest('output/'));
}
```

流（stream）所提供的主要的 API 是 `.pipe()` 方法，用于连接转换流（Transform streams）或可写流（Writable streams）。

```js
const {src, dest} = require('gulp');
const babel = require('gulp-babel');

exports.default = function () {
	return src('src/*.js')
		.pipe(babel())
		.pipe(dest('output/'));
}
```

`dest()` 接受一个输出目录作为参数，并且它还会产生一个 [Node 流（stream）](https://nodejs.org/api/stream.html) ，通常作为终止流（terminator
stream）。当它接收到通过管道（pipeline）传输的文件时，它会将文件内容及文件属性写入到指定的目录中。gulp 还提供了 `symlink()` 方法，其操作方式类似 `dest()`，但是创建的是链接而不是文件（
详情请参阅 [symlink()](https://www.gulpjs.com.cn/docs/api/symlink) ）。

大多数情况下，利用 `.pipe()` 方法将插件放置在 `src()` 和 `dest()` 之间，并转换流（stream）中的文件。

## 向流（stream）中添加文件

`src()` 也可以放在管道（pipeline）的中间，以根据给定的 glob
向流（stream）中添加文件。新加入的文件只对后续的转换可用。如果 [glob 匹配的文件与之前的有重复](https://www.gulpjs.com.cn/docs/getting-started/explaining-globs#overlapping-globs)
，仍然会再次添加文件。

这对于在添加普通的 JavaScript 文件之前先转换部分文件的场景很有用，添加新的文件后可以对所有文件统一进行压缩并混淆（uglifying）。

```js
const {src, dest} = require('gulp');
const babel = require('gulp-babel');
const uglify = require('gulp-uglify');

exports.default = function () {
	return src('src/*.js')
		.pipe(babel())
		.pipe(src('vendor/*.js'))
		.pipe(uglify())
		.pipe(dest('output/'));
}
```

## 分阶段输出

`dest()` 可以用在管道（pipeline）中间用于将文件的中间状态写入文件系统。当接收到一个文件时，当前状态的文件将被写入文件系统，文件路径也将被修改以反映输出文件的新位置，然后该文件继续沿着管道（pipeline）传输。

此功能可用于在同一个管道（pipeline）中创建未压缩（unminified）和已压缩（minified）的文件。

```js
const {src, dest} = require('gulp');
const babel = require('gulp-babel');
const uglify = require('gulp-uglify');
const rename = require('gulp-rename');

exports.default = function () {
	return src('src/*.js')
		.pipe(babel())
		.pipe(src('vendor/*.js'))
		.pipe(dest('output/'))
		.pipe(uglify())
		.pipe(rename({extname: '.min.js'}))
		.pipe(dest('output/'));
}
```

## 模式：流动（streaming）、缓冲（buffered）和空（empty）模式

`src()` 可以工作在三种模式下：缓冲（buffering）、流动（streaming）和空（empty）模式。这些模式可以通过对 `src()` 的 `buffer`
和 `read` [参数](https://www.gulpjs.com.cn/docs/api/src#options) 进行设置。

* 缓冲（Buffering）模式是默认模式，将文件内容加载内存中。插件通常运行在缓冲（buffering）模式下，并且许多插件不支持流动（streaming）模式。
* 流动（Streaming）模式的存在主要用于操作无法放入内存中的大文件，例如巨幅图像或电影。文件内容从文件系统中以小块的方式流式传输，而不是一次性全部加载。如果需要流动（streaming）模式，请查找支持此模式的插件或自己编写。
* 空（Empty）模式不包含任何内容，仅在处理文件元数据时有用。

# Glob 详解

glob 是由普通字符和/或通配字符组成的字符串，用于匹配文件路径。可以利用一个或多个 glob 在文件系统中定位文件。

`src()` 方法接受一个 glob 字符串或由多个 glob 字符串组成的数组作为参数，用于确定哪些文件需要被操作。glob 或 glob 数组必须至少匹配到一个匹配项，否则 `src()` 将报错。当使用 glob 数组时，将按照每个
glob 在数组中的位置依次执行匹配 - 这尤其对于取反（negative） glob 有用。

## 字符串片段与分隔符

字符串片段（segment）是指两个分隔符之间的所有字符组成的字符串。在 glob 中，分隔符永远是 `/` 字符 - 不区分操作系统 - 即便是在采用 `\\` 作为分隔符的 Windows 操作系统中。在 glob 中，`\\`
字符被保留作为转义符使用。

如下， `*` 被转义了，因此，`*` 将被作为一个普通字符使用，而不再是通配符了。

```
'glob_with_uncommon_\\*_character.js'
```

避免使用 Node 的 `path` 类方法来创建 glob，例如 `path.join`。在 Windows 中，由于 Node 使用 `\\` 作为路径分隔符，因此将会产生一个无效的 glob。还要避免使用 `__dirname`
和 `__filename` 全局变量，由于同样的原因，`process.cwd()` 方法也要避免使用。

```js
const invalidGlob = path.join(__dirname, 'src/*.js');
```

## 特殊字符： * (一个星号)

在一个字符串片段中匹配任意数量的字符，包括零个匹配。对于匹配单级目录下的文件很有用。

下面这个 glob 能够匹配类似 `index.js` 的文件，但是不能匹配类似 `scripts/index.js` 或 `scripts/nested/index.js` 的文件。

```
'*.js'
```

## 特殊字符： ** (两个星号)

在多个字符串片段中匹配任意数量的字符，包括零个匹配。 对于匹配嵌套目录下的文件很有用。请确保适当地限制带有两个星号的 glob 的使用，以避免匹配大量不必要的目录。

下面这个 glob 被适当地限制在 `scripts/` 目录下。它将匹配类似 `scripts/index.js`、`scripts/nested/index.js` 和 `scripts/nested/twice/index.js`
的文件。

```
'scripts/**/*.js'
```

在上面的示例中，如果没有 `scripts/` 这个前缀做限制，`node_modules` 目录下的所有目录或其他目录也都将被匹配。

## 特殊字符： ! (取反)

由于 glob 匹配时是按照每个 glob 在数组中的位置依次进行匹配操作的，所以 glob 数组中的取反（negative）glob 必须跟在一个非取反（non-negative）的 glob 后面。第一个 glob
匹配到一组匹配项，然后后面的取反 glob 删除这些匹配项中的一部分。如果取反 glob 只是由普通字符组成的字符串，则执行效率是最高的。

```
['script/**/*.js', '!scripts/vendor/']
```

如果任何非取反（non-negative）的 glob 跟随着一个取反（negative） glob，任何匹配项都不会被删除。

```
['script/**/*.js', '!scripts/vendor/', 'scripts/vendor/react.js']
```

取反（negative） glob 可以作为对带有两个星号的 glob 的限制手段。

```
['**/*.js', '!node_modules/']
```

在上面的示例中，如果取反（negative）glob 是 `!node_modules/**/*.js`，那么各匹配项都必须与取反 glob 进行比较，这将导致执行速度极慢。

## 匹配重叠（Overlapping globs）

两个或多个 glob 故意或无意匹配了相同的文件就被认为是匹配重叠（overlapping）了。如果在同一个 `src()` 中使用了会产生匹配重叠的 glob，gulp 将尽力去除重叠部分，但是在多个 `src()`
调用时产生的匹配重叠是不会被去重的。

# 使用插件

Gulp 插件实质上是 [Node 转换流（Transform Streams）](https://github.com/rvagg/through2)
，它封装了通过管道（pipeline）转换文件的常见功能，通常是使用 `.pipe()` 方法并放在 `src()` 和 `dest()` 之间。他们可以更改经过流（stream）的每个文件的文件名、元数据或文件内容。

托管在 npm 上的插件 - 标记有 "gulpplugin" 和 "gulpfriendly" 关键词 - 可以在 [插件搜索页面](https://gulpjs.com/plugins/) 上浏览和搜索。

每个插件应当只完成必要的工作，因此你可以把它们像构建块一样连接在一起。获得想要的结果可能需要把一组插件组合在一起使用。

```js
const {src, dest} = require('gulp');
const uglify = require('gulp-uglify');
const rename = require('gulp-rename');

exports.default = function () {
	return src('src/*.js')
		// gulp-uglify 插件并不改变文件名
		.pipe(uglify())
		// 因此使用 gulp-rename 插件修改文件的扩展名
		.pipe(rename({extname: '.min.js'}))
		.pipe(dest('output/'));
}
```

## 是否需要插件？

并非 gulp 中的一切都需要用插件来完成。虽然它们是一种快速上手的方法，但许多操作都应当通过使用独立的功能模块或库来实现。

```js
const {rollup} = require('rollup');

// Rollup 提供了基于 promise 的 API，在 `async` 任务（task）中工作的很好
exports.default = async function () {
	const bundle = await rollup.rollup({
		input: 'src/index.js'
	});

	return bundle.write({
		file: 'output/bundle.js',
		format: 'iife'
	});
}
```

插件应当总是用来转换文件的。其他操作都应该使用（非插件的） Node 模块或库来实现。

```js
const del = require('delete');

exports.default = function (cb) {
	// 直接使用 `delete` 模块，避免使用 gulp-rimraf 插件
	del(['output/*.js'], cb);
}
```

## 条件插件

因为插件的操作不应该针对特定文件类型，因此你可能需要使用像 [gulp-if](https://www.npmjs.com/package/gulp-if) 之类的插件来完成转换某些文件的操作。

```js
const {src, dest} = require('gulp');
const gulpif = require('gulp-if');
const uglify = require('gulp-uglify');

function isJavaScript(file) {
	// 判断文件的扩展名是否是 '.js'
	return file.extname === '.js';
}

exports.default = function () {
	// 在同一个管道（pipeline）上处理 JavaScript 和 CSS 文件
	return src(['src/*.js', 'src/*.css'])
		// 只对 JavaScript 文件应用 gulp-uglify 插件
		.pipe(gulpif(isJavaScript, uglify()))
		.pipe(dest('output/'));
}
```

## 内联插件（Inline plugins）

内联插件是一次性的转换流（Transform Streams），你可以通过在 gulpfile 文件直接书写需要的功能。

在两种情况下，创建内联插件很有用：

* 避免自己创建并维护插件。
* 避免 fork 一个已经存在的插件并添加自己所需的功能。

```js
const {src, dest} = require('gulp');
const uglify = require('uglify-js');
const through2 = require('through2');

exports.default = function () {
	return src('src/*.js')
		// 创建一个内联插件，从而避免使用 gulp-uglify 插件
		.pipe(through2.obj(function (file, _, cb) {
			if (file.isBuffer()) {
				const code = uglify.minify(file.contents.toString())
				file.contents = Buffer.from(code)
			}
			cb(null, file);
		}))
		.pipe(dest('output/'));
}
```

# 文件监控

gulp api 中的 `watch()` 方法利用文件系统的监控程序（file system watcher）将 [globs](https://www.gulpjs.com.cn/docs/getting-started/explaining-globs) 与 [任务（task）](https://www.gulpjs.com.cn/docs/getting-started/creating-tasks) 进行关联。它对匹配 glob 的文件进行监控，如果有文件被修改了就执行关联的任务（task）。如果被执行的任务（task）没有触发 [异步完成](https://www.gulpjs.com.cn/docs/getting-started/async-completion) 信号，它将永远不会再次运行了。

此 API 的默认设置是基于通常的使用场景的，而且提供了内置的延迟和排队机制。

```js
const { watch, series } = require('gulp');

function clean(cb) {
  // body omitted
  cb();
}

function javascript(cb) {
  // body omitted
  cb();
}

function css(cb) {
  // body omitted
  cb();
}

// 可以只关联一个任务
watch('src/*.css', css);
// 或者关联一个任务组合
watch('src/*.js', series(clean, javascript));
```

## 警告：避免同步任务

就像注册到任务系统中的任务（task）一样，与文件监控程序关联的任务（task）不能是同步任务（synchronous taks）。如果你将同步任务（task）关联到监控程序，则无法确定任务（task）的完成情况，任务（task）将不会再次运行（假定当前正在运行）。

由于文件监控程序会让你的 Node 进程保持持续运行，因此不会有错误或警告产生。由于进程没有退出，因此无法确定任务（task）是否已经完成还是运行了很久很久了。

## 可监控的事件

默认情况下，只要创建、更改或删除文件，文件监控程序就会执行关联的任务（task）。 如果你需要使用不同的事件，你可以在调用 `watch()` 方法时通过 `events` 参数进行指定。可用的事件有 `'add'`、`'addDir'`、`'change'`、`'unlink'`、`'unlinkDir'`、`'ready'`、`'error'`。此外，还有一个 `'all'` 事件，它表示除 `'ready'` 和 `'error'` 之外的所有事件。

```js
const { watch } = require('gulp');

// 所有事件都将被监控
watch('src/*.js', { events: 'all' }, function(cb) {
  // body omitted
  cb();
});
```

## 初次执行

调用 `watch()` 之后，关联的任务（task）是不会被立即执行的，而是要等到第一次文件修之后才执行。

如需在第一次文件修改之前执行，也就是调用 `watch()` 之后立即执行，请将 `ignoreInitial` 参数设置为 `false`。

```js
const { watch } = require('gulp');

// 关联的任务（task）将在启动时执行
watch('src/*.js', { ignoreInitial: false }, function(cb) {
  // body omitted
  cb();
});
```

## 队列

`watch()` 方法能够保证当前执行的任务（task）不会再次并发执行。当文件监控程序关联的任务（task）正在运行时又有文件被修改了，那么所关联任务的这次新的执行将被放到执行队列中等待，直到上一次关联任务执行完之后才能运行。每一次文件修改只产生一次关联任务的执行并放入队列中。

如需禁止队列，请将 `queue` 参数设置为 `false`。

```js
const { watch } = require('gulp');

// 每次文件修改之后关联任务都将执行（有可能并发执行）
watch('src/*.js', { queue: false }, function(cb) {
  // body omitted
  cb();
});
```

## 延迟

文件更改之后，只有经过 200 毫秒的延迟之后，文件监控程序所关联的任务（task）才会被执行。这是为了避免在同使更改许多文件时（例如查找和替换操作）过早启动任务（taks）的执行。

如需调整延迟时间，请为 `delay` 参数设置一个正整数。

```js
const { watch } = require('gulp');

// 文件第一次修改之后要等待 500 毫秒才执行关联的任务
watch('src/*.js', { delay: 500 }, function(cb) {
  // body omitted
  cb();
});
```

## 使用监控程序实例

你可能不会使用到此功能，但是如果你需要对被修改的文件进行完全的掌控 （例如访问文件路径或元数据）请使用从 `watch()` 返回的 [chokidar](https://www.npmjs.com/package/chokidar) 实例。

注意： 返回的 chokidar 实例没有队列、延迟和异步完成（async completion）这些功能。

## 可选的依赖项

Gulp 有一个名为 [fsevents](https://www.npmjs.com/package/fsevents) 的可选依赖项，他是一个特定于 Mac 系统的文件监控程序。如果你看到安装 fsevents 时出现的警告信息 - "npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents" - 这并不是什么问题，忽略即可。 如果跳过 fsevents 的安装，将使用一个备用文件监控程序，后续在 gulpfile 中产生的任何错误都将与此警告无关。

