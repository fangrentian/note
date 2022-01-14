<!--
 * @author: ares
 * @date: 2022-01-14 08:53:00
 * @lastEditTime: 2022-01-14 13:12:37
 * @lastEditors: ares
 * @description: 
 * 
-->
## 安装mysql容器

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

## 安装redis容器

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

## 安装nginx容器

### 先安装临时的nginx容器,主要是想拷贝临时容器的配置文件

```shell
docker run --name nginx -d nginx:latest
docker cp nginx:/etc/nginx/nginx.conf /home/docker/nginx/conf
docker cp nginx:/etc/nginx/conf.d /home/docker/nginx/conf
docker cp nginx:/usr/share/nginx/html /home/docker/nginx
```

### 删除临时容器

```shell
docker rm nginx
```

### 安装正式nginx容器

```shell
docker run -p 80:80 --name nginx --restart=always -v /home/docker/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /home/docker/nginx/conf/conf.d:/etc/nginx/conf.d -v /home/docker/nginx/html:/usr/share/nginx/html -v /home/docker/nginx/logs:/var/log/nginx -d nginx:latest
```

**参数说明**

+ -p  映射容器端口
+ --name  容器别名
+ --restart=always  开机启动
+ -v  挂载容器目录到主机
+ -d  后台运行

## 安装jenkins容器

### 拉取镜像

```shell
docker pull jenkins/jenkins
```

### 日志配置

在jenkins容器挂载的文件夹(例如: /home/docker/jenkins)下新建log.properties,输入内容

```
handlers=java.util.logging.ConsoleHandler
jenkins.level=FINEST
java.util.logging.ConsoleHandler.level=FINEST
```

### 如果挂载的文件夹权限不够, 配置下文件夹权限,执行:

```shell
chown -R 1000:1000 /home/docker/jenkins
```

或者加启动参数 -u root

### 安装jenkins容器

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

### nginx中代理jenkins

在/home/docker/nginx/conf/conf.d/default.conf中添加

```
location /jenkins/ {
    proxy_pass <http://121.5.64.100:8080>;
}
```

## 安装tomcat容器

```shell
docker run --name tomcat -p 9090:8080 -v /home/docker/tomcat/webapps:/usr/local/tomcat/webapps -v /home/docker/tomcat/logs:/usr/local/tomcat/logs -v /home/docker/tomcat/conf:/usr/local/tomcat/conf -d tomcat:8.5.28
```

**参数说明**

+ --name  容器别名
+ -p  映射容器端口
+ -v  挂载容器目录到主机
+ -d  后台运行
