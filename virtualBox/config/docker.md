<!--
 * @author: ares
 * @date: 2022-01-13 16:00:08
 * @lastEditTime: 2022-01-13 16:58:41
 * @lastEditors: ares
 * @description: 
 * 
-->

## docker环境配置

### 查看内核版本 <Docker 要求 CentOS 系统的内核版本高于 3.10>

```shell
uname -r
```

### 把yum包更新到最新

```shell
sudo yum update
```

### 安装需要的软件包, yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的

```shell
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

### 设置yum源

```shell
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

### 查看仓库中docker版本

```shell
yum list docker-ce --showduplicates | sort -r
```

### 安装docker

```shell
sudo yum install docker-ce
```

### 启动Docker|设置开机启动|停止Docker

```shell
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl stop docker
```

### 查看版本

```shell
docker version
```

### 试用 search 查一下

```shell
docker search mysql
```

### 查看状态

```shell
systemctl status docker.service 
```

## 卸载Docker,对于旧版本没安装成功,需要卸载掉

### 查询安装过的包

```shell
yum list installed | grep docker
```

例如:本机安装过旧版本 docker.x86_64,docker-client.x86_64,docker-common.x86_64
  
### 删除安装的软件包

```shell
yum -y remove docker.x86_64
yum -y remove docker-client.x86_64
yum -y remove docker-common.x86_64
```
