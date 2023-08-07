# VirtualBox AnolisOS8虚拟机环境下安装gitlab

## 添加yum源

```shell
[root@localhost ~]# curl 'https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/config_file.repo?os=centos&dist=8&source=script' --header 'User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/107.0.0.0 Safari/537.36' > /etc/yum.repos.d/gitlab_gitlab-ee.repo
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   773  100   773    0     0    593      0  0:00:01  0:00:01 --:--:--   593
```

## 更新yum源

```shell
[root@localhost ~]# yum -y makecache 
AnolisOS-8 - AppStream                                                                                                                    8.5 kB/s | 4.3 kB     00:00    
AnolisOS-8 - BaseOS                                                                                                                        16 kB/s | 4.3 kB     00:00    
AnolisOS-8 - Extras                                                                                                                        11 kB/s | 3.0 kB     00:00    
AnolisOS-8 - PowerTools                                                                                                                   4.4 kB/s | 4.3 kB     00:00    
gitlab_gitlab-ee                                                                                                                          242  B/s | 862  B     00:03    
gitlab_gitlab-ee                                                                                                                          3.2 kB/s | 3.1 kB     00:00    
导入 GPG 公钥 0x51312F3F:
 Userid: "GitLab B.V. (package repository signing key) <packages@gitlab.com>"
 指纹: F640 3F65 44A3 8863 DAA0 B6E0 3F01 618A 5131 2F3F
 来自: https://packages.gitlab.com/gitlab/gitlab-ee/gpgkey
gitlab_gitlab-ee                                                                                                                          3.1 kB/s | 4.6 kB     00:01    
导入 GPG 公钥 0xF27EAB47:
 Userid: "GitLab, Inc. <support@gitlab.com>"
 指纹: DBEF 8977 4DDB 9EB3 7D9F C3A0 3CFC F9BA F27E AB47
 来自: https://packages.gitlab.com/gitlab/gitlab-ee/gpgkey/gitlab-gitlab-ee-3D645A26AB9FBD22.pub.gpg
gitlab_gitlab-ee                                                                                                                          307 kB/s | 1.9 MB     00:06    
gitlab_gitlab-ee-source                                                                                                                   238  B/s | 862  B     00:03    
gitlab_gitlab-ee-source                                                                                                                   3.1 kB/s | 3.1 kB     00:01    
导入 GPG 公钥 0x51312F3F:
 Userid: "GitLab B.V. (package repository signing key) <packages@gitlab.com>"
 指纹: F640 3F65 44A3 8863 DAA0 B6E0 3F01 618A 5131 2F3F
 来自: https://packages.gitlab.com/gitlab/gitlab-ee/gpgkey
gitlab_gitlab-ee-source                                                                                                                   2.9 kB/s | 4.6 kB     00:01    
导入 GPG 公钥 0xF27EAB47:
 Userid: "GitLab, Inc. <support@gitlab.com>"
 指纹: DBEF 8977 4DDB 9EB3 7D9F C3A0 3CFC F9BA F27E AB47
 来自: https://packages.gitlab.com/gitlab/gitlab-ee/gpgkey/gitlab-gitlab-ee-3D645A26AB9FBD22.pub.gpg
gitlab_gitlab-ee-source                                                                                                                    53  B/s | 296  B     00:05    
元数据缓存已建立。
```

## 安装gitlab-ee

