## 什么是scss

Sass（英文全称：Syntactically Awesome Stylesheets）是一个最初由Hampton Catlin设计并由Natalie Weizenbaum开发的层叠样式表语言。在开发最初版本之后，Weizenbaum和Chris Eppstein继续通过SassScript来继续扩充Sass的功能。SassScript是一个在Sass文件中使用的小型脚本语言。

Sass是一个将脚本解析成CSS的脚本语言，即SassScript。Sass包括两套语法。最开始的语法叫做“缩进语法”，与Haml类似，使用缩进来区分代码块，并且用回车将不同规则分隔开。而较新的语法叫做“SCSS”，使用和CSS一样的块语法，即使用大括号将不同的规则分开，使用分号将具体的样式分开。通常情况下，这两套语法通过.sass和.scss两个文件扩展名区分开。

Sass 扩展了 CSS3，增加了规则、变量、混入、选择器、继承等等特性。Sass 生成良好格式化的 CSS 代码，易于组织和维护。
SASS是对CSS3（层叠样式表）的语法的一种扩充，它可以使用巢状、混入、选择子继承等功能，可以更有效有弹性的写出Stylesheet。Sass最后还是会编译出合法的CSS让浏览可以使用，也就是说它本身的语法并不太容易让浏览器识别（虽然它和CSS的语法非常的像，几乎一样），因为它不是标准的CSS格式，在它的语法内部可以使用动态变量等，所以它更像一种极简单的动态语言。

**参考文档**

[https://www.sass.hk/docs/](https://www.sass.hk/docs/)