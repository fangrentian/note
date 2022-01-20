# nvm是什么

nvm全英文也叫node.js version management，是一个nodejs的版本管理工具。nvm和n都是node.js版本管理工具，为了解决node.js各种版本存在不兼容现象可以通过它可以安装和切换不同版本的node.js。

## nvm下载

可在点此在[github](https://github.com/coreybutler/nvm-windows/releases)上下载最新版本,本次下载安装的是windows版本。打开网址我们可以看到有两个版本：

[nvm 1.1.7-setup.zip](http://nvm.uihtm.com/nvm1.1.7-setup.zip)：安装版，推荐使用
[nvm 1.1.7-noinstall.zip](http://nvm.uihtm.com/nvm1.1.7-noinstall.zip): 绿色免安装版，但使用时需进行配置。

# nvm常用命令

* nvm arch：显示node是运行在32位还是64位。

* nvm install <version> [arch] ：安装node， version是特定版本也可以是最新稳定版本latest。可选参数arch指定安装32位还是64位版本，默认是系统位数。可以添加--insecure绕过远程服务器的SSL。

* nvm list [available] ：显示已安装的列表。可选参数available，显示可安装的所有版本。list可简化为ls。

* nvm on ：开启node.js版本管理。

* nvm off ：关闭node.js版本管理。

* nvm proxy [url] ：设置下载代理。不加可选参数url，显示当前代理。将url设置为none则移除代理。

* nvm node_mirror [url] ：设置node镜像。默认是https://nodejs.org/dist/。如果不写url，则使用默认url。设置后可至安装目录settings.txt文件查看，也可直接在该文件操作。

* nvm npm_mirror [url] ：设置npm镜像。https://github.com/npm/cli/archive/。如果不写url，则使用默认url。设置后可至安装目录settings.txt文件查看，也可直接在该文件操作。

* nvm uninstall <version> ：卸载指定版本node。

* nvm use [version] [arch] ：使用制定版本node。可指定32/64位。

* nvm root [path] ：设置存储不同版本node的目录。如果未设置，默认使用当前目录。

* nvm version ：显示nvm版本。version可简化为v。

# nvm常见问题

如果下载node过慢，请更换国内镜像源, 在 nvm 的安装路径下，找到 settings.txt，设置node_mirro与npm_mirror为国内镜像地址。下载就飞快了~~

root: D:\nvm

path: D:\nodejs

node_mirror: https://npm.taobao.org/mirrors/node/

npm_mirror: https://npm.taobao.org/mirrors/npm/
