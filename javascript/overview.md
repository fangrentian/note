# 什么是javascript

JavaScript一种直译式脚本语言，是一种动态类型、弱类型、基于原型的语言，内置支持类型。它的解释器被称为JavaScript引擎，为浏览器的一部分，广泛用于客户端的脚本语言，最早是在HTML（标准通用标记语言下的一个应用）网页上使用，用来给HTML网页增加动态功能。

在1995年时，由Netscape公司的Brendan Eich，在网景导航者浏览器上首次设计实现而成。因为Netscape与Sun合作，Netscape管理层希望它外观看起来像Java，因此取名为JavaScript。但实际上它的语法风格与Self及Scheme较为接近。

为了取得技术优势，微软推出了JScript，CEnvi推出ScriptEase，与JavaScript同样可在浏览器上运行。为了统一规格，因为JavaScript兼容于ECMA标准，因此也称为ECMAScript。

**参考文档**

[https://www.w3cschool.cn/javascript/dict](https://www.w3cschool.cn/javascript/dict)

[https://wangdoc.com/javascript/](https://wangdoc.com/javascript/)

[https://wangdoc.com/es6/](https://wangdoc.com/es6/)

[https://es6.ruanyifeng.com/#README](https://es6.ruanyifeng.com/#README)

[https://ecma262.docschina.org/](https://ecma262.docschina.org/)

[https://zh.javascript.info/](https://zh.javascript.info/)

# 基本信息

javaScript是一种基于对象和事件驱动并具有相对安全性的客户端脚本语言。同时也是一种广泛用于客户端Web开发的脚本语言，常用来给HTML网页添加动态功能，比如响应用户的各种操作。它最初由网景公司（Netscape）的Brendan Eich设计，是一种动态、弱类型、基于原型的语言，内置支持类。JavaScript是Sun公司的注册商标。Ecma国际以JavaScript为基础制定了ECMAScript标准。JavaScript也可以用于其他场合，如服务器端编程。完整的JavaScript实现包含三个部分：ECMAScript，文档对象模型，字节顺序记号。

Netscape公司在最初将其脚本语言命名为LiveScript。在Netscape在与Sun合作之后将其改名为JavaScript。JavaScript最初受Java启发而开始设计的，目的之一就是“看上去像Java”，因此语法上有类似之处，一些名称和命名规范也借自Java。但JavaScript的主要设计原则源自Self和Scheme。JavaScript与Java名称上的近似，是当时网景为了营销考虑与Sun公司达成协议的结果。

为了取得技术优势，微软推出了VBScript来迎战JavaScript的脚本语言。为了互用性，Ecma国际（前身为欧洲计算机制造商协会）创建了ECMA-262标准（ECMAScript）。现在两者都属于ECMAScript的实现。尽管JavaScript作为给非程序人员的脚本语言，而非是作为给程序人员的编程语言来推广和宣传，但是JavaScript具有非常丰富的特性。

# 历史背景

大概在1992年，一家称作Nombas的公司开始开发一种叫做C--（C-minus-min us，简称Cmm）的嵌入式脚本语言。这个脚本语言捆绑在一个叫做CEnvi的共享软件产品中，当Netscape Navigator崭露头角时，Nombas开发了一个可以嵌入网页中的CEnvi的版本。这些早期的试验称为EspressoPage（浓咖啡般的页面），它们代表了第一个在万维网上使用的客户端脚本语言。而Nombas丝毫没有料到它的理念将会成为因特网的一块重要基石。

javascript最初由网景公司的布兰登·艾奇设计。JavaScript是甲骨文公司的注册商标。Ecma国际以JavaScript为基础制定了ECMAScript标准。JavaScript也可以用于其他场合，如服务器端编程。完整的JavaScript实现包含三个部分：ECMAScript，文档对象模型，浏览器对象模型。

Netscape在最初将其脚本语言命名为LiveScript，后来Netscape在与Sun合作之后将其改名为JavaScript。JavaScript最初受Java启发而开始设计的，目的之一就是“看上去像Java”，因此语法上有类似之处，一些名称和命名规范也借自Java。但JavaScript的主要设计原则源自Self和Scheme。JavaScript与Java名称上的近似，是当时网景为了营销考虑与太阳微系统达成协议的结果。为了取得技术优势，微软推出了JScript来迎战JavaScript的脚本语言。为了互用性，Ecma国际（前身为欧洲计算机制造商协会）创建了ECMA-262标准（ECMAScript）。现在两者都属于ECMAScript的实现。尽管JavaScript作为给非程序人员的脚本语言，而非作为给程序人员的脚本语言来推广和宣传，但是JavaScript具有非常丰富的特性。

发展初期，JavaScript的标准并未确定，同期有网景的JavaScript，微软的JScript和CEnvi的ScriptEase三足鼎立。1997年，在ECMA（欧洲计算机制造商协会）的协调下，由Netscape、Sun、微软、Borland组成的工作组确定统一标准：ECMA-262。

# 作者

布兰登·艾奇（Brendan Eich，1964年～），JavaScript的发明人，目前（2007年）在Mozilla公司担任首席技术长（Chief Technology Officer）。

# 组成部分

一个完整的 JavaScript 实现是由以下 3 个不同部分组成的：
核心（ECMAScript）、文档对象模型（Document Object Model，简称DOM）、浏览器对象模型（Browser Object Model，简称BOM）。

# 基本特点

网页嵌入技术有：Javascript、VBScript、Document Object Model（DOM，文档对象模型）、Layers和Cascading Style Sheets（CSS，层叠样式表）。

Javascript就是适应动态网页制作的需要而诞生的一种新的编程语言，如今越来越广泛地使用于Internet网页制作上。Javascript是由 Netscape公司开发的一种脚本语言（scripting language），或者称为描述语言。在HTML基础上，使用Javascript可以开发交互式Web网页。Javascript的出现使得网页和用户之间实现了一种实时性的、动态的、交互性的关系，使网页包含更多活跃的元素和更加精彩的内容。

运行用Javascript编写的程序需要能支持Javascript语言的浏览器。Netscape公司 Navigator 3．0以上版本的浏览器都能支持Javascript程序，微软公司Internet Explorer 3．0以上版本的浏览器基本上支持Javascript。微软公司还有自己开发的Javascript，称为JScript。Javascript和Jscript基本上是相同的，只是在一些细节上有出入。 Javascript短小精悍，又是在客户机上执行的，大大提高了网页的浏览速度和交互能力。 同时它又是专门为制作Web网页而量身定做的一种简单的编程语言。

JavaScript 使网页增加互动性。JavaScript 使有规律地重复的HTML文段简化，减少下载时间。JavaScript 能及时响应用户的操作，对提交表单做即时的检查，无需浪费时间交由CGI 验证。JavaScript 的特点是无穷无尽的，只要你有创意。

# 什么是es6

ES6， 全称 ECMAScript 6.0 ，是 JavaScript 的下一个版本标准，2015.06 发版。

ES6 主要是为了解决 ES5 的先天不足，比如 JavaScript 里并没有类的概念，但是目前浏览器的 JavaScript 是 ES5 版本，大多数高版本的浏览器也支持 ES6，不过只实现了 ES6 的部分特性和功能。

# ECMAScript的背景

JavaScript 是大家所了解的语言名称，但是这个语言名称是商标（ Oracle 公司注册的商标）。因此，JavaScript 的正式名称是 ECMAScript 。1996年11月，JavaScript 的创造者网景公司将 JS 提交给国际化标准组织 ECMA（European computer manufactures association，欧洲计算机制造联合会），希望这种语言能够成为国际标准，随后 ECMA 发布了规定浏览器脚本语言的标准，即 ECMAScript。这也有利于这门语言的开放和中立。

# ECMAScript的历史

ES6 是 ECMAScript 标准十余年来变动最大的一个版本，为其添加了许多新的语法特性。

* 1997 年 ECMAScript 1.0 诞生。

* 1998 年 6 月 ECMAScript 2.0 诞生，包含一些小的更改，用于同步独立的 ISO 国际标准。

* 1999 年 12 月 ECMAScript 3.0诞生，它是一个巨大的成功，在业界得到了广泛的支持，它奠定了 JS 的基本语法，被其后版本完全继承。直到今天，我们一开始学习 JS ，其实就是在学 3.0 版的语法。

* 2000 年的 ECMAScript 4.0 是当下 ES6 的前身，但由于这个版本太过激烈，对 ES 3 做了彻底升级，所以暂时被"和谐"了。

* 2009 年 12 月，ECMAScript 5.0 版正式发布。ECMA 专家组预计 ECMAScript 的第五个版本会在 2013 年中期到 2018 年作为主流的开发标准。2011年6月，ES 5.1 版发布，并且成为 ISO 国际标准。

* 2013 年，ES6 草案冻结，不再添加新的功能，新的功能将被放到 ES7 中；2015年6月， ES6 正式通过，成为国际标准。

# ES6 的目标与愿景

成为更好编写的开发语言有以下目标。

适应更复杂的应用；实现代码库之间的共享；不断迭代维护新版本。