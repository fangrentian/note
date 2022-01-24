# 介绍

Puppeteer 是一个 Node 库，它提供了一个高级 API
来通过 [DevTools](http://www.puppeteerjs.com/(https://chromedevtools.github.io/devtools-protocol/)) 协议控制 Chromium 或
Chrome。Puppeteer 默认以 [headless](https://developers.google.com/web/updates/2017/04/headless-chrome)
模式运行，但是可以通过修改配置文件运行“有头”模式。

# 作用

你可以在浏览器中手动执行的绝大多数操作都可以使用 Puppeteer 来完成！ 下面是一些示例：

* 生成页面 PDF。

* 抓取 SPA（单页应用）并生成预渲染内容（即“SSR”（服务器端渲染））。

* 自动提交表单，进行 UI 测试，键盘输入等。

* 创建一个时时更新的自动化测试环境。 使用最新的 JavaScript 和浏览器功能直接在最新版本的Chrome中执行测试。

* 捕获网站的 timeline trace，用来帮助分析性能问题。

* 测试浏览器扩展。

[演示地址](https://try-puppeteer.appspot.com/)

# 安装

在项目中使用 Puppeteer：

```shell
npm i puppeteer
# "yarn add puppeteer"
```

Note: 当你安装 Puppeteer 时，它会下载最新版本的Chromium（~170MB Mac，~282MB Linux，~280MB Win），以保证可以使用 API。
如果想要跳过下载，请阅读[环境变量](https://github.com/GoogleChrome/puppeteer/blob/v1.10.0/docs/api.md#environment-variables)

# puppeteer-core

自 1.7.0 版本以来，我们都会发布一个 [puppeteer-core](https://www.npmjs.com/package/puppeteer-core) 包，这个包默认不会下载 Chromium。

```shell
npm i puppeteer-core
# "yarn add puppeteer-core"
```

uppeteer-core 是一个的轻量级的 Puppeteer 版本，用于启动现有浏览器安装或连接到远程安装。

具体见 [puppeteer vs puppeteer-core](https://github.com/GoogleChrome/puppeteer/blob/master/docs/api.md#puppeteer-vs-puppeteer-core)

# 使用

Note: Puppeteer 至少需要 Node v6.4.0，下面的示例使用 async / await，它们仅在 Node v7.6.0 或更高版本中被支持。

Puppeteer 使用起来和其他测试框架类似。你需要创建一个 Browser
实例，打开页面，然后使用 [Puppeteer 的 API](https://github.com/GoogleChrome/puppeteer/blob/v1.10.0/docs/api.md#)

## 举例1

跳转到 https://example.com 并保存截图至 example.png, 文件为 example.js:

```js
const puppeteer = require('puppeteer');

(async () => {
	const browser = await puppeteer.launch();
	const page = await browser.newPage();
	await page.goto('https://example.com');
	await page.screenshot({path: 'example.png'});

	await browser.close();
})();
```

在命令行中执行 `node example.js`;

Puppeteer 初始化的屏幕大小默认为 800px x
600px。但是这个尺寸可以通过 [Page.setViewport()](https://github.com/GoogleChrome/puppeteer/blob/v1.10.0/docs/api.md#pagesetviewportviewport)
设置。

## 举例2

创建一个 PDF,文件为 hn.js:

```js
const puppeteer = require('puppeteer');

(async () => {
	const browser = await puppeteer.launch();
	const page = await browser.newPage();
	await page.goto('https://news.ycombinator.com', {waitUntil: 'networkidle2'});
	await page.pdf({path: 'hn.pdf', format: 'A4'});

	await browser.close();
})();
```

在命令行执行`node hn.js`;

查看 [Page.pdf()](https://github.com/GoogleChrome/puppeteer/blob/v1.10.0/docs/api.md#pagepdfoptions) 了解跟多内容。

## 举例3

在页面中执行脚本,文件为 get-dimensions.js:

```js
const puppeteer = require('puppeteer');

(async () => {
	const browser = await puppeteer.launch();
	const page = await browser.newPage();
	await page.goto('https://example.com');

	// Get the "viewport" of the page, as reported by the page.
	const dimensions = await page.evaluate(() => {
		return {
			width: document.documentElement.clientWidth,
			height: document.documentElement.clientHeight,
			deviceScaleFactor: window.devicePixelRatio
		};
	});

	console.log('Dimensions:', dimensions);

	await browser.close();
})();
```

在命令行中执行`node get-dimensions.js`;

查看 [Page.evaluate()](https://github.com/GoogleChrome/puppeteer/blob/v1.10.0/docs/api.md#pageevaluatepagefunction-args) 了解更多相关内容，该方法有点类似于 `evaluateOnNewDocument` 和 `exposeFunction`。

# 默认设置

## 使用无头模式

Puppeteer 运行 Chromium 的[headless mode](https://developers.google.com/web/updates/2017/04/headless-chrome) 。如果想要使用完全版本的 Chromium，设置 ['headless' option](https://github.com/GoogleChrome/puppeteer/blob/v1.10.0/docs/api.md#puppeteerlaunchoptions) 即可。

```js
const browser = await puppeteer.launch({headless: false}); // default is true
```

## 运行绑定的 Chromium 版本

默认情况下，Puppeteer 下载并使用特定版本的 Chromium 以及其 API 保证开箱即用。 如果要将 Puppeteer 与不同版本的 Chrome 或 Chromium 一起使用，在创建Browser实例时传入 Chromium 可执行文件的路径即可：

```js
const browser = await puppeteer.launch({executablePath: '/path/to/Chrome'});
```

具体见：[Puppeteer.launch()](https://github.com/GoogleChrome/puppeteer/blob/v1.10.0/docs/api.md#puppeteerlaunchoptions)

[了解 Chromium 与 Chrome 的不同](https://www.howtogeek.com/202825/what%E2%80%99s-the-difference-between-chromium-and-chrome/)

## 创建用户配置文件

Puppeteer 会创建自己的 Chromium 用户配置文件，它会在每次运行时清理。