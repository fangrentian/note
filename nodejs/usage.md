# 全局对象与全局变量

JavaScript 中有一个特殊的对象，称为全局对象（Global Object），它及其所有属性都可以在程序的任何地方访问，即全局变量。

在浏览器 JavaScript 中，通常 window 是全局对象， 而 Node.js 中的全局对象是 global，所有全局变量（除了 global 本身以外）都是 global 对象的属性。

在 Node.js 我们可以直接访问到 global 的属性，而不需要在应用中包含它。

global 最根本的作用是作为全局变量的宿主。按照 ECMAScript 的定义，满足以下条件的变量是全局变量：

* 在最外层定义的变量；
* 全局对象的属性；
* 隐式定义的变量（未定义直接赋值的变量）。

当你定义一个全局变量时，这个变量同时也会成为全局对象的属性，反之亦然。需要注 意的是，在Node.js 中你不可能在最外层定义变量，因为所有用户代码都是属于当前模块的， 而模块本身不是最外层上下文。

## __filename

__filename 表示当前正在执行的脚本的文件名。它将输出文件所在位置的绝对路径，且和命令行参数所指定的文件名不一定相同。 如果在模块中，返回的值是模块文件的路径。

## __dirname

__dirname 表示当前执行脚本所在的目录。

## setTimeout(cb, ms)

setTimeout(cb, ms) 全局函数在指定的毫秒(ms)数后执行指定函数(cb)。：setTimeout() 只执行一次指定函数。 返回一个代表定时器的句柄值。

```js
function printHello(){
   console.log( "Hello, World!");
}
// 两秒后执行以上函数
setTimeout(printHello, 2000);
```

## clearTimeout(t)

clearTimeout( t ) 全局函数用于停止一个之前通过 setTimeout() 创建的定时器。 参数 t 是通过 setTimeout() 函数创建的定时器。

```js
function printHello(){
   console.log( "Hello, World!");
}
// 两秒后执行以上函数
var t = setTimeout(printHello, 2000);

// 清除定时器
clearTimeout(t);
```

## setInterval(cb, ms)

setInterval(cb, ms) 全局函数在指定的毫秒(ms)数后执行指定函数(cb)。 返回一个代表定时器的句柄值。可以使用 clearInterval(t) 函数来清除定时器。 setInterval() 方法会不停地调用函数，直到 clearInterval() 被调用或窗口被关闭。

```js
function printHello(){
   console.log( "Hello, World!");
}
// 两秒后执行以上函数
setInterval(printHello, 2000);
```

## console

console 用于提供控制台标准输出，它是由 Internet Explorer 的 JScript 引擎提供的调试工具，后来逐渐成为浏览器的实施标准。

Node.js 沿用了这个标准，提供与习惯行为一致的 console 对象，用于向标准输出流（stdout）或标准错误流（stderr）输出字符。

### console.log([data][, ...])

向标准输出流打印字符并以换行符结束。该方法接收若干 个参数，如果只有一个参数，则输出这个参数的字符串形式。如果有多个参数，则 以类似于C 语言 printf() 命令的格式输出。

### console.info([data][, ...])

该命令的作用是返回信息性消息，这个命令与console.log差别并不大，除了在chrome中只会输出文字外，其余的会显示一个蓝色的惊叹号。

### console.error([data][, ...])

输出错误消息的。控制台在出现错误时会显示是红色的叉子。

### console.warn([data][, ...])

输出警告消息。控制台出现有黄色的惊叹号。

### console.dir(obj[, options])

用来对一个对象进行检查（inspect），并以易于阅读和打印的格式显示。

### console.time(label)

输出时间，表示计时开始。

### console.timeEnd(label)

结束时间，表示计时结束。

### console.trace(message[, ...])

当前执行的代码在堆栈中的调用路径，这个测试函数运行很有帮助，只要给想测试的函数里面加入 console.trace 就行了。

### console.assert(value[, message][, ...])