```shell
[root@localhost ~]# yum -y install gitlab-ee
上次元数据过期检查：0:01:30 前，执行于 2023年08月03日 星期四 10时37分24秒。
依赖关系解决。
==========================================================================================================================================================================
 软件包                                         架构                     版本                                                    仓库                                大小
==========================================================================================================================================================================
安装:
 gitlab-ee                                      x86_64                   16.2.2-ee.0.el8                                         gitlab_gitlab-ee                   1.4 G
升级:
 perl-Encode                                    x86_64                   4:2.97-3.0.1.an8                                        BaseOS                             1.4 M
安装依赖关系:
 dwz                                            x86_64                   0.14-3.an8                                              AppStream                          131 k
 ......
 systemtap-sdt-devel                            x86_64                   4.8-2.an8                                               AppStream                           87 k
安装弱的依赖:
 perl-Encode-Locale                             noarch                   1.05-10.module+an8.7.0+10932+615184cb                   AppStream                           20 k
 perl-TermReadKey                               x86_64                   2.37-7.0.1.an8                                          AppStream                           33 k

事务概要
==========================================================================================================================================================================
安装  114 软件包
升级    1 软件包

总下载：1.4 G
下载软件包：
(1/115): efi-srpm-macros-3-3.0.3.an8.noarch.rpm                                                                                            48 kB/s |  21 kB     00:00    
......
(115/115): gitlab-ee-16.2.2-ee.0.el8.x86_64.rpm                                                                                           3.3 MB/s | 1.4 GB     07:06    
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------
总计                                                                                                                                      3.2 MB/s | 1.4 GB     07:13     
gitlab_gitlab-ee                                                                                                                          3.3 kB/s | 3.1 kB     00:00    
导入 GPG 公钥 0x51312F3F:
 Userid: "GitLab B.V. (package repository signing key) <packages@gitlab.com>"
 指纹: F640 3F65 44A3 8863 DAA0 B6E0 3F01 618A 5131 2F3F
 来自: https://packages.gitlab.com/gitlab/gitlab-ee/gpgkey
导入公钥成功
gitlab_gitlab-ee                                                                                                                          3.1 kB/s | 4.6 kB     00:01    
导入 GPG 公钥 0xF27EAB47:
 Userid: "GitLab, Inc. <support@gitlab.com>"
 指纹: DBEF 8977 4DDB 9EB3 7D9F C3A0 3CFC F9BA F27E AB47
 来自: https://packages.gitlab.com/gitlab/gitlab-ee/gpgkey/gitlab-gitlab-ee-3D645A26AB9FBD22.pub.gpg
导入公钥成功
运行事务检查
事务检查成功。
运行事务测试
事务测试成功。
运行事务
  准备中  :                                                                                                                                                           1/1 
  升级    : perl-Encode-4:2.97-3.0.1.an8.x86_64                                                                                                                     1/116 
 ......
  安装    : make-1:4.2.1-11.0.1.an8.x86_64                                                                                                                         51/116 
  运行脚本: make-1:4.2.1-11.0.1.an8.x86_64                                                                                                                         51/116 
  ......
  运行脚本: gitlab-ee-16.2.2-ee.0.el8.x86_64                                                                                                                      115/116 
  安装    : gitlab-ee-16.2.2-ee.0.el8.x86_64                                                                                                                      115/116 
  运行脚本: gitlab-ee-16.2.2-ee.0.el8.x86_64                                                                                                                      115/116 
  清理    : perl-Encode-4:2.97-3.el8.x86_64                                                                                                                       116/116 
  运行脚本: gitlab-ee-16.2.2-ee.0.el8.x86_64                                                                                                                      116/116 
It looks like GitLab has not been configured yet; skipping the upgrade script.

       *.                  *.
      ***                 ***
     *****               *****
    .******             *******
    ********            ********
   ,,,,,,,,,***********,,,,,,,,,
  ,,,,,,,,,,,*********,,,,,,,,,,,
  .,,,,,,,,,,,*******,,,,,,,,,,,,
      ,,,,,,,,,*****,,,,,,,,,.
         ,,,,,,,****,,,,,,
            .,,,***,,,,
                ,*,.
  


     _______ __  __          __
    / ____(_) /_/ /   ____ _/ /_
   / / __/ / __/ /   / __ `/ __ \
  / /_/ / / /_/ /___/ /_/ / /_/ /
  \____/_/\__/_____/\__,_/_.___/
  

Thank you for installing GitLab!
GitLab was unable to detect a valid hostname for your instance.
Please configure a URL for your GitLab instance by setting `external_url`
configuration in /etc/gitlab/gitlab.rb file.
Then, you can start your GitLab instance by running the following command:
  sudo gitlab-ctl reconfigure

For a comprehensive list of configuration options please see the Omnibus GitLab readme
https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/README.md

