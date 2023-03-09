<!--
 * @author: ares
 * @date: 2022-01-14 08:53:00
 * @lastEditTime: 2022-01-14 13:12:37
 * @lastEditors: ares
 * @description: 
 * 
-->
# 安装mysql容器

```shell
docker run --name mysql -p 3306:3306 --restart=always -v /home/docker/mysql:/etc/mysql -e MYSQL_ROOT_PASSWORD=ares -d mysql:5.7.26
```

**参数说明**

+ --name  容器别名
+ -p  映射容器端口
+ --restart=always  开机启动
+ -v  挂载容器目录到主机
+ -e  MYSQL_ROOT_PASSWORD=ares 给root账户设置密码
+ -d  后台运行

# 安装redis容器

```shell
docker run --name redis -p 6379:6379 --restart=always -v /home/docker/redis/redis.conf:/etc/redis/redis.conf -v /home/docker/redis/data:/data -d redis:6.2.6 redis-server /etc/redis/redis.conf --appendonly yes --requirepass ares
```

**参数说明**

+ --name  容器别名
+ -p  映射容器端口
+ --restart=always  开机启动
+ -v  挂载容器目录到主机
+ -d  后台运行
+ redis-server /etc/redis/redis.conf  以配置文件启动redis，加载容器内的conf文件，最终用的是挂载目录的配置文件/home/docker/redis/redis.conf
+ --appendonly yes  开启redis 持久化
+ --requirepass ares 连接redis的密码

# 安装nginx容器

## 先安装临时的nginx容器,主要是想拷贝临时容器的配置文件

```shell
docker run --name nginx -d nginx:latest
docker cp nginx:/etc/nginx/nginx.conf /home/docker/nginx/conf
docker cp nginx:/etc/nginx/conf.d /home/docker/nginx/conf
docker cp nginx:/usr/share/nginx/html /home/docker/nginx
```

## 删除临时容器

```shell
docker rm nginx
```

## 安装正式nginx容器

```shell
docker run -p 80:80 --name nginx --restart=always -v /home/docker/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /home/docker/nginx/conf/conf.d:/etc/nginx/conf.d -v /home/docker/nginx/html:/usr/share/nginx/html -v /home/docker/nginx/logs:/var/log/nginx -d nginx:latest
```

**参数说明**

+ -p  映射容器端口
+ --name  容器别名
+ --restart=always  开机启动
+ -v  挂载容器目录到主机
+ -d  后台运行

# 安装jenkins容器

## 拉取镜像

```shell
docker pull jenkins/jenkins
```

## 日志配置

在jenkins容器挂载的文件夹(例如: /home/docker/jenkins)下新建log.properties,输入内容

```
handlers=java.util.logging.ConsoleHandler
jenkins.level=FINEST
java.util.logging.ConsoleHandler.level=FINEST
```

## 如果挂载的文件夹权限不够, 配置下文件夹权限,执行:

```shell
chown -R 1000:1000 /home/docker/jenkins
```

或者加启动参数 -u root

## 安装jenkins容器

```shell
docker run --name jenkins --restart=always -u root -p 8080:8080 -p 50000:50000 --env JAVA_OPTS="-Djava.util.logging.config.file=/var/jenkins_home/log.properties" --env JENKINS_OPTS="--prefix=/jenkins" -v /home/docker/jenkins:/var/jenkins_home -d jenkins/jenkins:latest
```

**参数说明**

+ --name  容器别名
+ --restart=always  开机启动
+ -u root  以root账号启动
+ -p  映射容器端口
+ --env JAVA_OPTS="-Djava.util.logging.config.file=/var/jenkins_home/log.properties"  配置日志
+ --env JENKINS_OPTS="--prefix=/jenkins"  配置前缀
+ -v  挂载容器目录到主机
+ -d  后台运行

## nginx中代理jenkins

在/home/docker/nginx/conf/conf.d/default.conf中添加

```
location /jenkins/ {
    proxy_pass <http://121.5.64.100:8080>;
}
```

# 安装tomcat容器

```shell
docker run --name tomcat -p 9090:8080 -v /home/docker/tomcat/webapps:/usr/local/tomcat/webapps -v /home/docker/tomcat/logs:/usr/local/tomcat/logs -v /home/docker/tomcat/conf:/usr/local/tomcat/conf -d tomcat:8.5.28
```

**参数说明**

+ --name  容器别名
+ -p  映射容器端口
+ -v  挂载容器目录到主机
+ -d  后台运行


# 安装registry容器, 搭建私有仓库

## 拉取镜像

```shell
docker pull registry
[root@centos7full ~]# docker pull registry
Using default tag: latest
Trying to pull repository docker.io/library/registry ... 
latest: Pulling from docker.io/library/registry
ef5531b6e74e: Pull complete 
a52704366974: Pull complete 
dda5a8ba6f46: Pull complete 
eb9a2e8a8f76: Pull complete 
25bb6825962e: Pull complete 
Digest: sha256:3f71055ad7c41728e381190fee5c4cf9b8f7725839dcf5c0fe3e5e20dc5db1fa
Status: Downloaded newer image for docker.io/registry:latest

```