用于判断某个表达式或变量是否为真，接收两个参数，第一个参数是表达式，第二个参数是字符串。只有当第一个参数为false，才会输出第二个参数，否则不会有任何结果。

## process

它用于描述当前Node.js 进程状态的对象，提供了一个与操作系统的简单接口。通常在你写本地命令行程序的时候，少不了要 和它打交道。下面将会介绍 process 对象的一些最常用的成员方法:

### exit

当进程准备退出时触发。

### beforeExit

当 node 清空事件循环，并且没有其他安排时触发这个事件。通常来说，当没有进程安排时 node 退出，但是 'beforeExit' 的监听器可以异步调用，这样 node 就会继续执行。

### uncaughtException

当一个异常冒泡回到事件循环，触发这个事件。如果给异常添加了监视器，默认的操作（打印堆栈跟踪信息并退出）就不会发生。

### Signal 事件

当进程接收到信号时就触发。信号列表详见标准的 POSIX 信号名，如 SIGINT、SIGUSR1 等。



# 创建一个简单的http服务

```js
var http = require('http')
var server = http.createServer()
server.on('request', function (request, response) {
	// 对收到客户端的请求做出处理和响应
	console.log('收到客户端的请求了，请求路径是：' + request.url)
	response.writeHead(200, {'Content-Type': 'text/plain'});
	// response 对象有一个方法：write 可以用来给客户端发送响应数据
	// write 可以使用多次，但是最后一定要使用 end 来结束响应，否则客户端会一直等待
	response.write('hello')
	response.write(' nodejs')
	// 告诉客户端，我的话说完了，你可以呈递给用户了
	response.end()
})

server.listen(3000, function () {
    console.log('服务器启动成功了，可以通过 http://127.0.0.1:3000/ 来进行访问')
})
```

# 文件系统

Node.js 文件系统（fs 模块）模块中的方法均有异步和同步版本，例如读取文件内容的函数有异步的 fs.readFile() 和同步的 fs.readFileSync()。 异步的方法函数最后一个参数为回调函数，回调函数的第一个参数包含了错误信息(error)。

## 读取文件 fs.readFile(filename[, options], callback)

```js
var fs = require("fs");
// 异步读取
fs.readFile('input.txt', function (err, data) {
   if (err) {
       return console.error(err);
   }
   console.log("异步读取: " + data.toString());
});
// 同步读取
var data = fs.readFileSync('input.txt');
console.log("同步读取: " + data.toString());
console.log("程序执行完毕。");
```

## 打开文件 fs.open(path, flags[, mode], callback)

* path: 文件的路径。
* flags: 文件打开的行为。具体值如下:
    * r:	以读取模式打开文件。如果文件不存在抛出异常。
    * r+:	以读写模式打开文件。如果文件不存在抛出异常。
    * rs:	以同步的方式读取文件。
    * rs+:	以同步的方式读取和写入文件。
    * w:	以写入模式打开文件，如果文件不存在则创建。
    * wx:	类似 'w'，但是如果文件路径存在，则文件写入失败。
    * w+:	以读写模式打开文件，如果文件不存在则创建。
    * wx+:	类似 'w+'， 但是如果文件路径存在，则文件读写失败。
    * a:	以追加模式打开文件，如果文件不存在则创建。
    * ax:	类似 'a'， 但是如果文件路径存在，则文件追加失败。
    * a+:	以读取追加模式打开文件，如果文件不存在则创建。
    * ax+:	类似 'a+'， 但是如果文件路径存在，则文件读取追加失败。
* mode: 设置文件模式(权限)，文件创建默认权限为 0666(可读，可写)。
* callback: 回调函数，带有两个参数如：callback(err, fd)。

```js
var fs = require("fs");

// 异步打开文件
console.log("准备打开文件！");
fs.open('input.txt', 'r+', function(err, fd) {
   if (err) {
       return console.error(err);
   }
  console.log("文件打开成功！");     
});
```

