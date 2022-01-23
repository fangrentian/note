# 介绍

UglifyJS是一个JavaScript解析器、缩小器、压缩器和美化器工具包。

注意:uglify-js支持JavaScript和ECMAScript中的大多数语言特性。

对于ECMAScript的更奇特的部分，在传递到uglify-js之前，用Babel之类的transpiler处理你的源文件。

uglify-js@3有一个简化的API和CLI，不向后兼容uglify-js@2。

# 安装

```shell
npm install uglify-js -g
```

# 命令行使用 uglifyjs [input files] [options]

UglifyJS可以接受多个输入文件。建议您先传递输入文件，然后传递选项。UglifyJS将按顺序解析输入文件，并应用任何压缩选项。这些文件在相同的全局范围内解析，也就是说，从一个文件到另一个文件中声明的某个变量/函数的引用将被正确匹配。

如果没有指定输入文件，UglifyJS将从STDIN读取。

如果你希望在输入文件之前传递你的选项，用双破折号将两者分开，以防止输入文件被用作选项参数:

```shell
glifyjs --compress --mangle -- input.js
```

options:

* -h | --help: 列出使用指南。使用`--help options` 获取可用选项的详情。
* -V | --version: 打印版本号。
* -p | --parse <options>: 指定解析器配置选项:

  * acorn:  使用 Acorn 来解析。
  * bare_returns:  允许在函数外return。 在压缩CommonJS模块或`.user.js `引擎调用被同步执行函数包裹的用户脚本 时会用到。
  * expression:  不是解析文件，而是解析一段表达式 (例如解析JSON).
  * spidermonkey:  输入文件是 SpiderMonkey AST 格式 (JSON).

*-c | --compress [options]:   启用压缩（true/false）/指定压缩配置:

* pure_funcs:  传一个函数名的列表，当这些函数返回值没被利用时，该函数会被安全移除。
* -m | --mangle [options]: 启用混淆（true/false）/指定混淆配置:

  * reserved: 不被混淆的名字列表。
* --mangle-props [options]: 混淆属性/指定压缩配置:

  * builtins: 混淆那些与标准JS全局变量重复的名字。
  * debug:  添加debug前缀和后缀。
  * domprops: 混淆那些鱼DOM属性名重复的名字。
  * keep_quoted: 只混淆没括起来的属性名。
  * regex: 只混淆匹配（该正则）的名字。
  * reserved: 不需要混淆的名字的列表（即保留）。
* -b | --beautify [options]:  是否美化输出（true/false）/指定输出配置：

  * beautify: 默认是启用.
  * preamble: 预设的输出文件头部。你可以插入一段注释，比如版权信息。它不会被解析，但sourcemap会因此调整。
  * quote_style: 括号类型:
    0: auto自动 1: single单引号 2: double双引号 3: original跟随原码
  * wrap_iife: 把立即执行函数括起来。注意：你或许应禁用压缩配置中的`negate_iife`选项。
* -o | --output <file>: 输出文件路径 (默认 STDOUT). 指定 `ast` 或`spidermonkey`的话分别是输出UglifyJS或SpiderMonkey AST。
* --comments [filter]: 保留版权注释。默认像Google Closure那样，保留包含"@license"或"@preserve"这样JSDoc风格的注释。你可以传以下的参数：
  *

  - "all": 保留全部注释

  * -: 一个合适的正则，如 `/foo/` 或 `/^!/`，保留匹配到的注释。 注意，在启用压缩时，因为死代码被移除或压缩声明为一行，并非*所有*的注释都会被保留。
* --config-file <file>: 从此JSON文件读取 `minify()` 配置。
* -d | --define <expr>[=value]: 定义全局变量。
* --ie8: 支持IE8。 等同于在`minify()`的`compress`、 `mangle` 和 `output`配置设置`ie8: true`。UglifyJS不会默认兼容IE8。
* --keep-fnames: 不要混淆、干掉的函数的名字。当代码依赖Function.prototype.name时有用。
* --name-cache <file>: 用来保存混淆map的文件。
* --self: 把UglifyJS本身也构建成一个依赖包(等同于`--wrap UglifyJS`)
* --source-map [options]: 启用 source map（true/false）/指定sourcemap配置:

  * base: 根路径，用于计算输入文件的相对路径。
  * content: 输入sourcemap。假如的你要编译的JS是另外的源码编译出来的。 假如该sourcemap包含在js内，请指定"inline"。
  * filename: 输出文件的名字或位置。
  * includeSources: 如果你要在sourcemap中加上源文件的内容作sourcesContent属性，就传这个参数吧。
  * root: 此路径中的源码编译后会产生sourcemap.
  * url: 如果指定此值，会添加sourcemap相对路径在`//#sourceMappingURL`中。
* --timings: 在STDERR显示操作运行时间。
* --toplevel: 压缩/混淆在最高作用域中声明的变量名。
* --verbose: 打印诊断信息。
* --warn: 打印警告信息。
* --wrap <name>: 把所有代码包裹在一个大函数中。让“exports”和“global”变量有效。 你需要传一个参数来指定此模块的名字，以便浏览器引用。

指定--output (-o)来明确输出文件，否则将在终端输出（STDOUT）

# CLI sourcemap选项

UglifyJS可以生成一份sourcemap文件，这非常有利于你调试压缩后的JS代码。传--source-map --output output.js来获取sorcemap文件（sorcemap会生成为output.js.map）。

* --source-map filename=<NAME>: 指定sourcemap名字。
* --source-map root=<URL>: 传一个源文件的路径。否则UglifyJS将假定已经用了HTTPX-SourceMap，并将省略//＃sourceMappingURL=指示。
* --source-map "names=false": 如果您想减少源映射文件的大小，可以省略符号名称。
* --source-map url=<URL>: 指定生成sourcemap的路径。

