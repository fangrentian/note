# 什么是HTML

超文本标记语言（英语：**HyperText Markup Language**，简称：**HTML**）是一种用于创建网页的标准标记语言. HTML 运行在浏览器上，由浏览器来解析. HTML代码里面是通过标签对的方式给文本添加语义，HTML使用的标签也称为元素，标签分为单标签和双标签. 其主要功能是负责页面的结构.

**参考文档** 

[https://www.w3cschool.cn/html/dict](https://www.w3cschool.cn/html/dict)

[https://wangdoc.com/html/](https://wangdoc.com/html/)

# 发展历史

* 第一版 ——在1993年6月作为互联网工程工作小组（IETF）工作草案发布（并非标准）

* HTML 2.0——1995年11月作为RFC 1866发布，在RFC 2854于2000年6月发布之后被宣布已经过时

* HTML 3.2——1997年1月14日，W3C推荐标准

* HTML 4.0——1997年12月18日，W3C推荐标准

* HTML 4.01（微小改进）——1999年12月24日，W3C推荐标准

* HTML5 —— 2014年10月29日，万维网联盟宣布，经过接近8年的艰苦努力，该标准规范终于制定完成

# 开发中的一些约定

**在编辑HTML文件和使用有关标记符时有一些约定或默认的要求:**

* 文本标记语言源程序的文件扩展名默认使用htm或html, 在使用文本编辑器时，注意修改扩展名;

* HTML源程序为文本文件，其列宽可不受限制，即多个标记可写成一行，甚至整个文件可写成一行；若写成多行，浏览器一般忽略文件中的回车符（标记指定除外）；对文件中的空格通常也不按源程序中的效果显示。完整的空格可使用特殊符号 `&nbsp;` (注意此字母必须小写，方可空格)表示非换行空格，如需换行可以输入`<br/>`；表示文件路径时使用符号“/”分隔，文件名及路径描述可用双引号也可不用引号括起;

* 标记符中的标记元素用尖括号括起来，如："<"">"，带斜杠的元素表示该标记说明结束；大多数标记符必须成对使用，以表示作用的起始和结束；标记元素忽略大小写；许多标记元素具有属性说明，可用参数对元素作进一步的限定，多个参数或属性项说明次序不限，其间用空格分隔即可；一个标记元素的内容可以写成多行;

* 标记符号，包括尖括号、标记元素、属性项等必须使用半角的西文字符，而不能使用全角字符;

* 所有标签写在`<html>`标签中, 展示给用户看的内容写在`<body>`标签中,样式文件在`<head>`标签中通过`<link>`标签引入或写在`<style>`标签中, `<script>`标签在`</body>`之前引入;

# HTML文档的后缀名

html和htm都可以作为html文档的后缀名，两种后缀名没有区别，都可以使用。

# HTML的主体结构/骨架

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>基本框架</title>
</head>
<body>
  
</body>
</html>
```

## `<html>`标签

`<html>`标签是 **HTML** 的核心, 所有的 **HTML** 标签都应该写在这对标签当中, 虽然不这样写网页也能正常显示.

## `<head>`标签

主要写一些网页预定义属性，更准确一点来说叫做预定义信息，大部分信息不是给用户看的，而是给浏览器或者搜索引擎看的.

**`<head>标签中包含的常见标签`**

* `<title>`标签: 文档标题,内容应该贴合页面主题，能够让用户清楚的知道页面的主题，也能够方便SEO的搜索

* `<base>`标签: 可以预定义整个页面路径的前缀，也就是说定义默认路径，文档中的所有连接（外部、本地链接、图片）的路 径都在base定义的路径之下

* `<link>`标签: 引入外部样式文件

* `<style>`标签: 在当前文档中写入样式的地方

* `<script src="">`标签: 引入外部javascript文件

* `<script>`标签: 在当前文旦中写入javascript代码的地方

* `<body>`标签: 规范的做法是给用户看的内容都写在这里

* `<meta>`标签: 其核心作用是SEO优化、设定字符集、关键字、自动刷新重定向、控制网页等,最常见得用法是放置字符集，让网页中不出现乱码.
  * `<meta>`标签常用属性属性: **name** , **content** , **http-equiv**, 如
    ```html
        <meta charset="utf-8">
        <meta name="keywords" content="关键词">
        <meta name="description" content="网站描述">
        <meta name="author" content="作者">
        <meta name="generator" content="编辑器名称">
        <meta name="copyright" content="版权">
        <meta name="renderer" content="webkit">
        <!-- robots用来告诉搜索引擎机器人哪些页面需要索引，哪些页面不需要索引 -->
        <meta name="robots" content="all"> <!-- 文件将被检索，而且页面上的链接可以被查询 -->
        <meta name="robots" content="none"> <!-- 文件将不被检索，并且页面上的链接不可以被查询 -->
        <meta name="robots" content="index"> <!-- 文件将被检索 -->
        <!-- http-equiv相当于http的文件头，可以向浏览器传递一些有用的信息，以帮助正确的显示页面 -->
        <meta http-equiv="expires" content="时间期限"> <!-- 用来设置网页到期时间，必须采用GMT的时间格式 -->
        <meta http-equiv="pragma" content="no/yes-cache"> <!-- 用来设置cache模式，即设置是否可以脱机浏览 -->
        <meta http-equiv="refresh" content="时间，url=网址"> <!-- 用来设置自动刷新，并是否跳转到其它页面 -->
        <!-- 强制浏览器的渲染方式，默认使用chrome来渲染，然后再按照IE浏览器的最新版本来渲染,前提是本地机器安装了Chrome Frame或IE最新浏览器 -->
        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
        <!-- 设置视图模式 -->
        <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
    ```

# 文档声明DTD

## thml5文档声明

```html
<!DOCTYPE html>
```

## html4.01文档声明

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
```

**HTML5文档声明的兼容是从ie9开始的，而HTML4文档声明的兼容性是从IE6、7、8开始的。**

**Html4.01**分为两大类（**HTML4.01**和**XHTML1.0**）6小类（两大类各包含**strict**严格的、**Transitional**普通的、**Frameset**带有框架的页面）