## 获取文件信息 fs.stat(path, callback)

* path: 文件路径。
* callback: 回调函数，带有两个参数如：(err, stats), stats 是 fs.Stats 对象, 其包含以下方法: 
    * stats.isFile():	如果是文件返回 true，否则返回 false。
    * stats.isDirectory():	如果是目录返回 true，否则返回 false。
    * stats.isBlockDevice():	如果是块设备返回 true，否则返回 false。
    * stats.isCharacterDevice():	如果是字符设备返回 true，否则返回 false。
    * stats.isSymbolicLink():	如果是软链接返回 true，否则返回 false。
    * stats.isFIFO():	如果是FIFO，返回true，否则返回 false。FIFO是UNIX中的一种特殊类型的命令管道。
    * stats.isSocket():	如果是 Socket 返回 true，否则返回 false。

fs.stat(path)执行后，会将stats类的实例返回给其回调函数。可以通过stats类中的提供方法判断文件的相关属性。例如判断是否为文件：

```js
var fs = require("fs");

console.log("准备打开文件！");
fs.stat('input.txt', function (err, stats) {
	if (err) {
		return console.error(err);
	}
	console.log(stats);
	console.log("读取文件信息成功！");

	// 检测文件类型
	console.log("是否为文件(isFile) ? " + stats.isFile());
	console.log("是否为目录(isDirectory) ? " + stats.isDirectory());
});
```

## 写入文件 fs.writeFile(file, data[, options], callback)

* file: 文件名或文件描述符。
* data: 要写入文件的数据，可以是 String(字符串) 或 Buffer(缓冲) 对象。
* options: 该参数是一个对象，包含 {encoding, mode, flag}。默认编码为 utf8, 模式为 0666 ， flag 为 'w'
* callback: 回调函数，回调函数只包含错误信息参数(err)，在写入失败时返回。

writeFile 直接打开文件默认是 w 模式，所以如果文件存在，该方法写入的内容会覆盖旧的文件内容。

```js
var fs = require("fs");

console.log("准备写入文件");
fs.writeFile('input.txt', '我是通 过fs.writeFile 写入文件的内容',  function(err) {
   if (err) {
       return console.error(err);
   }
   console.log("数据写入成功！");
   console.log("--------我是分割线-------------")
   console.log("读取写入的数据！");
   fs.readFile('input.txt', function (err, data) {
      if (err) {
         return console.error(err);
      }
      console.log("异步读取文件数据: " + data.toString());
   });
});
```

## 读取文件 fs.read(fd, buffer, offset, length, position, callback)

* fd: 通过 fs.open() 方法返回的文件描述符。
* buffer: 数据写入的缓冲区。
* offset: 缓冲区写入的写入偏移量。
* length: 要从文件中读取的字节数。
* position: 文件读取的起始位置，如果 position 的值为 null，则会从当前文件指针的位置读取。
* callback: 回调函数，有三个参数err, bytesRead, buffer，err 为错误信息， bytesRead 表示读取的字节数，buffer 为缓冲区对象。

```js
var fs = require("fs");
var buf = new Buffer.alloc(1024);

console.log("准备打开已存在的文件！");
fs.open('input.txt', 'r+', function(err, fd) {
   if (err) {
       return console.error(err);
   }
   console.log("文件打开成功！");
   console.log("准备读取文件：");
   fs.read(fd, buf, 0, buf.length, 0, function(err, bytes){
      if (err){
         console.log(err);
      }
      console.log(bytes + "  字节被读取");
      
      // 仅输出读取的字节
      if(bytes > 0){
         console.log(buf.slice(0, bytes).toString());
      }
   });
});
```

## 关闭文件 fs.close(fd, callback)

* fd: 通过 fs.open() 方法返回的文件描述符。
* callback: 回调函数，没有参数。

