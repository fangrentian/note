# 核心属性

## class
全局属性，给标签起的名字，相当于人的小名，整个页面中class值可以重复；

## id
全局属性，同样是给标签起的名字，相当于身份证上的正式名称，同一页面中ID值不能重复，即保证ID的唯一性；

## style
全局属性，设置css的样式；

## title
全局属性，鼠标放置其上时起到提示信息的作用；

## accesskey
在标签中设置属性，值为你要设置的键盘按钮值，使用时快捷键配合alt键；

## tabindex
设置键序，即设置tab键访问标签的顺序，拒绝负值，负值的标签将被跳过，而不访问；

# `<a>`标签的功能
* 锚点
* 超链接

`<a href="">空连接</a>` 这样的空连接中隐藏了一个返回顶部的锚点,实际效果等同刷新页面, 如果要抑制这种效果, 可以采用 `<a href="##">抑制空连接</a>` 方式, 而像 `<a href="#linked">定位锚点</a>` 的连接可以定位到页面中ID为 **linked** 的元素.

## `<a>`作为锚点使用

定义一个锚点,采用类似 `<a href="#linked">锚点</a>` 方式, **href** 属性定义为 '**#**' + 页面中另外一个元素的 **id** 值或另外一个 `<a>`元素的 **name** 值.

* 在同一个页面实现锚点跳转
* 在不同的页面中实现锚点的跳转
* 跳转到别的网站的某个位置

## `<a>` 标签的target属性

* _blank: 在新页面中打开
* _self: 在当前页面打开，是默认值

# 绝对路径与相对路径

