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


# VirtualBox CentOs7虚拟机环境下配置java环境

## 查看java包
```shell
[root@centos7full ~]# yum list java*
已加载插件：fastestmirror, product-id, search-disabled-repos, subscription-manager

This system is not registered with an entitlement server. You can use subscription-manager to register.

Loading mirror speeds from cached hostfile
 * base: mirrors.bupt.edu.cn
 * extras: mirrors.bupt.edu.cn
 * updates: mirrors.tuna.tsinghua.edu.cn
可安装的软件包
java-1.6.0-openjdk.x86_64                                              1:1.6.0.41-1.13.13.1.el7_3                                 base   
java-1.6.0-openjdk-demo.x86_64                                         1:1.6.0.41-1.13.13.1.el7_3                                 base   
java-1.6.0-openjdk-devel.x86_64                                        1:1.6.0.41-1.13.13.1.el7_3                                 base   
java-1.6.0-openjdk-javadoc.x86_64                                      1:1.6.0.41-1.13.13.1.el7_3                                 base   
java-1.6.0-openjdk-src.x86_64                                          1:1.6.0.41-1.13.13.1.el7_3                                 base   
java-1.7.0-openjdk.x86_64                                              1:1.7.0.261-2.6.22.2.el7_8                                 base   
java-1.7.0-openjdk-accessibility.x86_64                                1:1.7.0.261-2.6.22.2.el7_8                                 base   
java-1.7.0-openjdk-demo.x86_64                                         1:1.7.0.261-2.6.22.2.el7_8                                 base   
java-1.7.0-openjdk-devel.x86_64                                        1:1.7.0.261-2.6.22.2.el7_8                                 base   
java-1.7.0-openjdk-headless.x86_64                                     1:1.7.0.261-2.6.22.2.el7_8                                 base   
java-1.7.0-openjdk-javadoc.noarch                                      1:1.7.0.261-2.6.22.2.el7_8                                 base   
java-1.7.0-openjdk-src.x86_64                                          1:1.7.0.261-2.6.22.2.el7_8                                 base   
java-1.8.0-openjdk.i686                                                1:1.8.0.342.b07-1.el7_9                                    updates
java-1.8.0-openjdk.x86_64                                              1:1.8.0.342.b07-1.el7_9                                    updates
java-1.8.0-openjdk-accessibility.i686                                  1:1.8.0.342.b07-1.el7_9                                    updates
java-1.8.0-openjdk-accessibility.x86_64                                1:1.8.0.342.b07-1.el7_9                                    updates
java-1.8.0-openjdk-demo.i686                                           1:1.8.0.342.b07-1.el7_9                                    updates
java-1.8.0-openjdk-demo.x86_64                                         1:1.8.0.342.b07-1.el7_9                                    updates
java-1.8.0-openjdk-devel.i686                                          1:1.8.0.342.b07-1.el7_9                                    updates
java-1.8.0-openjdk-devel.x86_64                                        1:1.8.0.342.b07-1.el7_9                                    updates
java-1.8.0-openjdk-headless.i686                                       1:1.8.0.342.b07-1.el7_9                                    updates
java-1.8.0-openjdk-headless.x86_64                                     1:1.8.0.342.b07-1.el7_9                                    updates
java-1.8.0-openjdk-javadoc.noarch                                      1:1.8.0.342.b07-1.el7_9                                    updates
java-1.8.0-openjdk-javadoc-zip.noarch                                  1:1.8.0.342.b07-1.el7_9                                    updates
java-1.8.0-openjdk-src.i686                                            1:1.8.0.342.b07-1.el7_9                                    updates
java-1.8.0-openjdk-src.x86_64                                          1:1.8.0.342.b07-1.el7_9                                    updates
java-11-openjdk.i686                                                   1:11.0.16.0.8-1.el7_9                                      updates
java-11-openjdk.x86_64                                                 1:11.0.16.0.8-1.el7_9                                      updates
java-11-openjdk-demo.i686                                              1:11.0.16.0.8-1.el7_9                                      updates
java-11-openjdk-demo.x86_64                                            1:11.0.16.0.8-1.el7_9                                      updates
java-11-openjdk-devel.i686                                             1:11.0.16.0.8-1.el7_9                                      updates
java-11-openjdk-devel.x86_64                                           1:11.0.16.0.8-1.el7_9                                      updates
java-11-openjdk-headless.i686                                          1:11.0.16.0.8-1.el7_9                                      updates
java-11-openjdk-headless.x86_64                                        1:11.0.16.0.8-1.el7_9                                      updates
java-11-openjdk-javadoc.i686                                           1:11.0.16.0.8-1.el7_9                                      updates
java-11-openjdk-javadoc.x86_64                                         1:11.0.16.0.8-1.el7_9                                      updates
java-11-openjdk-javadoc-zip.i686                                       1:11.0.16.0.8-1.el7_9                                      updates
java-11-openjdk-javadoc-zip.x86_64                                     1:11.0.16.0.8-1.el7_9                                      updates
java-11-openjdk-jmods.i686                                             1:11.0.16.0.8-1.el7_9                                      updates
java-11-openjdk-jmods.x86_64                                           1:11.0.16.0.8-1.el7_9                                      updates
java-11-openjdk-src.i686                                               1:11.0.16.0.8-1.el7_9                                      updates
java-11-openjdk-src.x86_64                                             1:11.0.16.0.8-1.el7_9                                      updates
java-11-openjdk-static-libs.i686                                       1:11.0.16.0.8-1.el7_9                                      updates
java-11-openjdk-static-libs.x86_64                                     1:11.0.16.0.8-1.el7_9                                      updates
java-atk-wrapper.i686                                                  0.30.4-5.el7                                               base   
java-atk-wrapper.x86_64                                                0.30.4-5.el7                                               base   
java_cup.noarch                                                        1:0.11a-16.el7                                             base   
java_cup-javadoc.noarch                                                1:0.11a-16.el7                                             base   
java_cup-manual.noarch                                                 1:0.11a-16.el7                                             base   
javacc.noarch                                                          5.0-10.el7                                                 base   
javacc-demo.noarch                                                     5.0-10.el7                                                 base   
javacc-javadoc.noarch                                                  5.0-10.el7                                                 base   
javacc-manual.noarch                                                   5.0-10.el7                                                 base   
javacc-maven-plugin.noarch                                             2.6-17.el7                                                 base   
javacc-maven-plugin-javadoc.noarch                                     2.6-17.el7                                                 base   
javamail.noarch                                                        1.4.6-8.el7                                                base   
javamail-javadoc.noarch                                                1.4.6-8.el7                                                base   
javapackages-tools.noarch                                              3.4.1-11.el7                                               base   
javassist.noarch                                                       3.16.1-10.el7                                              base   
javassist-javadoc.noarch                                               3.16.1-10.el7                                              base  
```

## 安装java-1.8.0-openjdk
```shell
[root@centos7full ~]# yum install java-1.8.0-openjdk*
```

## 查看java环境
```shell
[root@centos7full ~]# java -version
openjdk version "1.8.0_342"
OpenJDK Runtime Environment (build 1.8.0_342-b07)
OpenJDK 64-Bit Server VM (build 25.342-b07, mixed mode)
```