```js
var fs = require("fs");
var buf = new Buffer.alloc(1024);

console.log("准备打开文件！");
fs.open('input.txt', 'r+', function(err, fd) {
   if (err) {
       return console.error(err);
   }
   console.log("文件打开成功！");
   console.log("准备读取文件！");
   fs.read(fd, buf, 0, buf.length, 0, function(err, bytes){
      if (err){
         console.log(err);
      }

      // 仅输出读取的字节
      if(bytes > 0){
         console.log(buf.slice(0, bytes).toString());
      }

      // 关闭文件
      fs.close(fd, function(err){
         if (err){
            console.log(err);
         } 
         console.log("文件关闭成功");
      });
   });
});
```

## 截取文件 fs.ftruncate(fd, len, callback)

* fd: 通过 fs.open() 方法返回的文件描述符。
* len: 文件内容截取的长度。
* callback: 回调函数，没有参数。

```js
var fs = require("fs");
var buf = new Buffer.alloc(1024);

console.log("准备打开文件！");
fs.open('input.txt', 'r+', function(err, fd) {
   if (err) {
       return console.error(err);
   }
   console.log("文件打开成功！");
   console.log("截取10字节内的文件内容，超出部分将被去除。");
   
   // 截取文件
   fs.ftruncate(fd, 10, function(err){
      if (err){
         console.log(err);
      } 
      console.log("文件截取成功。");
      console.log("读取相同的文件"); 
      fs.read(fd, buf, 0, buf.length, 0, function(err, bytes){
         if (err){
            console.log(err);
         }

         // 仅输出读取的字节
         if(bytes > 0){
            console.log(buf.slice(0, bytes).toString());
         }

         // 关闭文件
         fs.close(fd, function(err){
            if (err){
               console.log(err);
            } 
            console.log("文件关闭成功！");
         });
      });
   });
});
```

## 删除文件 fs.unlink(path, callback)

* path: 文件路径。
* callback: 回调函数，没有参数。

```js
var fs = require("fs");

console.log("准备删除文件！");
fs.unlink('input.txt', function(err) {
   if (err) {
       return console.error(err);
   }
   console.log("文件删除成功！");
});
```

## 创建目录 fs.mkdir(path[, options], callback)

* path: 文件路径。
* options 参数可以是：recursive - 是否以递归的方式创建目录，默认为 false。mode - 设置目录权限，默认为 0777。
* callback: 回调函数，没有参数。

```js
var fs = require("fs");
// tmp 目录必须存在
console.log("创建目录 /tmp/test/");
fs.mkdir("/tmp/test/",function(err){
   if (err) {
       return console.error(err);
   }
   console.log("目录创建成功。");
});
```

## 读取目录 fs.readdir(path, callback)

* path: 文件路径。
* callback: 回调函数，回调函数带有两个参数err, files，err 为错误信息，files 为 目录下的文件数组列表。

```js
var fs = require("fs");

console.log("查看 /tmp 目录");
fs.readdir("/tmp/",function(err, files){
   if (err) {
       return console.error(err);
   }
   files.forEach( function (file){
       console.log( file );
   });
});
```

## 删除目录 fs.rmdir(path, callback)

* path: 文件路径。
* callback: 回调函数，没有参数。

```js
var fs = require("fs");
// 执行前创建一个空的 /tmp/test 目录
console.log("准备删除目录 /tmp/test");
fs.rmdir("/tmp/test",function(err){
   if (err) {
       return console.error(err);
   }
   console.log("读取 /tmp 目录");
   fs.readdir("/tmp/",function(err, files){
      if (err) {
          return console.error(err);
      }
      files.forEach( function (file){
          console.log( file );
      });
   });
});
```

# 路径模块

Node.js路径（path）模块包含一系列用于处理和转换文件路径的工具集。基本所有的反复都仅对字符串转换。文件系统不会检查路径是否有效。 你可以通过`require('path')`来访问这个模块.

## path.normalize(p)

用于规范化路径，注意'..'和'.'。 发现多个斜杠时，会替换成一个斜杠。当路径末尾包含一个斜杠时，保留。 Windows系统使用反斜杠。

