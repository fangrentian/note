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
+ -v  挂载容器目录到主机
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

## 支持https

### 修改 `openssl` 配置

一般情况下，证书只支持域名访问，要使其支持IP地址访问，需要修改配置文件 `/etc/pki/tls/openssl.cnf` ,在 `[ v3_req ]` 选项的 `keyUsage = nonRepudiation, digitalSignature, keyEncipherment`后
追加内容, 最终如下

```shell
[ v3_req ]

# Extensions to add to a certificate request

basicConstraints = CA:FALSE
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
subjectAltName = @alt_names

[alt_names]

# 改成自己的域名
# #DNS.1 = kb.example.com
# #DNS.2 = helpdesk.example.org
# #DNS.3 = systems.example.net
#
# # 改成自己的ip
IP.1 = 192.168.1.222

```

### 生成自签名证书

```shell
[root@centos7full ~]# openssl req -x509 -days 36500 -new -nodes -newkey rsa:2048 -keyout /opt/registry/config/domain.key -out /opt/registry/config/domain.crt
Generating a 2048 bit RSA private key
..............+++
..........................+++
writing new private key to '/opt/registry/config/domain.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:
State or Province Name (full name) []:
Locality Name (eg, city) [Default City]:
Organization Name (eg, company) [Default Company Ltd]:
Organizational Unit Name (eg, section) []:
Common Name (eg, your name or your server's hostname) []:192.168.1.222
Email Address []:
```
### 添加信任证书

在 `docker` 证书目录添加 自定义的证书目录

```shell
[root@centos7full ~]# mkdir -p /etc/docker/certs.d/192.168.1.222:5000
```

将生成的证书复制到刚新建的自定义证书目录

```shell
[root@centos7full ~]# cp /opt/registry/config/domain.crt  /etc/docker/certs.d/192.168.1.222\:5000/ca.crt
```

`ca-bundle` 里追加配置

```shell
[root@centos7full ~]# cat /opt/registry/config/domain.crt >> /etc/pki/tls/certs/ca-bundle.crt
```

### 删除原来的 `registry` 容器

```shell
[root@centos7full ~]# docker rm registry
registry
```

### 主机上创建需要挂载的文件夹及文件

文件夹: `/opt/registry/auth` `/opt/registry/certs` `/opt/registry/config` `/opt/registry/data`

文件: `/opt/registry/config/config.yml`

### 生成鉴权文件

```shell
[root@centos7full ~]# echo "user:root passwd:ares" >/opt/registry/auth/htpasswd
```

### 加密鉴权文件

```shell
docker run --entrypoint htpasswd registry:latest -Bbn root ares  >> /opt/registry/auth/htpasswd
```

**报错**

```shell
[root@centos7full ~]# docker run --entrypoint htpasswd registry:latest -Bbn root ares  > /opt/registry/auth/htpasswd
container_linux.go:290: starting container process caused "exec: \"htpasswd\": executable file not found in $PATH"
/usr/bin/docker-current: Error response from daemon: oci runtime error: container_linux.go:290: starting container process caused "exec: \"htpasswd\": executable file not found in $PATH".
```