```shell
uglifyjs js/file1.js js/file2.js \
             -o foo.min.js -c -m \
             --source-map root="http://foo.com/src",url=foo.min.js.map
```

上述配置会压缩和混淆file1.js、file2.js，输出文件foo.min.js
和sourcemapfoo.min.js.map，sourcemap会建立http://foo.com/src/js/file1.js、 http://foo.com/src/js/file2.js的映射。（实际上，sourcemap根目录是http://foo.com/src，所以相当于源文件路径是js/file1.js、js/file2.js）

## 关联sourcemap

假如你的JS代码是用其他编译器（例如coffeescript）生成的，那么映射到JS代码就没什么用了，你肯定希望映射到CoffeeScript源码。UglifyJS有一个选项可以输入sourcemap，假如你有一个从CoffeeScript
→ 编译后JS的map的话，UglifyJS可以生成一个从CoffeeScript->压缩后JS的map映射到源码位置。

你可以传入 `--source-map content="/path/to/input/source.map"`或来尝试此特性，如果sourcemap包含在js内，则写 `--source-map content=inline`。

# CLI 压缩选项

需要通过——compress (-c)来启用压缩机。你也可以传递一个逗号分隔的压缩选项列表。

选项的形式是foo=bar，或者foo(后者意味着一个布尔值选项，你想设置为true;这是foo=true的有效快捷方式)。

```shell
uglifyjs file.js -c toplevel,sequences=false
```

# CLI 混淆选项

你需要传入--mangle (-m)来使启用混淆功能。支持以下选项（用逗号隔开）：

* eval(默认值:false): 在使用eval或with的作用域中更改可见的名称。
* reserved (default: []): 启用了混淆，但您希望阻止某些名称被混淆,你可以用`——mangle reserved`来传递一个逗号分隔的名称列表来声明这些名称。例如:

```shell
uglifyjs ... -m reserved=['$','require','exports']
```

这样能防止require, exports和 $被混淆改变。

## 混淆属性名 --mangle-props

这可能会破坏您的代码。更改属性名是一个单独的步骤，与更改变量名不同。通过`——mangle-props`来启用它。它会破坏输入代码中的所有属性，除了内置DOM属性和核心JavaScript类中的属性。例如:

```js
// example.js
var x = {
	baz_: 0,
	foo_: 1,
	calc: function () {
		return this.foo_ + this.baz_;
	}
};
x.bar_ = 2;
x["baz_"] = 3;
console.log(x.calc());
```

混淆所有属性(除了JavaScript内置):

```shell
uglifyjs example.js -c -m --mangle-props
```

结果:

```js
var x = {
	o: 0, _: 1, l: function () {
		return this._ + this.o
	}
};
x.t = 2, x.o = 3, console.log(x.l());
```

混淆除保留属性外的所有属性:

```shell
uglifyjs example.js -c -m --mangle-props reserved=[foo_,bar_]
```

结果:

```js
var x = {
	o: 0, foo_: 1, _: function () {
		return this.foo_ + this.o
	}
};
x.bar_ = 2, x.o = 3, console.log(x._());
```

混淆所有匹配正则表达式的属性:

```shell
uglifyjs example.js -c -m --mangle-props regex=/_$/
```

结果:

```js
var x = {
	o: 0, _: 1, calc: function () {
		return this._ + this.o
	}
};
x.l = 2, x.o = 3, console.log(x.calc());
```

组合混淆选项:

```shell
uglifyjs example.js -c -m --mangle-props regex=/_$/,reserved=[bar_]
```

结果:

```js
var x = {
	o: 0, _: 1, calc: function () {
		return this._ + this.o
	}
};
x.bar_ = 2, x.o = 3, console.log(x.calc());
```

为了混淆正常使用，我们默认避免混淆标准JS内置的名字（`--mangle-props builtins`可以强制混淆）。

tools/domprops.json 里有一个默认的排除名单，包括绝大部分标准JS和多种浏览器中的DOM属性名。传入`--mangle-props domprops` 可以让此名单失效。

可以用正则表达式来定义该混淆的属性名。例如`--mangle-props regex=/^_/`，只混淆下划线开头的属性。

当你压缩多个文件时，为了保证让它们最终能同时工作，我们要让他们中同样的属性名混淆成相同的结果。传入`--name-cache filename.json`，UglifyJS会维护一个共同的映射供他们复用。这个json一开始应该是空的，例如：

```shell
rm -f /tmp/cache.json  # start fresh
$ uglifyjs file1.js file2.js --mangle-props --name-cache /tmp/cache.json -o part1.js
$ uglifyjs file3.js file4.js --mangle-props --name-cache /tmp/cache.json -o part2.js
```

这样part1.js 和 part2.js会知晓对方混淆的属性名。

假如你把所有文件压缩成同一个文件，那就不需要启用名字缓存了。

## 混淆没括起来的名字 --mangle-props keep_quoted

使用括号属性名 (o["foo"])以保留属性名(foo)。这会让整个脚本中其余此属性的引用(o.foo)也不被混淆。例如：

```js
// stuff.js
var o = {
	"foo": 1,
	bar: 3
};
o.foo += o.bar;
console.log(o.foo);
```

```shell
uglifyjs stuff.js --mangle-props keep_quoted -c -m
```

结果:

```js
var o = {foo: 1, o: 3};
o.foo += o.o, console.log(o.foo);
```

## 调试属性名混淆

为了混淆属性时不至于完全分不清，你可以传入--mangle-props debug来调试。例如o.foo会被混淆成o._$foo$_。这让源码量大、属性被混淆时也可以debug，可以看清混淆会把哪些属性搞乱。