```js
path.normalize('/foo/bar//baz/asdf/quux/..')
// returns
'/foo/bar/baz/asdf'
```

## path.join([path1][, path2][, ...])

用于连接所有的参数，并规范化输出路径。 参数必须是字符串。在v0.8版本，非字符参数会被忽略。v0.10之后的版本后抛出异常。

```js
path.join('/foo', 'bar', 'baz/asdf', 'quux', '..')
// returns
'/foo/bar/baz/asdf'

path.join('foo', {}, 'bar')
// throws exception
TypeError: Arguments to path.join must be strings
```

## path.resolve([from ...], to)

能够将to参数解析为绝对路径。 如果参数to不是一个相对于参数from的绝对路径，to会添加到from右侧，直到找到一个绝对路径为止。如果使用所有from参数后，还是没有找到绝对路径，将会使用当前工作目录。返回的路径已经规范化过，并且去掉了尾部的斜杠（除非是根目录）。非字符串的参数会被忽略。

```js
path.resolve('/foo/bar', './baz')
// returns
'/foo/bar/baz'

path.resolve('/foo/bar', '/tmp/file/')
// returns
'/tmp/file'

path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif')
// if currently in /home/myself/node, it returns
'/home/myself/node/wwwroot/static_files/gif/image.gif'
```

## path.isAbsolute(path)

判断参数path是否是绝对路径。一个绝对路径解析后都会指向相同的位置，无论当前的工作目录在哪里。

## path.relative(from, to)

解决从from到to的相对路径。 有时我们会有2个绝对路径，需要从中找到相对目录。这是path.resolve的逆实现：

```js
path.relative('C:\\orandea\\test\\aaa', 'C:\\orandea\\impl\\bbb')
// returns
'..\\..\\impl\\bbb'

path.relative('/data/orandea/test/aaa', '/data/orandea/impl/bbb')
// returns
'../../impl/bbb'
```

## path.dirname(p)

返回路径p所在的目录。和Unixdirname命令类似。

```js
path.dirname('/foo/bar/baz/asdf/quux')
// returns
'/foo/bar/baz/asdf'
```

## path.basename(p[, ext])

返回路径的最后一个部分。和Unixbasename命令类似。

```js
path.basename('/foo/bar/baz/asdf/quux.html')
// returns
'quux.html'

path.basename('/foo/bar/baz/asdf/quux.html', '.html')
// returns
'quux'
```

## path.extname(p)

返回路径p的扩展名，从最后一个'.'到字符串的末尾。如果最后一个部分没有'.' ，或者路径是以'.'开头，则返回空字符串。例如：

```js
path.extname('index.html')
// returns
'.html'

path.extname('index.coffee.md')
// returns
'.md'

path.extname('index.')
// returns
'.'

path.extname('index')
// returns
```

## path.sep

特定平台的文件分隔符，'\\'或'/'。

```js
// *nix上的例子
'foo/bar/baz'.split(path.sep)
// returns
['foo', 'bar', 'baz']

// Windows的例子
'foo\\bar\\baz'.split(path.sep)
// returns
	['foo', 'bar', 'baz']
```

## path.delimiter

特定平台的分隔符，;或者':'。

```js
// *nix上的例子
console.log(process.env.PATH)
// '/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin'

process.env.PATH.split(path.delimiter)
// returns
['/usr/bin', '/bin', '/usr/sbin', '/sbin', '/usr/local/bin']

// Windows的例子
console.log(process.env.PATH)
// 'C:\Windows\system32;C:\Windows;C:\Program Files\nodejs\'

process.env.PATH.split(path.delimiter)
// returns
['C:\\Windows\\system32', 'C:\\Windows', 'C:\\Program Files\\nodejs\\']
```

## path.parse(pathString)

返回路径字符串的对象。

