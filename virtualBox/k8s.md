
# k8s(1.33)环境配置

## 设备信息

### master
> IP: 192.168.4.108   主机名: ares-master   系统: AnolisOS-8.10-x86_64
### slave
> IP: 192.168.4.109   主机名: ares-slave    系统: AnolisOS-8.10-x86_64
### slave1
> IP: 192.168.4.110   主机名: ares-slave1   系统: AnolisOS-8.10-x86_64

## 查看系统及内核版本 <k8s 1.33 要求系统的内核版本高于 5.13>

```shell
[ares@ares-master ~]$ uname -r
5.10.134-18.an8.x86_64
[ares@ares-master ~]$ cat /etc/anolis-release
Anolis OS release 8.10
```

## 使用 ELRepo 仓库升级内核
> 参照清华大学开源软件镜像站[https://mirrors.tuna.tsinghua.edu.cn/help/elrepo/]
### 导入 ELRepo 仓库的公共密钥
```shell
[ares@ares-master ~]$ sudo rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
```
### 安装对应版本的 ELRepo
```shell
[ares@ares-master ~]$ sudo yum install https://www.elrepo.org/elrepo-release-8.el8.elrepo.noarch.rpm
上次元数据过期检查：0:03:55 前，执行于 2025年07月07日 星期一 13时04分20秒。
elrepo-release-8.el8.elrepo.noarch.rpm                                                                                 12 kB/s |  19 kB     00:01    
依赖关系解决。
======================================================================================================================================================
 软件包                               架构                         版本                                      仓库                                大小
======================================================================================================================================================
安装:
 elrepo-release                       noarch                       8.4-2.el8.elrepo                          @commandline                        19 k

事务概要
======================================================================================================================================================
安装  1 软件包

总计：19 k
安装大小：8.3 k
确定吗？[y/N]： y
下载软件包：
运行事务检查
事务检查成功。
运行事务测试
事务测试成功。
运行事务
  准备中  :                                                                                                                                       1/1 
  安装    : elrepo-release-8.4-2.el8.elrepo.noarch                                                                                                1/1 
  验证    : elrepo-release-8.4-2.el8.elrepo.noarch                                                                                                1/1 

已安装:
  elrepo-release-8.4-2.el8.elrepo.noarch                                                                                                              

完毕！
```

### 备份 elrepo
```shell
[ares@ares-master ~]$ sudo cp /etc/yum.repos.d/elrepo.repo /etc/yum.repos.d/elrepo.repo.bak
```

### 编辑elrepo配置

然后编辑 /etc/yum.repos.d/elrepo.repo 文件，在 mirrorlist= 开头的行前面加 # 注释掉；并将 `http://elrepo.org/linux` 替换为`https://mirrors.tuna.tsinghua.edu.cn/elrepo`

### 重新建立 `yum`缓存
```shell
[ares@ares-master ~]$ sudo yum makecache
[sudo] ares 的密码：
AnolisOS-8 - AppStream                                                                                                 81 kB/s | 4.3 kB     00:00    
AnolisOS-8 - BaseOS                                                                                                    41 kB/s | 4.3 kB     00:00    
AnolisOS-8 - Extras                                                                                                    80 kB/s | 3.0 kB     00:00    
AnolisOS-8 - PowerTools                                                                                                93 kB/s | 4.3 kB     00:00    
AnolisOS-8 - Kernel 5.10                                                                                              119 kB/s | 4.3 kB     00:00    
ELRepo.org Community Enterprise Linux Repository - el8                                                                588 kB/s | 223 kB     00:00    
元数据缓存已建立。
```

### 查看可用内核
```shell
[ares@ares-master ~]$ sudo yum list available --disablerepo='*' --enablerepo=elrepo-kernel
ELRepo.org Community Enterprise Linux Kernel Repository - el8                                                         2.4 MB/s | 2.2 MB     00:00    
上次元数据过期检查：0:00:01 前，执行于 2025年07月07日 星期一 13时24分22秒。
可安装的软件包
kernel-lt.x86_64                                                           5.4.295-1.el8.elrepo                                          elrepo-kernel
kernel-lt-core.x86_64                                                      5.4.295-1.el8.elrepo                                          elrepo-kernel
kernel-lt-devel.x86_64                                                     5.4.295-1.el8.elrepo                                          elrepo-kernel
kernel-lt-doc.noarch                                                       5.4.295-1.el8.elrepo                                          elrepo-kernel
kernel-lt-headers.x86_64                                                   5.4.295-1.el8.elrepo                                          elrepo-kernel
kernel-lt-modules.x86_64                                                   5.4.295-1.el8.elrepo                                          elrepo-kernel
kernel-lt-modules-extra.x86_64                                             5.4.295-1.el8.elrepo                                          elrepo-kernel
kernel-lt-tools.x86_64                                                     5.4.295-1.el8.elrepo                                          elrepo-kernel
kernel-lt-tools-libs.x86_64                                                5.4.295-1.el8.elrepo                                          elrepo-kernel
kernel-lt-tools-libs-devel.x86_64                                          5.4.295-1.el8.elrepo                                          elrepo-kernel
kernel-ml.x86_64                                                           6.15.4-1.el8.elrepo                                           elrepo-kernel
kernel-ml-core.x86_64                                                      6.15.4-1.el8.elrepo                                           elrepo-kernel
kernel-ml-devel.x86_64                                                     6.15.4-1.el8.elrepo                                           elrepo-kernel
kernel-ml-doc.noarch                                                       6.15.4-1.el8.elrepo                                           elrepo-kernel
kernel-ml-headers.x86_64                                                   6.15.4-1.el8.elrepo                                           elrepo-kernel
kernel-ml-modules.x86_64                                                   6.15.4-1.el8.elrepo                                           elrepo-kernel
kernel-ml-modules-extra.x86_64                                             6.15.4-1.el8.elrepo                                           elrepo-kernel
kernel-ml-tools.x86_64                                                     6.15.4-1.el8.elrepo                                           elrepo-kernel
kernel-ml-tools-libs.x86_64                                                6.15.4-1.el8.elrepo                                           elrepo-kernel
kernel-ml-tools-libs-devel.x86_64                                          6.15.4-1.el8.elrepo                                           elrepo-kernel
perf.x86_64                                                                6.15.4-1.el8.elrepo                                           elrepo-kernel
python3-perf.x86_64  
```

> 常见选项：
kernel-ml: 最新主线稳定版（推荐）
kernel-lt: 长期维护版

### 安装 `kernel-ml`最新版内核
```shell
[ares@ares-master ~]$ sudo yum install --enablerepo=elrepo-kernel kernel-ml
[sudo] ares 的密码：
上次元数据过期检查：0:05:05 前，执行于 2025年07月07日 星期一 13时24分22秒。
依赖关系解决。
======================================================================================================================================================
 软件包                                 架构                        版本                                     仓库                                大小
======================================================================================================================================================
安装:
 kernel-ml                              x86_64                      6.15.4-1.el8.elrepo                      elrepo-kernel                      154 k
安装依赖关系:
 kernel-ml-core                         x86_64                      6.15.4-1.el8.elrepo                      elrepo-kernel                       67 M
 kernel-ml-modules                      x86_64                      6.15.4-1.el8.elrepo                      elrepo-kernel                       62 M

事务概要
======================================================================================================================================================
安装  3 软件包

总下载：129 M
安装大小：175 M
确定吗？[y/N]： y
下载软件包：
(1/3): kernel-ml-6.15.4-1.el8.elrepo.x86_64.rpm                                                                       602 kB/s | 154 kB     00:00    
(2/3): kernel-ml-core-6.15.4-1.el8.elrepo.x86_64.rpm                                                                  5.8 MB/s |  67 MB     00:11    
(3/3): kernel-ml-modules-6.15.4-1.el8.elrepo.x86_64.rpm                                                               2.9 MB/s |  62 MB     00:21    
------------------------------------------------------------------------------------------------------------------------------------------------------
总计                                                                                                                  6.0 MB/s | 129 MB     00:21     
ELRepo.org Community Enterprise Linux Kernel Repository - el8                                                         1.6 MB/s | 1.7 kB     00:00    
file:///etc/pki/rpm-gpg/RPM-GPG-KEY-elrepo.org 的 GPG 公钥(0xBAADAE52)已安装
ELRepo.org Community Enterprise Linux Kernel Repository - el8                                                         3.0 MB/s | 3.1 kB     00:00    
导入 GPG 公钥 0xEAA31D4A:
 Userid: "elrepo.org (RPM Signing Key v2 for elrepo.org) <secure@elrepo.org>"
 指纹: B8A7 5587 4DA2 40C9 DAC4 E715 5160 0989 EAA3 1D4A
 来自: /etc/pki/rpm-gpg/RPM-GPG-KEY-v2-elrepo.org
确定吗？[y/N]： y
导入公钥成功
运行事务检查
事务检查成功。
运行事务测试
事务测试成功。
运行事务
  准备中  :                                                                                                                                       1/1 
  安装    : kernel-ml-core-6.15.4-1.el8.elrepo.x86_64                                                                                             1/3 
  运行脚本: kernel-ml-core-6.15.4-1.el8.elrepo.x86_64                                                                                             1/3 
/usr/sbin/ldconfig: /usr/lib64/llvm15/lib/libclang.so.15 不是符号链接


  安装    : kernel-ml-modules-6.15.4-1.el8.elrepo.x86_64                                                                                          2/3 
  运行脚本: kernel-ml-modules-6.15.4-1.el8.elrepo.x86_64                                                                                          2/3 
  安装    : kernel-ml-6.15.4-1.el8.elrepo.x86_64                                                                                                  3/3 
  运行脚本: kernel-ml-core-6.15.4-1.el8.elrepo.x86_64                                                                                             3/3 
  运行脚本: kernel-ml-6.15.4-1.el8.elrepo.x86_64                                                                                                  3/3 
/sbin/ldconfig: /usr/lib64/llvm15/lib/libclang.so.15 不是符号链接


  验证    : kernel-ml-6.15.4-1.el8.elrepo.x86_64                                                                                                  1/3 
  验证    : kernel-ml-core-6.15.4-1.el8.elrepo.x86_64                                                                                             2/3 
  验证    : kernel-ml-modules-6.15.4-1.el8.elrepo.x86_64                                                                                          3/3 

已安装:
  kernel-ml-6.15.4-1.el8.elrepo.x86_64         kernel-ml-core-6.15.4-1.el8.elrepo.x86_64         kernel-ml-modules-6.15.4-1.el8.elrepo.x86_64        

完毕！
```
### 更新 `grub` 配置
```shell
[ares@ares-master ~]$ sudo grubby --set-default /boot/vmlinuz-$(rpm -q kernel-ml --last | head -n1 | awk '{print $2}')
[sudo] ares 的密码：
The param /boot/vmlinuz-2025年07月07日 is incorrect
[ares@ares-master ~]$ rpm -q kernel-ml --last | head -n1
kernel-ml-6.15.4-1.el8.elrepo.x86_64          2025年07月07日 星期一 14时02分35秒
[ares@ares-master ~]$ rpm -q kernel-ml --last | head -n1 | awk '{print $2}'
2025年07月07日
```
系统语言环境是中文（LANG=zh_CN.UTF-8),命令获取到的是日期,并赋值给grubby,导致错误,改用下面的命令
```shell
[ares@ares-master ~]$ sudo grubby --set-default /boot/vmlinuz-$(rpm -q kernel-ml --last | head -n1 | awk '{print $1}' | sed 's/^kernel-ml-//')
The default is /boot/loader/entries/ae98a3d5fd894cfdafe57f283ea0755e-6.15.4-1.el8.elrepo.x86_64.conf with index 0 and kernel /boot/vmlinuz-6.15.4-1.el8.elrepo.x86_64
```
重启系统后查看内核
```shell
[ares@ares-master ~]$ uname -r
6.15.4-1.el8.elrepo.x86_64
```
内核已经更新到`6.15.4-1.el8.elrepo.x86_64`