据说registry镜像在2.7相关版本中删除了/usr/bin/htpasswd文件，导致创建容器时提醒找不到可执行文件,解决办法用老版 [https://github.com/docker/distribution-library-image/issues/106](https://github.com/docker/distribution-library-image/issues/106)

> 追加内容开始 2025年07月15日 星期二

用了`registry3`, 好像通过宿主机的`htpasswd`工具,或者docker的`httpd:2`镜像也可以,目的是获得加密内容, 如下两种方法都可以
```shell
[ares@ares-test ~]$ sudo docker run --rm --entrypoint htpasswd httpd:2 -Bbn root ares
[sudo] ares 的密码：
Unable to find image 'httpd:2' locally
2: Pulling from library/httpd
3da95a905ed5: Pull complete
71018c0d16cb: Pull complete
4f4fb700ef54: Pull complete
3b93b8c3888b: Pull complete
c5094faccdbb: Pull complete
14bacd2841b0: Pull complete
Digest: sha256:f84fe51ff5d35124e024f51215b443b16c939b24eae747025a515200e71c7d07
Status: Downloaded newer image for httpd:2
root:$2y$05$WwHEYerrrhkgq6SGvLd0xO.eDndsEGO4hGiq75xsxdX/DLMX5sFke
```
```shell
[ares@ares-test ~]$ sudo yum install httpd-tools
[sudo] ares 的密码：
上次元数据过期检查：1:36:45 前，执行于 2025年07月14日 星期一 13时11分38秒。
依赖关系解决。
===============================================================================================
 软件包             架构     版本                                            仓库         大小
===============================================================================================
安装:
 httpd-tools        x86_64   2.4.37-65.0.1.module+an8.9.0+11302+fdfeea98.3   AppStream   112 k
安装依赖关系:
 apr                x86_64   1.7.0-12.an8                                    AppStream   135 k
 apr-util           x86_64   1.6.1-9.an8                                     AppStream   105 k
安装弱的依赖:
 apr-util-bdb       x86_64   1.6.1-9.an8                                     AppStream    24 k
 apr-util-openssl   x86_64   1.6.1-9.an8                                     AppStream    26 k
启用模块流:
 httpd                       2.4                                                              

事务概要
===============================================================================================
安装  5 软件包

总下载：402 k
安装大小：727 k
确定吗？[y/N]： y
下载软件包：
(1/5): apr-util-bdb-1.6.1-9.an8.x86_64.rpm                     153 kB/s |  24 kB     00:00    
(2/5): apr-util-1.6.1-9.an8.x86_64.rpm                         479 kB/s | 105 kB     00:00    
(3/5): apr-1.7.0-12.an8.x86_64.rpm                             514 kB/s | 135 kB     00:00    
(4/5): apr-util-openssl-1.6.1-9.an8.x86_64.rpm                 241 kB/s |  26 kB     00:00    
(5/5): httpd-tools-2.4.37-65.0.1.module+an8.9.0+11302+fdfeea98 784 kB/s | 112 kB     00:00    
-----------------------------------------------------------------------------------------------
总计                                                           1.1 MB/s | 402 kB     00:00     
运行事务检查
事务检查成功。
运行事务测试
事务测试成功。
运行事务
  准备中  :                                                                                1/1 
  安装    : apr-1.7.0-12.an8.x86_64                                                        1/5 
  安装    : apr-util-bdb-1.6.1-9.an8.x86_64                                                2/5 
  安装    : apr-util-openssl-1.6.1-9.an8.x86_64                                            3/5 
  安装    : apr-util-1.6.1-9.an8.x86_64                                                    4/5 
  运行脚本: apr-util-1.6.1-9.an8.x86_64                                                    4/5 
/sbin/ldconfig: /usr/lib64/llvm15/lib/libclang.so.15 不是符号链接


  安装    : httpd-tools-2.4.37-65.0.1.module+an8.9.0+11302+fdfeea98.3.x86_64               5/5 
  运行脚本: httpd-tools-2.4.37-65.0.1.module+an8.9.0+11302+fdfeea98.3.x86_64               5/5 
/sbin/ldconfig: /usr/lib64/llvm15/lib/libclang.so.15 不是符号链接


  验证    : apr-1.7.0-12.an8.x86_64                                                        1/5 
  验证    : apr-util-1.6.1-9.an8.x86_64                                                    2/5 
  验证    : apr-util-bdb-1.6.1-9.an8.x86_64                                                3/5 
  验证    : apr-util-openssl-1.6.1-9.an8.x86_64                                            4/5 
  验证    : httpd-tools-2.4.37-65.0.1.module+an8.9.0+11302+fdfeea98.3.x86_64               5/5 

已安装:
  apr-1.7.0-12.an8.x86_64                                                                      
  apr-util-1.6.1-9.an8.x86_64                                                                  
  apr-util-bdb-1.6.1-9.an8.x86_64                                                              
  apr-util-openssl-1.6.1-9.an8.x86_64                                                          
  httpd-tools-2.4.37-65.0.1.module+an8.9.0+11302+fdfeea98.3.x86_64                             

完毕！
[ares@ares-test ~]$ htpasswd -Bbn ares heidelberg
ares:$2y$05$YGFt03SK.jrKanUbiZMs2OyFfvJ.NkNcOqgfame1WP.r1EAK4unyq
```

registry `config.yml`配置,环境变量可以直接配置在里面, `docker run`时就不用在指定环境变量了,只要挂载这个`config.yml`配置文件就行
```yaml
version: 0.1
log:
  level: debug
  fields:
    service: registry
    environment: development
storage:
  delete:
    enabled: true
  cache:
    blobdescriptor: inmemory
  filesystem:
    rootdirectory: /var/lib/registry
  tag:
    concurrencylimit: 5
http:
  addr: :5000
  debug:
    addr: :5001
    prometheus:
      enabled: true
      path: /metrics
  tls:
    certificate: /certs/domain.crt
    key: /certs/domain.key
health:
  storagedriver:
    enabled: true
    interval: 10s
    threshold: 3
auth:
  htpasswd:
    realm: basic-realm
    path: /auth/htpasswd
```
```shell
[ares@ares-test ~]$ sudo docker run --name registry --restart=always -p 5000:5000 -v /opt/registry/config/config.yml:/etc/distribution/config.yml -v /opt/registry/data:/var/lib/registry -v /opt/registry/certs:/certs -v /opt/registry/auth:/auth -d registry:latest
37a3e91ac14629c5829e47f4b23dd1724092b43a884ccbae407c68ac68b86339
[ares@ares-test ~]$ sudo docker ps -a
CONTAINER ID   IMAGE             COMMAND                   CREATED         STATUS         PORTS                                       NAMES
37a3e91ac146   registry:latest   "/entrypoint.sh /etc…"   9 seconds ago   Up 8 seconds   0.0.0.0:5000->5000/tcp, :::5000->5000/tcp   registry
```

> 追加内容结束

这里降级使用`registry:2.6.2`
```shell
[root@centos7full ~]# docker pull registry:2.6.2
Trying to pull repository docker.io/library/registry ... 
2.6.2: Pulling from docker.io/library/registry
486039affc0a: Pull complete 
ba51a3b098e6: Pull complete 
470e22cd431a: Pull complete 
1048a0cdabb0: Pull complete 
ca5aa9d06321: Pull complete 
Digest: sha256:c4bdca23bab136d5b9ce7c06895ba54892ae6db0ebfc3a2f1ac413a470b17e47
Status: Downloaded newer image for docker.io/registry:2.6.2
[root@centos7full ~]# docker run --entrypoint htpasswd registry:2.6.2 -Bbn root ares  >> /opt/registry/auth/htpasswd
```

### 重新安装 `registry` 容器

```shell
[root@centos7full ~]# docker run --name registry --restart=always -p 5000:5000 -e "REGISTRY_AUTH=htpasswd" -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" -e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key -v /opt/registry/config:/etc/docker/registry -v /opt/registry/data:/var/lib/registry -v /opt/registry/certs:/certs -v /opt/registry/auth:/auth -d registry:2.6.2
433b7e6e53b42e332cf3cbc249d5ff63fb1c11d1feb6ca6c2e7cf4ec75129d10
```

**参数说明**

+ --name  容器别名
+ --restart=always  开机启动
+ -p  映射容器端口
+ -v  挂载容器目录到主机
+ -d  后台运行
+ -e  传递环境变量参数,配置证书(路径为容器的路径)


**遇到了个异常**

```shell
/usr/bin/docker-current: Error response from daemon: oci runtime error: container_linux.go:290: starting container process caused "container init exited prematurely".
```

以为是挂载目录 `-v /opt/registry/config.yml:/etc/docker/registry/config.yml` 时没有提前创建好 `/opt/registry/config.yml`目录, 创建了对应目录,结果还是报同样的错误, 所以应该是系统把 `/etc/docker/registry/config.yml` 当作目录来处理了, 实际上没有这个目录, 这是个文件, 那么直接挂载该文件所在目录 `-v /opt/registry/config:/etc/docker/registry` 试试, 结果没有报错了, **需要
提前在 `下创建 `config.yml` 配置文件**,  不然容器启动后有异常, 会报错'configuration error: open /etc/docker/registry/config.yml: no such file or directory'


### 其他机器访问

将证书分发到其他机器即可

```shell
[root@centos7full ~]# ssh root@192.168.1.231 "mkdir -p /etc/docker/certs.d/192.168.1.222:5000"
root@192.168.1.231's password: 

[root@centos7full ~]# scp /opt/registry/certs/domain.crt root@192.168.1.231:/etc/docker/certs.d/192.168.1.222\:5000/ca.crt
root@192.168.1.231's password: 
domain.crt   
```

### 未使用鉴权测试

```shell
[root@centos7full ~]# curl -k https://192.168.1.222:5000/v2/_catalog
{"repositories":[]}
```

### 加鉴权后测试

直接推送会提示失败,  登录后重新推送,  成功

```shell
[root@centos7full ~]# docker push 192.168.1.222:5000/comparer:test
The push refers to a repository [192.168.1.222:5000/comparer]
01e8df0e0edb: Preparing 
5723392d8fc1: Preparing 
55308b4854c8: Preparing 
029ffaa3de03: Preparing 
37a43b138e10: Preparing 
8d768709f9c6: Preparing 
d8a33133e477: Preparing 
no basic auth credentials
[root@centos7full ~]# docker login 192.168.1.222:5000
Username: root
Password: 
Login Succeeded
[root@centos7full ~]# docker push 192.168.1.222:5000/comparer:test
The push refers to a repository [192.168.1.222:5000/comparer]
01e8df0e0edb: Layer already exists 
5723392d8fc1: Layer already exists 
55308b4854c8: Layer already exists 
029ffaa3de03: Layer already exists 
37a43b138e10: Layer already exists 
8d768709f9c6: Layer already exists 
d8a33133e477: Layer already exists 
test: digest: sha256:010a9a4c27afba2db2f5816f77cd33d6620096d7709a0da3a06a1c09e8edbaad size: 1796
```

```shell
[root@centos7full ~]# curl -k -u root:ares https://192.168.1.222:5000/v2/_catalog
{"repositories":["comparer","root/comparer"]}
[root@centos7full ~]# curl -k -u root:ares https://192.168.1.222:5000/v2/comparer/tags/list
{"name":"comparer","tags":["test","1"]}
```
