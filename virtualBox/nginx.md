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