* 相对路径：从当前代码的目录开始找目标文件, **../** 表示当前目录的上层目录, 当前目录用 **./** 表示，省略 **./** 也可以表示当前目录；
* 绝对路径：从磁盘盘符开始查找，输入完整的文件路径

# `<img>`标签
```html
<img src="" alt="">
```
## `<img>`标签常用属性

* src: 引入图像的地址
* width: 宽度
* height: 高度
* alt：图像不能正常加载的时候，显示的替代性文字
* ismap：将图像定义为服务端的图像映射
* usemap：图像映射，应用于客户端，也叫图像热区, 需要配合 `<map>` 标签使用, usemap值与 `<map>` 标签的 **name** 或 **id** 保持一致, `<map>` 标签的**name**和**id**同时设置，并且相同，是为了解决一些老旧浏览器的兼容问题.

```html
<img src="planets.jpg" usemap="#planetmap" alt="Planets" />
<map name="planetmap" id="planetmap">
  <area shape="circle" coords="180,139,14" href ="连接地址" alt="Venus" />
  <area shape="circle" coords="129,161,10" href ="连接地址" alt="Mercury" />
  <area shape="rect" coords="0,0,110,260" href ="连接地址" alt="Sun" />
</map>
```

# 网页布局方式

* table布局（属于web1.0时代的产物）
* DIV + CSS
* flex弹性盒模型

# `<input>`标签

如果`<input>`标签的**name**属性和**id**属性相同，在老版本浏览器中，js中getElementById方法可能提取到的是name值和id值相同的标签；

## `<input>`标签value属性

`<input>`元素的默认值（需删除后再输入）, H5中可以设置**placeholder='默认值'**（用此方法时,元素获取焦点后默认值自动消失）

## `<input>`标签type属性

* text：文本
* password：密码
* submit：提交
* button：按钮
* radio：单选按钮, 单选按钮同组的name属性值必须相同
* checkbox:复选框, 同组的name属性值设置相同
* email：邮箱（H5中的功能）
* reset：重置，注意容易被p标签影响，不要放在p标签中
* file：上传文件专用
* image：引入图片，可以做按钮
* color：拾色器

## `<input>`标签其它常用属性

* checked：默认选项
* disabled:禁用
* maxlength:输入字段的最大长度
* size：字段可视宽度,以字符数计算
* readonly：只读

# 常用框架

* frameset（淘汰）
* frame（淘汰）
* iframe

## iframe框架常用属性

* frameborder：值为1（显示边框）和0（不显示边框）；
* width和height：宽和高；
* longdesc：对当前iframe的一个描述，通常是给搜索引擎解析阅读用的；
* marginheight：框架内的内容距离顶部的距离。在别人的网页中可能没有效果；
* marginwidth：距左侧的距离；
* scrolling: 滚动条，值为yes、no、auto

# H5中新增的语义化元素

header footer nav section article aside figcaption figure等

# IE条件注释

它是由Microsoft从IE5开始就提出并提供的一种非标准逻辑语句。 灵活的判断不同IE版本，且导入不同的html/css/js等。 属于Microsoft官方给出的兼容解决方案，并且能顺利通过W3C标准的效验。只有IE浏览器认识，当我们在非IE浏览器下条件注释会被当做注释忽略掉，得不到执行。条件注释不适用于IE10级以上版本.

* lt ：Less than的简写，也就是小于的意思
* lte ：Less than or equal to的简写，也就是小于或等于的意思
* gt ：Greater than的简写，也就是大于的意思
* gte：Greater than or equal to简写，也就是大于或等于的意思
* !：不等于

## 只有IE可以识别[ IE5 ]

只有IE5以上的版本才刚开始支持IE条件注释，所以只有IE可以识别它。
```html
<!--[if IE]>
<script src="https://cdn.bootcss.com/vue/2.6.10/vue.min.js"></script>
<![endif]-->
```

## 只有特定版本才能识别 [ = ]

IE8才能识别，IE7和IE9都无法识别
```html
<!--[if IE 8]>
<link rel="stylesheet" href="bootstrap.min.css" />  
<![endif]-->
```

## 只有不是特定版本才能识别 [ >< ]

除了IE6不能识别，都能识别，但要在IE5以上
```html
<!--[if !IE 6]>
<script src="https://cdn.bootcss.com/vue/2.6.10/vue.runtime.min.js"></script>
<![endif]-->
```

## 只有高于特定版本才能识别[ > ]

必须高于IE6版本才能识别，IE6版本也无法识别
```html
<!--[if gt IE 6]>
<script src="https://cdn.bootcss.com/angular.js/2.0.0-beta.17/angular2.min.js"></script>
<![endif]-->
```

## 等于或高于特定版本才能识别[ >= ]

IE8或更高才能识别，IE7无法识别
```html
<!--[if gte IE 8]>
<script src="https://cdn.bootcss.com/angular.js/2.0.0-beta.17/angular2-polyfills.js"></script> 
<![endif]-->
```

## 低于特定版本才能识别[ < ]

IE9以下无法识别，IE9也无法识别
```html
<!--[if lt IE 9]>
<script src="https://cdn.bootcss.com/lodash.js/4.17.12-pre/lodash.min.js"></script>
<![endif]-->
```

## 等于或低于特定的版本才能识别[ <= ]

IE8及IE8以下可以识别，IE9无法识别
```html
<!--[if lte IE 8]>
<script src="https://cdn.bootcss.com/react/16.9.0-alpha.0/cjs/react.production.min.js"></script>  
<![endif]-->
```

# IE10/11不支持条件性注释后的替代方法

## 使用IE=EmulateIE9属性指示浏览器采用IE9渲染技术

```html
<meta http-equiv="X-UA-Compatible" content="IE=EmulateIE9">
```
在html网页的head里加入上面的元标记，这样IE10/11就能识别条件性注释了，我们也就可以像IE6/7/8那样编写针对性的CSS代码了。但这样做有个弊端，很显然，浏览器这样就会采用IE9的渲染模式，而不是最新的IE10/11技术。

## 使用媒体查询语句+-ms-high-contrast属性

CSS的媒体查询语句(media query)是一种高级的CSS条件语句，它能根据一些属性和属性值计算判断CSS是否可以生效。在这里，我们要使用一个IE10/11独有的属性，它就是-ms-high-contrast，只有IE10/11实现了这个属性，它可以有两个值active或none，使用下面的媒体查询语句：

```css
@media all and (-ms-high-contrast: none), (-ms-high-contrast: active) {
	/* IE10+ CSS styles go here */
}
```
火狐浏览器、谷歌浏览器不能识别这个属性，所以不会执行这个查询语句里的CSS，从而实现了条件性执行的效果.

## 使用Javascript判断浏览器的类型

先用JavaSCript判断是否是IE浏览器，如果是，就在页面的<html>标记上添加一个“ie”的类名：
```js
var ms_ie = false;
var ua = window.navigator.userAgent;
var old_ie = ua.indexOf('MSIE ');
var new_ie = ua.indexOf('Trident/');
    
if ((old_ie > -1) || (new_ie > -1)) {
    ms_ie = true;
}
    
if ( ms_ie ) {
   document.documentElement.className += " ie";
}
```
有了这个标志性css class后，我们就可以在CSS里区别性的编写css代码了。

```css
.testClass{
    /*这里写通用的css*/ 
}

.ie .testClass{
    /*这里写专门针对IE的css*/
}
```