## DNS解析, 配置`hosts`
编辑`hosts`文件
```shell
127.0.0.1   localhost ares-master localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost ares-master localhost.localdomain localhost6 localhost6.localdomain6
192.168.4.108 ares-master
192.168.4.109 ares-slave
192.168.4.110 ares-slave1
```

三台服务器相互用主机名`ping`一下看看上面的配置是否生效
```shell
[ares@ares-slave1 ~]$ ping ares-master
PING ares-master (192.168.4.108) 56(84) bytes of data.
64 bytes from ares-master (192.168.4.108): icmp_seq=1 ttl=64 time=1.94 ms
64 bytes from ares-master (192.168.4.108): icmp_seq=2 ttl=64 time=0.697 ms
64 bytes from ares-master (192.168.4.108): icmp_seq=3 ttl=64 time=0.898 ms
^C
--- ares-master ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2027ms
rtt min/avg/max/mdev = 0.697/1.178/1.940/0.545 ms
[ares@ares-slave1 ~]$ ^C
[ares@ares-slave1 ~]$ ping ares-slave1
PING ares-slave1(localhost (::1)) 56 data bytes
64 bytes from localhost (::1): icmp_seq=1 ttl=64 time=0.112 ms
64 bytes from localhost (::1): icmp_seq=2 ttl=64 time=0.063 ms
^C
--- ares-slave1 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1055ms
rtt min/avg/max/mdev = 0.063/0.087/0.112/0.026 ms
```