## 查看镜像

```shell
[root@centos7full ~]# docker images
REPOSITORY                         TAG                 IMAGE ID            CREATED             SIZE
docker.io/registry                 latest              0d153fadf70b        3 weeks ago         24.2 MB
java                               8                   9a667facd251        6 months ago        444 MB
docker.io/nginx                    1.22.0              1b84ed9be2d4        6 months ago        142 MB
docker.io/suranagivinod/openjdk8   latest              cd836f0eb6c3        2 years ago         334 MB
```

## 配置`/etc/docker/daemon.json`

```json
{
  "insecure-registries": ["192.168.1.222:5000"],
  "registry-mirrors": ["https://zvj5fyew.mirror.aliyuncs.com"]
}
```

**说明**

+ `insecure-registries`: 配置私有仓库地址
+ `registry-mirrors`: 加速源

## 重启daemon和docker

```shell
[root@centos7full ~]# systemctl daemon-reload
[root@centos7full ~]# systemctl restart docker
```

## 安装registry容器
```shell
[root@centos7full ~]# docker run -d -v /opt/registry:/var/lib/registry -p 5000:5000 --restart=always --name registry registry:latest
ffdf6e7a8311fe53b95390bd9e40cc4faf8aa36475924a2138aec6cdd9ba4e20
```

**参数说明**

+ --name  容器别名
+ --restart=always  开机启动
+ -p  映射容器端口
+ -v  挂载容器目录到主机(Registry服务默认会将上传的镜像保存在容器的/var/lib/registry，我们将主机的/opt/registry目录挂载到该目录，即可实现将镜像保存到主机的/opt/registry目录了。)
+ -d  后台运行

## 查看容器

```shell
[root@centos7full ~]# docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                    NAMES
ffdf6e7a8311        registry:latest     "/entrypoint.sh /e..."   About a minute ago   Up About a minute   0.0.0.0:5000->5000/tcp   registry
```

## 验证

### 查看本地新搭建的仓库
```shell
[root@centos7full ~]# curl http://192.168.1.222:5000/v2/_catalog
{"repositories":[]}
```

刚搭建的仓库,还没有镜像.

### 为想要推送到本地私有仓库的镜像打上本地仓库的标签

```shell
[root@centos7full ~]# docker tag nginx:1.22.0 192.168.1.222:5000/nginx:test
```

### 重新查看镜像

```shell
[root@centos7full ~]# docker images
REPOSITORY                         TAG                 IMAGE ID            CREATED             SIZE
docker.io/registry                 latest              0d153fadf70b        3 weeks ago         24.2 MB
java                               8                   9a667facd251        6 months ago        444 MB
192.168.1.222:5000/nginx           test                1b84ed9be2d4        6 months ago        142 MB
docker.io/nginx                    1.22.0              1b84ed9be2d4        6 months ago        142 MB
docker.io/suranagivinod/openjdk8   latest              cd836f0eb6c3        2 years ago         334 MB
```

发现多了一个 `192.168.1.222:5000/nginx:test` 的镜像

### 将标记为本地仓库镜像的某个镜像推送到本地仓库

```shell
[root@centos7full ~]# docker push 192.168.1.222:5000/nginx:test
The push refers to a repository [192.168.1.222:5000/nginx]
9760b00a1245: Retrying in 1 second 
053c9e98185f: Retrying in 1 second 
fdf60923d025: Retrying in 1 second 
59dc71c10295: Retrying in 1 second 
e08753aa4850: Retrying in 1 second 
6485bed63627: Waiting 
received unexpected HTTP status: 500 Internal Server Error
```

发现报错了,  使用`tailf /var/log/messages`或 `docker logs -f registry`查看下日志,发现包含一下信息

```
err.detail="filesystem: mkdir /var/lib/registry/docker: permission denied"
```

看起来好像是没权限, 有人说是可能是 一个 `selinux` 问题，需要在服务器上对挂载目录进行处理：

```shell
[root@centos7full ~]# chcon -Rt svirt_sandbox_file_t /opt/registry/
```

重新推送

```shell
[root@centos7full ~]# docker push 192.168.1.222:5000/nginx:test
The push refers to a repository [192.168.1.222:5000/nginx]
9760b00a1245: Pushed 
053c9e98185f: Pushed 
fdf60923d025: Pushed 
59dc71c10295: Pushed 
e08753aa4850: Pushed 
6485bed63627: Pushed 
test: digest: sha256:cae0639640038c2498fdc166683e3ade877672fa760ba0814ee3c3658edb6228 size: 1570
```

好像成功了

### 查看本地仓库

```shell
[root@centos7full ~]# curl http://192.168.1.222:5000/v2/_catalog
{"repositories":["nginx"]}
```

```shell
[root@centos7full ~]# curl http://192.168.1.222:5000/v2/nginx/tags/list
{"name":"nginx","tags":["test"]}
```

本地仓库已经多了一个`nginx:test`的镜像, 说明推送成功了

### 拉取本地仓库镜像