```shell
uglifyjs stuff.js --mangle-props debug -c -m
```

结果:

```js
var o = {_$foo$_: 1, _$bar$_: 3};
o._$foo$_ += o._$bar$_, console.log(o._$foo$_);
```

你可以用`--mangle-props-debug=XYZ`来传入自定义后缀。让o.foo 混淆成 o._$foo$XYZ_，
你可以在每次编译是都改变一下，来辨清属性名怎么被混淆的。一个小技巧，你可以每次编译时传随机数来模仿混淆操作（例如你更新了脚本，有了新的属性名），这有助于识别混淆时的出错。

# API参考

假如是通过NPM安装的，你可以在你的应用中这样加载UglifyJS：

```js
var UglifyJS = require("uglify-js");
```

这输出一个高级函数**minify(code, options)**，它能根据配置，实现多种最小化（即压缩、混淆等）。 minify()默认启用压缩和混淆选项。例子：

```js
var code = "function add(first, second) { return first + second; }";
var result = UglifyJS.minify(code);
console.log(result.error); // runtime error, or `undefined` if no error
console.log(result.code);  // minified output: function add(n,d){return n+d}
```

你可以通过一个对象（key为文件名，value为代码）来同时最小化多个文件：

```js
var code = {
	"file1.js": "function add(first, second) { return first + second; }",
	"file2.js": "console.log(add(1 + 2, 3 + 4));"
};
var result = UglifyJS.minify(code);
console.log(result.code);
// function add(d,n){return d+n}console.log(add(3,7));
```

toplevel选项例子：

```js
var code = {
	"file1.js": "function add(first, second) { return first + second; }",
	"file2.js": "console.log(add(1 + 2, 3 + 4));"
};
var options = {toplevel: true};
var result = UglifyJS.minify(code, options);
console.log(result.code);
// console.log(3+7);
```

nameCache 选项例子:

```js
var options = {
	mangle: {
		toplevel: true,
	},
	nameCache: {}
};
var result1 = UglifyJS.minify({
	"file1.js": "function add(first, second) { return first + second; }"
}, options);
var result2 = UglifyJS.minify({
	"file2.js": "console.log(add(1 + 2, 3 + 4));"
}, options);
console.log(result1.code);
// function n(n,r){return n+r}
console.log(result2.code);
// console.log(n(3,7));
```

您可以通过以下方式将名称缓存持久化到文件系统:

```js
var cacheFileName = "/tmp/cache.json";
var options = {
	mangle: {
		properties: true,
	},
	nameCache: JSON.parse(fs.readFileSync(cacheFileName, "utf8"))
};
fs.writeFileSync("part1.js", UglifyJS.minify({
	"file1.js": fs.readFileSync("file1.js", "utf8"),
	"file2.js": fs.readFileSync("file2.js", "utf8")
}, options).code, "utf8");
fs.writeFileSync("part2.js", UglifyJS.minify({
	"file3.js": fs.readFileSync("file3.js", "utf8"),
	"file4.js": fs.readFileSync("file4.js", "utf8")
}, options).code, "utf8");
fs.writeFileSync(cacheFileName, JSON.stringify(options.nameCache), "utf8");
```

综合使用多种minify()选项的例子:

```js
var code = {
	"file1.js": "function add(first, second) { return first + second; }",
	"file2.js": "console.log(add(1 + 2, 3 + 4));"
};
var options = {
	toplevel: true,
	compress: {
		global_defs: {
			"@console.log": "alert"
		},
		passes: 2
	},
	output: {
		beautify: false,
		preamble: "/* uglified */"
	}
};
var result = UglifyJS.minify(code, options);
console.log(result.code);
// /* uglified */
// alert(10);"
```

生成警告提示：

```js
var code = "function f(){ var u; return 2 + 3; }";
var options = { warnings: true };
var result = UglifyJS.minify(code, options);
console.log(result.error);    // runtime error, `undefined` in this case
console.log(result.warnings); // [ 'Dropping unused variable u [0:1,18]' ]
console.log(result.code);     // function f(){return 5}
```

生成错误提示：

```js
var result = UglifyJS.minify({"foo.js" : "if (0) else console.log(1);"});
console.log(JSON.stringify(result.error));
// {"message":"Unexpected token: keyword (else)","filename":"foo.js","line":1,"col":7,"pos":7}
```

注意:不像uglify-js@2.x, 3.x API不会抛出错误。要达到类似的效果，可以这样做:

```js
var result = UglifyJS.minify(code, options);
if (result.error) throw result.error;
```

# 最小化选项