## 关闭 SELinux
Kubernetes 组件与容器运行时（如 containerd / Docker）在 SELinux enforcing 模式下可能会遇到权限问题。
如果没有关闭, 采用下面的命令关闭 SELinux(我的系统默认已经关闭了SELinux,所以这里就不写了)
```shell
sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config
```

##  关闭防火墙（firewalld）
```shell
[ares@ares-master ~]$ sudo systemctl stop firewalld
[ares@ares-master ~]$ sudo systemctl disable firewalld
Removed /etc/systemd/system/multi-user.target.wants/firewalld.service.
Removed /etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service.
```

##  关闭Swap分区
Kubernetes 默认不允许节点启用 Swap。
```shell
[ares@ares-master ~]$ sudo swapoff -a
[ares@ares-master ~]$ sudo sed -i '/swap/d' /etc/fstab
```

## 同步时间（NTP）
确保所有节点时间一致，避免证书或调度异常。
```shell
[ares@ares-master ~]$ sudo yum install -y chrony
上次元数据过期检查：1:55:34 前，执行于 2025年07月07日 星期一 13时23分07秒。
软件包 chrony-4.5-2.0.1.an8.x86_64 已安装。
依赖关系解决。
无需任何处理。
完毕！
[ares@ares-master ~]$ sudo systemctl enable --now chronyd
[ares@ares-master ~]$ sudo timedatectl set-ntp true
[ares@ares-master ~]$ timedatectl
               Local time: 一 2025-07-07 15:19:44 CST
           Universal time: 一 2025-07-07 07:19:44 UTC
                 RTC time: 一 2025-07-07 07:19:44
                Time zone: Asia/Shanghai (CST, +0800)
System clock synchronized: yes
              NTP service: active
          RTC in local TZ: no
```

## 加载必要的内核模块
```shell
[ares@ares-master ~]$ sudo cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
> overlay
> br_netfilter
> EOF
[sudo] ares 的密码：
overlay
br_netfilter
[ares@ares-master ~]$ sudo modprobe overlay
[ares@ares-master ~]$ sudo modprobe br_netfilter
```

## 配置必要的 `sysctl` 参数
```shell
[ares@ares-master ~]$ sudo cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
> net.bridge.bridge-nf-call-ip6tables = 1
> net.bridge.bridge-nf-call-iptables = 1
> net.ipv4.ip_forward = 1
> EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
[ares@ares-master ~]$ sudo sysctl --system
* Applying /usr/lib/sysctl.d/10-default-yama-scope.conf ...
kernel.yama.ptrace_scope = 0
* Applying /usr/lib/sysctl.d/50-coredump.conf ...
kernel.core_pattern = |/usr/lib/systemd/systemd-coredump %P %u %g %s %t %c %h %e
kernel.core_pipe_limit = 16
* Applying /usr/lib/sysctl.d/50-default.conf ...
kernel.sysrq = 16
kernel.core_uses_pid = 1
kernel.kptr_restrict = 1
net.ipv4.conf.all.rp_filter = 1
net.ipv4.conf.all.accept_source_route = 0
net.ipv4.conf.all.promote_secondaries = 1
net.core.default_qdisc = fq_codel
fs.protected_hardlinks = 1
fs.protected_symlinks = 1
* Applying /usr/lib/sysctl.d/50-libkcapi-optmem_max.conf ...
net.core.optmem_max = 81920
* Applying /usr/lib/sysctl.d/50-pid-max.conf ...
kernel.pid_max = 4194304
* Applying /usr/lib/sysctl.d/60-libvirtd.conf ...
fs.aio-max-nr = 1048576
* Applying /usr/lib/sysctl.d/60-qemu-postcopy-migration.conf ...
* Applying /etc/sysctl.d/99-sysctl.conf ...
* Applying /etc/sysctl.d/k8s.conf ...
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
* Applying /etc/sysctl.conf ...
```

## 容器运行时准备（containerd 推荐）

