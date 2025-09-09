# VirtualBox CentOs7虚拟机环境下安装nginx

## 安装`yum-utils`
```shell
yum install yum-utils
```

## 配置源,创建`nginx.repo`文件,路径`/etc/yum.repos.d/nginx.repo`

```shell
[root@centos7full ~]# cat > /etc/yum.repos.d/nginx.repo <<EOF
> [nginx]
> name=nginx stable repo
> baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
> gpgcheck=1
> enabled=1
> gpgkey=https://nginx.org/keys/nginx_signing.key
> module_hotfixes=true
> EOF
```

当然也可以配置多个源

```
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
```

## 查看`$releasever`和`$basearch`
```shell
[root@centos7full ~]# rpm -qi centos-release
Name        : centos-release
Version     : 7
Release     : 9.2009.1.el7.centos
Architecture: x86_64
Install Date: 2022年08月29日 星期一 08时42分52秒
Group       : System Environment/Base
Size        : 44787
License     : GPLv2
Signature   : RSA/SHA256, 2020年12月03日 星期四 00时35分28秒, Key ID 24c6a8a7f4a80eb5
Source RPM  : centos-release-7-9.2009.1.el7.centos.src.rpm
Build Date  : 2020年11月23日 星期一 23时08分41秒
Build Host  : x86-01.bsys.centos.org
Relocations : (not relocatable)
Packager    : CentOS BuildSystem <http://bugs.centos.org>
Vendor      : CentOS
Summary     : CentOS Linux release file
Description :
CentOS Linux release files
```

## 切换源,如上配置多个源,默认情况下，使用`stable`源。如果希望使用`mainline`源，使用以下命令切换:

```shell
yum-config-manager --enable nginx-mainline
```

## 安装nginx

```shell
yum install nginx
```

## 配置nginx开机启动
```shell
[root@centos7full ~]# systemctl enable nginx
Created symlink from /etc/systemd/system/multi-user.target.wants/nginx.service to /usr/lib/systemd/system/nginx.service.
```

## 启动nginx
```shell
systemctl start nginx
```

## 直接访问可能会失败, 查看防火墙是否开放`80`端口
```shell
[root@centos7full ~]# firewall-cmd --zone=public --list-ports
3306/tcp
```
查看所有开放的端口, `80`端口未开放

### 防火墙开放`80`端口
```shell
[root@centos7full ~]# firewall-cmd --zone=public --add-port=80/tcp --permanent
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
3306/tcp 80/tcp
```

再次访问,已经可以正常访问了!!

## 查看安装的nginx包含那些模块, 注意参数是大写`V`,小写`v`只会列出nginx版本号
```shell
[root@centos7full ~]# nginx -V
nginx version: nginx/1.22.0
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-44) (GCC) 
built with OpenSSL 1.0.2k-fips  26 Jan 2017
TLS SNI support enabled
configure arguments: --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -fPIC' --with-ld-opt='-Wl,-z,relro -Wl,-z,now -pie'
```

# CentOs6.9环境下安装nginx

## CentOS 6 已经结束了生命周期（EOL），因此官方和许多镜像站点不再提供对其的支持. 重新配置源

/etc/yum.repos.d/epel.repo
```shell
[epel]
name=Extra Packages for Enterprise Linux 6 - $basearch
enabled=0
failovermethod=priority
baseurl=http://mirrors.cloud.aliyuncs.com/epel/6/$basearch
gpgcheck=0
gpgkey=http://mirrors.cloud.aliyuncs.com/epel/RPM-GPG-KEY-EPEL-6
```

/etc/yum.repos.d/nginx.repo
```shell
[nginx]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
```

## 更新源

