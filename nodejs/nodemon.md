# 介绍

Nodemon是一个基于node.js的应用程序的帮助开发工具，当检测到目录中的文件更改时，它会自动重启node应用程序。

Nodemon不需要对您的代码或开发方法进行任何额外的更改。Nodemon是node的替代包装器。要使用nodemon，请在执行脚本时在命令行中替换单词node。

# 安装

```shell
npm install -g nodemon 
# or using yarn: yarn global add nodemon
# npm install --save-dev nodemon 
# or using yarn: yarn add nodemon -D
```

# 用法 nodemon [your node app]

* 使用nodemon很简单，如果应用程序接受一个主机和端口作为参数，可这样启动它:

```shell
nodemon ./server.js localhost 8080
```

此脚本的任何输出都以[nodemon]为前缀，否则应用程序的所有输出(包括错误)将按照预期返回。

* 也可以像往常一样通过命令行将**inspect**标志传递给node:

```shell
nodemon --inspect ./server.js 80
```

如果有package.Json文件，你可以完全省略主脚本，nodemon将读取package.Json的**main**
属性，并使用该值作为应用程序[ref](https://github.com/remy/nodemon/issues/14)

Nodemon也会搜索package.Json中的**scripts.start**属性(从nodemon .1.x开始)。

# 自动重新运行

Nodemon最初是用来重启挂起的进程，比如web服务器，但现在支持干净地退出的应用程序。如果您的脚本干净地退出，nodemon将继续监视目录(或多个目录)，并在有任何更改时重新启动脚本。

# 手动重新启动

当nodemon正在运行时，如果你需要手动重启你的应用程序，而不是停止并重新启动nodemon，你可以输入带有回车符的**rs**, nodemon会重新启动你的进程。

# 配置文件

Nodemon支持本地和全局配置文件。配置文件命名为nodemon.Json，配置文件可以位于当前工作目录或您的主目录。要使用其它配置文件可以用 `——config <配置文件>` 命令参数来指定。

配置文件优先级: 命令行参数 > 本地配置 > 全球配置

配置文件可以将任何命令行参数作为JSON键值来配置，例如:

```json
{
	"verbose": true,
	"ignore": [
		"*.test.js",
		"fixtures/*"
	],
	"execMap": {
		"rb": "ruby",
		"pde": "processing --sketch={{pwd}} --run"
	}
}
```

上述nodemon.son文件可能是全局配置，这样就可以支持ruby文件和处理文件，执行`nodemon demo.Pde`
命令,nodemon将自动知道如何运行脚本。[查看更多配置](https://github.com/remy/nodemon/blob/master/doc/sample-nodemon.md)

# 配置文件 package.json

nodemon 支持在package.json中指定**nodemonConfig**属性来配置nodemon.

```json
{
	"name": "nodemon",
	"homepage": "http://nodemon.io",
	"...": "... other standard package.json values",
	"nodemonConfig": {
		"ignore": [
			"test/*",
			"docs/*"
		],
		"delay": 2500
	}
}
```

如果通过 **--config <配置文件>** 指定配置文件, 或本地有nodemon.json文件, 那么package.json对nodemon将被忽略.

# 运行非nodejs脚本

Nodemon还可以用于执行和监视其他程序。如果没有nodemon.json文件, nodemon将读取将要运行的脚本的扩展名,而不是**.js**文件.

```shell
nodemon --exec "python -v" ./app.py
```

上面的命令作用是:nodemon将使用python在verbose模式下运行**app.py**(注意，如果你没有向exec程序传递参数，你就不需要引号)，并寻找扩展名为.py的新文件或修改过的文件。

# 默认的可执行文件

使用nodemon.json可以使用**execMap**属性定义你自己的默认可执行文件。如果您正在使用nodemon默认不支持的语言，这一点尤其有用。

要添加nodemon对**.pl**扩展名(用于Perl)的支持， nodemon.Json文件将添加:

```json
{
	"execMap": {
		"pl": "perl"
	}
}
```

现在运行 `nodemon script.pl` ,nodemon将知道用perl语言来运行脚本.

# 监测多个目录

默认情况下，nodemon监视当前工作目录。如果你想控制这个选项，使用——watch选项来添加特定的路径:

```shell
nodemon --watch app --watch libs app/server.js
```

现在，nodemon只会在**./app**或**./libs**目录发生变化时才会重启。默认情况下，nodemon将遍历子目录，因此没有必要显式地包含子目录。

# 指定扩展监视列表

默认情况下，nodemon会查找扩展名为.js、.mjs、.coffee、.litcoffee和.json的文件。如果你使用——exec选项并监视app.py, nodemon将监视扩展名为.py的文件。然而，你可以用-e(或——ext)
开关指定你自己的列表，如下所示:

```shell
nodemon -e js,pug
```

# 忽略文件

默认情况下，nodemon只会在**.js** 文件更改时重启。在某些情况下，您可能希望忽略某些特定的文件、目录或文件模式，以防止nodemon过早地重新启动您的应用程序。

```shell
nodemon --ignore lib/ --ignore tests/
nodemon --ignore lib/app.js
nodemon --ignore 'lib/*.js'
```

# 应用程序不重启

在一些网络环境中(例如容器在挂载的驱动器上运行nodemon)，您需要使用**legacyWatch: true**参数，来启用Chokidar轮询机制。

通过CLI，使用——legacy-watch或-L简称:

```shell
nodemon - l
```

这是最后没办法的办法，因为它将轮询它能找到的每个文件。

# 推迟重启

在某些情况下，您可能希望等待，直到许多文件已经更改。检查新文件更改之前的超时时间为1秒。如果你正在上传大量的文件，这需要一段时间，这可能会导致你的应用程序重启多次不必要的。

```shell
nodemon --delay 10 server.js
nodemon --delay 2.5 server.js
nodemon --delay 2500ms server.js
```

延迟数字是重新启动之前延迟的秒数(也可以是毫秒数,需要显式指定)。因此，nodemon只会在最后一次文件更改后给定的秒数重启应用程序。

如果您在nodemon.Json中中设置此值，该值总是以毫秒为单位进行解释。例如，以下是等价的:

```shell
nodemon --delay 2.5
```

```json
{
	"delay": 2500
}
```

# 优雅地重新加载脚本

nodemon可以向应用程序发送您指定的任何信号

```shell
nodemon --signal **SIGHUP** server.js
```

您的应用程序可以按照以下方式处理该信号

```js
process.once("SIGHUP", function () {
	reloadSomeConfiguration();
})
```

请注意，nodemon将向进程树中的每个进程发送此信号。

如果您正在使用集群，那么每个worker(以及master)都将接收到这个信号。如果你希望在接收到**SIGHUP**时终止所有的worker，一个常见的模式是在master中捕获**SIGHUP**，并将**SIGTERM**
转发给所有worker，同时确保所有worker忽略**SIGHUP**。

```js
if (cluster.isMaster) {
	process.on("SIGHUP", function () {
		for (const worker of Object.values(cluster.workers)) {
			worker.process.kill("SIGTERM");
		}
	});
} else {
	process.on("SIGHUP", function () {
	})
}
```

# 控制脚本的关闭

Nodemon在看到文件更新时向应用程序发送终止信号。如果您需要在脚本中清理关机信号，您可以捕获并自己处理它。

下面的例子将监听一次SIGUSR2信号(nodemon使用它来重启)，运行清理进程，然后杀死自己，让nodemon继续控制:

```js
process.once('SIGUSR2', function () {
	gracefulShutdown(function () {
		process.kill(process.pid, 'SIGUSR2');
	});
});
```

# nodemon状态改变时触发事件

如果你想在nodemon重启时发出通知，或者在事件发生时触发一个动作，那么你可以要求nodemon或向nodemon添加**events**属性到nodemon.json文件中。

例如，在Mac上触发nodemon重启时的通知，nodemon.Json看起来像这样:

```json
{
	"events": {
		"restart": "osascript -e 'display notification \"app restarted\" with title \"nodemon\"'"
	}
}
```

# 管道输出到其他地方

```js
nodemon({
	script: ...,
	stdout: false // important: this tells nodemon not to output to console
}).on('readable', function () { // the `readable` event indicates that data is ready to pick up
	this.stdout.pipe(fs.createWriteStream('output.txt'));
	this.stderr.pipe(fs.createWriteStream('err.txt'));
});
```

# 