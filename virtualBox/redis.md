# VirtualBox CentOs7虚拟机环境下安装redis

## 安装依赖

redis是由C语言开发，因此安装之前必须要确保服务器已经安装了gcc，可以通过如下命令查看机器是否安装：
```shell
[root@centos7full ~]# gcc -v
使用内建 specs。
COLLECT_GCC=gcc
COLLECT_LTO_WRAPPER=/usr/libexec/gcc/x86_64-redhat-linux/4.8.5/lto-wrapper
目标：x86_64-redhat-linux
配置为：../configure --prefix=/usr --mandir=/usr/share/man --infodir=/usr/share/info --with-bugurl=http://bugzilla.redhat.com/bugzilla --enable-bootstrap --enable-shared --enable-threads=posix --enable-checking=release --with-system-zlib --enable-__cxa_atexit --disable-libunwind-exceptions --enable-gnu-unique-object --enable-linker-build-id --with-linker-hash-style=gnu --enable-languages=c,c++,objc,obj-c++,java,fortran,ada,go,lto --enable-plugin --enable-initfini-array --disable-libgcj --with-isl=/builddir/build/BUILD/gcc-4.8.5-20150702/obj-x86_64-redhat-linux/isl-install --with-cloog=/builddir/build/BUILD/gcc-4.8.5-20150702/obj-x86_64-redhat-linux/cloog-install --enable-gnu-indirect-function --with-tune=generic --with-arch_32=x86-64 --build=x86_64-redhat-linux
线程模型：posix
gcc 版本 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC)
```

如果没有安装, 通过`yum`命令安装一下
```shell
yum install -y gcc
```

## 下载redis安装包并解压, 下载目录根据实际情况自己确定, 我是在`tmp`目录下操作的

```shell
[root@centos7full tmp]# wget https://download.redis.io/releases/redis-6.2.6.tar.gz
```

```shell
[root@centos7full tmp]# tar -zxvf redis-6.2.6.tar.gz
```

解压后在`tmp`目录下生成`redis-6.2.6`文件夹

## 编译redis
```shell
[root@centos7full tmp]# cd redis-6.2.6
[root@centos7full redis-6.2.6]# make
```

## 安装redis到指定目录
```shell
[root@centos7full redis-6.2.6]# make install PREFIX=/usr/local/redis-6.2.6
```

## 复制安装包下的配置文件,作为启动redis的配置文件来用
```shell
[root@centos7full redis-6.2.6]# cp redis.conf /etc/redis/redis.conf
```

## 修改配置文件,将`daemonize`设置为`yes`, 不改这个配置的话,后面用`systemctl`启动时报错
```shell
vim /etc/redis/redis.conf
```

## 创建并编辑`redis.service`文件
```shell
[root@centos7full ~]# touch /lib/systemd/system/redis.service
[root@centos7full ~]# vim /lib/systemd/system/redis.service
```

配置`redis.service`内容
```
[Unit]
Description=redis-server
After=network.target

[Service]
Type=forking
# ExecStart需要按照实际情况修改成自己的地址
ExecStart=/usr/local/redis-6.2.6/bin/redis-server /etc/redis/redis.conf
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

## 配置开机启动
```shell
[root@centos7full ~]# systemctl enable redis.service
Created symlink from /etc/systemd/system/multi-user.target.wants/redis.service to /usr/lib/systemd/system/redis.service.
```

## 启动redis
```shell
systemctl start redis.service
```

## 查看redis是否启动成功
```shell
[root@centos7full ~]# systemctl status redis.service
● redis.service - redis-server
   Loaded: loaded (/usr/lib/systemd/system/redis.service; enabled; vendor preset: disabled)
   Active: active (running) since 三 2022-08-31 12:49:12 CST; 10s ago
  Process: 25734 ExecStart=/usr/local/redis-6.2.6/bin/redis-server /etc/redis/redis.conf (code=exited, status=0/SUCCESS)
 Main PID: 25735 (redis-server)
   CGroup: /system.slice/redis.service
           └─25735 /usr/local/redis-6.2.6/bin/redis-server 127.0.0.1:6379

8月 31 12:49:12 centos7full systemd[1]: Starting redis-server...
8月 31 12:49:12 centos7full systemd[1]: Started redis-server.
```

redis服务启动成功!!

## 客户端不能访问

### 防火墙开放`6379`端口
```shell
[root@centos7full ~]# firewall-cmd --zone=public --add-port=6379/tcp --permanent
success
```

### 更新防火墙
```shell
[root@centos7full ~]# firewall-cmd --reload
success
```

### 重新查看开放端口
```shell
[root@centos7full ~]# firewall-cmd --zone=public --list-ports
3306/tcp 80/tcp 6379/tcp
```

### 修改配置,将`protected-mode`设置为`no`, 注释掉`bind 127.0.0.1 -::1`行
```shell
vim /etc/redis/redis.conf
```
```
# bind 127.0.0.1 -::1
protected-mode no
```

重启redis后,用客户端连接测试,可以正常连接!!!