Help us improve the installation experience, let us know how we did with a 1 minute survey:
https://gitlab.fra1.qualtrics.com/jfe/form/SV_6kVqZANThUQ1bZb?installation=omnibus&release=16-2


  运行脚本: perl-Encode-4:2.97-3.el8.x86_64                                                                                                                       116/116 
  验证    : dwz-0.14-3.an8.x86_64                                                                                                                                   1/116 
  ......
  验证    : perl-Encode-4:2.97-3.el8.x86_64                                                                                                                       116/116 

已升级:
  perl-Encode-4:2.97-3.0.1.an8.x86_64                                                                                                                                     
已安装:
  dwz-0.14-3.an8.x86_64                                                                    efi-srpm-macros-3-3.0.3.an8.noarch                                            
  ......                                       
  system-rpm-config-129-1.0.2.an8.noarch                                                   systemtap-sdt-devel-4.8-2.an8.x86_64                                          

完毕！
```

### 修改访问gitlab的url, 配置 `external_url`参数
```shell
[root@localhost ~]# vim /etc/gitlab/gitlab.rb
```

### 使配置生效

```shell
[root@localhost ~]# gitlab-ctl reconfigure
```

## 安装时指定url

```shell
[root@localhost ~]# EXTERNAL_URL="http://192.168.4.180" yum install -y gitlab-ee
```


# VirtualBox AnolisOS8虚拟机环境下安装指定版本gitlab

## 下载安装包

```shell
[root@localhost ~]# wget --content-disposition https://packages.gitlab.com/gitlab/gitlab-ee/packages/el/8/gitlab-ee-14.6.1-ee.0.el8.x86_64.rpm/download.rpm
--2023-08-04 09:38:04--  http://wget/
正在解析主机 wget (wget)... 失败：未知的名称或服务。
wget: 无法解析主机地址 “wget”
--2023-08-04 09:38:04--  https://packages.gitlab.com/gitlab/gitlab-ee/packages/el/8/gitlab-ee-14.6.1-ee.0.el8.x86_64.rpm/download.rpm
正在解析主机 packages.gitlab.com (packages.gitlab.com)... 104.18.20.224, 104.18.21.224, 2606:4700::6812:14e0, ...
正在连接 packages.gitlab.com (packages.gitlab.com)|104.18.20.224|:443... 已连接。
已发出 HTTP 请求，正在等待回应... 302 Found
位置：https://d20rj4el6vkp4c.cloudfront.net/7/11/el/8/package_files/78137.rpm?t=1691113385_1fabb6aff7e359fe6a16219c85a8c685a9236b21 [跟随至新的 URL]
--2023-08-04 09:38:05--  https://d20rj4el6vkp4c.cloudfront.net/7/11/el/8/package_files/78137.rpm?t=1691113385_1fabb6aff7e359fe6a16219c85a8c685a9236b21
正在解析主机 d20rj4el6vkp4c.cloudfront.net (d20rj4el6vkp4c.cloudfront.net)... 99.84.224.205, 99.84.224.10, 99.84.224.114, ...
正在连接 d20rj4el6vkp4c.cloudfront.net (d20rj4el6vkp4c.cloudfront.net)|99.84.224.205|:443... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度：1063028270 (1014M) [application/x-rpm]
正在保存至: “gitlab-ee-14.6.1-ee.0.el8.x86_64.rpm”

gitlab-ee-14.6.1-ee.0.el8.x86_64.rpm                   100%[==========================================================================================================================>]   1014M  1.83MB/s  用时 4m 41s  

2023-08-04 09:42:47 (3.61 MB/s) - 已保存 “gitlab-ee-14.6.1-ee.0.el8.x86_64.rpm” [1063028270/1063028270])

下载完毕 --2023-08-04 09:42:47--
总用时：4m 43s
下载了：1 个文件，4m 41s (3.61 MB/s) 中的 1014M
```

## 安装postgresql(如果没装的话)

```shell
[root@localhost ~]# yum -y install postgresql
上次元数据过期检查：2:21:40 前，执行于 2023年08月04日 星期五 07时21分22秒。
依赖关系解决。
==========================================================================================================================================================================================================================
 软件包                                        架构                                      版本                                                                          仓库                                          大小