* annotations: 传递`false`表示忽略所有注释，并从输出中删除它们。例如，当外部工具错误地应用`/*@__PURE__*/`或`/*#__PURE__*/`时很有用。将`true`传递给压缩和保留输出中的注释注解，以允许后续处理。
* compress (默认: {}): 传`false`就完全跳过压缩。传一个对象来自定义 [压缩配置](https://www.npmjs.com/package/uglify-js#compress-options)。
* ie8 (默认 `false`): 传 `true` 来支持 IE8.
* keep_fnames (默认: `false`): 传递`true`以防止函数名被丢弃或被打乱。对于依赖Function.prototype.name的代码很有用.
* mangle (默认: `true`): 传 `false` 就跳过混淆名字。传对象来指定[混淆配置](https://www.npmjs.com/package/uglify-js#mangle-options)

  * mangle.properties (默认: `false`): 传一个对象来自定义[混淆属性配置](https://github.com/LiPinghai/UglifyJSDocCN/blob/master/README.md#mangle-properties-options)
* nameCache (默认: `null`): 如果你要缓存 minify()多处调用的经混淆的变量名、属性名，就传一个空对象{}或先前用过的nameCache对象。 注意:这是个可读/可写属性。minify()会读取这个对象的nameCache状态，并在最小化过程中更新，以便保留和供用户在外部使用。
* output (默认: `null`): 要自定义就传个对象来指定额外的 [输出配置](https://www.npmjs.com/package/uglify-js#output-options). 默认是压缩到最优化
* parse (默认: `{}`): 如果你要指定额外的[解析配置](https://www.npmjs.com/package/uglify-js#parse-options),传配置对象
* sourceMap (默认: `false`): 传一个对象来自定义 [sourcemap配置](https://www.npmjs.com/package/uglify-js#source-map-options)
* toplevel (默认: `false`): 如果你要混淆（和干掉没引用的）最高作用域中的变量和函数名，就传true
* v8 (默认: `false`): 启用V8引擎,为Chrome和Node.js的bug启用工作区
* warnings (默认: `false`): 传递`true`返回result.warnings中的警告。使用“verbose”值可获得更详细的警告。
* webkit (默认: `false`): 为Safari/WebKit bug启用工作区。PhantomJS用户应该将此选项设置为true

# 最小化配置的结构

```
{
    parse: {
        // parse options
    },
    compress: {
        // compress options
    },
    mangle: {
        // mangle options

        properties: {
            // mangle property options
        }
    },
    output: {
        // output options
    },
    sourceMap: {
        // source map options
    },
    nameCache: null, // or specify a name cache object
    toplevel: false,
    warnings: false,
}
```

## sourcemap配置

生成sourcemap：

```js
var result = UglifyJS.minify({"file1.js": "var a = function() {};"}, {
    sourceMap: {
        filename: "out.js",
        url: "out.js.map"
    }
});
console.log(result.code); // minified output
console.log(result.map);  // source map
```

要注意，此时sourcemap并不会保存为一份文件，它只会返回在`result.map`中。 `sourceMap.url` 传入的值只用来在`result.code`中设置`//# sourceMappingURL=out.js.map` ，`filename` 的值只用来在sourcemap文件中设置 file属性(详情看 [规范](https://docs.google.com/document/d/1U1RGAehQwRypUTovF1KRlpiOFze0b-_2gc6fAH0KY0k))。

你可以把`sourceMap.url`设为`inline` ，这样sourcemap会加在代码末尾。

你也可以指定sourcemap中的源文件根目录（sourceRoot）属性：

```js
var result = UglifyJS.minify({"file1.js": "var a = function() {};"}, {
    sourceMap: {
        root: "http://example.com/src",
        url: "out.js.map"
    }
});
```

如果你要压缩从其他文件编译得来的带一份sourcemap的JS文件，你可以用`sourceMap.content`参数：

```js
var result = UglifyJS.minify({"compiled.js": "compiled code"}, {
    sourceMap: {
        content: "content from compiled.js.map",
        url: "minified.js.map"
    }
});
// same as before, it returns `code` and `map`
```

如果你要用X-SourceMap请求头，你可以忽略 sourceMap.url。

如果你希望减少源映射的文件大小，将选项`sourceMap.names`设为`false`，所有的符号名称将被省略。

# 解析配置

* bare_returns (默认 `false`): 支持在顶级作用域中 return 声明
* html5_comments (默认 `true`)
* shebang (默认 `true`): 支持在第一行用 #!command

# 压缩配置

* annotations (默认: `true`): 传递`false`以禁用把可能丢弃的函数标记为“pure”。如果在函数调用之前有注释注解`/*@__PURE__*/`或`/*#__PURE__*/`，函数调用将被标记为“pure”。例如:`/ * @__PURE__ * / foo ()`;
* arguments (默认: `true`): 尽可能将`arguments[index]`替换为函数参数名;
* arrows (默认: `true`): 对箭头函数应用优化;
* assignments (默认: `true`): 对赋值表达式应用优化;
* awaits (默认: `true`): — 对`await`表达式应用优化;
* booleans (默认: `true`): 布尔上下文的各种优化,例如: `!!a ? b : c → a ? b : c` ;
* collapse_vars (默认: `true`): — 折叠一次性使用的非常量变量，允许副作用;
* comparisons (默认: `true`): 把结果必然的运算优化成二元运算，例如 `!(a <= b) → a > b`,  (只有设置了 unsafe_comps时才生效)；尽量转成否运算。例如 `a = !b && !c && !d && !e → a=!(b||c||d||e)` ;
* conditionals (默认: `true`): — 对if语句和条件表达式应用优化;
* dead_code (默认: `true`): 移除没被引用的代码;
* default_values (默认: `true`): — 移除无效默认值;
* directives (默认: `true`): — 删除冗余或非标准指令;
* drop_console (默认: `false`): 传`true`的话会干掉`console.*`函数。如果你要干掉特定的函数比如`console.info` ，又想删掉后保留其参数中的副作用，那用`pure_funcs`来处理吧。
* drop_debugger (默认: `true`): — 移除 `debugger`;
* evaluate (默认: `true`): 尝试计算常量表达式, 求表达式的值以求得更短的常数表示。在可能的情况下，传递"eager"函数调用总是替换函数调用，或者传递一个正整数来指定每个单独求值的字符数上限;
* expression (默认: `false`): — 传`true`来保留终端语句中没有"return"的完成值。例如在bookmarklets。
* functions (默认: `true`): — 尽可能将函数声明从var转换为`function`, 函数表达式转换为函数声明;
* global_defs (默认: `{}`): 查看[conditional compilation](https://www.npmjs.com/package/uglify-js#conditional-compilation)
* hoist_exports (默认: `true`): 提升`export`语句，以方便各种压缩和mangle优化;
* hoist_funs (默认: `false`): 提升函数声明;
* hoist_props (默认: `true`): — 将属性从常量对象和数组字面量提升到受一组约束的常规变量, 例如: `var o={p:1, q:2}; f(o.p, o.q)`; 转变为`f(1, 2)`;. 注意: `hoist_props` 最好配合 `toplevel` 和 `mangle` 一起使用, 同时压缩选项`passes`设置为2或更高;
* hoist_vars (默认: `false`): 提升 var 声明 (默认是false,因为那会加大文件的size);
* if_return (默认: `true`): — 优化 `if/return` 和 `if/continue`;
* imports (默认: `true`): 当使用`unused`配置时,移除未引用的`import`;
* inline (默认: `true`): 使用simple/return语句对函数进行内联调用:

  * false: 等同于0
  * 0: 禁用
  * 1: 内联简单函数
  * 2: 带参数的内联函数
  * 3: 带参数和变量的内联函数
  * true: 等同于3
* join_vars (默认: `true`): 连接连续的var语句;
* keep_fargs (默认: `false`): — 去掉未使用的函数参数，这样做可能不安全, 例如代码依赖于`Function.prototype.length`, 传递`true`以始终保留函数参数;
* keep_infinity (默认: `false`): 传递 `true` 以防止将 `Infinity` 压缩成`1/0`, 这可能会在Chrome中导致性能问题;
* loops (默认: `true`): 当do、while 、 for循环的判断条件可以确定是，对其进行优化;
* merge_vars (默认: `true`): 组合和重用变量;
* negate_iife (默认: `true`): 当立即执行函数（IIFE）的返回值没用时，取消之。避免代码生成器会插入括号;
* objects (默认: `true`): — 压缩对象字面量中的重复键;
* passes (默认: 1): 运行压缩的最大次数。在某些情况下，多次传递会导致进一步压缩代码。记住，通过的次数越多，花费的时间就越多;
* properties (默认: `true`): 使用点表示法重写属性访问, 例如 `foo["bar"] → foo.bar`;
* pure_funcs (默认: `null`): 你可以传入一个名字的数组，UglifyJS会假设这些函数没有函数副作用。**警告：** 假如名字在作用域中重新定义，不会再次检测。例如`var q = Math.floor(a/b)`，假如变量`q`没有被引用，UglifyJS会干掉它，但 `Math.floor(a/b)`会被保留，没有人知道它是干嘛的。你可以设置`pure_funcs: [ 'Math.floor' ]` ，这样该函数会被认为没有函数副作用，这样整个声明会被废弃。在目前的执行情况下，会增加开销（压缩会变慢）。
* pure_getters (默认: "strict"): 传递`true`,UglifyJS假设对象属性访问(例如`foo.Bar或foo[" Bar "]`)没有任何副作用。指定“strict”将`foo.Bar`作为无副作用处理，只有当`foo`确定不为`null`或`undefined`;
* reduce_funcs (默认: `true`): 在允许进一步优化的情况下，将一次性函数内联为函数表达式。默认启用。`选项取决于是否启用了reduce_vars`选项。如果禁用此选项，一些代码在Chrome V8引擎中运行得更快。不会对其他主流浏览器产生负面影响;
* reduce_vars (默认: `true`): 优化赋值和用作常量的变量;
* rests (默认: `true`): 对rest参数应用优化;
* sequences (默认: `true`): 连续声明变量，用逗号隔开来。可以设置为正整数来指定连续声明的最大长度。如果设为`true` 表示默认200个，设为`false`或0则禁用。 sequences至少要是2,是1的话等同于`true`（即200)。默认的sequences设置有极小几率会导致压缩很慢，所以推荐设置成20或以下。
* side_effects (默认: `true`): 删除不影响运行时执行结果的多余代码;
* spreads (默认: `true`): 压缩展开的表达式;
* strings (默认: `true`): 压缩连接操作符;
* switches (默认: `true`): 删除重复以及用不到的`switch`语句;
* templates (默认: `true`): 通过嵌入表达式和/或转换为字符串文字来压缩模板文字, 例如: `foo ${42}` → "foo 42"
* top_retain (默认: `null`): 当设了unused时，保留顶层作用域中的某些函数变量。(可以写成数组，用逗号隔开，也可以用正则或函数. 参考`toplevel`选项);
* toplevel (默认: `false`): 干掉顶层作用域中没有被引用的函数 ("funcs")和/或变量("vars") (默认是`false` , `true` 的话即函数变量都干掉);
* typeofs (默认: `true`): — 转换 t`ypeof foo == "undefined"` 成 `foo === void 0`. 注意：如果要适配IE10或以下，由于已知的问题，推荐设成false 。
* unsafe (默认: `false`): 应用“不安全的”转换;
* unsafe_comps (默认: `false`): 压缩表达式，如`a <= b`，假设所有操作数都不能(强制)为`NaN`;
* unsafe_Function (默认: `false`): 当`args`和`code`都是字符串字面值时，压缩和混淆`Function(args, code)`;
* unsafe_math (默认: `false`): 将`2 * x * 3`等数值表达式优化为`6 * x`，这可能会给出不精确的浮点结果;
* unsafe_proto (默认: `false`): 优化 `Array.prototype.slice.call(a)` 为 `[].slice.call(a)`;
* unsafe_regexp (默认: `false`): 允许用RegExp值替换变量，就像它们是常量一样;
* unsafe_undefined (默认: `false`): 如果作用域中有一个名为`undefined`的变量，则替换为`void 0`(变量名将被打乱，通常减少到一个字符);
* unused (默认: `true`): 干掉没有被引用的函数和变量。（除非设置"keep_assign"，否则变量的简单直接赋值也不算被引用）;
* varify (默认: `true`): 在安全的情况下将块范围的声明转换为`var`;
* yields (默认: `true`): — 优化 `yield` 表达式;

# 混淆选项

* eval (默认: `false`): 混淆那些在`with`或`eval`中出现的名字;
* reserved (默认: `[]`): 传一个不需要混淆的名字的数组。 Example: ["foo", "bar"];
* toplevel (默认: `false`): 混淆那些定义在顶层作用域的名字;

```js
// test.js
var globalVar;
function funcName(firstLongName, anotherLongName) {
    var myVariable = firstLongName +  anotherLongName;
}
var code = fs.readFileSync("test.js", "utf8");

UglifyJS.minify(code).code;
// 'function funcName(a,n){}var globalVar;'

UglifyJS.minify(code, { mangle: { reserved: ['firstLongName'] } }).code;
// 'function funcName(firstLongName,a){}var globalVar;'

UglifyJS.minify(code, { mangle: { toplevel: true } }).code;
// 'function n(n,a){}var a;'
```

## 混淆属性选项

* builtins (默认: `false`): 传 `true`的话，允许混淆内置的DOM属性名。不推荐使用;
* debug (默认: `false`): 用原名字来组成混淆后的名字. 传空字符串"" 来启用,或者非空字符串作为debu后缀。（例如"abc", `foo.bar=>foo.barabc`)
* keep_quoted (默认: `false`): 只混淆不在括号内的属性名;
* regex (默认: `null`): 传一个正则，只混淆匹配该正则的属性名;
* reserved (默认: `[]`): 不混淆在reserved 数组里的属性名;

# 输出选项

代码生成器默认会尽量输出最简短的代码。假如你要美化一下输出代码，可以传--beautify (-b)。你也可以传更多的参数来控制输出代码：

* annotations (默认: `false`): 通过true保留注释 `/*@__PURE__*/` 或 `/*#__PURE__*/`, 否则，即使设置了注释，它们也会被丢弃;
* ascii_only (默认: `false`): 忽略字符串和正则（导致非ascii字符失效）中的Unicode字符;
* beautify (默认: `true`): 是否美化输出代码。传-b的话就是设成true。假如你想生成最小化的代码同时又要用其他设置来美化代码，你可以设-b beautify=false;
* braces (默认: `false`): 永远在`if`, `for`,`do`, `while`, `with`后面加上大括号，即使循环体只有一句;
* comments (默认: `false`): 传 `true` 或 "all"保留全部注释,传 "some"保留部分，传正则 (例如 `/^!/`) 或者函数也行, 例如:

```js
function(node, comment) {
    return comment.value.indexOf("@type " + node.TYPE) >= 0;
}
```

* galio (默认: `false`): — 支持 ANT Galio bugs 环境
* indent_level (默认: 4): 缩进格数;
* indent_start (默认: 0): 在所有行前面加上指定数目空格;
* inline_script (默认: `true`): 避免字符串中出现`</script>`中的斜杠;
* keep_quoted_props (默认: `false`): 如果启用，会保留对象属性名的引号;
* max_line_len (默认: `false`): — 最大行宽（压缩后的代码）;
* preamble (默认: `null`): 如果要传的话，必须是字符串。它会被加在输出文档的前面。sourcemap会随之调整。例如可以用来插入版权信息;
* preserve_line (默认: `false`): 传 `true` 就保留空行，但只在`beautify` 设为`false`时有效;
* quote_keys (默认: `false`): 传`true`的话会在对象所有的键加上括号;
* quote_style (默认: 0): 影响字符串的括号格式（也会影响属性名和指令）:

  * 0: 倾向使用双引号，字符串里还有引号的话就是单引号;
  * 1: 永远单引号;
  * 2: 永远双引号;
  * 3: 永远是本来的引号;
* semicolons (默认: `true`): 用分号分开多个声明。如果你传false,则总会另起一行，增强输出文件的可读性。（gzip前体积更小，gzip后稍大一点点）;
* shebang (默认: `true`): 保留开头的 shebang #! (bash 脚本);
* width (默认: 80): 仅在美化时生效，设定一个行宽让美化器尽量实现。这会影响行中文字的数量（不包括缩进）。当前本功能实现得不是非常好，但依然让美化后的代码可读性大大增强;
* wrap_iife (默认: `false`): 传true的话，把立即执行函数括起来。 [查看更多详情](https://github.com/mishoo/UglifyJS2/issues/640)

# 其他参数

## 保留版权告示或其他注释

你可以传入`--comments`让输出文件中保留某些注释。默认时会保留JSDoc-style的注释（包含"@preserve","@license" 或 "@cc_on"（为IE所编译））。你可以传入`--comments all`来保留全部注释，或者传一个合法的正则来保留那些匹配到的注释。例如`--comments /^!/会保留/*! Copyright Notice */`这样的注释。

注意，无论如何，总会有些注释在某些情况下会丢失。例如：

```js
function f() {
    /** @preserve Foo Bar */
    function g() {
        // this function is never called
    }
    return something();
}
```

即使里面带有"@preserve"，注释依然会被丢弃。因为内部的函数g（注释所依附的抽象语法树节点）没有被引用、会被压缩器干掉。

书写版权信息（或其他需要在输出文件中保留的信息）的最安全位置是全局节点。

## 不安全压缩选项

在某些刻意营造的案例中，启用某些转换有可能会打断代码的逻辑，但绝大部分情况下是安全的。你可能会想尝试一下，因为这毕竟会减少文件体积。以下是某些例子：

* `new Array(1, 2, 3)` 或 `Array(1, 2, 3)` → [ 1, 2, 3 ]
* `new Object()` → {}
* `String(exp)` 或 `exp.toString()` → "" + exp
* `new Object/RegExp/Function/Error/Array (...)` → 我们干掉用new的
* `void 0` → undefined (假如作用域中有一个变量名叫"undefined";我们这么做是因为变量名会被混淆成单字符）

## 编译条件语句

Uglify会假设全局变量都是常量(不管是否在局部域中定义了)，你可以用--define (-d)来实现定义全局变量。例如你传--define DEBUG=false，UglifyJS会在输出中干掉下面代码：

```js
if (DEBUG) {
    console.log("debug stuff");
}
```

你可以像`--define env.DEBUG=false`这样写嵌套的常量。

在干掉那些永否的条件语句以及不可达代码时，UglifyJS会给出警告。现在没有选项可以禁用此特性，但你可以设置 `warnings=false` 来禁掉所有警告。

另一个定义全局常量的方法是，在一个独立的文档中定义，再引入到构建中。例如你有一个这样的build/defines.js:

```js
const DEBUG = false;
const PRODUCTION = true;
// 等等
```

这样构建你的代码：

```shell
uglifyjs build/defines.js js/foo.js js/bar.js... -c
```

UglifyJS会注意到这些常量。因为它们无法改变，所以它们会被认为是没被引用而被照样干掉。如果你用`const`声明，构建后还会被保留。如果你的运行环境低于**ES6**、不支持`const`，请用`var`声明加上`reduce_vars`设置（默认启用）来实现。

## 编译条件语句API

你也可以通过程序API来设置编译配置。其中有差别的是一个压缩器属性`global_defs`：

```js
var result = UglifyJS.minify(fs.readFileSync("input.js", "utf8"), {
    compress: {
        dead_code: true,
        global_defs: {
            DEBUG: false
        }
    }
});
```

在`global_defs`配"@"前缀的表达式，UglifyJS才会替换成语句表达式：

```js
UglifyJS.minify("alert('hello');", {
    compress: {
        global_defs: {
            "@alert": "console.log"
        }
    }
}).code;
// returns: 'console.log("hello");'
```

否则会替换成字符串:

```js
UglifyJS.minify("alert('hello');", {
    compress: {
        global_defs: {
            "alert": "console.log"
        }
    }
}).code;
// returns: '"console.log"("hello");'
```

## 使用minify()获得原生UglifyJS ast

```js
// 例子: 只解析代码，获得原生Uglify AST

var result = UglifyJS.minify(code, {
    parse: {},
    compress: false,
    mangle: false,
    output: {
        ast: true,
        code: false  // optional - faster if false
    }
});

// result.ast 即是原生 Uglify AST
```

```js
// 例子: 输入原生 Uglify AST，接着把它压缩并混淆，生成代码和原生ast

var result = UglifyJS.minify(ast, {
    compress: {},
    mangle: {},
    output: {
        ast: true,
        code: true  // 可选，false更快
    }
});

// result.ast 是原生 Uglify AST
// result.code 是字符串格式的最小化后的代码
```

# 使用 Uglify AST

可以通过[TreeWalker](https://github.com/mishoo/UglifyJS/blob/master/lib/ast.js)和[TreeTransformer](https://github.com/mishoo/UglifyJS/blob/master/lib/transform.js)分别横截和转换原生AST。

# ESTree/SpiderMonkey AST

UglifyJS有自己的抽象语法树格式；为了某些[现实的原因](http://lisperator.net/blog/uglifyjs-why-not-switching-to-spidermonkey-ast/) 我们无法在内部轻易地改成使用SpiderMonkey AST。但UglifyJS现在有了一个可以输入SpiderMonkeyAST的转换器。 例如[Acorn](https://github.com/ternjs/acorn) ，这是一个超级快的生成SpiderMonkey AST的解释器。它带有一个实用的迷你CLI，能解释一个文件、把AST转存为JSON并标准输出。可以这样用UglifyJS来压缩混淆：

```shell
acorn file.js | uglifyjs --spidermonkey -m -c
```

`-p --spidermonkey`选项能让UglifyJS知道输入文件并非JavaScript，而是SpiderMonkey AST生成的JSON代码。这事我们不用自己的解释器，只把AST转成我们内部AST。

# 使用 Acorn 来解释代码

更有趣的是，我们加了 `-p --acorn`选项来使用Acorn解释所有代码。如果你传入这个选项，UglifyJS会require("acorn")

Acorn确实非常快（650k代码原来要380ms，现在只需250ms），但转换Acorn产生的SpiderMonkey树会额外花费150ms。所以总共比UglifyJS自己的解释器还要多花一点时间。

# Uglify快速最小化模式

这一点并不广为人知，但对于大多数JavaScript来说，在缩减后的代码中，删除空白和符号混乱占了95%——而不是复杂的代码转换。你可以简单地禁用压缩来加速Uglify构建3到5倍。


| d3.js                                         | minify size | gzip size | minify time (seconds) |
| ----------------------------------------------- | ------------- | ----------- | ----------------------- |
| original                                      | 511,371     | 119,932   |                       |
| uglify-js@3.13.0 mangle=false, compress=false | 363,988     | 95,695    | 0.56                  |
| uglify-js@3.13.0 mangle=true, compress=false  | 253,305     | 81,281    | 0.99                  |
| uglify-js@3.13.0 mangle=true, compress=true   | 244,436     | 79,854    | 5.30                  |


使用CLI命令开启快速缩容模式:

```shell
uglifyjs file.js -m
```

使用API来启用快速缩小模式:

```js
UglifyJS.minify(code, { compress: false, mangle: true });
```

# 源映射和调试

各种简化、重排、内联和删除代码的压缩转换都对源代码映射的调试有不利影响。这是预期的，因为代码被优化了，映射通常是不可能的，因为一些代码已经不存在了。要在源映射调试中获得最高的保留，请禁用Uglify`compress`选项并使用`mangle`选项。

# 编译器假设

为了实现更好的优化，编译器做出了各种假设:

* 代码不依赖于保持其运行时性能特征。通常丑化的代码会运行得更快，因为更少的指令和更容易的内联，但在特定平台上可能会很慢，例如，参见`reduce_funcs`选项;

* `.tostring()`和`.valueof()`没有副作用，对于内置对象，它们没有被重写;

* `undefined`，`NaN`和`Infinity`并没有被外部重新定义;

* `arguments.callee`, `arguments.caller` 和 `Function.prototype.caller`没有被使用;

* 代码不期望`Function.prototype.toString()`或`Error.prototype.stack`的内容是任何特定的内容;

* 获取和设置普通对象的属性不会导致其他副作用(使用`.watch()`或`Proxy`);

* 对象属性可以被添加、移除和修改(不会被`Object.defineproperty()`、`Object.defineproperties()`、`Object.freeze()`、`Object.preventextensions()`或`Object.seal()`阻止);

* 如果存在数组解构，则在`Array.prototype`中类似索引的属性没有被重写;

```js
Object.prototype[0] = 42;
var [ a ] = [];
var { 0: b } = {};
// 42 undefined
console.log([][0], a);
// 42 42
console.log({}[0], b);
```

* 早期版本的JavaScript会抛出如下的`SyntaxError`:

```js
({
    p: 42,
    get p() {},
});
// SyntaxError: Object literal may not have data and accessor property with
//              the same name
```

UglifyJS可以修改输入，从而抑制这些错误。

* 在Chrome和Node.js的后续版本中，包含扩展语法的对象上键的迭代顺序可能会被改变;

* 当启用`tolevel`时，UglifyJS有效地假设输入代码被包装在`function(){…}`，从而禁止已声明的全局变量的别名:

```js
A = "FAIL";
var B = "FAIL";
// can be `global`, `self`, `window` etc.
var top = function() {
    return this;
}();
// "PASS"
top.A = "PASS";
console.log(A);
// "FAIL" after compress and/or mangle
top.B = "PASS";
console.log(B);
```

* 使用参数和解构函数参数，例如`function({}， arguments){}`,在早期版本的Chrome和Node.js中将导致`SyntaxError`,UglifyJS可以通过修改输入，从而可能抑制这些错误。

* 早期版本的Chrome和Node.js将抛出如下的`ReferenceError`:

```js
var a;
try {
    throw 42;
} catch ({
    [a]: b,
    // ReferenceError: a is not defined
}) {
    let a;
}
```
UglifyJS可以修改输入，从而抑制这些错误。

* 后续版本的JavaScript会抛出如下的`SyntaxError`:

```js
a => {
    let a;
};
// SyntaxError: Identifier 'a' has already been declared
```
UglifyJS可以修改输入，从而抑制这些错误。

* 后续版本的JavaScript会抛出如下的`SyntaxError`:

```js
try {
    // ...
} catch ({ message: a }) {
    var a;
}
// SyntaxError: Identifier 'a' has already been declared
```
UglifyJS可以修改输入，从而抑制这些错误。

* 一些版本的Chrome和Node.js会抛出如下的`ReferenceError`:

```js
console.log(((a, b = function() {
    return a;
    // ReferenceError: a is not defined
}()) => b)());
```
UglifyJS可以修改输入，从而抑制这些错误。

* 一些使用`BigInt`的算术操作可能抛出`TypeError`:

```js
1n + 1;
// TypeError: can't convert BigInt to number
```
UglifyJS可以修改输入，从而抑制这些错误。

* 某些版本的JavaScript会抛出`SyntaxError`:

```js
console.log(String.raw`\uFo`);
// SyntaxError: Invalid Unicode escape sequence
```
UglifyJS可以修改输入，从而抑制这些错误。

* 一些版本的JavaScript会抛出`SyntaxError`:

```js
try {} catch (e) {
    for (var e of []);
}
// SyntaxError: Identifier 'e' has already been declared
```
UglifyJS可以修改输入，从而抑制这些错误。

* 一些版本的Chrome和Node.js将给出以下错误的结果:

```js
console.log({
    ...{
        set 42(v) {},
        42: "PASS",
    },
});
// Expected: { '42': 'PASS' }
// Actual:   { '42': undefined }
```
UglifyJS可以修改输入，从而抑制这些错误。

* 后续版本的JavaScript将抛出`SyntaxError`:

```js
var await;
async function f() {
    class A {
        static p = await;
    }
}
// SyntaxError: Unexpected reserved word
```
UglifyJS可以修改输入，从而抑制这些错误。

* 后续版本的JavaScript将抛出`SyntaxError`:

```js
var async;
for (async of []);
// SyntaxError: The left-hand side of a for-of loop may not be 'async'.
```
UglifyJS可以修改输入，从而抑制这些错误。

* 以后版本的Chrome和Node.js将给出以下错误的结果:

```js
console.log({
    ...console,
    get 42() {
        return "FAIL";
    },
    [42]: "PASS",
}[42]);
// Expected: "PASS"
// Actual:   "FAIL"
```
UglifyJS可以修改输入，从而抑制这些错误。

* 早期版本的JavaScript会抛出`TypeError`:

```js
(function() {
    {
        const a = "foo";
    }
    {
        const a = "bar";
    }
})();
// TypeError: const 'a' has already been declared
```