### 安装一些工具
```shell
[ares@ares-master ~]$ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
[sudo] ares 的密码：
上次元数据过期检查：2:22:56 前，执行于 2025年07月07日 星期一 13时23分07秒。
软件包 device-mapper-persistent-data-0.9.0-7.0.1.an8.x86_64 已安装。
软件包 lvm2-8:2.03.14-14.0.1.an8.x86_64 已安装。
依赖关系解决。
======================================================================================================================================================
 软件包                                       架构                       版本                                        仓库                        大小
======================================================================================================================================================
安装:
 yum-utils                                    noarch                     4.0.21-25.an8                               BaseOS                      75 k
升级:
 device-mapper                                x86_64                     8:1.02.181-15.0.1.an8                       BaseOS                     378 k
 device-mapper-event                          x86_64                     8:1.02.181-15.0.1.an8                       BaseOS                     273 k
 device-mapper-event-libs                     x86_64                     8:1.02.181-15.0.1.an8                       BaseOS                     271 k
 device-mapper-libs                           x86_64                     8:1.02.181-15.0.1.an8                       BaseOS                     410 k
 lvm2                                         x86_64                     8:2.03.14-15.0.1.an8                        BaseOS                     1.7 M
 lvm2-libs                                    x86_64                     8:2.03.14-15.0.1.an8                        BaseOS                     1.2 M

事务概要
======================================================================================================================================================
安装  1 软件包
升级  6 软件包

总下载：4.2 M
下载软件包：
(1/7): yum-utils-4.0.21-25.an8.noarch.rpm                                                                             570 kB/s |  75 kB     00:00    
(2/7): device-mapper-event-libs-1.02.181-15.0.1.an8.x86_64.rpm                                                        784 kB/s | 271 kB     00:00    
(3/7): device-mapper-event-1.02.181-15.0.1.an8.x86_64.rpm                                                             562 kB/s | 273 kB     00:00    
(4/7): device-mapper-1.02.181-15.0.1.an8.x86_64.rpm                                                                   362 kB/s | 378 kB     00:01    
(5/7): device-mapper-libs-1.02.181-15.0.1.an8.x86_64.rpm                                                              684 kB/s | 410 kB     00:00    
(6/7): lvm2-2.03.14-15.0.1.an8.x86_64.rpm                                                                             938 kB/s | 1.7 MB     00:01    
(7/7): lvm2-libs-2.03.14-15.0.1.an8.x86_64.rpm                                                                        937 kB/s | 1.2 MB     00:01    
------------------------------------------------------------------------------------------------------------------------------------------------------
总计                                                                                                                  1.8 MB/s | 4.2 MB     00:02     
AnolisOS-8 - BaseOS                                                                                                   3.0 MB/s | 3.0 kB     00:00    
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
  准备中  :                                                                                                                                       1/1 
  运行脚本: device-mapper-libs-8:1.02.181-15.0.1.an8.x86_64                                                                                       1/1 
  升级    : device-mapper-libs-8:1.02.181-15.0.1.an8.x86_64                                                                                      1/13 
  升级    : device-mapper-8:1.02.181-15.0.1.an8.x86_64                                                                                           2/13 
  升级    : device-mapper-event-libs-8:1.02.181-15.0.1.an8.x86_64                                                                                3/13 
  升级    : device-mapper-event-8:1.02.181-15.0.1.an8.x86_64                                                                                     4/13 
  运行脚本: device-mapper-event-8:1.02.181-15.0.1.an8.x86_64                                                                                     4/13 
  升级    : lvm2-libs-8:2.03.14-15.0.1.an8.x86_64                                                                                                5/13 
  升级    : lvm2-8:2.03.14-15.0.1.an8.x86_64                                                                                                     6/13 
  运行脚本: lvm2-8:2.03.14-15.0.1.an8.x86_64                                                                                                     6/13 
  安装    : yum-utils-4.0.21-25.an8.noarch                                                                                                       7/13 
  运行脚本: lvm2-8:2.03.14-14.0.1.an8.x86_64                                                                                                     8/13 
  清理    : lvm2-8:2.03.14-14.0.1.an8.x86_64                                                                                                     8/13 
  运行脚本: lvm2-8:2.03.14-14.0.1.an8.x86_64                                                                                                     8/13 
  清理    : lvm2-libs-8:2.03.14-14.0.1.an8.x86_64                                                                                                9/13 
  运行脚本: device-mapper-event-8:1.02.181-14.0.1.an8.x86_64                                                                                    10/13 
  清理    : device-mapper-event-8:1.02.181-14.0.1.an8.x86_64                                                                                    10/13 
  清理    : device-mapper-event-libs-8:1.02.181-14.0.1.an8.x86_64                                                                               11/13 
  清理    : device-mapper-libs-8:1.02.181-14.0.1.an8.x86_64                                                                                     12/13 
  清理    : device-mapper-8:1.02.181-14.0.1.an8.x86_64                                                                                          13/13 
  运行脚本: device-mapper-8:1.02.181-14.0.1.an8.x86_64                                                                                          13/13 
/sbin/ldconfig: /usr/lib64/llvm15/lib/libclang.so.15 不是符号链接


  验证    : yum-utils-4.0.21-25.an8.noarch                                                                                                       1/13 
  验证    : device-mapper-8:1.02.181-15.0.1.an8.x86_64                                                                                           2/13 
  验证    : device-mapper-8:1.02.181-14.0.1.an8.x86_64                                                                                           3/13 
  验证    : device-mapper-event-8:1.02.181-15.0.1.an8.x86_64                                                                                     4/13 
  验证    : device-mapper-event-8:1.02.181-14.0.1.an8.x86_64                                                                                     5/13 
  验证    : device-mapper-event-libs-8:1.02.181-15.0.1.an8.x86_64                                                                                6/13 
  验证    : device-mapper-event-libs-8:1.02.181-14.0.1.an8.x86_64                                                                                7/13 
  验证    : device-mapper-libs-8:1.02.181-15.0.1.an8.x86_64                                                                                      8/13 
  验证    : device-mapper-libs-8:1.02.181-14.0.1.an8.x86_64                                                                                      9/13 
  验证    : lvm2-8:2.03.14-15.0.1.an8.x86_64                                                                                                    10/13 
  验证    : lvm2-8:2.03.14-14.0.1.an8.x86_64                                                                                                    11/13 
  验证    : lvm2-libs-8:2.03.14-15.0.1.an8.x86_64                                                                                               12/13 
  验证    : lvm2-libs-8:2.03.14-14.0.1.an8.x86_64                                                                                               13/13 

已升级:
  device-mapper-8:1.02.181-15.0.1.an8.x86_64                                   device-mapper-event-8:1.02.181-15.0.1.an8.x86_64                       
  device-mapper-event-libs-8:1.02.181-15.0.1.an8.x86_64                        device-mapper-libs-8:1.02.181-15.0.1.an8.x86_64                        
  lvm2-8:2.03.14-15.0.1.an8.x86_64                                             lvm2-libs-8:2.03.14-15.0.1.an8.x86_64                                  
已安装:
  yum-utils-4.0.21-25.an8.noarch                                                                                                                      

完毕！
```

### 添加阿里云docker镜像
```shell
[ares@ares-master ~]$ sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
添加仓库自：https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

### 安装 containerd
```shell
[ares@ares-master ~]$ sudo yum install -y containerd.io
Docker CE Stable - x86_64                                                                                             506 kB/s |  66 kB     00:00    
错误：
 问题: 安装的软件包的问题 podman-4:4.9.4-20.0.1.module+an8.9.0+11309+b166eedd.x86_64
  - package podman-4:4.9.4-20.0.1.module+an8.9.0+11309+b166eedd.x86_64 from @System requires runc >= 1.0.0-57, but none of the providers can be installed
  - package podman-4:4.9.4-20.0.1.module+an8.9.0+11309+b166eedd.x86_64 from AppStream requires runc >= 1.0.0-57, but none of the providers can be installed
  - package containerd.io-1.6.32-3.1.el8.x86_64 from docker-ce-stable conflicts with runc provided by runc-1:1.1.12-6.0.1.module+an8.9.0+11309+b166eedd.x86_64 from @System
  - package containerd.io-1.6.32-3.1.el8.x86_64 from docker-ce-stable obsoletes runc provided by runc-1:1.1.12-6.0.1.module+an8.9.0+11309+b166eedd.x86_64 from @System
  - package containerd.io-1.6.32-3.1.el8.x86_64 from docker-ce-stable conflicts with runc provided by runc-1:1.1.12-6.0.1.module+an8.9.0+11309+b166eedd.x86_64 from AppStream
  - package containerd.io-1.6.32-3.1.el8.x86_64 from docker-ce-stable obsoletes runc provided by runc-1:1.1.12-6.0.1.module+an8.9.0+11309+b166eedd.x86_64 from AppStream
  - 无法为该任务安装最佳候选
  - package runc-1.0.0-66.rc10.module+an8.4.0+10551+0624d0f7.x86_64 from AppStream is filtered out by modular filtering
  - package runc-1.0.0-73.rc95.0.1.module+an8.7.0+11024+102aebd0.x86_64 from AppStream is filtered out by modular filtering
  - package runc-1:1.1.12-1.0.1.module+an8.9.0+11182+f2fff746.x86_64 from AppStream is filtered out by modular filtering
