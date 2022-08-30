# VirtualBox CentOs7虚拟机环境下安装mysql8.0.30

## `rpm`导入mysql源

```shell
rpm -Uvh https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm
```

导入源后会在`/etc/yum.repos.d/`目录下生成 `mysql-community.repo` 和 `mysql-community-source.repo`

```shell
[root@centos7full ~]# ls /etc/yum.repos.d
CentOS-Base.repo       CentOS-Media.repo          mysql-community.repo
CentOS-CR.repo         CentOS-Sources.repo        mysql-community-source.repo
CentOS-Debuginfo.repo  CentOS-Vault.repo
CentOS-fasttrack.repo  CentOS-x86_64-kernel.repo
```

## 用`yum`命令安装mysql

```shell
yum --enablerepo=mysql80-community install mysql-community-server
```

### 安装失败,提示公钥尚未安装,类似一下提示

![](./images/微信截图_20220830110702.png)

#### 安装公钥
```shell
rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
```

## 重新安装
```shell
yum --enablerepo=mysql80-community install mysql-community-server
```

## 设置开机启动, 启动mysql, 查看mysql状态
```shell
systemctl enable mysqld
```
```shell
systemctl start mysqld
```
```shell
systemctl status mysqld
```
![](./images/微信截图_20220830111827.png)

## 查看安装后自动生成的`root`账户密码
```shell
grep "A temporary password" /var/log/mysqld.log
```
```shell
2022-08-30T03:15:42.457238Z 6 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: bJw!pA<k?5wW
```

## 用临时密码登录mysql
```mysql
[root@centos7full ~]# mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.30

Copyright (c) 2000, 2022, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
```

## 修改`root'账户默认密码
```mysql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'new password';
```
### 如果密码太简单会报错,提示新密码不满足密码验证策略
```mysql
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
```
处理方法: 1.修改密码,适应密码验证策略;2.修改密码验证策略,适应简单密码, 但是貌似修改策略也要先重置密码, 那就先设个复杂的密码,再改下策略,然后再改个简单的密码吧;

### 查看密码验证策略,但是重置密码前该命令用不了
```mysql
mysql> SHOW VARIABLES LIKE 'validate_password.%';
+--------------------------------------+--------+
| Variable_name                        | Value  |
+--------------------------------------+--------+
| validate_password.check_user_name    | ON     |
| validate_password.dictionary_file    |        |
| validate_password.length             | 8      |
| validate_password.mixed_case_count   | 1      |
| validate_password.number_count       | 1      |
| validate_password.policy             | MEDIUM |
| validate_password.special_char_count | 1      |
+--------------------------------------+--------+
7 rows in set (0.01 sec)

```

我用的密码比较简单,所以只修改下密码复杂度和密码长度
#### 密码复杂度`validate_password.policy`,设置为0
```mysql
set global validate_password.policy=0;
```

#### 密码长度`validate_password.length`,设置为4
```mysql
set global validate_password.length=4;
```

#### 改个简单点的密码
```mysql
ALTER USER 'root'@'localhost' IDENTIFIED BY '1234';
```

##配置ssh访问 

###查看mysql用户
```mysql
mysql> use mysql;
mysql> select host, user from user;
+-----------+------------------+
| host      | user             |
+-----------+------------------+
| localhost | mysql.infoschema |
| localhost | mysql.session    |
| localhost | mysql.sys        |
| localhost | root             |
+-----------+------------------+
4 rows in set (0.00 sec)
```
可以看到`root`用户的`host`为`localhost`, 意思`root`用户只能本地访问,将其设置为`%`;

```mysql
mysql> update user set host='%' where user='root';
Query OK, 1 row affected (0.01 sec)
Rows matched: 1  Changed: 1  Warnings: 0
```

刷新权限
```mysql
mysql> FLUSH PRIVILEGES;
```

再次查看
```mysql
mysql> select host, user from user;
+-----------+------------------+
| host      | user             |
+-----------+------------------+
| %         | root             |
| localhost | mysql.infoschema |
| localhost | mysql.session    |
| localhost | mysql.sys        |
+-----------+------------------+
4 rows in set (0.00 sec)
```

###防火墙开放`3306`端口
```shell
[root@centos7full ~]# firewall-cmd --zone=public --add-port=3306/tcp --permanent
success
```

###重启防火墙
```shell
systemctl restart firewalld.service
```

###Navicat连接mysql报错

![](./images/微信截图_20220830125211.png)

##Navicat连接mysql报错处理

原因: mysql8的加密方式规则不一样，是 `caching_sha2_password`, 把加密方式改成 `mysql_native_password` 就行了

```mysql
mysql> use mysql;
mysql> select host, user, plugin from user;
+-----------+------------------+-----------------------+
| host      | user             | plugin                |
+-----------+------------------+-----------------------+
| %         | root             | caching_sha2_password |
| localhost | mysql.infoschema | caching_sha2_password |
| localhost | mysql.session    | caching_sha2_password |
| localhost | mysql.sys        | caching_sha2_password |
+-----------+------------------+-----------------------+
4 rows in set (0.00 sec)
```

###修改`root`账户的加密方式
```mysql
ALTER USER '[用户名]'@'%' IDENTIFIED WITH mysql_native_password BY '[密码]';
```

```mysql
mysql> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '1234';
Query OK, 0 rows affected (0.01 sec)
```

```mysql
mysql> select host, user, plugin from user;
+-----------+------------------+-----------------------+
| host      | user             | plugin                |
+-----------+------------------+-----------------------+
| %         | root             | mysql_native_password |
| localhost | mysql.infoschema | caching_sha2_password |
| localhost | mysql.session    | caching_sha2_password |
| localhost | mysql.sys        | caching_sha2_password |
+-----------+------------------+-----------------------+
4 rows in set (0.00 sec)
```

密码简单的话可能报密码策略验证失败的错误

```mysql
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
```

前面已经修改了策略, 不知道这里为什么又报这个错,查看下策略,又变成默认的了
```mysql
mysql> SHOW VARIABLES LIKE 'validate_password.%';
+--------------------------------------+--------+
| Variable_name                        | Value  |
+--------------------------------------+--------+
| validate_password.check_user_name    | ON     |
| validate_password.dictionary_file    |        |
| validate_password.length             | 8      |
| validate_password.mixed_case_count   | 1      |
| validate_password.number_count       | 1      |
| validate_password.policy             | MEDIUM |
| validate_password.special_char_count | 1      |
+--------------------------------------+--------+
7 rows in set (0.01 sec)
```

按上面改密码验证策略的方法再改一次密码验证策略
```mysql
set global validate_password.length=4;
set global validate_password.policy=0;
```

再次查看密码验证策略
```mysql
mysql> SHOW VARIABLES LIKE 'validate_password.%';
+--------------------------------------+-------+
| Variable_name                        | Value |
+--------------------------------------+-------+
| validate_password.check_user_name    | ON    |
| validate_password.dictionary_file    |       |
| validate_password.length             | 4     |
| validate_password.mixed_case_count   | 1     |
| validate_password.number_count       | 1     |
| validate_password.policy             | LOW   |
| validate_password.special_char_count | 1     |
+--------------------------------------+-------+
7 rows in set (0.01 sec)
```

再次修改账户加密方式
```mysql
mysql> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '1234';
Query OK, 0 rows affected (0.01 sec)
```

Navicat连接下试试,成功了!!

![](./images/微信截图_20220830131012.png)