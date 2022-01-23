# 简介

ShellJS是一个可移植的(Windows/Linux/OS X) Unix shell命令在Node.js
API之上的实现。您可以使用它来消除您的shell脚本对Unix的依赖，同时仍然保留其熟悉和强大的命令。你也可以全局安装它，这样你就可以在Node项目之外运行它——跟那些粗糙的Bash脚本说再见吧!

```shell
npm install [-g] shelljs
```

```js
var shell = require('shelljs');

if (!shell.which('git')) {
	shell.echo('Sorry, this script requires git');
	shell.exit(1);
}

// Copy files to release dir
shell.rm('-rf', 'out/Release');
shell.cp('-R', 'stuff/', 'out/Release');

// Replace macros in each .js file
shell.cd('lib');
shell.ls('*.js').forEach(function (file) {
	shell.sed('-i', 'BUILD_VERSION', 'v0.1.2', file);
	shell.sed('-i', /^.*REMOVE_THIS_LINE.*$/, '', file);
	shell.sed('-i', /.*REPLACE_LINE_WITH_MACRO.*\n/, shell.cat('macro.js'), file);
});
shell.cd('..');

// Run external tool synchronously
if (shell.exec('git commit -am "Auto-commit"').code !== 0) {
	shell.echo('Error: Git commit failed');
	shell.exit(1);
}
```

# 排除选项

如果你需要传递一个看起来像选项的参数，你可以这样做:

```js
shell.grep('--', '-v', 'path/to/file'); // Search for "-v", no grep options

shell.cp('-R', '-dir', 'outdir'); // If already using an option, you're done
```

# 全局与局部

我们不再推荐使用全局导入ShellJS(即require(' ShellJS /global'))。虽然这仍然是为了方便而支持的，但它会污染全局名称空间，因此应该谨慎使用。 相反，我们建议使用本地导入(npm包的标准):

```js
var shell = require('shelljs');
shell.echo('hello world');
```

# 命令参考

