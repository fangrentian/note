# 基本使用, 声明 program 变量

```js
// 为简化使用，Commander 提供了一个全局对象
const { program } = require('commander');
program.version('0.0.1');

// 如果程序较为复杂，用户需要以多种方式来使用 Commander，如单元测试等。创建本地Command对象是一种更好的方式：
const { Command } = require('commander');
const program = new Command();
program.version('0.0.1');

// 要在 ECMAScript 模块中使用命名导入，可从commander/esm.mjs中导入。
import { Command } from 'commander/esm.mjs';
const program = new Command();
```

# 配置选项

Commander 使用.option()方法来定义配置选项，同时可以附加选项的简介。每个选项可以定义一个短选项名称（-后面接单个字符）和一个长选项名称（--后面接一个或多个单词），使用逗号、空格或|分隔。

解析后的选项可以通过Command对象上的.opts()方法获取，同时会被传递给命令处理函数。可以使用.getOptionValue()和.setOptionValue()操作单个选项的值。

对于多个单词的长选项，选项名会转为驼峰命名法（camel-case），例如--template-engine选项可通过program.opts().templateEngine获取。

多个短选项可以合并简写，其中最后一个选项可以附加参数。 例如，-a -b -p 80也可以写为-ab -p80，甚至进一步简化为-abp80。

--可以标记选项的结束，后续的参数均不会被命令解释，可以正常使用。

默认情况下，选项在命令行中的顺序不固定，一个选项可以在其他参数之前或之后指定。

## 常用选项类型，boolean 型选项和带参数选项

有两种最常用的选项，一类是 boolean 型选项，选项无需配置参数，另一类选项则可以设置参数（使用尖括号声明在该选项后，如--expect <value>）。如果在命令行中不指定具体的选项及参数，则会被定义为undefined。

```js
program
  .option('-d, --debug', 'output extra debugging')
  .option('-s, --small', 'small pizza size')
  .option('-p, --pizza-type <type>', 'flavour of pizza');

program.parse(process.argv);

const options = program.opts();
if (options.debug) console.log(options);
console.log('pizza details:');
if (options.small) console.log('- small pizza size');
if (options.pizzaType) console.log(`- ${options.pizzaType}`);
```

通过program.parse(arguments)方法处理参数，没有被使用的选项会存放在program.args数组中。该方法的参数是可选的，默认值为process.argv。

## 选项的默认值

```js
program
    .option('-c, --cheese <type>', 'add the specified type of cheese', 'blue');

program.parse();

console.log(`cheese: ${program.opts().cheese}`);
```

## 其他的选项类型，取反选项，以及可选参数的选项

可以定义一个以no-开头的 boolean 型长选项。在命令行中使用该选项时，会将对应选项的值置为false。当只定义了带no-的选项，未定义对应不带no-的选项时，该选项的默认值会被置为true。

如果已经定义了--foo，那么再定义--no-foo并不会改变它本来的默认值。可以为一个 boolean 类型的选项指定一个默认的布尔值，在命令行里可以重写它的值。

```js
program
  .option('--no-sauce', 'Remove sauce')
  .option('--cheese <flavour>', 'cheese flavour', 'mozzarella')
  .option('--no-cheese', 'plain with no cheese')
  .parse();

const options = program.opts();
const sauceStr = options.sauce ? 'sauce' : 'no sauce';
const cheeseStr = (options.cheese === false) ? 'no cheese' : `${options.cheese} cheese`;
console.log(`You ordered a pizza with ${sauceStr} and ${cheeseStr}`);
```

选项的参数使用方括号声明表示参数是可选参数（如--optional [value]）。该选项在不带参数时可用作 boolean 选项，在带有参数时则从参数中得到值。

```js
program
    .option('-c, --cheese [type]', 'Add cheese with optional type');

program.parse(process.argv);

const options = program.opts();
if (options.cheese === undefined) console.log('no cheese');
else if (options.cheese === true) console.log('add cheese');
else console.log(`add cheese type ${options.cheese}`);
```

