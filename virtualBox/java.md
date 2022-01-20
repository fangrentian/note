<!--
 * @author: ares
 * @date: 2022-01-13 15:53:33
 * @lastEditTime: 2022-01-14 08:54:25
 * @lastEditors: ares
 * @description: 
 * 
-->
# java环境配置

## 下载jdk

在主机下载压缩文件 jdk-8u301-linux-x64.tar.gz, 上传到虚拟机的/tmp目录下(上传目录不限)

## 解压jdk

```shell
tar -zxvf /tmp/jdk-8u301-linux-x64.tar.gz -C /usr/local/java/
```

## 修改profile

在/etc/profile中添加以下配置

```
export JAVA_HOME=/usr/local/java/jdk1.8.0_301
export CLASSPATH=.:${JAVA_HOME}/jre/lib/rt.jar:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar
export PATH=$PATH:${JAVA_HOME}/bin
```

## 让设置立即生效

```shell
source /etc/profile 
```
