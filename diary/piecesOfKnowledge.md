# window环境下把Nginx服务化,并配置为开机启动

## 创建Nginx服务
```shell
sc create nginx binpath= "C:\testlogs\nginx\nginx.exe" start= auto
```
## 启动Nginx服务
```shell
net start nginx
```
## 删除Nginx服务
```shell
sc create nginx
```


# window环境下添加清除Nginx日志的任务计划
```shell
schtasks /create /tn "清除Nginx日志" /tr "cmd /c del /q /s C:\testlogs\nginx\logs\*.*" /sc monthly /d 1 /st 01:00
```

# window环境下删除清除Nginx日志的任务计划
```shell
schtasks /delete /tn "清除Nginx日志" /f
```




# linux环境下删除nginx的7天前日志 
## 新建删除日志的脚本
```shell
vim /usr/local/bin/cleanup_nginx_logs.sh
```
## cleanup_nginx_logs.sh内容
```shell
#!/bin/bash
# Nginx日志路径
LOG_PATH="/var/log/nginx/"
# 删除7天前的日志文件
find $LOG_PATH -mtime +7 -type f -name *.log -exec rm -rf {} \;
```
## 给cleanup_nginx_logs.sh添加执行权限
```shell
chmod +x /usr/local/bin/cleanup_nginx_logs.sh
```
## 添加定时任务命令
```shell
crontab -e
```
## 添加定时任务
```shell
0 0 * * 1 /usr/local/bin/cleanup_nginx_logs.sh
```
