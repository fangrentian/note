<!--
 * @author: ares
 * @date: 2022-01-13 15:54:21
 * @lastEditTime: 2022-01-13 15:57:32
 * @lastEditors: ares
 * @description: 
 * 
-->
# maven环境配置

## 下载maven

```shell
wget https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
```

## 解压maven

```shell
tar -zxvf apache-maven-3.6.3-bin.tar.gz -C /usr/local/maven/
```

## 修改profile

在/etc/profile中添加以下配置

```
export MAVEN_HOME=/usr/local/maven/apache-maven-3.6.3
export PATH=$PATH:${MAVEN_HOME}/bin
```
