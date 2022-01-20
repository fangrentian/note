# CSS HACK

写 CSS 样式的时候，恐怕最头疼的就是各个浏览器下的兼容性问题，即 CSS hack，不同的浏览器对 CSS 的解析结果是不同的，因此会导致相同的 CSS 输出的页面效果不同，这就需要 CSS hack来解决浏览器局部的兼容性问题。使用 CSS 、 hack将会使用你的 CSS 代码部分失去作用，然后借助条件样式，使用其原 CSS 代码在一些浏览器解析，而 CSS hack代码在符合条件要求的浏览器中替代原 CSS 那部分代码。

CSS hack除了可以处理与浏览器不兼容的 CSS 代码，也能够让我们通过 CSS hack给不同版本的浏览器定制编写不同的CSS效果。

# css hack基本概念

CSS hack是通过在CSS样式中加入一些特殊的符号，让不同的浏览器识别不同的符号（什么样的浏览器识别什么样的符号是有标准的，CSS hack 就是让你记住这个标准），以达到应用不同的 CSS 样式的目的，比如 .kwstu{width:300px;_width:200px;}，一般浏览器会先给元素使用 width:300px; 的样式，紧接着后面还有个_width:200px; 由于下划线 _width 只有 IE6 可以识别，所以此样式在 IE6 中实际设置对象的宽度为200px，后面的把前面的给覆盖了，而其他浏览器不识别 _width 不会执行 _width:200px; 这句样式，所以在其他浏览器中设置对象的宽度就是 300px;

简单地讲， CSS hack 指各版本及各品牌浏览器之间对 CSS 解释后出现网页内容的误差(比如我们常说错位)的处理。由于各浏览器的内核不同，所以会造成一些误差就像 JS 一样，一个 JS 网页特效，在微软 IE6、IE7、IE8 浏览器有效果，但可能在火狐（Mozilla Firefox）谷歌浏览器无效，这样就叫做 JS hack ，所以我们对于 CSS 来说他们来解决各浏览器对 CSS 解释不同所采取的区别不同浏览器制作不同的 CSS 样式的设置来解决这些问题就叫作 CSS Hack。

CSS Hack主要分为CSS 属性 Hack、CSS 选择符 Hack 以及 IE 条件注释 Hack， Hack 主要针对IE 浏览器。

# 常见CSS Hack

## 属性级 Hack

比如 IE6 能识别下划线_和星号 * ，IE7 能识别星号 * ，但不能识别下划线_，而 firefox 两个都不能认识。

## 选择符级 Hack

比如 IE6 能识别 *html .class{}，IE7能识别 *+html .class{} 或者 *:first-child+html .class{}。

## IE 条件注释 Hack

IE条件注释是微软从 IE5 开始就提供的一种非标准逻辑语句。比如针对所有IE：<!–[if IE]><!–您的代码–><![endif]–>，针对 IE6 及以下版本：<!–[if lt IE 7]><!–您的代码–><![endif]–>，这类 Hack 不仅对 CSS 生效，对写在判断语句里面的所有代码都会生效。

PS：条件注释只有在 IE 浏览器下才能执行，这个代码在非 IE 浏览下被当做注释视而不见。可以通过IE条件注释载入不同的 CSS、JS、HTML 和服务器代码等。

# 各浏览器下 Hack 的写法

## Firefox

仅仅被Firefox浏览器识别的写法,支持所有firefox版本
```css
@-moz-document url-prefix() { 
	.selector { 
		property: value; 
	} 
}

#selector[id=selector] { 
	property: value; 
}
```
支持所有Gecko内核的浏览器 (包括Firefox)
```css
*>.selector { property: value; }
```

## Webkit 内核核浏览器(chrome and safari)

```css
@media screen and (-webkit-min-device-pixel-ratio:0) { 
	Selector { 
		property: value; 
	} 
}
```

## Opera 浏览器

```css
html:first-child>body Selector {property:value;}

@media all and (min-width:0) { 
	Selector {
		property: value;
	} 
}

@media all and (-webkit-min-device-pixel-ratio:10000), not all and (-webkit-min-device-pixel-ratio:0) { 
	head~body Selector { 
		property: value; 
	} 
}
```

## IE9浏览器

最后的9不能省略
```css
:root Selector {property: value9;}
```

## IE9以及IE9以下版本

这种写法只有IE9以及IE9以下版本能识别，这里需要注意此处“9”只能是“9”不能是别的
```css
Selector {property:value9;}
```

## IE8浏览器

```css
@media \0screen{
	Selector {property: value;}     
}
```

## IE8以及IE8以上的版本

```css
Selector {property: value\0;}
```

## IE7浏览器

```css
*+html Selector{property:value;}

*:first-child+html Selector {property:value;}
```

## IE7及IE7以下版本浏览器

```css
Selector {*property: value;}
```

## IE6浏览器

```css
Selector {_property/**/:/**/value;}

Selector {_property: value;}

*html Selector {property: value;}
```

# 使用\0的CSS Hack注意事项

网上许多与 CSS Hack 相关的文章中说，在 CSs 的属性值和分号之间添加字符 \0，可以实现对 IE 8 或  IE 9的 CSS hack (有的说，仅支持 IE8)

```css
.css-hack {
    color: red; /* 其他浏览器显示红色 */
    color: blue\0; /* IE8、IE9 显示蓝色 */
    +color: green; /* IE7 显示绿色 */
    _color: brown; /* IE6 显示棕色 */
}
```
1. IE10 也能够识别添加了字符 \0 的 CSS 属性值。

2. 属性值和 \0 之间不能有空格，有一个空格的话(例如：blue \0)，在 IE 8 中就失效了，仅对 IE 9/IE 10有效。

