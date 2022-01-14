<!--
 * @author: ares
 * @date: 2022-01-13 14:13:38
 * @lastEditTime: 2022-01-13 16:02:22
 * @lastEditors: ares
 * @description: 
 * 
-->

## 虚拟机网络配置

### 修改网卡对应的配置文件

```shell
vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
```

设置BOOTPROTO为dhcp，ONBOOT为yes

### 修改网关

```shell
vi /etc/sysconfig/network
```

设置NETWORKING为yes，GATEWAY为VirtualBox虚拟网卡的ip地址

### 修改DNS

```shell
vi /etc/resolv.cof
```

设置nameserver为host主机的nameserver

### 重启网络服务

```shell
service network restart
```
