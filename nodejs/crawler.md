# 简介

node-crawler 是一个轻量级的node.js爬虫工具，兼顾了高效与便利性，支持分布式爬虫系统，支持硬编码，支持http前级代理。

node-crawler 完全由nodejs写成，天生支持非阻塞异步IO，为爬虫的流水线作业机制提供了极大便利。同时支持对 DOM 的快速选择，对于抓取网页的特定部分的任务可以说是杀手级功能，无需再手写正则表达式，提高爬虫开发效率。

# 特点

* DOM 元素快速解析 & 符合jQuery语法的选择器功能(默认使用Cheerio，支持更换为 JSDOM 等其它DOM解析器)
* 支持连接池模式，并发数和重连数均可配置
* 支持请求队列的优先权（即不同URL的请求能有不同的优先级）
* 支持延时功能（某些服务器对每分钟内连接数有限制）
* 支持 forceUTF8 模式以应对复杂的编码问题，当然你也可以自己为不同的连接设置编码
* 支持4.x及更高版本的Nodejs

# 快速上手

## 安装

```shell
npm install crawler
```

## 最简单的使用实例

```js
var Crawler = require("crawler");

var c = new Crawler({
    // 在每个请求处理完毕后将调用此回调函数
    callback : function (error, res, done) {
        if(error){
            console.log(error);
        }else{
            var $ = res.$;
            // $ 默认为 Cheerio 解析器
            // 它是核心jQuery的精简实现，可以按照jQuery选择器语法快速提取DOM元素
            console.log($("title").text());
        }
        done();
    }
});

// 将一个URL加入请求队列，并使用默认回调函数
c.queue('http://www.amazon.com');

// 将多个URL加入请求队列
c.queue(['http://www.google.com/','http://www.yahoo.com']);

// 对单个URL使用特定的处理参数并指定单独的回调函数
c.queue([{
    uri: 'http://parishackers.org/',
    jQuery: false,

    // The global callback won't be called
    callback: function (error, res, done) {
        if(error){
            console.log(error);
        }else{
            console.log('Grabbed', res.body.length, 'bytes');
        }
        done();
    }
}]);

// 将一段HTML代码加入请求队列，即不通过抓取，直接交由回调函数处理（可用于单元测试）
c.queue([{
    html: '<p>This is a <strong>test</strong></p>'
}]);
```

## 并发控制及慢速模式

使用参数 maxConnections 可控制最大并发数。

```js
var Crawler = require("crawler");

var c = new Crawler({
    // 最大并发数默认为10
    maxConnections : 1,

    callback : function (error, res, done) {
        if(error){
            console.log(error);
        }else{
            var $ = res.$;
            console.log($("title").text());
        }
        done();
    }
});

c.queue('http://www.amazon.com');
```

使用参数 rateLimit 启用慢速模式，两次请求之间会闲置 rateLimit 毫秒，而 maxConnections 将被强行修改为 1 。

```js
var Crawler = require("crawler");

var c = new Crawler({
    // `maxConnections` 将被强制修改为 1
    maxConnections : 10,

    // 两次请求之间将闲置1000ms
    rateLimit: 1000,

    callback : function (error, res, done) {
        if(error){
            console.log(error);
        }else{
            var $ = res.$;
            console.log($("title").text());
        }
        done();
    }
});

c.queue(['http://www.google.com/','http://www.yahoo.com']);
```

# 高级功能

## 自定义参数

在很多时候，一次爬取过程会由几次网页抓取组成。例如，抓取某新闻站点所有新闻，需要先爬取列表页获得新闻ID，再根据ID构造新闻详细页的URL进行抓取，并最后将新闻以ID-新闻内容的key-value形式存储。为了应对这种情况，Node-Crawler 允许自定义参数，所有参数均存储在res.options，并可在callback函数中获得。

```js
var Crawler = require("crawler");

var c = new Crawler({
    callback : function (error, res, done) {
        if(error){
            console.log(error);
        }else{
            var $ = res.$;
            console.log($("title").text());
            // 获取输入的自定义参数并打印。
            console.log(res.options.parameter1);
        }
        done();
    }
});

c.queue({
    uri:"http://www.baidu.com",
    parameter1:"value1",
    parameter2:"value2",
    parameter3:"value3"
});
```

## 使用http代理

Node-Crawler 支持http代理，以应对在极端情况下，爬虫运行的节点IP可能无法访问服务器的情况。代理服务器输入格式需要符合 RFC1738 规范，即 scheme://user:pass@host:port 的形式。

```js
var Crawler = require("crawler");

var c = new Crawler({
    callback : function (error, res, done) {
        if(error){
            console.log(error);
        }else{
            var $ = res.$;
            console.log($("title").text());
        }
        done();
    }
});

c.queue({
    uri:"http://www.baidu.com",
    proxy:"http://user:pass@host:port"
});
```

