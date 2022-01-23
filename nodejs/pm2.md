# 简介

P(rocess)M(anager)2是node进程管理工具，可以利用它来简化很多node应用管理的繁琐任务，如性能监控、自动重启、负载均衡等，而且使用非常简单。命令行启动即可,例如 `pm2 start app.js` 

# 管理进程

```shell
pm2 restart app_name
pm2 reload app_name
pm2 stop app_name
pm2 delete app_name
```

可以用以下参数代替 **app_name**

* all: 操作所有进程
* id: 操作指定id进程

# 查询管理的应用

```shell
pm2 [list|ls|status]
```

# 显示日志

```shell
// 实时日志
pm2 logs

// 最近200条日志
pm2 logs --lines 200
```

# 终端控制面板

```shell
pm2 monit
```

# 基于Web的控制面板，跨服务器的诊断系统

```shell
pm2 plus
```

# 集群模式

对于Node.js应用程序，PM2包含一个自动负载均衡器，它将在每个生成的进程之间共享所有HTTP /Websocket/TCP/UDP连接。

```shell
pm2 start app.js -i max
```

# 生态系统文件

您还可以创建一个名为“生态系统文件”的配置文件来管理多个应用程序。命令如下:

```shell
pm2 ecosystem
```

以上命令将生成一个名为 **ecosystem.config.js** 的配置文件:

```js
module.exports = {
	apps : [{
		name: "app",
		script: "./app.js",
	env: {
		NODE_ENV: "development",
	},
	env_production: {
		NODE_ENV: "production",
	}
	}, {
		name: 'worker',
		script: 'worker.js'
	}]
}
```
运行配置

```shell
pm2 start process.yml
```

# 设置启动脚本

在服务器启动/重启时,管理的进程重新启动PM2

```shell
pm2 startup
pm2 save
```

# 有变化是重启应用

```shell
pm2 start env.js --watch --ignore-watch="node_modules"
```

# 常用命令汇总

```shell
# Fork mode
pm2 start app.js --name my-api # Name process

# Cluster mode
pm2 start app.js -i 0        # Will start maximum processes with LB depending on available CPUs
pm2 start app.js -i max      # Same as above, but deprecated.
pm2 scale app +3             # Scales `app` up by 3 workers
pm2 scale app 2              # Scales `app` up or down to 2 workers total

# Listing

pm2 list               # Display all processes status
pm2 jlist              # Print process list in raw JSON
pm2 prettylist         # Print process list in beautified JSON

pm2 describe 0         # Display all informations about a specific process

pm2 monit              # Monitor all processes

# Logs

pm2 logs [--raw]       # Display all processes logs in streaming
pm2 flush              # Empty all log files
pm2 reloadLogs         # Reload all logs

# Actions

pm2 stop all           # Stop all processes
pm2 restart all        # Restart all processes

pm2 reload all         # Will 0s downtime reload (for NETWORKED apps)

pm2 stop 0             # Stop specific process id
pm2 restart 0          # Restart specific process id

pm2 delete 0           # Will remove process from pm2 list
pm2 delete all         # Will remove all processes from pm2 list

# Misc

pm2 reset <process>    # Reset meta data (restarted time...)
pm2 updatePM2          # Update in memory pm2
pm2 ping               # Ensure pm2 daemon has been launched
pm2 sendSignal SIGUSR2 my-app # Send system signal to script
pm2 start app.js --no-daemon
pm2 start app.js --no-vizion
pm2 start app.js --no-autorestart
```

# 常用命令参数

```shell
# Specify an app name
--name <app_name>

# Watch and Restart app when files change
--watch

# Set memory threshold for app reload
--max-memory-restart <200MB>

# Specify log file
--log <log_path>

# Pass extra arguments to the script
-- arg1 arg2 arg3

# Delay between automatic restarts
--restart-delay <delay in ms>

# Prefix logs with time
--time

# Do not auto restart app
--no-autorestart

# Specify cron for forced restart
--cron <cron_pattern>

# Attach to application log
--no-daemon
```