#### 删除本地 `192.168.1.222:5000/nginx:test` 的tag

```shell
[root@centos7full ~]# docker rmi 192.168.1.222:5000/nginx:test
Untagged: 192.168.1.222:5000/nginx:test
Untagged: 192.168.1.222:5000/nginx@sha256:cae0639640038c2498fdc166683e3ade877672fa760ba0814ee3c3658edb6228
```

#### 再次查看镜像

```shell
[root@centos7full ~]# docker images
REPOSITORY                         TAG                 IMAGE ID            CREATED             SIZE
docker.io/registry                 latest              0d153fadf70b        3 weeks ago         24.2 MB
java                               8                   9a667facd251        6 months ago        444 MB
docker.io/nginx                    1.22.0              1b84ed9be2d4        6 months ago        142 MB
docker.io/suranagivinod/openjdk8   latest              cd836f0eb6c3        2 years ago         334 MB
```

`192.168.1.222:5000/nginx:test` 的tag已经没有了

#### 拉取本地仓库的 `nginx:test` 镜像

```shell
[root@centos7full ~]# docker pull 192.168.1.222:5000/nginx:test
Trying to pull repository 192.168.1.222:5000/nginx ... 
test: Pulling from 192.168.1.222:5000/nginx
Digest: sha256:cae0639640038c2498fdc166683e3ade877672fa760ba0814ee3c3658edb6228
Status: Downloaded newer image for 192.168.1.222:5000/nginx:test
```

#### 再次查看镜像

```shell
[root@centos7full ~]# docker images
REPOSITORY                         TAG                 IMAGE ID            CREATED             SIZE
docker.io/registry                 latest              0d153fadf70b        3 weeks ago         24.2 MB
java                               8                   9a667facd251        6 months ago        444 MB
192.168.1.222:5000/nginx           test                1b84ed9be2d4        6 months ago        142 MB
docker.io/nginx                    1.22.0              1b84ed9be2d4        6 months ago        142 MB
docker.io/suranagivinod/openjdk8   latest              cd836f0eb6c3        2 years ago         334 MB
```

拉取成功

## 修改 `registry` 配置, 支持删除本地仓库镜像

### 修改配置

```shell
[root@centos7full ~]# docker exec -it registry vi /etc/docker/registry/config.yml
```

增加 `delete` 字段, `enabled` 设置为 `true`

```yaml
version: 0.1
log:
  fields:
    service: registry
storage:
  cache:
    blobdescriptor: inmemory
  filesystem:
    rootdirectory: /var/lib/registry
  delete:     
    enabled: true
http:        
  addr: :5000                        
  headers:                           
    X-Content-Type-Options: [nosniff]
health:          
  storagedriver: 
    enabled: true
    interval: 10s
    threshold: 3
```

### 重启 registry

```shell
[root@centos7full ~]# docker restart registry
registry
```

### 获取待删除本地私有仓库镜像的sha256值

命令 `curl --header "Accept:application/vnd.docker.distribution.manifest.v2+json" -I -XGET http://镜像地址/v2/镜像名称/manifests/镜像版本`

示例

```shell
[root@centos7full ~]# curl --header "Accept:application/vnd.docker.distribution.manifest.v2+json" -I -XGET http://192.168.1.222:5000/v2/nginx/manifests/test
HTTP/1.1 200 OK
Content-Length: 1570
Content-Type: application/vnd.docker.distribution.manifest.v2+json
Docker-Content-Digest: sha256:cae0639640038c2498fdc166683e3ade877672fa760ba0814ee3c3658edb6228
Docker-Distribution-Api-Version: registry/2.0
Etag: "sha256:cae0639640038c2498fdc166683e3ade877672fa760ba0814ee3c3658edb6228"
X-Content-Type-Options: nosniff
Date: Thu, 09 Mar 2023 05:38:42 GMT
```

### 删除获取了sha256的本地私有仓库镜像

命令

`curl -I -XDELETE 私有仓库地址/v2/镜像名称/manifests/镜像对应sha256值`

示例

```shell
[root@centos7full ~]# curl -I -XDELETE http://192.168.1.222:5000/v2/nginx/manifests/sha256:cae0639640038c2498fdc166683e3ade877672fa760ba0814ee3c3658edb6228
HTTP/1.1 202 Accepted
Docker-Distribution-Api-Version: registry/2.0
X-Content-Type-Options: nosniff
Date: Thu, 09 Mar 2023 05:45:12 GMT
Content-Length: 0
```

### 查看本地私有仓库

```shell
[root@centos7full ~]# curl http://192.168.1.222:5000/v2/_catalog
{"repositories":["nginx"]}
[root@centos7full ~]# curl http://192.168.1.222:5000/v2/nginx/tags/list
{"name":"nginx","tags":null
```

再次拉取 `192.168.1.222:5000/nginx:test`, 发现镜像已经没有了

```shell
[root@centos7full ~]# docker pull 192.168.1.222:5000/nginx:test
Trying to pull repository 192.168.1.222:5000/nginx ... 
Pulling repository 192.168.1.222:5000/nginx
Error: image nginx:test not found
```