```shell
[root@iZuf61hp5ebl72mk8ifn72Z ~]# sudo yum clean all
Loaded plugins: fastestmirror
Cleaning repos: base extras nginx updates
Cleaning up Everything
Cleaning up list of fastest mirrors
[root@iZuf61hp5ebl72mk8ifn72Z ~]# sudo yum makecache
Loaded plugins: fastestmirror
Determining fastest mirrors
base                                                                                                                     | 3.7 kB     00:00     
base/group_gz                                                                                                            | 242 kB     00:00     
base/filelists_db                                                                                                        | 6.4 MB     00:00     
base/primary_db                                                                                                          | 4.7 MB     00:00     
base/other_db                                                                                                            | 2.8 MB     00:00     
extras                                                                                                                   | 3.4 kB     00:00     
extras/filelists_db                                                                                                      |  24 kB     00:00     
extras/prestodelta                                                                                                       | 2.2 kB     00:00     
extras/primary_db                                                                                                        |  29 kB     00:00     
extras/other_db                                                                                                          |  14 kB     00:00     
nginx                                                                                                                    | 2.9 kB     00:00     
nginx/filelists_db                                                                                                       |  75 kB     00:00     
nginx/primary_db                                                                                                         |  64 kB     00:00     
nginx/other_db                                                                                                           |  35 kB     00:00     
updates                                                                                                                  | 3.4 kB     00:00     
updates/filelists_db                                                                                                     | 8.4 MB     00:00     
updates/prestodelta                                                                                                      | 357 kB     00:00     
updates/primary_db                                                                                                       |  12 MB     00:00     
updates/other_db                                                                                                         | 479 kB     00:00     
Metadata Cache Created
[root@iZuf61hp5ebl72mk8ifn72Z ~]# yum search nginx
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
============================================================== N/S Matched: nginx ==============================================================
nginx-debug.x86_64 : debug version of nginx
nginx-debuginfo.x86_64 : Debug information for package nginx
nginx-module-geoip.x86_64 : nginx geoip module
nginx-module-geoip-debuginfo.x86_64 : Debug information for package nginx-module-geoip
nginx-module-image-filter.x86_64 : nginx image filter module
nginx-module-image-filter-debuginfo.x86_64 : Debug information for package nginx-module-image-filter
nginx-module-njs.x86_64 : nginx nJScript module
nginx-module-njs-debuginfo.x86_64 : Debug information for package nginx-module-njs
nginx-module-perl.x86_64 : nginx perl module
nginx-module-perl-debuginfo.x86_64 : Debug information for package nginx-module-perl
nginx-module-xslt.x86_64 : nginx xslt module
nginx-module-xslt-debuginfo.x86_64 : Debug information for package nginx-module-xslt
nginx-nr-agent.noarch : New Relic agent for NGINX and NGINX Plus
pcp-pmda-nginx.x86_64 : Performance Co-Pilot (PCP) metrics for the Nginx Webserver
nginx.x86_64 : High performance web server

  Name and summary matches only, use "search all" for everything.
[root@iZuf61hp5ebl72mk8ifn72Z ~]# yum install nginx
Loaded plugins: fastestmirror
Setting up Install Process
Loading mirror speeds from cached hostfile
Resolving Dependencies
--> Running transaction check
---> Package nginx.x86_64 0:1.18.0-2.el6.ngx will be installed
--> Finished Dependency Resolution

Dependencies Resolved

================================================================================================================================================
 Package                        Arch                            Version                                    Repository                      Size
================================================================================================================================================
Installing:
 nginx                          x86_64                          1.18.0-2.el6.ngx                           nginx                          975 k

Transaction Summary
================================================================================================================================================
Install       1 Package(s)

Total download size: 975 k
Installed size: 2.6 M
Is this ok [y/N]: y
Downloading Packages:
nginx-1.18.0-2.el6.ngx.x86_64.rpm                                                                                        | 975 kB     00:01     
warning: rpmts_HdrFromFdno: Header V4 RSA/SHA1 Signature, key ID 7bd9bf62: NOKEY
Retrieving key from https://nginx.org/keys/nginx_signing.key
Importing GPG key 0xB49F6B46:
 Userid: "nginx signing key <signing-key-2@nginx.com>"
 From  : https://nginx.org/keys/nginx_signing.key
Is this ok [y/N]: y
Importing GPG key 0x7BD9BF62:
 Userid: "nginx signing key <signing-key@nginx.com>"
 From  : https://nginx.org/keys/nginx_signing.key
Is this ok [y/N]: y
Importing GPG key 0x8D88A2B3:
 Userid: "nginx signing key <signing-key-3@nginx.com>"
 From  : https://nginx.org/keys/nginx_signing.key
Is this ok [y/N]: y
Running rpm_check_debug
Running Transaction Test
Transaction Test Succeeded
Running Transaction
  Installing : nginx-1.18.0-2.el6.ngx.x86_64                                                                                                1/1 
----------------------------------------------------------------------

Thanks for using nginx!

Please find the official documentation for nginx here:
* http://nginx.org/en/docs/

Please subscribe to nginx-announce mailing list to get
the most important news about nginx:
* http://nginx.org/en/support.html

Commercial subscriptions for nginx are available on:
* http://nginx.com/products/

----------------------------------------------------------------------
  Verifying  : nginx-1.18.0-2.el6.ngx.x86_64                                                                                                1/1 

Installed:
  nginx.x86_64 0:1.18.0-2.el6.ngx                                                                                                               

Complete!

[root@iZuf61hp5ebl72mk8ifn72Z ~]# service nginx status
nginx is stopped
[root@iZuf61hp5ebl72mk8ifn72Z ~]# chkconfig nginx on
[root@iZuf61hp5ebl72mk8ifn72Z ~]# service nginx start
Starting nginx:                                            [  OK  ]
```

## 修改配置文件后重启nginx, 用命令重启失败, 先杀进程,然后启动
```shell
[root@iZuf61hp5ebl72mk8ifn72Z ~]# service nginx restart
Stopping nginx:                                            [FAILED]
Starting nginx: nginx: [emerg] bind() to 0.0.0.0:8001 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:8001 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:8001 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:8001 failed (98: Address already in use)
nginx: [emerg] bind() to 0.0.0.0:8001 failed (98: Address already in use)
nginx: [emerg] still could not bind()
                                                           [FAILED]
[root@iZuf61hp5ebl72mk8ifn72Z ~]# killall nginx
[root@iZuf61hp5ebl72mk8ifn72Z ~]# service nginx status
nginx is stopped
[root@iZuf61hp5ebl72mk8ifn72Z ~]# service nginx start
Starting nginx:                                            [  OK  ]
[root@iZuf61hp5ebl72mk8ifn72Z ~]# curl http://localhost:8001
heidelberg
```

## 定时删除nginx日志

### 编写定时任务脚本
/usr/local/bin/cleanup_nginx_logs.sh
```shell
#!/bin/bash

# Nginx日志路径
LOG_PATH="/var/log/nginx/"

# 删除7天前的日志文件
find $LOG_PATH -mtime +7 -type f -name *.log -exec rm -rf {} \;
```
### 添加定时任务
使用命令 `crontab -e`, 添加定时任务命令
```shell
0 0 * * 1 /usr/local/bin/cleanup_nginx_logs.sh
```

```shell
[root@iZuf61hp5ebl72mk8ifn72Z ~]# crontab -e
no crontab for root - using an empty one
crontab: installing new crontab
```

### 重启crond服务
```shell
[root@iZuf61hp5ebl72mk8ifn72Z ~]# service crond restart
Stopping crond:                                            [  OK  ]
Starting crond:                                            [  OK  ]
```