所有命令同步运行，除非另有说明。所有命令都接受标准的bash globbing字符(*，?，等等)，与[node glob模块](https://github.com/isaacs/node-glob)兼容。

对于不常用的命令和功能，请查看[wiki page](https://github.com/shelljs/shelljs/wiki)。

## cat([options,] file [, file ...]) | cat([options,] file_array)

options:

* -n: 给所有输出行编号

```js
var str = cat('file*.txt');
var str = cat('file1', 'file2');
var str = cat(['file1', 'file2']); // same as above
```

返回包含给定文件的字符串，或者如果给出多个文件，则返回包含该文件的连接字符串(在每个文件之间引入一个新的行字符)。

## cd([dir])

在脚本运行期间更改目录到dir。如果没有提供参数，则更改到主目录。

## chmod([options,] octal_mode || octal_string, file) | chmod([options,] symbolic_mode, file)

options:

* -v: 为处理的每个文件输出诊断信息
* -c: 类似于冗长，但只有在进行更改时才报告
* -R: 递归地更改文件和目录

```js
chmod(755, '/Users/brandon');
chmod('755', '/Users/brandon'); // same as above
chmod('u+x', '/Users/brandon');
chmod('-R', 'a-w', '/Users/brandon');
```

通过以八进制形式指定绝对权限或以符号表示更改来更改文件或目录的权限。这个命令试图尽可能地模仿POSIX行为。值得注意的例外:

* 在符号模式下，a-r和-r是相同的。没有考虑到umask。
* 没有“quiet”选项，因为默认行为是静默运行。

## cp([options,] source [, source ...], dest) | cp([options,] source_array, dest)

options:

* -f: 强制(默认行为)
* -n: no-clobber
* -u只有当source比dest新时才复制
* -r | -R: 递归的
* -L: 遵循符号链接
* -P: 不遵循符号链接

```js
cp('file1', 'dir1');
cp('-R', 'path/to/dir/', '~/newCopy/');
cp('-Rf', '/tmp/*', '/usr/local/*', '/home/tmp');
cp('-Rf', ['/tmp/*', '/usr/local/*'], '/home/tmp'); // same as above
```

## pushd([options,] [dir | '-N' | '+N'])

options:

* -n: 在将目录添加到堆栈时，抑制对目录的正常更改，以便只对堆栈进行操作。
* -q: 抑制输出到控制台。

参数:

* dir: 将当前工作目录设置为堆栈的顶部，然后执行相当于cd dir的命令。
* +N: 通过旋转堆栈将第n个目录(从dirs打印的列表左侧开始，从0开始)移到列表的顶部。
* -N: 通过旋转堆栈将第n个目录(从dirs打印的列表的右侧开始，从0开始)移到列表的顶部。

```js
// process.cwd() === '/usr'
pushd('/etc'); // Returns /etc /usr
pushd('+1');   // Returns /usr /etc
```

将当前目录保存在目录堆栈的顶部，然后cd到dir。在没有参数的情况下，pushd交换了前两个目录。返回堆栈中路径的数组。

## popd([options,] ['-N' | '+N'])

options:

* -n: 当从堆栈中删除目录时，禁止正常的目录更改，以便只操作堆栈。
* -q: 抑制输出到控制台。

参数:

* +N: 删除第n个目录(从dirs打印的列表的左边开始)，从0开始。
* -N: 删除第n个目录(从dirs打印的列表的右侧开始)，从0开始。

```js
echo(process.cwd()); // '/usr'
pushd('/etc');       // '/etc /usr'
echo(process.cwd()); // '/etc'
popd();              // '/usr'
echo(process.cwd()); // '/usr'
```

当没有给出参数时，popd将从堆栈中删除顶部目录，并对新的顶部目录执行cd操作。元素从0开始编号，从dirs列出的第一个目录开始(即，popd相当于popd +0)。返回堆栈中路径的数组。

## dirs([options | '+N' | '-N'])

options:
-c: 通过删除所有元素来清除目录堆栈。 -q: 抑制输出到控制台。 参数:
+N: 显示第n个目录(在没有选项调用时从dirs打印的列表的左边开始)，从0开始。 -N: 显示第n个目录(在无选项调用时从dirs打印的列表的右侧开始)，从0开始。

显示当前记住的目录列表。返回堆栈中的路径数组，如果指定了+N或-N，则返回单个路径。

## echo([options,] string [, string ...])

options:

* -e: 解释反斜杠转义(默认)
* -n: 从输出中删除末尾换行符

```js
echo('hello world');
var str = echo('hello world');
echo('-n', 'no newline at end');
```

将字符串打印到stdout，并通过额外的实用方法(如.to())返回字符串。

## exec(command [, options] [, callback])

options:

* async: 异步执行。如果提供了回调函数，它将被设置为true，不管传递的值是什么(默认值:false)。
* silent: 不要将程序输出回显到控制台(默认值:false)。
* encoding: 要使用的字符编码。影响返回到stdout和stderr的值，以及在非静默模式时写入stdout和stderr的内容(默认值:'utf8')。
* 其它Node.js的child_process.exec()的任何选项

```js
var version = exec('node --version', {silent: true}).stdout;

var child = exec('some_long_running_process', {async: true});
child.stdout.on('data', function (data) {
	/* ... do something with data ... */
});

exec('some_long_running_process', function (code, stdout, stderr) {
	console.log('Exit code:', code);
	console.log('Program output:', stdout);
	console.log('Program stderr:', stderr);
});
```

同步执行给定的命令，除非另有说明。在同步模式下，这将返回一个ShellString(与ShellJS v0.6兼容。X，返回一个形式为{code:…stdout:……stderr:……})。否则，这将返回子进程对象，回调函数将接收参数(
code、stdout、stderr)。

没有看到你想要的行为?exec()默认通过sh(或Windows上的cmd.exe)运行一切，这与bash不同。如果您需要bash特定的行为，请尝试{shell: 'path/to/bash'}选项。

## find(path [, path ...]) | find(path_array)

```js
find('src', 'lib');
find(['src', 'lib']); // same as above
find('.').filter(function (file) {
	return file.match(/\.js$/);
});
```

返回指定路径中所有文件的数组(不论深度)。

与ls('-R'， path)的主要区别是，生成的文件名包括基本目录(例如，lib/resources/file1而不仅仅是file1)。

## grep([options,] regex_filter, file [, file ...]) | grep([options,] regex_filter, file_array)

options:

* -v: 反转regex_filter(只打印不匹配的行)。
* -l: 只打印匹配文件的文件名。
* -i: 忽略大小写

```js
grep('-v', 'GLOBAL_VARIABLE', '*.js');
grep('GLOBAL_VARIABLE', '*.js');
```

从给定的文件读取输入字符串，并返回一个字符串，该字符串包含与给定的regex_filter匹配的文件的所有行。

## head([{'-n': <num>},] file [, file ...]) | head([{'-n': <num>},] file_array)

读取文件的开头

* -n <num>: 显示文件的前<num>行

```js
var str = head({'-n': 1}, 'file*.txt');
var str = head('file1', 'file2');
var str = head(['file1', 'file2']); // same as above
```

## ln([options,] source, dest)

options:

* -s: 符号链接
* -f: 强制的

```js
ln('file', 'newlink');
ln('-sf', 'file', 'existing');
```

将source链接到dest。如果dest已经存在，使用-f强制链接。

## ls([options,] [path, ...]) | ls([options,] path_array)

options:

* -R: 递归的
* -A: 所有文件(包括以**.**开头的文件，除了**.**和**..**)
* -L: 遵循符号链接
* -d: 列出目录本身，而不是它们的内容
* -l: 列出表示每个文件的对象，每个文件的字段包含`ls -l`输出字段。[查看更多信息](https://nodejs.org/api/fs.html#fs_class_fs_stats)

```js
ls('projs/*.js');
ls('-R', '/users/me', '/tmp');
ls('-R', ['/users/me', '/tmp']); // same as above
ls('-l', 'file.txt'); // { name: 'file.txt', mode: 33188, nlink: 1, ...}
```

返回给定路径下文件的数组，如果没有提供路径，则返回当前目录下的文件。

## mkdir([options,] dir [, dir ...]) | mkdir([options,] dir_array)

创建目录

options:
-p: 全路径(如有必要，并创建中间目录)

```js
mkdir('-p', '/tmp/a/b/c/d', '/tmp/e/f/g');
mkdir('-p', ['/tmp/a/b/c/d', '/tmp/e/f/g']); // same as above
```

## mv([options ,] source [, source ...], dest') | mv([options ,] source_array, dest')

将source文件移动到dest

options:
-f: 强制的(默认行为)
-n: no-clobber

```js
mv('-n', 'file', 'dir/');
mv('file1', 'file2', 'dir/');
mv(['file1', 'file2'], 'dir/'); // same as above
```

## pwd()

返回当前目录

## rm([options,] file [, file ...]) | rm([options,] file_array)

删除文件

options:

* -f: 强制的
* -r | -R:  递归的

```js
rm('-rf', '/tmp/*');
rm('some_file.txt', 'another_file.txt');
rm(['some_file.txt', 'another_file.txt']); // same as above
```

## sed([options,] search_regex, replacement, file [, file ...]) | sed([options,] search_regex, replacement, file_array)

options:

* -i: 就地替换文件的内容。注意，不会创建备份!

```js
sed('-i', 'PROGRAM_VERSION', 'v0.1.3', 'source.js');
sed(/.*DELETE_THIS_LINE.*\n/, '', 'source.js');
```

从文件中读取输入字符串，并使用给定的search_regex和替换字符串或函数对输入执行JavaScript替换()。返回替换后的新字符串。

和unix sed一样，ShellJS sed也支持捕获组。捕获组使用$n语法指定:

```js
sed(/(\w+)\s(\w+)/, '$2, $1', 'file.txt');
```

## set(options)

设置全局配置变量

options:

* +/-e: 错误时退出(config.fatal)
* +/-v: verbose: 显示所有命令(config.verbose)
* +/-f: 禁用文件名扩展(globbing)

```js
set('-e'); // exit upon first error
set('+e'); // this undoes a "set('-e')"
```

## sort([options,] file [, file ...]) | sort([options,] file_array)

options:
-r: 反转结果 -n: 根据数值比较

```js
sort('foo.txt', 'bar.txt');
sort('-r', 'foo.txt');
```

返回文件的内容，逐行排序。对多个文件进行排序混合了它们的内容(就像unix sort所做的那样)。

## tail([{'-n': <num>},] file [, file ...]) | tail([{'-n': <num>},] file_array)

读取文件的末尾

* -n <num>: 显示文件的后<num>行

```js
var str = tail({'-n': 1}, 'file*.txt');
var str = tail('file1', 'file2');
var str = tail(['file1', 'file2']); // same as above
```

## tempdir()

```js
var tmp = tempdir(); // "/tmp" for most *nix platforms
```

：搜索并返回包含可写的、平台相关的临时目录的字符串。遵循Python的[tempfile算法](http://docs.python.org/library/tempfile.html#tempfile.tempdir)

## test(expression)

expression:

* '-b', 'path': 如果path是块设备，则为True
* '-c', 'path': 如果path是字符设备则为True
* '-d', 'path': 如果path是一个目录，则为True
* '-e', 'path': 如果路径存在，则为True
* '-f', 'path': 如果path是一个常规文件，则为True
* '-L', 'path': 如果path是符号链接，则为True
* '-p', 'path': 如果path是一个管道(FIFO)则为True
* '-S', 'path': 如果path是套接字，则为True

```js
if (test('-d', path)) { /* do something with dir */
}
;
if (!test('-f', path)) continue; // skip if it's a regular file
```

使用可用的主元素计算表达式并返回相应的值。

## ShellString.prototype.to(file)

```js
cat('input.txt').to('output.txt');
```

类似于Unix中的重定向操作符>，但适用于shellstring(例如由cat、grep等返回的那些)。像Unix重定向一样，to()将覆盖任何现有文件!

## ShellString.prototype.toEnd(file)

```js
cat('input.txt').toEnd('output.txt');
```

类似于Unix中的重定向和追加操作符>>，但适用于ShellStrings(例如由cat、grep等返回的那些)。

## touch([options,] file [, file ...]) | touch([options,] file_array)

options:

* -a: 只修改访问时间
* -c: 不创建任何文件
* -m: 只修改修改时间
* -d DATE: 解析DATE并使用它而不是当前时间
* -r FILE: 使用FILE的时间代替当前时间

```js
touch('source.js');
touch('-c', '/path/to/some/dir/source.js');
touch({'-r': FILE}, '/path/to/some/dir/source.js');
```

将每个FILE的访问和修改次数更新为当前时间。一个不存在的FILE参数被创建为空，除非提供`-c`。这是touch(1)的部分实现。

## uniq([options,] [input, [output]])

options:

* -i: 比较时忽略大小写
* -c: 按出现次数为行添加前缀
* -d: 只打印重复的行，每组相同的行各打印一行

```js
uniq('foo.txt');
uniq('-i', 'foo.txt');
uniq('-cd', 'foo.txt', 'bar.txt');
```

## which(command)

在系统的PATH中搜索命令。在Windows上，如果扩展还不能执行，则使用PATHEXT变量追加扩展。返回包含命令的绝对路径的字符串。

```js
var nodeExec = which('node');
```

## exit(code)

使用给定的退出代码退出当前进程。

## error()

测试最后一个命令中是否出现错误。如果返回错误，则返回真值，否则返回假值。

注意:不要依赖于返回值作为一个错误消息。如果需要最后一条错误消息，则使用最后一个命令的返回值中的.stderr属性。

## ShellString(str)

将普通字符串转换为与每个命令返回的类似字符串的对象。它有一些特殊的方法，比如.to()和. toend()。

```js
var foo = ShellString('hello world');
```

## env['VAR_NAME']

包含环境变量(包括getter和setter)的对象。process.env的捷径。

## Pipes

命令可以以类似管道的方式将输出发送到另一个命令。sed、grep、cat、exec、to和toEnd可以出现在管道的右侧。管道可以被连接起来。

```js
grep('foo', 'file1.txt', 'file2.txt').sed(/o/g, 'a').to('output.txt');
echo('files with o\'s in the name:\n' + ls().grep('o'));
cat('test.js').exec('node'); // pipe to exec() call
```

# 配置

## silent

如果为真，则抑制所有命令输出，除了echo()调用。默认是假的。

```js
var sh = require('shelljs');
var silentState = sh.config.silent; // save old silent state
sh.config.silent = true;
/* ... */
sh.config.silent = silentState; // restore old silent state
```

## fatal

如果为true，当任何shell.js命令遇到错误时，脚本将抛出一个Javascript错误。默认是假的。这类似于Bash的`set-e`。

```js
require('shelljs/global');
config.fatal = true; // or set('-e');
cp('this_file_does_not_exist', '/dev/null'); // throws Error here
/* more commands... */
```

## verbose

```js
config.verbose = true; // or set('-v');
cd('dir/');
rm('-rf', 'foo.txt', 'bar.txt');
exec('echo hello');
```

将打印每个命令如下:

```shell
cd dir/
rm -rf foo.txt bar.txt
exec echo hello
```

## globOptions

使用这个值来调用glob.sync()而不是默认选项。

```js
config.globOptions = {nodir: true};
```

## reset()

```js
var shell = require('shelljs');
// Make changes to shell.config, and do stuff...
/* ... */
shell.config.reset(); // reset to original state
// Do more stuff, but with original settings
/* ... */
```

重置**shell.config**配置为默认值:

```
{
  fatal: false,
  globOptions: {},
  maxdepth: 255,
  noglob: false,
  silent: false,
  verbose: false,
}
```