<!--
 * @author: ares
 * @date: 2022-01-13 15:56:26
 * @lastEditTime: 2022-01-13 15:56:26
 * @lastEditors: ares
 * @description: 
 * 
-->
# nodejs环境配置

## 方式一：使用NVM安装多个Node.js版本

NVM（Node Version Manager）是Node.js的版本管理软件，使您可以轻松在Node.js各个版本间进行切换。适用于长期做node开发的人员或有快速更新node版本、快速切换node版本的场景。具体操作步骤如下：

1. 安装分布式版本管理系统Git

```shell
yum install git -y
```

2. 使用Git将NVM的源码克隆到本地的/usr/local/nvm目录下，并检查最新版本

```shell
git clone git://github.com/cnpm/nvm.git /usr/local/nvm && cd /usr/local/nvm && git checkout `git describe --abbrev=0 --tags`
```

3. 执行以下命令，配置NVM的环境变量

```shell
echo ". /usr/local/nvm/nvm.sh" >> /etc/profile
```

或者添加. /usr/local/nvm/nvm.sh到/etc/profile

4. 让配置生效

```shell
source /etc/profile
```

### nvm使用

1. 安装Node.js版本

```shell
nvm install v6.9.5
```

2. 列出已安装的nodejs版本

```shell
nvm list
```
3. 启用某个版本的nodejs

```shell
nvm use 6.9.5
```

## 方式二：使用二进制文件安装

该方式使用的安装包是已编译好的二进制文件。解压文件之后，在bin文件夹中就已存在node和npm，无需重复编译。以安装Node.js v6.9.5版本为例，具体操作说明如下：

1. 下载Node.js安装包

```shell
wget <https://nodejs.org/dist/v6.9.5/node-v6.9.5-linux-x64.tar.xz>
```

2. 解压文件

```shell
tar xvf node-v6.9.5-linux-x64.tar.xz
```

3. 依次运行以下命令，创建node和npm的软链接,创建软链接后，您可以在任意目录下直接使用node和npm命令

```shell
ln -s /root/node-v6.9.5-linux-x64/bin/node /usr/local/bin/node
ln -s /root/node-v6.9.5-linux-x64/bin/npm /usr/local/bin/npm
```

4. 依次查看node、npm版本信息,验证安装是否成功

```
node -v
npm -v
```

能够正常显示版本号，说明nodejs环境已安装完毕。软件默认安装在/root/node-v6.9.5-linux-x64/目录下。

### 如果您需要将该软件安装到其他目录下，例如：/opt/node/，可以如下操作：

1. 创建/opt/node/路径

```shell
mkdir -p /opt/node/
```

2. 将Node.js的所有文件移动至/opt/node/

```shell
mv /root/node-v6.9.5-linux-x64/* /opt/node/
```

3. 依次运行以下命令，移除源路径中node和npm的软链接

```shell
rm -f /usr/local/bin/node
rm -f /usr/local/bin/npm
```

4. 依次运行以下命令，在/opt/node/中新建node和npm的软链接

```shell
ln -s /opt/node/bin/node /usr/local/bin/node
ln -s /opt/node/bin/npm /usr/local/bin/npm
```