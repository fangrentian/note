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
git clone https://github.com/cnpm/nvm.git /usr/local/nvm && cd /usr/local/nvm && git checkout `git describe --abbrev=0 --tags`
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
[ares@localhost ~]$ sudo mkdir /usr/local/nodejs
[ares@localhost ~]$ cd /usr/local/nodejs
[ares@localhost nodejs]$ sudo chmod -R 777 /usr/local/nodejs
[ares@localhost nodejs]$ wget https://nodejs.org/dist/v14.16.0/node-v14.16.0-linux-x64.tar.xz
--2023-08-08 09:32:26--  https://nodejs.org/dist/v14.16.0/node-v14.16.0-linux-x64.tar.xz
正在解析主机 nodejs.org (nodejs.org)... 104.20.22.46, 104.20.23.46, 2606:4700:10::6814:162e, ...
正在连接 nodejs.org (nodejs.org)|104.20.22.46|:443... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度：21385948 (20M) [application/x-xz]
正在保存至: “node-v14.16.0-linux-x64.tar.xz”

node-v14.16.0-linux-x64.tar.xz                100%[===============================================================================================>]  20.39M  5.89MB/s  用时 4.4s    

2023-08-08 09:32:32 (4.66 MB/s) - 已保存 “node-v14.16.0-linux-x64.tar.xz” [21385948/21385948])
```

2. 解压文件

```shell
[ares@localhost nodejs]$ tar xvf node-v14.16.0-linux-x64.tar.xz 
```

3. 依次运行以下命令，创建node和npm的软链接,创建软链接后，您可以在任意目录下直接使用node和npm命令

```shell
[ares@localhost nodejs]$ sudo ln -s node-v14.16.0-linux-x64/bin/node /usr/local/bin/node
[ares@localhost nodejs]$ sudo ln -s node-v14.16.0-linux-x64/bin/npm /usr/local/bin/npm
```

4. 添加环境变量到 `/etc/profile`

```shell
export NODE_HOME=/usr/local/nodejs/node-v14.16.0-linux-x64
export PATH=$NODE_HOME/bin:$PATH
```

5. 重载 `/etc/profile`

```shell
[ares@localhost ~]$ source /etc/profile
```

7. 依次查看node、npm版本信息,验证安装是否成功

```
node -v
npm -v
```

能够正常显示版本号，说明nodejs环境已安装完毕。软件默认安装在/usr/local/nodejs/node-v14.16.0-linux-x64/目录下。

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

## 安装方式三 gitee源

### 安装

```shell
bash -c "$(curl -fsSL https://gitee.com/RubyKids/nvm-cn/blob/main/install.sh)"
```

### 卸载

```shell
bash -c "$(curl -fsSL https://gitee.com/RubyKids/nvm-cn/blob/main/uninstall.sh)"
```


# VirtualBox CentOs7虚拟机环境下配置NVM

## 安装方式一
```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

## 安装方式二
```shell
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

以上两种方式因为墙的原因访问不了!

## 安装方式三 git(v1.7.10+)方式
```shell
cd ~/
git clone https://github.com/nvm-sh/nvm.git .nvm
cd ~/.nvm
./nvm.sh
```

## 安装方式四 手动安装

### 手动下载nvm仓库, 上传到`~/`目录, 执行`.nvm/nvm.sh` , 先要给`nvm.sh`增加执行权限
```shell
[root@centos7full ~]# cd .nvm/
[root@centos7full .nvm]# ./nvm.sh
-bash: ./nvm.sh: 权限不够
[root@centos7full .nvm]# chmod +x nvm.sh
[root@centos7full .nvm]# ./nvm.sh
```

### 在`.bashrc`中添加以下配置
```shell
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

### 刷新`.bashrc`
```shell
[root@centos7full ~]# source .bashrc
```

## 查看nvm版本
```shell
[root@centos7full ~]# nvm -v
0.39.1
```

nvm安装成功!!!
