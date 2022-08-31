<!--
 * @author: ares
 * @date: 2022-01-13 16:00:08
 * @lastEditTime: 2022-01-13 16:58:41
 * @lastEditors: ares
 * @description: 
 * 
-->

# docker环境配置

## 查看内核版本 <Docker 要求 CentOS 系统的内核版本高于 3.10>

```shell
uname -r
```

## 把yum包更新到最新

```shell
sudo yum update
```

## 安装需要的软件包,`yum-util` 提供`yum-config-manager`功能，另外两个是`devicemapper`驱动依赖的

```shell
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

## 设置yum源

```shell
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

## 查看仓库中docker版本

```shell
yum list docker-ce --showduplicates | sort -r
```

## 安装docker

```shell
sudo yum install docker-ce
```

## 启动Docker|设置开机启动|停止Docker

```shell
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl stop docker
```

## 查看版本

```shell
docker version
```

## 试用 search 查一下

```shell
docker search mysql
```

## 查看状态

```shell
systemctl status docker.service
```

# 卸载Docker,对于旧版本没安装成功,需要卸载掉

## 查询安装过的包

```shell
yum list installed | grep docker
```

例如:本机安装过旧版本`docker.x86_64,docker-client.x86_64`,`docker-common.x86_64`

## 删除安装的软件包

```shell
yum -y remove docker.x86_64
yum -y remove docker-client.x86_64
yum -y remove docker-common.x86_64
```


# VirtualBox CentOs7虚拟机环境下安装docker

## 查看自己系统内核,据说Docker 要求 CentOS 系统的内核版本高于 3.10
```shell
[root@centos7full ~]# uname -r
3.10.0-1160.71.1.el7.x86_64
```

## 查看docker包
```shell
[root@centos7full ~]# yum list docker*
已加载插件：fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.nju.edu.cn
 * extras: mirrors.bfsu.edu.cn
 * updates: mirrors.bfsu.edu.cn
可安装的软件包
docker.x86_64                                                     2:1.13.1-209.git7d71120.el7.centos                               extras
docker-client.x86_64                                              2:1.13.1-209.git7d71120.el7.centos                               extras
docker-client-latest.x86_64                                       1.13.1-58.git87f2fab.el7.centos                                  extras
docker-common.x86_64                                              2:1.13.1-209.git7d71120.el7.centos                               extras
docker-distribution.x86_64                                        2.6.2-2.git48294d9.el7                                           extras
docker-latest.x86_64                                              1.13.1-58.git87f2fab.el7.centos                                  extras
docker-latest-logrotate.x86_64                                    1.13.1-58.git87f2fab.el7.centos                                  extras
docker-latest-v1.10-migrator.x86_64                               1.13.1-58.git87f2fab.el7.centos                                  extras
docker-logrotate.x86_64                                           2:1.13.1-209.git7d71120.el7.centos                               extras
docker-lvm-plugin.x86_64                                          2:1.13.1-209.git7d71120.el7.centos                               extras
docker-novolume-plugin.x86_64                                     2:1.13.1-209.git7d71120.el7.centos                               extras
docker-registry.x86_64                                            0.9.1-7.el7                                                      extras
docker-v1.10-migrator.x86_64                                      2:1.13.1-209.git7d71120.el7.centos                               extras
```

已经有可用软件包

## 安装
```shell
yum install docker
```

## 查看docker版本
```shell
[root@centos7full ~]# docker -v
Docker version 1.13.1, build 7d71120/1.13.1
```

## 设置开机启动
```shell
[root@centos7full ~]# systemctl enable docker
Created symlink from /etc/systemd/system/multi-user.target.wants/docker.service to /usr/lib/systemd/system/docker.service.
```

## 启动docker
```shell
systemctl start docker
```

## 查看docker状态
```shell
[root@centos7full ~]# systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running) since 三 2022-08-31 14:10:10 CST; 32s ago
     Docs: http://docs.docker.com
 Main PID: 11877 (dockerd-current)
   CGroup: /system.slice/docker.service
           ├─11877 /usr/bin/dockerd-current --add-runtime docker-runc=/usr/libexec/docker/docker-runc-current --default-runtime=docker...
           └─11882 /usr/bin/docker-containerd-current -l unix:///var/run/docker/libcontainerd/docker-containerd.sock --metrics-interva...

8月 31 14:10:09 centos7full dockerd-current[11877]: time="2022-08-31T14:10:09.004580510+08:00" level=info msg="libcontainerd: n...11882"
8月 31 14:10:10 centos7full dockerd-current[11877]: time="2022-08-31T14:10:10.067065322+08:00" level=info msg="Graph migration ...conds"
8月 31 14:10:10 centos7full dockerd-current[11877]: time="2022-08-31T14:10:10.068207551+08:00" level=info msg="Loading containe...tart."
8月 31 14:10:10 centos7full dockerd-current[11877]: time="2022-08-31T14:10:10.090561045+08:00" level=info msg="Firewalld running: true"
8月 31 14:10:10 centos7full dockerd-current[11877]: time="2022-08-31T14:10:10.197081714+08:00" level=info msg="Default bridge (...dress"
8月 31 14:10:10 centos7full dockerd-current[11877]: time="2022-08-31T14:10:10.270726264+08:00" level=info msg="Loading containe...done."
8月 31 14:10:10 centos7full dockerd-current[11877]: time="2022-08-31T14:10:10.279355673+08:00" level=info msg="Daemon has compl...ation"
8月 31 14:10:10 centos7full dockerd-current[11877]: time="2022-08-31T14:10:10.279369168+08:00" level=info msg="Docker daemon" c...1.13.1
8月 31 14:10:10 centos7full systemd[1]: Started Docker Application Container Engine.
8月 31 14:10:10 centos7full dockerd-current[11877]: time="2022-08-31T14:10:10.285597703+08:00" level=info msg="API listen on /v....sock"
Hint: Some lines were ellipsized, use -l to show in full.
```

docker已正常运行!