(尝试在命令行中添加 '--allowerasing' 来替换冲突的软件包 或 '--skip-broken' 来跳过无法安装的软件包 或 '--nobest' 来不只使用软件包的最佳候选)
```
说明你在安装 containerd.io 的时候，系统中已有的 runc 版本与 containerd 不兼容，并且和 podman 等组件有冲突。使用 --allowerasing 强制替换冲突的 runc

```shell
[ares@ares-master ~]$ sudo yum install -y containerd.io --allowerasing
上次元数据过期检查：0:03:12 前，执行于 2025年07月07日 星期一 15时49分37秒。
依赖关系解决。
======================================================================================================================================================
 软件包                          架构                 版本                                                       仓库                            大小
======================================================================================================================================================
安装:
 containerd.io                   x86_64               1.6.32-3.1.el8                                             docker-ce-stable                35 M
     替换  runc.x86_64 1:1.1.12-6.0.1.module+an8.9.0+11309+b166eedd
移除依赖的软件包:
 buildah                         x86_64               2:1.33.12-1.module+an8.9.0+11299+72deb0a4                  @AppStream                      31 M
 cockpit-podman                  noarch               84.1-1.module+an8.9.0+11261+8fb088a9                       @AppStream                     682 k
 containers-common               x86_64               2:1-82.0.1.module+an8.9.0+11261+8fb088a9                   @AppStream                     576 k
 podman                          x86_64               4:4.9.4-20.0.1.module+an8.9.0+11309+b166eedd               @AppStream                      53 M
 podman-catatonit                x86_64               4:4.9.4-20.0.1.module+an8.9.0+11309+b166eedd               @AppStream                     794 k

事务概要
======================================================================================================================================================
安装  1 软件包
移除  5 软件包

总下载：35 M
下载软件包：
containerd.io-1.6.32-3.1.el8.x86_64.rpm                                                                               1.6 MB/s |  35 MB     00:22    
------------------------------------------------------------------------------------------------------------------------------------------------------
总计                                                                                                                  1.6 MB/s |  35 MB     00:22     
Docker CE Stable - x86_64                                                                                              19 kB/s | 1.6 kB     00:00    
导入 GPG 公钥 0x621E9F35:
 Userid: "Docker Release (CE rpm) <docker@docker.com>"
 指纹: 060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35
 来自: https://mirrors.aliyun.com/docker-ce/linux/centos/gpg
导入公钥成功
运行事务检查
事务检查成功。
运行事务测试
事务测试成功。
运行事务
  准备中  :                                                                                                                                       1/1 
  运行脚本: containerd.io-1.6.32-3.1.el8.x86_64                                                                                                   1/1 
  安装    : containerd.io-1.6.32-3.1.el8.x86_64                                                                                                   1/7 
  运行脚本: containerd.io-1.6.32-3.1.el8.x86_64                                                                                                   1/7 
  删除    : buildah-2:1.33.12-1.module+an8.9.0+11299+72deb0a4.x86_64                                                                              2/7 
  删除    : cockpit-podman-84.1-1.module+an8.9.0+11261+8fb088a9.noarch                                                                            3/7 
  运行脚本: podman-4:4.9.4-20.0.1.module+an8.9.0+11309+b166eedd.x86_64                                                                            4/7 
  删除    : podman-4:4.9.4-20.0.1.module+an8.9.0+11309+b166eedd.x86_64                                                                            4/7 
  运行脚本: podman-4:4.9.4-20.0.1.module+an8.9.0+11309+b166eedd.x86_64                                                                            4/7 
  删除    : containers-common-2:1-82.0.1.module+an8.9.0+11261+8fb088a9.x86_64                                                                     5/7 
  删除    : podman-catatonit-4:4.9.4-20.0.1.module+an8.9.0+11309+b166eedd.x86_64                                                                  6/7 
  运行脚本: podman-catatonit-4:4.9.4-20.0.1.module+an8.9.0+11309+b166eedd.x86_64                                                                  6/7 
  废弃    : runc-1:1.1.12-6.0.1.module+an8.9.0+11309+b166eedd.x86_64                                                                              7/7 
  运行脚本: runc-1:1.1.12-6.0.1.module+an8.9.0+11309+b166eedd.x86_64                                                                              7/7 
/sbin/ldconfig: /usr/lib64/llvm15/lib/libclang.so.15 不是符号链接


  验证    : containerd.io-1.6.32-3.1.el8.x86_64                                                                                                   1/7 
  验证    : runc-1:1.1.12-6.0.1.module+an8.9.0+11309+b166eedd.x86_64                                                                              2/7 
  验证    : buildah-2:1.33.12-1.module+an8.9.0+11299+72deb0a4.x86_64                                                                              3/7 
  验证    : cockpit-podman-84.1-1.module+an8.9.0+11261+8fb088a9.noarch                                                                            4/7 
  验证    : containers-common-2:1-82.0.1.module+an8.9.0+11261+8fb088a9.x86_64                                                                     5/7 
  验证    : podman-4:4.9.4-20.0.1.module+an8.9.0+11309+b166eedd.x86_64                                                                            6/7 
  验证    : podman-catatonit-4:4.9.4-20.0.1.module+an8.9.0+11309+b166eedd.x86_64                                                                  7/7 

已安装:
  containerd.io-1.6.32-3.1.el8.x86_64                                                                                                                 
已移除:
  buildah-2:1.33.12-1.module+an8.9.0+11299+72deb0a4.x86_64                       cockpit-podman-84.1-1.module+an8.9.0+11261+8fb088a9.noarch          
  containers-common-2:1-82.0.1.module+an8.9.0+11261+8fb088a9.x86_64              podman-4:4.9.4-20.0.1.module+an8.9.0+11309+b166eedd.x86_64          
  podman-catatonit-4:4.9.4-20.0.1.module+an8.9.0+11309+b166eedd.x86_64          

