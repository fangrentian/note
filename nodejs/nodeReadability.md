# 介绍

把任何网页变成一个干净的视图,提取标题和内容。这个模块是基于arc90的可读性项目。

# 特点

* 优化更多网站
* 支持HTML5标签(`<article>`, `<section>`)和MicrodataAPI
* 同时注重准确性和性能。比arc90版本快4倍
* 支持GBK、GB2312等编码
* 将图片和链接的相对url自动转换为绝对url

# 安装

```shell
npm install node-readability
```

# 使用 read(html [, options], callback)

* html: url 或 html.
* options: 配置对象
* callback: 接收三个参数 `callback(error, article, meta)`

```js
var read = require('node-readability');

read('http://howtonode.org/really-simple-file-uploads', function (err, article, meta) {
	// Main Article
	console.log(article.content);
	// Title
	console.log(article.title);

	// HTML Source Code
	console.log(article.html);
	// DOM
	console.log(article.document);

	// Response Object from Request Lib
	console.log(meta);

	// Close article to clean up jsdom and prevent leaks
	article.close();
});
```

注意:如果页面标记了utf-8以外的字符集，它将被自动转换。还支持GBK、GB2312等字符集。

# 配置选项

node-readability直接将配置选项传递给请求.

* cleanRulers: 允许为标签设置自己的验证规则

如果true,规则有效，否则无效。`options.cleanRulers = [callback(obj, tagName)]`

```js
read(url, {
	cleanRulers: [
		function (obj, tag) {
			if (tag === 'object') {
				if (obj.getAttribute('class') === 'BrightcoveExperience') {
					return true;
				}
			}
		}
	]
}, function (err, article, response) {
	//...
});
```

* preprocess:
  是一个函数,在将下载的源代码传递给node-readability之前，检查或修改它。`options.preprocess = callback(source, response, contentType, callback);`

```js
read(url, {
	preprocess: function (source, response, contentType, callback) {
		if (source.length > maxBodySize) {
			return callback(new Error('too big'));
		}
		callback(null, source);
	}, function(err, article, response) {
		//...
	});
```

# 回调函数中的article参数

* content: 文章内容的网页。如果失败返回false

* title: 网页的文章标题。它可能与<title>标签中的文本不相同

* textBody: 包含页面上找到的所有文本的字符串

* html: 网页的原始html

* document: 由jsdom生成的网页文档。您可以使用它直接访问DOM(例如, `article.document.getElementById('main')`).

# 回调函数中的meta参数

请求库中的响应对象。如果你需要得到当前url后，所有重定向或获得一些头信息，这将是很有用的。