3. 如果我们只想对 IE 8/IE 9进行CSS Hack 呢？这个时候，我们去掉后面两行与 IE6、IE7 有关的代码。

# IE条件注释

```html
<!--[if !IE]><!--> 除IE外都可识别 <!--<![endif]-->
<!--[if IE]> 所有的IE可识别 <![endif]-->
<!--[if IE 6]> 仅IE6可识别 <![endif]-->
<!--[if lt IE 6]> IE6以及IE6以下版本可识别 <![endif]-->
<!--[if gte IE 6]> IE6以及IE6以上版本可识别 <![endif]-->
<!--[if IE 7]> 仅IE7可识别 <![endif]-->
<!--[if lt IE 7]> IE7以及IE7以下版本可识别 <![endif]-->
<!--[if gte IE 7]> IE7以及IE7以上版本可识别 <![endif]-->
<!--[if IE 8]> 仅IE8可识别 <![endif]-->
<!--[if IE 9]> 仅IE9可识别 <![endif]-->
```

# IE属性hack总结

* \9    ：所有 IE 浏览器都支持
* _和-  ：仅 IE6 支持
* `*`     ：IE6、E7 支持
* \0    ：IE8、IE9 支持，opera 部分支持
* \9\0  ：IE8 部分支持、IE9 支持
* \0\9  ：IE8、IE9 支持

# css hack兼容技巧

* FF中 div 设置 padding 后会导致 width 和 height 增加, 但 IE 不会.(可用 !important 解决)
* 居中问题
  * 垂直居中.将 line-height 设置为 当前 div 相同的高度, 再通过 vertical-align: middle.( 注意内容不要换行.)
  * 水平居中. margin: 0 auto;(当然不是万能)
* 若需给 a 标签内内容加上样式, 需要设置 display: block;(常见于导航标签)
* FF 和 IE 对 BOX 理解的差异导致相差 2px 
* 设为 float的div在ie下 margin加倍等问题
* ul 标签在 FF 下面默认有 list-style 和 padding 
* 作为外部 wrapper 的 div 不要定死高度, 最好还加上 overflow: hidden.以达到高度自适应
* 手形光标 **cursor: pointer**. 而**cursor: hand** 只适用于 IE

# 针对 firefox ie6 ie7 的 css 样式

可以用 !important 来 hack，对于 ie6 和 firefox 可以正常显示，但是 ie7 对 !important 可以正确解释，会导致页面没按要求显示！找到一个针对 IE7 不错的 hack 方式就是使用“*+html”，现在用IE7浏览一下，应该没有问题了。

```css
#1 { color: #333; } /* Moz */
* html #1 { color: #666; } /* IE6 */
*+html #1 { color: #999; } /* IE7 */
```
# CSS 布局中的居中问题

首先在父级元素定义 text-align: center; 这个的意思就是在父级元素内的内容居中；对于 IE 这样设定就已经可以了。

但在 mozilla 中不能居中。解决办法就是在子元素定义时候设定时再加上margin-right: auto;margin-left: auto;

# 盒模型

## 标准盒模型
![](./images/2015-10-03-css-27.jpg)
## IE盒模型
![](./images/2015-10-03-css-30.jpg)
```css
box-sizing: content-box /**是W3C盒子模型 */
box-sizing: border-box /*是IE盒子模型*/
```

# 浮动 IE 产生的双倍距离

```css
#box{ 
	float:left; 
	width:100px; 
	margin:0 0 0 100px; //这种情况之下IE会产生200px的距离 
	display:inline; //使浮动忽略
}
```

# IE 不识别 min-属性

IE 不认得 min-这个定义，但实际上它把正常的 width 和 height 当作有 min 的情况来使。这样问题就大了，如果只用宽度和高度，正常的浏览器里这两个值就不会变，如果只用 min-width 和 min-height 的话，IE下面根本等于没有设置宽度和高度。比如要设置背景图片，这个宽度是比较重要的。要解决这个问题，可以这样：
```css
#box{ 
	width: 80px; 
	height: 35px;
}
html>body #box{ 
	width: auto; 
	height: auto; 
	min-width: 80px; 
	min-height: 35px;
}
```

# 页面的最小宽度

min-width 是个非常方便的 CSS 命令，它可以指定元素最小也不能小于某个宽度，这样就能保证排版一直正确。但 IE 不认得这个，而它实际上把 width 当做最小宽度来使。为了让这一命令在 IE 上也能用，可以把一个<div> 放到 <body> 标签下，然后为 div 指定一个类,然后 CSS 这样设计：

```css
#container{ 
	min-width: 600px; 
	width: expression(document.body.clientWidth < 600? “600px”: “auto” );
}
```
第一个min-width是正常的；但第2行的 width 使用了 JavaScript，这只有IE才认得，这也会让你的 HTML 文档不太正规。它实际上通过 Javascript 的判断来实现最小宽度。

# 清除浮动

```css
.hackbox{ display:table; //将对象作为块元素级的表格显示}

.hackbox{ clear:both;}

#box:after{ 
	content: “.”; 
	display: block; 
	height: 0; 
	clear: both; 
	visibility: hidden;
}
```

# 高度不适应

高度不适应是当内层对象的高度发生变化时外层高度不能自动进行调节，特别是当内层对象使用 margin 或 padding 时。

```css
#box {background-color:#eee; }

#box p {margin-top: 20px;margin-bottom: 20px; text-align:center; }
```
解决方法：在 P 对象上下各加 1个空的 div 对象,并设置样式为 **height:0px;overflow:hidden;** 或者为 DIV 加上 border 属性。