## 处理原始返回数据

在爬取图片、PDF文档等二进制文件时，需要对服务器返回的原始数据进行处理，此时需通过指定 encoding 参数为 null 来禁止 Node-Crawler 将其转换为字节流，同时也需要将 jQuery 参数指定为 false 来禁止 DOM 元素提取。 下面是一个抓取图片并保存为本地图片的例子。

```js
var Crawler = require("crawler");
var fs      = require("fs");

var c = new Crawler({
    encoding:null,
    jQuery:false,// set false to suppress warning message.
    callback:function(err, res, done){
        if(err){
            console.error(err.stack);
        }else{
            fs.createWriteStream(res.options.filename).write(res.body);
        }

        done();
    }
});

c.queue({
    uri:"https://nodejs.org/static/images/logos/nodejs-1920x1200.png",
    filename:"nodejs-1920x1200.png"
});
```

# Crawler参数手册

如果你想修改一些默认值，可以在构造 Crawler() 的时候配置相关的参数，此时的参数将在全局范围内生效。如果你只想对单个请求配置独立的参数，你可以在调用 queue() 函数时覆盖参数。

Crawler 使用了 [request库](https://github.com/mikeal/request#requestoptions-callback) ，所以 Crawler 可供配置的参数列表是 request 库的参数列表的超集，即 request 库中所有的配置在 Crawler 中均适用。

## 基本请求参数

* uri: String 想要爬取的站点的URL
* timeout : Number 超时时间（单位：毫秒，默认值15000）
* 所有符合request包的参数均能兼容

## 回调函数 callback(error, res, done)

请求完成后将被调用的回调函数

* error: Error 错误信息
* res: http.IncomingMessage 服务器响应的数据资源合集
    * res.statusCode: Number HTTP状态码，正常返回 200
    * res.body: Buffer | String HTTP响应实体部分
    * res.headers: Object HTTP响应头部
    * res.request: Request Request 的一个实例（并非 http.ClientRequest）
        * res.request.uri: urlObject 处理后的URL的HTTP请求实体
        * res.request.method: String HTTP 请求方法,例如 GET,POST
        * res.request.headers: Object HTTP 请求头部
    * res.options: Options 此次请求的参数
    * $: jQuery Selector HTML或XML文档的jQuery选择器
* done: Function 在处理结束之后必须调用此函数

## 调度参数

* maxConnections: Number 工作线程池的大小 (默认为 10).
* rateLimit: Number 两次请求之间的默认间隔时间 (默认为 0).
* priorityRange: Number 有效的优先值范围，从0开始 (默认为 10).
* priority: Number 请求的默认优先值 (默认为 5). 此值越低优先度越高.

## 重试参数

retries: Number 请求失败后的重试次数 (默认为 3),
retryTimeout: Number 重试的默认等待时间，单位为毫秒 (默认为 10000).

## DOM选项

* jQuery: Boolean|String|Object 设置为true或者"cheerio"时，将使用 cheerio 作为解析器，并使用默认配置. 当然你也可以使用自定义配置的 cheerio 或使用符合 Parser options 的其它解析器。如果此值为false，将禁用注入jQuery选择器。 注意，如果你发现项目存在内存泄漏问题，请使用 "whacko" 作为解析器来避免这个问题你, "whacko" 是一个可以用来代替的解析器。 (默认为 true)

## 编码设置

* forceUTF8: Boolean 此值为 true 时，crawler 将会从HTTP头部或者meta标签提取字符编码，并将其强制转换为 UTF8格式。 再也不用为不同编码的问题操心了！ (默认为 true),
* incomingEncoding: String 一般和forceUTF8一起使用，设定此值将可以手动指定待转换的字符编码（默认为 null）。可以像这么使用： incomingEncoding : 'windows-1255'。这里是支持的所有编码

## 缓存设置

* skipDuplicates: Boolean 此值为 true 时，将跳过已经爬取的URI，且不会调用callback() (默认为 false)。 这不是推荐做法，更好的做法是在 Crawler 之外使用 seenreq 处理。

## 其它参数

* rotateUA: Boolean 此值为 true 时, userAgent 应该是一个UA的Array，此时Crawler将在不同的请求中轮换UA。(默认为 false)
* userAgent: String|[Array] 如果 rotateUA 为 false, 且 userAgent 为 array, crawler 将使用array中的第一个UA。
* referer: String 设定此值将可以修改HTTP referer header
* queue(uri, options): 将任务加入队列，并等待其被执行
    * uri String
    * options Options
* queueSize: Number 当前队列长度，只读。

