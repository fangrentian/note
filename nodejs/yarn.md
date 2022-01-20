# 什么是yarn

Yarn 是一个软件包管理器，还可以作为项目管理工具。无论你是小型项目还是大型单体仓库（monorepos），无论是业余爱好者还是企业用户，Yarn 都能满足你的需求。

Yarn 对你的代码来说是一个软件包管理器， 你可以通过它使用全世界开发者的代码，或者分享自己的代码给全世界的开发者。Yarn 做这些事情快捷、安全、可靠，所以你不用担心什么。

通过 Yarn 你可以使用其他开发者针对不同问题的解决方案，简化你开发软件的过程。 如果使用过程中遇到问题，你可以将其上报或者贡献解决方案。一旦问题被修复， 你可以使用 Yarn 更新。

代码通过 软件包（package） 的方式被共享。一个软件包里包含了所有需要共享的代码，以及一个描述软件包信息的文件 package.json （叫做 清单）。

# 安装

## 全局安装

```shell
npm install -g yarn
```

## 按项目安装

1. 全局安装 Yarn 的最新版本：

```shell
npm install -g yarn
```

2. 进入你的项目目录：

```shell
cd ~/path/to/your-project
```

3. 运行以下命令：

```shell
yarn set version berry
```

Berry" 是 Yarn 2 发布序列的代号，同时也是 [代码仓库](https://github.com/yarnpkg/berry) 的名称！

# 用法

## 更换源

```shell
yarn config set registry https://registry.npm.taobao.org --global
yarn config set disturl https://npm.taobao.org/dist --global
```

## 设置node-sass镜像源

```shell
yarn config set sass_binary_site http://cdn.npm.taobao.org/dist/node-sass -g
```

## 显示命令列表

```shell
yarn help
```

## 初始化一个新项目

```shell
yarn init
```

## 安装所有依赖项

```shell
yarn
yarn install
```

## 添加依赖项

```shell
yarn add [package]
yarn add [package]@[version]
yarn add [package]@[tag]
```

## 将依赖项添加到不同的依赖类别中

```shell
yarn add [package] --dev  # dev dependencies
yarn add [package] --peer # peer dependencies
```

## 更新依赖项

```shell
yarn up [package]
yarn up [package]@[version]
yarn up [package]@[tag]
```

## 删除依赖项

```shell
yarn remove [package]
```

## 更新 Yarn 本体

```shell
yarn set version latest
yarn set version from sources
```

## 获取二进制脚本的路径

```shell
yarn bin [name]
```

## 删除共享缓存文件

```shell
yarn cache clean
```

## 读取配置

```shell
yarn config get <name>
```

## 设置配置

```shell
yarn config set <name> <value>
```

## 取消配置

```shell
yarn config unset <name>
```

## 显示当前配置

```shell
yarn config
```

## 将本地项目连接到另一个项目

```shell
yarn link <destination>
```

## 运行package.json中定义的脚本

```shell
yarn run <scriptName>
```