完毕！
```
#### 初始化containerd默认配置文件
```shell
[ares@ares-master ~]$ sudo mkdir -p /etc/containerd
[ares@ares-master ~]$ containerd config default | sudo tee /etc/containerd/config.toml
disabled_plugins = []
imports = []
oom_score = 0
plugin_dir = ""
required_plugins = []
root = "/var/lib/containerd"
state = "/run/containerd"
temp = ""
version = 2

[cgroup]
  path = ""

[debug]
  address = ""
  format = ""
  gid = 0
  level = ""
  uid = 0

[grpc]
  address = "/run/containerd/containerd.sock"
  gid = 0
  max_recv_message_size = 16777216
  max_send_message_size = 16777216
  tcp_address = ""
  tcp_tls_ca = ""
  tcp_tls_cert = ""
  tcp_tls_key = ""
  uid = 0

[metrics]
  address = ""
  grpc_histogram = false

[plugins]

  [plugins."io.containerd.gc.v1.scheduler"]
    deletion_threshold = 0
    mutation_threshold = 100
    pause_threshold = 0.02
    schedule_delay = "0s"
    startup_delay = "100ms"

  [plugins."io.containerd.grpc.v1.cri"]
    device_ownership_from_security_context = false
    disable_apparmor = false
    disable_cgroup = false
    disable_hugetlb_controller = true
    disable_proc_mount = false
    disable_tcp_service = true
    drain_exec_sync_io_timeout = "0s"
    enable_selinux = false
    enable_tls_streaming = false
    enable_unprivileged_icmp = false
    enable_unprivileged_ports = false
    ignore_deprecation_warnings = []
    ignore_image_defined_volumes = false
    max_concurrent_downloads = 3
    max_container_log_line_size = 16384
    netns_mounts_under_state_dir = false
    restrict_oom_score_adj = false
    sandbox_image = "registry.k8s.io/pause:3.6"
    selinux_category_range = 1024
    stats_collect_period = 10
    stream_idle_timeout = "4h0m0s"
    stream_server_address = "127.0.0.1"
    stream_server_port = "0"
    systemd_cgroup = false
    tolerate_missing_hugetlb_controller = true
    unset_seccomp_profile = ""

    [plugins."io.containerd.grpc.v1.cri".cni]
      bin_dir = "/opt/cni/bin"
      conf_dir = "/etc/cni/net.d"
      conf_template = ""
      ip_pref = ""
      max_conf_num = 1

    [plugins."io.containerd.grpc.v1.cri".containerd]
      default_runtime_name = "runc"
      disable_snapshot_annotations = true
      discard_unpacked_layers = false
      ignore_rdt_not_enabled_errors = false
      no_pivot = false
      snapshotter = "overlayfs"

      [plugins."io.containerd.grpc.v1.cri".containerd.default_runtime]
        base_runtime_spec = ""
        cni_conf_dir = ""
        cni_max_conf_num = 0
        container_annotations = []
        pod_annotations = []
        privileged_without_host_devices = false
        runtime_engine = ""
        runtime_path = ""
        runtime_root = ""
        runtime_type = ""

        [plugins."io.containerd.grpc.v1.cri".containerd.default_runtime.options]

      [plugins."io.containerd.grpc.v1.cri".containerd.runtimes]

        [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
          base_runtime_spec = ""
          cni_conf_dir = ""
          cni_max_conf_num = 0
          container_annotations = []
          pod_annotations = []
          privileged_without_host_devices = false
          runtime_engine = ""
          runtime_path = ""
          runtime_root = ""
          runtime_type = "io.containerd.runc.v2"

          [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
            BinaryName = ""
            CriuImagePath = ""
            CriuPath = ""
            CriuWorkPath = ""
            IoGid = 0
            IoUid = 0
            NoNewKeyring = false
            NoPivotRoot = false
            Root = ""
            ShimCgroup = ""
            SystemdCgroup = false

      [plugins."io.containerd.grpc.v1.cri".containerd.untrusted_workload_runtime]
        base_runtime_spec = ""
        cni_conf_dir = ""
        cni_max_conf_num = 0
        container_annotations = []
        pod_annotations = []
        privileged_without_host_devices = false
        runtime_engine = ""
        runtime_path = ""
        runtime_root = ""
        runtime_type = ""

        [plugins."io.containerd.grpc.v1.cri".containerd.untrusted_workload_runtime.options]

    [plugins."io.containerd.grpc.v1.cri".image_decryption]
      key_model = "node"

    [plugins."io.containerd.grpc.v1.cri".registry]
      config_path = ""

      [plugins."io.containerd.grpc.v1.cri".registry.auths]

      [plugins."io.containerd.grpc.v1.cri".registry.configs]

      [plugins."io.containerd.grpc.v1.cri".registry.headers]

      [plugins."io.containerd.grpc.v1.cri".registry.mirrors]

    [plugins."io.containerd.grpc.v1.cri".x509_key_pair_streaming]
      tls_cert_file = ""
      tls_key_file = ""

  [plugins."io.containerd.internal.v1.opt"]
    path = "/opt/containerd"

  [plugins."io.containerd.internal.v1.restart"]
    interval = "10s"

  [plugins."io.containerd.internal.v1.tracing"]

  [plugins."io.containerd.metadata.v1.bolt"]
    content_sharing_policy = "shared"

  [plugins."io.containerd.monitor.v1.cgroups"]
    no_prometheus = false

  [plugins."io.containerd.runtime.v1.linux"]
    no_shim = false
    runtime = "runc"
    runtime_root = ""
    shim = "containerd-shim"
    shim_debug = false

  [plugins."io.containerd.runtime.v2.task"]
    platforms = ["linux/amd64"]
    sched_core = false

  [plugins."io.containerd.service.v1.diff-service"]
    default = ["walking"]

  [plugins."io.containerd.service.v1.tasks-service"]
    rdt_config_file = ""

  [plugins."io.containerd.snapshotter.v1.aufs"]
    root_path = ""

  [plugins."io.containerd.snapshotter.v1.devmapper"]
    async_remove = false
    base_image_size = ""
    discard_blocks = false
    fs_options = ""
    fs_type = ""
    pool_name = ""
    root_path = ""

  [plugins."io.containerd.snapshotter.v1.native"]
    root_path = ""

  [plugins."io.containerd.snapshotter.v1.overlayfs"]
    mount_options = []
    root_path = ""
    sync_remove = false
    upperdir_label = false

  [plugins."io.containerd.snapshotter.v1.zfs"]
    root_path = ""

  [plugins."io.containerd.tracing.processor.v1.otlp"]

[proxy_plugins]

[stream_processors]

  [stream_processors."io.containerd.ocicrypt.decoder.v1.tar"]
    accepts = ["application/vnd.oci.image.layer.v1.tar+encrypted"]
    args = ["--decryption-keys-path", "/etc/containerd/ocicrypt/keys"]
    env = ["OCICRYPT_KEYPROVIDER_CONFIG=/etc/containerd/ocicrypt/ocicrypt_keyprovider.conf"]
    path = "ctd-decoder"
    returns = "application/vnd.oci.image.layer.v1.tar"

  [stream_processors."io.containerd.ocicrypt.decoder.v1.tar.gzip"]
    accepts = ["application/vnd.oci.image.layer.v1.tar+gzip+encrypted"]
    args = ["--decryption-keys-path", "/etc/containerd/ocicrypt/keys"]
    env = ["OCICRYPT_KEYPROVIDER_CONFIG=/etc/containerd/ocicrypt/ocicrypt_keyprovider.conf"]
    path = "ctd-decoder"
    returns = "application/vnd.oci.image.layer.v1.tar+gzip"

[timeouts]
  "io.containerd.timeout.bolt.open" = "0s"
  "io.containerd.timeout.shim.cleanup" = "5s"
  "io.containerd.timeout.shim.load" = "5s"
  "io.containerd.timeout.shim.shutdown" = "3s"
  "io.containerd.timeout.task.state" = "2s"

[ttrpc]
  address = ""
  gid = 0
  uid = 0
```

#### 修改配置以支持 systemd cgroup 驱动
把/etc/containerd/config.toml文件中的`SystemdCgroup`配置改为true

#### 重启containerd服务,查看状态, 查看版本
```shell
[ares@ares-master ~]$ sudo systemctl restart containerd
[ares@ares-master ~]$ systemctl status containerd
● containerd.service - containerd container runtime
   Loaded: loaded (/usr/lib/systemd/system/containerd.service; disabled; vendor preset: disabled)
   Active: active (running) since Mon 2025-07-07 16:06:56 CST; 15s ago
     Docs: https://containerd.io
  Process: 46980 ExecStartPre=/sbin/modprobe overlay (code=exited, status=0/SUCCESS)
 Main PID: 46982 (containerd)
    Tasks: 11
   Memory: 14.9M
   CGroup: /system.slice/containerd.service
           └─46982 /usr/bin/containerd

7月 07 16:06:56 ares-master containerd[46982]: time="2025-07-07T16:06:56.647372728+08:00" level=info msg="Start subscribing containerd event"
7月 07 16:06:56 ares-master containerd[46982]: time="2025-07-07T16:06:56.647435724+08:00" level=info msg="Start recovering state"
7月 07 16:06:56 ares-master containerd[46982]: time="2025-07-07T16:06:56.647464650+08:00" level=info msg=serving... address=/run/containerd/container>
7月 07 16:06:56 ares-master containerd[46982]: time="2025-07-07T16:06:56.647654324+08:00" level=info msg=serving... address=/run/containerd/container>
7月 07 16:06:56 ares-master containerd[46982]: time="2025-07-07T16:06:56.647697192+08:00" level=info msg="Start event monitor"
7月 07 16:06:56 ares-master containerd[46982]: time="2025-07-07T16:06:56.647730910+08:00" level=info msg="Start snapshots syncer"
7月 07 16:06:56 ares-master containerd[46982]: time="2025-07-07T16:06:56.647756142+08:00" level=info msg="Start cni network conf syncer for default"
7月 07 16:06:56 ares-master containerd[46982]: time="2025-07-07T16:06:56.647797550+08:00" level=info msg="Start streaming server"
7月 07 16:06:56 ares-master containerd[46982]: time="2025-07-07T16:06:56.647854802+08:00" level=info msg="containerd successfully booted in 0.021519s"
7月 07 16:06:56 ares-master systemd[1]: Started containerd container runtime.
[ares@ares-master ~]$ containerd --version
containerd containerd.io 1.6.32 8b3b7ca2e5ce38e8f31a34f35b2b68ceb8470d89
```
#### 设置containerd开机启动
```shell
[ares@ares-master ~]$ sudo systemctl enable containerd
Created symlink /etc/systemd/system/multi-user.target.wants/containerd.service → /usr/lib/systemd/system/containerd.service.
```

## 安装nerdctl工具
nerdctl 是一个 兼容 Docker CLI 命令风格 的容器管理工具。它是为 containerd 设计的，适用于不使用 Docker Engine 的环境。支持 Compose、Build、Image 管理等高级功能（需要额外组件如 buildkit）。

```shell
[ares@ares-slave1 ~]$ sudo wget https://github.com/containerd/nerdctl/releases/download/v2.1.2/nerdctl-full-2.1.2-linux-amd64.tar.gz
[sudo] ares 的密码：
--2025-07-07 16:21:50--  https://github.com/containerd/nerdctl/releases/download/v2.1.2/nerdctl-full-2.1.2-linux-amd64.tar.gz
正在解析主机 github.com (github.com)... 20.205.243.166
正在连接 github.com (github.com)|20.205.243.166|:443... 已连接。
```
在线下载太慢,改下载离线包,上传到服务器,解压到指定目录
```shell
[ares@ares-master ~]$ sudo tar Cxzvf /usr/local nerdctl-full-2.1.2-linux-amd64.tar.gz 
bin/
bin/buildctl
bin/buildg
bin/buildkit-cni-LICENSE
bin/buildkit-cni-README.md
bin/buildkit-cni-bandwidth
bin/buildkit-cni-bridge
bin/buildkit-cni-dhcp
bin/buildkit-cni-dummy
bin/buildkit-cni-firewall
bin/buildkit-cni-host-device
bin/buildkit-cni-host-local
bin/buildkit-cni-ipvlan
bin/buildkit-cni-loopback
bin/buildkit-cni-macvlan
bin/buildkit-cni-portmap
bin/buildkit-cni-ptp
bin/buildkit-cni-sbr
bin/buildkit-cni-static
bin/buildkit-cni-tap
bin/buildkit-cni-tuning
bin/buildkit-cni-vlan
bin/buildkit-cni-vrf
bin/buildkitd
bin/bypass4netns
bin/bypass4netnsd
bin/containerd
bin/containerd-fuse-overlayfs-grpc
bin/containerd-rootless-setuptool.sh
bin/containerd-rootless.sh
bin/containerd-shim-runc-v2
bin/containerd-stargz-grpc
bin/ctd-decoder
bin/ctr
bin/ctr-enc
bin/ctr-remote
bin/fuse-overlayfs
bin/gomodjail
bin/nerdctl
bin/nerdctl.gomodjail
bin/rootlessctl
bin/rootlesskit
bin/runc
bin/slirp4netns
bin/stargz-store-helper
bin/tini
lib/
lib/systemd/
lib/systemd/system/
lib/systemd/system/buildkit.service
lib/systemd/system/containerd.service
lib/systemd/system/stargz-snapshotter.service
libexec/
libexec/cni/
libexec/cni/LICENSE
libexec/cni/README.md
libexec/cni/bandwidth
libexec/cni/bridge
libexec/cni/dhcp
libexec/cni/dummy
libexec/cni/firewall
libexec/cni/host-device
libexec/cni/host-local
libexec/cni/ipvlan
libexec/cni/loopback
libexec/cni/macvlan
libexec/cni/portmap
libexec/cni/ptp
libexec/cni/sbr
libexec/cni/static
libexec/cni/tap
libexec/cni/tuning
libexec/cni/vlan
libexec/cni/vrf
share/
share/doc/
share/doc/nerdctl/
share/doc/nerdctl/README.md
share/doc/nerdctl/docs/
share/doc/nerdctl/docs/build.md
share/doc/nerdctl/docs/builder-debug.md
share/doc/nerdctl/docs/cni.md
share/doc/nerdctl/docs/command-reference.md
share/doc/nerdctl/docs/compose.md
share/doc/nerdctl/docs/config.md
share/doc/nerdctl/docs/cosign.md
share/doc/nerdctl/docs/cvmfs.md
share/doc/nerdctl/docs/dev/
share/doc/nerdctl/docs/dev/auditing_dockerfile.md
share/doc/nerdctl/docs/dev/store.md
share/doc/nerdctl/docs/dir.md
share/doc/nerdctl/docs/experimental.md
share/doc/nerdctl/docs/faq.md
share/doc/nerdctl/docs/freebsd.md
share/doc/nerdctl/docs/gpu.md
share/doc/nerdctl/docs/images/
share/doc/nerdctl/docs/images/nerdctl-white.svg
share/doc/nerdctl/docs/images/nerdctl.svg
share/doc/nerdctl/docs/images/rootlessKit-network-design.png
share/doc/nerdctl/docs/ipfs.md
share/doc/nerdctl/docs/multi-platform.md
share/doc/nerdctl/docs/notation.md
share/doc/nerdctl/docs/nydus.md
share/doc/nerdctl/docs/ocicrypt.md
share/doc/nerdctl/docs/overlaybd.md
share/doc/nerdctl/docs/registry.md
share/doc/nerdctl/docs/rootless.md
share/doc/nerdctl/docs/soci.md
share/doc/nerdctl/docs/stargz.md
share/doc/nerdctl/docs/testing/
share/doc/nerdctl/docs/testing/README.md
share/doc/nerdctl/docs/testing/tools.md
share/doc/nerdctl-full/
share/doc/nerdctl-full/README.md
share/doc/nerdctl-full/SHA256SUMS
```
### 查看版本
```shell
[ares@ares-master ~]$ nerdctl --version
nerdctl version 2.1.2
```

### 安装 buildkit (可选)
支持构建镜像的功能
```shell
[ares@ares-master ~]$ sudo tar Cxzvf /usr/local buildkit-v0.23.2.linux-amd64.tar.gz 
[sudo] ares 的密码：
bin/
bin/buildctl
bin/buildkit-cni-bridge
bin/buildkit-cni-firewall
bin/buildkit-cni-host-local
bin/buildkit-cni-loopback
bin/buildkit-qemu-aarch64
bin/buildkit-qemu-arm
bin/buildkit-qemu-i386
bin/buildkit-qemu-ppc64le
bin/buildkit-qemu-riscv64
bin/buildkit-qemu-s390x
bin/buildkit-runc
bin/buildkitd
```
#### 重载daemon-reload,设置buildkit开机启动,并启动buildkit服务
```shell
[ares@ares-master ~]$ sudo systemctl daemon-reload
[ares@ares-master ~]$ sudo systemctl enable buildkit
Created symlink /etc/systemd/system/multi-user.target.wants/buildkit.service → /usr/local/lib/systemd/system/buildkit.service.
[ares@ares-master ~]$ sudo systemctl start buildkit
[ares@ares-master ~]$ sudo systemctl status buildkit
● buildkit.service
   Loaded: loaded (/usr/local/lib/systemd/system/buildkit.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2025-07-07 17:01:01 CST; 3s ago
  Process: 56541 ExecStartPre=/sbin/modprobe overlay (code=exited, status=0/SUCCESS)
 Main PID: 56543 (buildkitd)
    Tasks: 10
   Memory: 10.7M
   CGroup: /system.slice/buildkit.service
           └─56543 /usr/local/bin/buildkitd

7月 07 17:01:01 ares-master buildkitd[56543]: time="2025-07-07T17:01:01+08:00" level=warning msg="CDI setup error /etc/buildkit/>
7月 07 17:01:01 ares-master buildkitd[56543]: time="2025-07-07T17:01:01+08:00" level=warning msg="CDI setup error /etc/cdi: fail>
7月 07 17:01:01 ares-master buildkitd[56543]: time="2025-07-07T17:01:01+08:00" level=warning msg="CDI setup error /var/run/cdi: >
7月 07 17:01:01 ares-master buildkitd[56543]: time="2025-07-07T17:01:01+08:00" level=warning msg="using host network as the defa>
7月 07 17:01:01 ares-master buildkitd[56543]: time="2025-07-07T17:01:01+08:00" level=warning msg="git source cannot be enabled: >
7月 07 17:01:01 ares-master buildkitd[56543]: time="2025-07-07T17:01:01+08:00" level=info msg="found worker \"60arkvk30akz8frtf8>
7月 07 17:01:01 ares-master buildkitd[56543]: time="2025-07-07T17:01:01+08:00" level=info msg="found 2 workers, default=\"oddjx0>
7月 07 17:01:01 ares-master buildkitd[56543]: time="2025-07-07T17:01:01+08:00" level=warning msg="currently, only the default wo>
7月 07 17:01:01 ares-master buildkitd[56543]: time="2025-07-07T17:01:01+08:00" level=info msg="running server on /run/buildkit/b>
7月 07 17:01:01 ares-master systemd[1]: Started buildkit.service.
```
#### 启动 buildkitd 服务（可配置 systemd）