==========================================================================================================================================================================================================================
安装:
 postgresql                                    x86_64                                    10.23-1.0.1.module+an8.8.0+11076+d3425ca4                                     AppStream                                    1.5 M
安装依赖关系:
 libpq                                         x86_64                                    13.5-1.0.1.an8                                                                AppStream                                    197 k
启用模块流:
 postgresql                                                                              10                                                                                                                              

事务概要
==========================================================================================================================================================================================================================
安装  2 软件包

总下载：1.7 M
安装大小：6.2 M
下载软件包：
(1/2): libpq-13.5-1.0.1.an8.x86_64.rpm                                                                                                                                                    194 kB/s | 197 kB     00:01    
(2/2): postgresql-10.23-1.0.1.module+an8.8.0+11076+d3425ca4.x86_64.rpm                                                                                                                    1.0 MB/s | 1.5 MB     00:01    
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
总计                                                                                                                                                                                      1.1 MB/s | 1.7 MB     00:01     
AnolisOS-8 - AppStream                                                                                                                                                                    278 kB/s | 3.0 kB     00:00    
导入 GPG 公钥 0x4873F7C5:
 Userid: "Anolis OS <os@openanolis.org>"
 指纹: F87D B652 2967 855E 986F 840C 6191 4008 4873 F7C5
 来自: /etc/pki/rpm-gpg/RPM-GPG-KEY-ANOLIS
导入公钥成功
运行事务检查
事务检查成功。
运行事务测试
事务测试成功。
运行事务
  准备中  :                                                                                                                                                                                                           1/1 
  安装    : libpq-13.5-1.0.1.an8.x86_64                                                                                                                                                                               1/2 
  安装    : postgresql-10.23-1.0.1.module+an8.8.0+11076+d3425ca4.x86_64                                                                                                                                               2/2 
  运行脚本: postgresql-10.23-1.0.1.module+an8.8.0+11076+d3425ca4.x86_64                                                                                                                                               2/2 
  验证    : libpq-13.5-1.0.1.an8.x86_64                                                                                                                                                                               1/2 
  验证    : postgresql-10.23-1.0.1.module+an8.8.0+11076+d3425ca4.x86_64                                                                                                                                               2/2 

已安装:
  libpq-13.5-1.0.1.an8.x86_64                                                                 postgresql-10.23-1.0.1.module+an8.8.0+11076+d3425ca4.x86_64                                                                

完毕！
```

### 安装gitlab

> 安装过程可能超时退出, 重新安装一次就好

```shell
[root@localhost ~]# EXTERNAL_URL="http://192.168.4.180" rpm -ivh gitlab-ee-14.6.1-ee.0.el8.x86_64.rpm
警告：gitlab-ee-14.6.1-ee.0.el8.x86_64.rpm: 头V4 RSA/SHA256 Signature, 密钥 ID f27eab47: NOKEY
Verifying...                          ################################# [100%]
准备中...                          ################################# [100%]
正在升级/安装...
   1:gitlab-ee-14.6.1-ee.0.el8        ################################# [100%]
Starting Chef Infra Client, version 15.17.4
resolving cookbooks for run list: ["gitlab-ee"]
Synchronizing Cookbooks:
  - gitlab-ee (0.0.1)
  - patroni (0.1.0)
  - runit (5.1.3)
  - logrotate (0.1.0)
  - redis (0.1.0)
  - gitlab (0.0.1)
  - package (0.1.0)
  - consul (0.1.0)
  - pgbouncer (0.1.0)
  - registry (0.1.0)
  - gitaly (0.1.0)
  - praefect (0.1.0)
  - gitlab-kas (0.1.0)
  - gitlab-pages (0.1.0)
  - nginx (0.1.0)
  - acme (4.1.4)
  - crond (0.1.0)
  - postgresql (0.1.0)
  - mattermost (0.1.0)
  - letsencrypt (0.1.0)
  - monitoring (0.1.0)
Installing Cookbook Gems:
Compiling Cookbooks...
Recipe: gitlab::default
......
      软件包 gitlab-ee-14.6.1-ee.0.el8.x86_64 已经安装
```

