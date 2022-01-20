# 什么是npm

NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：

* 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
* 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
* 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

# 常用使用方法

## 查看版本

```shell
npm -v
```

## 升级

```shell
npm install npm -g
```

## 淘宝镜像 cnpm

```shell
npm install -g cnpm --registry=https://registry.npmmirror.com
```

## 安装模块

```shell
npm install <Module Name>     //本地安装
npm install <Module Name> -g  //全局安装
```

## 查看所有全局安装的模块

```shell
npm list -g
```

## 某个模块的版本号

```shell
npm list <Module Name>
```

## 卸载模块

```shell
npm uninstall <Module Name>
```

## 更新模块

```shell
npm update <Module Name>
```

## 搜索模块

```shell
npm search <Module Name>
```

## 版本号

语义版本号分为X.Y.Z三位，分别代表主版本号、次版本号和补丁版本号。当代码变更时，版本号按以下原则更新。

* 如果只是修复bug，需要更新Z位。
* 如果是新增了功能，但是向下兼容，需要更新Y位。
* 如果有大变动，向下不兼容，需要更新X位。

版本号有了这个保证后，在申明第三方包依赖时，除了可依赖于一个固定版本号外，还可依赖于某个范围的版本号。例如"argv": "0.0.x"表示依赖于0.0.x系列的最新版argv。