```js
// *nix上的例子
path.parse('/home/user/dir/file.txt')
// returns
{
	root : "/",
		dir : "/home/user/dir",
	base : "file.txt",
	ext : ".txt",
	name : "file"
}

// Windows的例子
path.parse('C:\\path\\dir\\index.html')
// returns
{
	root : "C:\\",
		dir : "C:\\path\\dir",
	base : "index.html",
	ext : ".html",
	name : "index"
}
```

## path.format(pathObject)

从对象中返回路径字符串，和path.parse相反：

```js
path.format({
    root : "/",
    dir : "/home/user/dir",
    base : "file.txt",
    ext : ".txt",
    name : "file"
})
// returns
'/home/user/dir/file.txt'
```

## path.posix
提供上述path路径访问，不过总是以posix兼容的方式交互。

## path.win32
提供上述path路径访问，不过总是以win32兼容的方式交互。

# URL模块

Node.js的URL模块提供了用于分析和解析URL的实用程序。可以调用`require('url')`来访问它.解析URL对象有以下内容，依赖于他们是否在URL字符串里存在。任何不在URL字符串里的部分，都不会出现在解析对象里。例子如下：

`'http://user:pass@host.com:8080/p/a/t/h?query=string#hash'`

* href：准备解析的完整的URL，包含协议和主机（小写）。 例子：'http://user:pass@host.com:8080/p/a/t/h?query=string#hash'
* protocol: 请求协议，小写。 例子：'http:'
* slashes: 协议要求的斜杠（冒号后） 例子：true或false
* host: 完整的URL小写主机部分，包含端口信息。 例子：'host.com:8080'
* auth: url中的验证信息。 例子：'user:pass'
* hostname: 域名中的小写主机名 例子：'host.com'
* port: 主机的端口号 例子：'8080'
* pathname: URL中的路径部分，在主机名后，查询字符前，包含第一个斜杠。 例子：'/p/a/t/h'
* search: URL中得查询字符串，包含开头的问号。 例子：'?query=string'
* path: pathname和search连在一起。 例子：'/p/a/t/h?query=string'
* query: 查询字符串中得参数部分，或者使用querystring.parse()解析后返回的对象。 例子：'query=string'或者{'query':'string'}
* hash: URL的“#”后面部分（包括 # 符号） 。例子：'#hash'

## url.parse(urlStr[, parseQueryString][, slashesDenoteHost])

输入URL字符串，返回一个对象。

第二个参数为true时，使用querystring来解析查询字符串。如果为true，query属性将会一直赋值为对象，并且search属性将会一直是字符串(可能为空)。默认为false。

第三个参数为true，把//foo/bar当做{ host: 'foo', pathname: '/bar' } ，而不是{ pathname: '//foo/bar' }。默认为false。

## url.format(urlObj)

输入一个解析过的URL对象，返回格式化过的字符串。

格式化的工作流程：

* href: 会被忽略
* protocol无论是否有末尾的 : (冒号)，会同样的处理
    * http，https，ftp，gopher，file协议会被添加后缀://
    * mailto，xmpp，aim，sftp，foo等协议添加后缀:
* slashes如果协议需要://，设置为true。
    * 仅需对之前列出的没有斜杠的协议，比如议mongodb://localhost:8000/
* auth如果出现将会使用.
* hostname仅在缺少host时使用
* port仅在缺少host时使用
* host用来替换hostname和port
* pathname无论结尾是否有“/”将会同样处理
* search将会替 query属性
   * 无论前面是否有“/”将会同样处理
* query (对象；参见querystring) 如果没有search，将会使用
* hash无论前面是否有#，都会同样处理

## url.resolve(from, to)

给一个基础URL，href URL，如同浏览器一样的解析它们可以带上锚点，例如：

```js
url.resolve('/one/two/three', 'four')         // '/one/two/four'
url.resolve('http://example.com/', '/one')    // 'http://example.com/one'
url.resolve('http://example.com/one', '/two') // 'http://example.com/two'
```