关于可能有歧义的用例，请见[可变参数的选项](https://github.com/tj/commander.js/blob/43f4743864e2f670db5eebcf88c92aa4612c54f1/docs/zh-CN/%E5%8F%AF%E5%8F%98%E5%8F%82%E6%95%B0%E7%9A%84%E9%80%89%E9%A1%B9.md)。

## 必填选项

通过.requiredOption()方法可以设置选项为必填。必填选项要么设有默认值，要么必须在命令行中输入，对应的属性字段在解析时必定会有赋值。该方法其余参数与.option()一致。

```js
program
    .requiredOption('-c, --cheese <type>', 'pizza must have cheese');

program.parse();
```

## 变长参数选项

定义选项时，可以通过使用...来设置参数为可变长参数。在命令行中，用户可以输入多个参数，解析后会以数组形式存储在对应属性字段中。在输入下一个选项前（-或--开头），用户输入的指令均会被视作变长参数。与普通参数一样的是，可以通过--标记当前命令的结束。

```js
program
  .option('-n, --number <numbers...>', 'specify numbers')
  .option('-l, --letter [letters...]', 'specify letters');

program.parse();

console.log('Options: ', program.opts());
console.log('Remaining arguments: ', program.args);
```
关于可能有歧义的用例，请见[可变参数的选项](https://github.com/tj/commander.js/blob/43f4743864e2f670db5eebcf88c92aa4612c54f1/docs/zh-CN/%E5%8F%AF%E5%8F%98%E5%8F%82%E6%95%B0%E7%9A%84%E9%80%89%E9%A1%B9.md)。

## 版本选项

.version()方法可以设置版本，其默认选项为-V和--version，设置了版本后，命令行会输出当前的版本号。

```js
program.version('0.0.1');
```

版本选项也支持自定义设置选项名称，可以在.version()方法里再传递一些参数（长选项名称、描述信息），用法与.option()方法类似。

```js
program.version('0.0.1', '-v, --vers', 'output the current version');
```

## 其他选项配置

大多数情况下，选项均可通过.option()方法添加。但对某些不常见的用例，也可以直接构造Option对象，对选项进行更详尽的配置。

```js
program
  .addOption(new Option('-s, --secret').hideHelp())
  .addOption(new Option('-t, --timeout <delay>', 'timeout in seconds').default(60, 'one minute'))
  .addOption(new Option('-d, --drink <size>', 'drink size').choices(['small', 'medium', 'large']));
```

## 自定义选项处理

选项的参数可以通过自定义函数来处理，该函数接收两个参数，即用户新输入的参数值和当前已有的参数值（即上一次调用自定义处理函数后的返回值），返回新的选项参数值。

自定义函数适用场景包括参数类型转换，参数暂存，或者其他自定义处理的场景。

可以在自定义函数的后面设置选项参数的默认值或初始值（例如参数用列表暂存时需要设置一个初始空列表)。

```js
function myParseInt(value, dummyPrevious) {
  // parseInt 参数为字符串和进制数
  const parsedValue = parseInt(value, 10);
  if (isNaN(parsedValue)) {
    throw new commander.InvalidArgumentError('Not a number.');
  }
  return parsedValue;
}

function increaseVerbosity(dummyValue, previous) {
  return previous + 1;
}

function collect(value, previous) {
  return previous.concat([value]);
}

function commaSeparatedList(value, dummyPrevious) {
  return value.split(',');
}

program
  .option('-f, --float <number>', 'float argument', parseFloat)
  .option('-i, --integer <number>', 'integer argument', myParseInt)
  .option('-v, --verbose', 'verbosity that can be increased', increaseVerbosity, 0)
  .option('-c, --collect <value>', 'repeatable value', collect, [])
  .option('-l, --list <items>', 'comma separated list', commaSeparatedList)
;

program.parse();

const options = program.opts();
if (options.float !== undefined) console.log(`float: ${options.float}`);
if (options.integer !== undefined) console.log(`integer: ${options.integer}`);
if (options.verbose > 0) console.log(`verbosity: ${options.verbose}`);
if (options.collect.length > 0) console.log(options.collect);
if (options.list !== undefined) console.log(options.list);
```

# 命令

通过.command()或.addCommand()可以配置命令，有两种实现方式：为命令绑定处理函数，或者将命令单独写成一个可执行文件（详述见后文）。子命令支持嵌套（示例代码）。

.command()的第一个参数为命令名称。命令参数可以跟在名称后面，也可以用.argument()单独指定。参数可为必选的（尖括号表示）、可选的（方括号表示）或变长参数（点号表示，如果使用，只能是最后一个参数）。

使用.addCommand()向program增加配置好的子命令。

例如:

```js
// 通过绑定处理函数实现命令（这里的指令描述为放在`.command`中）
// 返回新生成的命令（即该子命令）以供继续配置
program
  .command('clone <source> [destination]')
  .description('clone a repository into a newly created directory')
  .action((source, destination) => {
    console.log('clone command called');
  });

// 通过独立的的可执行文件实现命令 (注意这里指令描述是作为`.command`的第二个参数)
// 返回最顶层的命令以供继续添加子命令
program
  .command('start <service>', 'start named service')
  .command('stop [service]', 'stop named service, or all if no name supplied');

// 分别装配命令
// 返回最顶层的命令以供继续添加子命令
program
  .addCommand(build.makeBuildCommand());
```

使用.command()和addCommand()来指定选项的相关设置。当设置hidden: true时，该命令不会打印在帮助信息里。当设置isDefault: true时，若没有指定其他子命令，则会默认执行这个命令（样例）。

# 命令参数

如上所述，字命令的参数可以通过.command()指定。对于有独立可执行文件的子命令来书，参数只能以这种方法指定。而对其他子命令，参数也可用以下方法。

在Command对象上使用.argument()来按次序指定命令参数。该方法接受参数名称和参数描述。参数可为必选的（尖括号表示，例如<required>）或可选的（方括号表示，例如[optional]）。

```js
program
  .version('0.1.0')
  .argument('<username>', 'user to login')
  .argument('[password]', 'password for user, if required', 'no password given')
  .action((username, password) => {
    console.log('username:', username);
    console.log('password:', password);
  });
```

在参数名后加上...来声明可变参数，且只有最后一个参数支持这种用法。可变参数会以数组的形式传递给处理函数。例如：

```js
program
  .version('0.1.0')
  .command('rmdir')
  .argument('<dirs...>')
  .action(function (dirs) {
    dirs.forEach((dir) => {
      console.log('rmdir %s', dir);
    });
  });
```

有一种便捷方式可以一次性指定多个参数，但不包含参数描述：

```js
program
  .arguments('<username> <password>');
```
# 其他参数配置

有少数附加功能可以直接构造Argument对象，对参数进行更详尽的配置。

```js
program
  .addArgument(new commander.Argument('<drink-size>', 'drink cup size').choices(['small', 'medium', 'large']))
  .addArgument(new commander.Argument('[timeout]', 'timeout in seconds').default(60, 'one minute'))
```

# 自定义参数处理

选项的参数可以通过自定义函数来处理（与处理选项参数时类似），该函数接收两个参数：用户新输入的参数值和当前已有的参数值（即上一次调用自定义处理函数后的返回值），返回新的命令参数值。

处理后的参数值会传递给命令处理函数，同时可通过.processedArgs获取。

可以在自定义函数的后面设置命令参数的默认值或初始值。

示例代码：arguments-custom-processing.js

```js
program
  .command('add')
  .argument('<first>', 'integer argument', myParseInt)
  .argument('[second]', 'integer argument', myParseInt, 1000)
  .action((first, second) => {
    console.log(`${first} + ${second} = ${first + second}`);
  });
```

# 处理函数

命令处理函数的参数，为该命令声明的所有参数，除此之外还会附加两个额外参数：一个是解析出的选项，另一个则是该命令对象自身。

```js
program
  .argument('<name>')
  .option('-t, --title <honorific>', 'title to use before name')
  .option('-d, --debug', 'display some debugging')
  .action((name, options, command) => {
    if (options.debug) {
      console.error('Called %s with options %o', command.name(), options);
    }
    const title = options.title ? `${options.title} ` : '';
    console.log(`Thank-you ${title}${name}`);
  });
```

处理函数支持async，相应的，需要使用.parseAsync代替.parse。

```js
async function run() { /* 在这里编写代码 */ }

async function main() {
  program
    .command('run')
    .action(run);
  await program.parseAsync(process.argv);
}
```

使用命令时，所给的选项和命令参数会被验证是否有效。凡是有未知的选项，或缺少所需的命令参数，都会报错。 如要允许使用未知的选项，可以调用.allowUnknownOption()。默认情况下，传入过多的参数并不报错，但也可以通过调用.allowExcessArguments(false)来启用过多参数的报错。

# 独立的可执行（子）命令

当.command()带有描述参数时，就意味着使用独立的可执行文件作为子命令。 Commander 将会尝试在入口脚本（例如./examples/pm）的目录中搜索program-command形式的可执行文件，例如pm-install、pm-search。通过配置选项executableFile可以自定义名字。

你可以在可执行文件里处理（子）命令的选项，而不必在顶层声明它们。

示例代码：[pm](https://github.com/tj/commander.js/blob/43f4743864e2f670db5eebcf88c92aa4612c54f1/examples/pm)

```js
program
  .version('0.1.0')
  .command('install [name]', 'install one or more packages')
  .command('search [query]', 'search with optional query')
  .command('update', 'update installed packages', { executableFile: 'myUpdateSubCommand' })
  .command('list', 'list packages installed', { isDefault: true });

program.parse(process.argv);
```

如果该命令需要支持全局安装，请确保有对应的权限，例如755。

# 生命周期钩子

可以在命令的生命周期事件上设置回调函数。

```js
program
  .option('-t, --trace', 'display trace statements for commands')
  .hook('preAction', (thisCommand, actionCommand) => {
    if (thisCommand.opts().trace) {
      console.log(`About to call action handler for subcommand: ${actionCommand.name()}`);
      console.log('arguments: %O', actionCommand.args);
      console.log('options: %o', actionCommand.opts());
    }
  });
```

钩子函数支持async，相应的，需要使用.parseAsync代替.parse。一个事件上可以添加多个钩子。

支持的事件有：

preAction：在本命令或其子命令的处理函数执行前
postAction：在本命令或其子命令的处理函数执行后
钩子函数的参数为添加上钩子的命令，及实际执行的命令。

# 自动化帮助信息

帮助信息是 Commander 基于你的程序自动生成的，默认的帮助选项是-h,--help。

示例代码：pizza

```shell
node ./examples/pizza --help
Usage: pizza [options]

An application for pizza ordering

Options:
  -p, --peppers        Add peppers
  -c, --cheese <type>  Add the specified type of cheese (default: "marble")
  -C, --no-cheese      You do not want any cheese
  -h, --help           display help for command
```

如果你的命令中包含了子命令，会默认添加help命令，它可以单独使用，也可以与子命令一起使用来提示更多帮助信息。用法与shell程序类似：

```shell
shell help
shell --help

shell help spawn
shell spawn --help
```

# 自定义帮助

可以添加额外的帮助信息，与内建的帮助一同展示。

```js
program
  .option('-f, --foo', 'enable some foo');

program.addHelpText('after', `

Example call:
  $ custom-help --help`);
```

将会输出以下的帮助信息：

```shell
Usage: custom-help [options]

Options:
  -f, --foo   enable some foo
  -h, --help  display help for command

Example call:
  $ custom-help --help
```
位置参数对应的展示方式如下：

* beforeAll：作为全局标头栏展示
* before：在内建帮助信息之前展示
* after：在内建帮助信息之后展示
* afterAll：作为全局末尾栏展示

beforeAll和afterAll两个参数作用于命令及其所有的子命令。

第二个参数可以是一个字符串，也可以是一个返回字符串的函数。对后者而言，为便于使用，该函数可以接受一个上下文对象，它有如下属性：

* error：boolean 值，代表该帮助信息是否由于不当使用而展示
* command：代表展示该帮助信息的Command对象

# 在出错后展示帮助信息

默认情况下，出现命令用法错误时只会显示错误信息。可以选择在出错后展示完整的帮助或自定义的帮助信息。

```js
program.showHelpAfterError();
// 或者
program.showHelpAfterError('(add --help for additional information)');
```

# 使用代码展示帮助信息

.help()：展示帮助信息并退出。可以通过传入{ error: true }来让帮助信息从 stderr 输出，并以代表错误的状态码退出程序。

.outputHelp()：只展示帮助信息，不退出程序。传入{ error: true }可以让帮助信息从 stderr 输出。

.helpInformation()：得到字符串形式的内建的帮助信息，以便用于自定义的处理及展示。

# .usage 和 .name

通过这两个选项可以修改帮助信息的首行提示，name属性也可以从参数中推导出来。例如：

```js
program
  .name("my-command")
  .usage("[global options] command")
```

# .helpOption(flags, description)

每一个命令都带有一个默认的帮助选项。可以重写flags和description参数。传入false则会禁用内建的帮助信息。

```js
program.helpOption('-e, --HELP', 'read more information');
```

# .addHelpCommand()

如果一个命令拥有子命令，它也将有一个默认的帮助子命令。使用.addHelpCommand()和.addHelpCommand(false)可以打开或关闭默认的帮助子命令。

也可以自定义名字和描述：

```js
program.addHelpCommand('assist [command]', 'show assistance');
```

# 其他帮助配置

内建帮助信息通过Help类进行格式化。如有需要，可以使用.configureHelp()来更改其数据属性和方法，或使用.createHelp()来创建子类，从而配置Help类的行为。

数据属性包括：

* helpWidth：指明帮助信息的宽度。可在单元测试中使用。
* sortSubcommands：以字母序排列子命令
* sortOptions：以字母序排列选项

可以得到可视化的参数列表，选项列表，以及子命令列表。列表的每个元素都具有_term_和_description_属性，并可以对其进行格式化。关于其使用方式，请参考.formatHelp()。

```js
program.configureHelp({
    sortSubcommands: true,
    subcommandTerm: (cmd) => cmd.name() // 显示名称，而非用法
});
```

# 自定义事件监听

监听命令和选项可以执行自定义函数。

```js
program.on('option:verbose', function () {
    process.env.VERBOSE = this.opts().verbose;
});
```

# 零碎知识

## .parse() 和 .parseAsync()

.parse的第一个参数是要解析的字符串数组，也可以省略参数而使用process.argv。

如果参数遵循与 node 不同的约定，可以在第二个参数中传递from选项：

* node：默认值，argv[0]是应用，argv[1]是要跑的脚本，后续为用户参数；
* electron：argv[1]根据 electron 应用是否打包而变化；
* user：来自用户的所有参数。

```js
program.parse(process.argv); // 指明，按 node 约定
program.parse(); // 默认，自动识别 electron
program.parse(['-f', 'filename'], { from: 'user' });
```

# 解析配置

当默认的解析方式无法满足需要，Commander 也提供了其他的解析行为。

默认情况下，程序的选项在子命令前后均可被识别。如要只允许选项出现在子命令之前，可以使用.enablePositionalOptions()。这样可以在命令和子命令中使用意义不同的同名选项。

```js
program
  .enablePositionalOptions()
  .option('-p, --progress');

program
  .command('upload <file>')
  .option('-p, --port <number>', 'port number', 80)
  .action((file, options) => {
    if (program.opts().progress) console.log('Starting upload...');
    console.log(`Uploading ${file} to port ${options.port}`);
    if (program.opts().progress) console.log('Finished upload');
  });

program.parse();
```

当启用了带顺序的选项解析，以下程序中，-b选项在第一行中将被解析为程序顶层的选项，而在第二行中则被解析为子命令的选项：

```shell
program -b subcommand
program subcommand -b
```

默认情况下，选项在命令参数前后均可被识别。如要使选项仅在命令参数前被识别，可以使用.passThroughOptions()。这样可以把参数和跟随的选项传递给另一程序，而无需使用--来终止选项解析。 如要在子命令中使用此功能，必须首先启用带顺序的选项解析。

```js
program
  .argument('<utility>')
  .argument('[args...]')
  .passThroughOptions()
  .option('-d, --dry-run')
  .action((utility, args, options) => {
    const action = options.dryRun ? 'Would run' : 'Running';
    console.log(`${action}: ${utility} ${args.join(' ')}`);
  });

program.parse();
```

当启用此功能时，以下程序中，--port=80在第一行中会被解析为程序的选项，而在第二行中则会被解析为一个命令参数：

```shell
program --port=80 arg
program arg --port=80
```

默认情况下，使用未知选项会提示错误。如要将未知选项视作普通命令参数，并继续处理其他部分，可以使用.allowUnknownOption()。这样可以混用已知和未知的选项。

默认情况下，传入过多的命令参数并不会报错。可以使用.allowExcessArguments(false)来启用这一检查。

# 作为属性的遗留选项

在 Commander 7 以前，选项的值是作为属性存储在命令对象上的。 这种处理方式便于实现，但缺点在于，选项可能会与Command的已有属性相冲突。通过使用.storeOptionsAsProperties()，可以恢复到这种旧的处理方式，并可以不加改动地继续运行遗留代码。

```js
program
  .storeOptionsAsProperties()
  .option('-d, --debug')
  .action((commandAndOptions) => {
    if (commandAndOptions.debug) {
      console.error(`Called ${commandAndOptions.name()}`);
    }
  });
```

# TypeScript

如果你使用 ts-node，并有.ts文件作为独立可执行文件，那么需要用 node 运行你的程序以使子命令能正确调用，例如：

```shell
node -r ts-node/register pm.ts
```

# createCommand()

使用这个工厂方法可以创建新命令，此时不需要使用new，如

```js
const { createCommand } = require('commander');
const program = createCommand();
```

createCommand同时也是Command对象的一个方法，可以创建一个新的命令（而非子命令），使用.command()创建子命令时内部会调用该方法，具体使用方式可参考 [custom-command-class.js](https://github.com/tj/commander.js/blob/43f4743864e2f670db5eebcf88c92aa4612c54f1/examples/custom-command-class.js)。

# Node 选项，如 --harmony

要使用--harmony等选项有以下两种方式：

  * 在子命令脚本中加上#!/usr/bin/env node --harmony。注：Windows 系统不支持；
  * 调用时加上--harmony参数，例如node --harmony examples/pm publish。--harmony选项在开启子进程时仍会保留。
  
# 调试子命令

一个可执行的子命令会作为单独的子进程执行。

如果使用 node inspector 的node -inspect等命令来调试可执行命令，对于生成的子命令，inspector 端口会递增 1。

如果想使用 VSCode 调试，则需要在launch.json配置文件里设置"autoAttachChildProcesses": true。

# 重写退出和输出

默认情况下，在检测到错误、打印帮助信息或版本信息时 Commander 会调用process.exit方法。其默认实现会抛出一个CommanderError，可以重写该方法并提供一个回调函数（可选）。

回调函数的参数为CommanderError，属性包括 Number 型的exitCode、String 型的code和message。子命令完成调用后会开始异步处理。正常情况下，打印错误信息、帮助信息或版本信息不会被重写影响，因为重写会发生在打印之后。

```js
program.exitOverride();

try {
  program.parse(process.argv);
} catch (err) {
  // 自定义处理...
}
```

Commander 默认用作命令行应用，其输出写入 stdout 和 stderr。 对于其他应用类型，这一行为可以修改。并且可以修改错误信息的展示方式。

```js
function errorColor(str) {
  // 添加 ANSI 转义字符，以将文本输出为红色
  return `\x1b[31m${str}\x1b[0m`;
}

program
  .configureOutput({
    // 此处使输出变得容易区分
    writeOut: (str) => process.stdout.write(`[OUT] ${str}`),
    writeErr: (str) => process.stdout.write(`[ERR] ${str}`),
    // 将错误高亮显示
    outputError: (str, write) => write(errorColor(str))
  });
```