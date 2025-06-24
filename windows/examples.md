# CMD常用命令

## 把某个应用服务化, 并设置开机启动

**语法**

```shell
sc.exe [<servername>] create [<servicename>] [type= {own | share | kernel | filesys | rec | interact type= {own | share}}] [start= {boot | system | auto | demand | disabled | delayed-auto}] [error= {normal | severe | critical | ignore}] [binpath= <binarypathname>] [group= <loadordergroup>] [tag= {yes | no}] [depend= <dependencies>] [obj= {<accountname> | <objectname>}] [displayname= <displayname>] [password= <password>]
```

**常用参数说明**

+ create    创建服务
+ type      指定服务类型
+ start     启动方式
+ binpath   被配置为服务的可执行文件路径
+ depend    新配置的服务所依赖的服务

**使用示例**

```shell
C:\Users\ares wei# sc create nginx binpath= "C:\testlogs\nginx\nginx.exe" start= auto
[SC] CreateService 成功
C:\Users\ares wei# sc delete nginx
[SC] DeleteService 成功
```

## 配置任务计划

### `at`命令

> 适用范围：Windows Server 2022、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

将命令和程序计划在指定的时间和日期在计算机上运行。 仅当计划服务正在运行时，才能使用 at。 如果使用 at 时不带参数，则会列出已计划的命令。 若要运行此命令，您必须是本地 Administrators 组的成员。

**语法**

```shell
at [\computername] [[id] [/delete] | /delete [/yes]]
at [\computername] <time> [/interactive] [/every:date[,...] | /next:date[,...]] <command>
```

**常用参数说明**


| `\<computername\>` | 指定远程计算机。 如果省略此参数，则 at 将计划本地计算机上的命令和程序。                                                                                                                                                                                                           |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `<id>`             | 指定分配给已计划命令的标识号。                                                                                                                                                                                                                                                    |
| /delete            | 取消已计划的命令。 如果省略*ID* ，将取消计算机上的所有已计划的命令。                                                                                                                                                                                                              |
| /yes               | 删除计划事件时，对来自系统的所有查询回答“是”。                                                                                                                                                                                                                                  |
| `<time>`           | 指定要运行命令的时间。 时间 24 小时表示法表示（即从 00：00（午夜）到 23：59），格式为“小时:分钟”。                                                                                                                                                                              |
| 交互式             | 允许命令与在命令运行时登录的用户的桌面进行交互。                                                                                                                                                                                                                                  |
| 间隔：             | 在每周或每月的每个指定日（例如，每个星期四或每月的第三天）运行命令。                                                                                                                                                                                                              |
| `<date>`           | 指定要运行命令的日期。 你可以指定一周中的一天或多天（即，键入 M、T、W、Th、F、S、Su），也可以指定一个月中的一天或多天（即，键入 1 到 31）。<br />用逗号分隔多个日期条目。 如果省略 date，则 at 使用该月的当前日期。                                                               |
| next:              | 在下一个当天日期（例如，下个星期四）运行命令。                                                                                                                                                                                                                                    |
| `<command>`        | 指定要运行的 Windows 命令、程序（即.exe 或.com 文件）或批处理程序（即.bat 或.cmd 文件）。<br />当命令需要路径作为参数时，请使用绝对路径（即以驱动器号开头的整个路径）。 <br />如果该命令位于远程计算机上，请为服务器和共享名称指定通用命名约定 (UNC) 表示法，而不是远程驱动器号。 |
| /?                 | 在命令提示符下显示帮助。                                                                                                                                                                                                                                                          |

### `schtasks`命令

将命令和程序计划为定期运行或在特定时间运行、在计划中添加和删除任务、按需启动和停止任务，以及显示和更改计划任务。

**所需的权限**

+ 若要计划、查看和更改远程计算机上的所有任务，你必须是远程计算机上 Administrators 组的成员，或者必须使用 /u 参数来提供远程计算机的 Administrator 的凭据。
+ 如果本地计算机和远程计算机位于同一域中，或者如果本地计算机位于远程计算机域信任的域中，则可以使用 /create 或 /change 中的 /u 参数。 否则，远程计算机无法对指定的用户帐户进行身份验证，并且无法验证该帐户是否为 Administrators 组的成员。
+ 计划运行的任务必须具有相应的权限；这些权限因任务而异。 默认情况下，使用本地计算机的当前用户的权限运行任务，或者使用 /u 参数指定的用户权限（如果包含）。 若要以不同用户帐户的权限或系统权限运行任务，请使用 /ru 参数。

**命令**

[`schtasks change`](https://learn.microsoft.com/zh-cn/windows-server/administration/windows-commands/schtasks-change)

[`schtasks create`](https://learn.microsoft.com/zh-cn/windows-server/administration/windows-commands/schtasks-create)

[`schtasks delete`](https://learn.microsoft.com/zh-cn/windows-server/administration/windows-commands/schtasks-delete)

[`schtasks end`](https://learn.microsoft.com/zh-cn/windows-server/administration/windows-commands/schtasks-end)

[`schtasks query`](https://learn.microsoft.com/zh-cn/windows-server/administration/windows-commands/schtasks-query)

[`schtasks run`](https://learn.microsoft.com/zh-cn/windows-server/administration/windows-commands/schtasks-run)

**使用示例**

```shell
C:\Users\ares wei# schtasks /create /tn "清除Nginx日志" /tr "cmd /c del /q /s C:\testlogs\nginx\logs\*.*" /sc monthly /d 1 /st 01:00
成功: 成功创建计划任务 "清除Nginx日志"。
C:\Users\ares wei# schtasks /delete /tn "清除Nginx日志"
警告: 确实要删除任务 "清除Nginx日志" 吗 (Y/N )? y
成功: 计划的任务 "清除Nginx日志" 被成功删除。
```

## `DISM` 部署映像服务和管理

> 在 Windows 10 和 Windows 11 系统中，内置了一个强大的 DISM（部署映像服务和管理）命令行工具。它可以用来配置、调整、优化以及修复「Windows 系统映像」。当你的电脑遇到一些难以解决的异常问题时，使用 DISM 进行检测和修复，往往能搞定许多棘手的问题。请在管理员命令行中使用 DISM 命令。

### 快速检查 (CheckHealth)

检查本地映像是否存在数据损坏,如果系统没有问题，你会看到「未检测到组件存储损坏」提示。如果检测到问题，则会显示「组件存储已损坏」。

```shell
DISM /Online /Cleanup-Image /CheckHealth
```

### 深入扫描 (ScanHealth)

进行更全面的检查, 如果 CheckHealth 没有发现，但 Windows 还是存在问题，可以使用/ScanHealth参数进行更深入的扫描。这会彻底检查「组件存储」是否存有损坏，并将结果保存到日志文件中。

```shell
DISM /Online /Cleanup-Image /ScanHealth
```

### 修复系统 (RestoreHealth)

修复本地系统映像, 如果前两步发现了问题，可以使用 RestoreHealth 参数来修复 Windows 10/11 系统映像。

```shell
DISM /Online /Cleanup-Image /RestoreHealth
```

### 使用 install.wim 作为修复源（可选）

如果你的电脑无法连接到互联网，或者要修复「Windows 更新」组件本身，可以手动挂载 Windows 安装镜像，并指定install.wim或install.esd作为修复源：

1. 下载 [Windows 10 或 11 安装镜像](https://www.sysgeek.cn/microsoft-windows-downloads/)，ISO 版本要与你当前系统版本相同。例如，如果你使用的是 Windows 11 23H2，就别挂载个 22H2 来修复。
2. 双击下载的 ISO 文件，系统会自动将其挂载为虚拟光驱。记住系统分配给该虚拟光盘的盘符，例如F:\。

如果不使用/LimitAccess参数，DISM 会先尝试使用「Windows 更新」查找修复源文件。如果无法连接到 Windows Update 或找不到所需文件，才会使用/Source参数中指定的install.wim或install.esd文件。 `yourpath`是你下载的`install.wim`绝对路径。

```shell
DISM /Online /Cleanup-Image /RestoreHealth /Source:yourpath\install.wim /LimitAccess
```

# 如何在Windows 11/10家庭版中安装沙盒

## 先决条件

1. PC 或笔记本电脑必须装有 Windows 11 或 Windows 10 家庭版版本 1903 或更高版本。
2. 系统的CPU必须支持虚拟化技术（Intel VT-x或AMD-V）。
3. 必须有 4GB RAM 和 1GB 可用磁盘空间用于系统安装新的所需文件。

## 在 Windows Home Edition 上启用 Sandbox 的步骤

### 检查虚拟化支持是否启用

您应该开始的第一步是确认您的系统是否启用了虚拟化支持。 因为没有它，沙盒功能将无法工作。 如果您不知道如何检查，请参阅我们的教程 – 确认 Windows 11 / 10 上是否启用虚拟化。

### 创建脚本文件

新建`bat`后缀的文件，并输入以下内容：
```shell
@echo off

echo Checking for permissions
>nul 2>&1 "%SYSTEMROOT%\system32\cacls.exe" "%SYSTEMROOT%\system32\config\system"

echo Permission check result: %errorlevel%

REM --> If error flag set, we do not have admin.
if '%errorlevel%' NEQ '0' (
echo Requesting administrative privileges...
goto UACPrompt
) else ( goto gotAdmin )

:UACPrompt
echo Set UAC = CreateObject^("Shell.Application"^) > "%temp%\getadmin.vbs"
echo UAC.ShellExecute "%~s0", "", "", "runas", 1 >> "%temp%\getadmin.vbs"

echo Running created temporary "%temp%\getadmin.vbs"
timeout /T 2
"%temp%\getadmin.vbs"
exit /B

:gotAdmin
if exist "%temp%\getadmin.vbs" ( del "%temp%\getadmin.vbs" )
pushd "%CD%"
CD /D "%~dp0" 

echo Batch was successfully started with admin privileges
echo .
cls
Title Sandbox Installer

pushd "%~dp0"

dir /b %SystemRoot%\servicing\Packages\*Containers*.mum >sandbox.txt

for /f %%i in ('findstr /i . sandbox.txt 2^>nul') do dism /online /norestart /add-package:"%SystemRoot%\servicing\Packages\%%i"

del sandbox.txt

Dism /online /enable-feature /featurename:Containers-DisposableClientVM /LimitAccess /ALL

pause
```

### 开始在Windows 10或11 Home上安装Sandbox

以管理员身份运行刚创建的`bat`文件。等待几分钟，让脚本完成 Windows 10 或 11 家庭版上沙盒功能的安装过程。完成后重启电脑。(尝试后发现沙盒不能初始化)


# 恢复电脑自带应用操作

1. 获取所有用户的Appx包（UWP应用）。
2. 遍历这些包，对每个包执行操作。
3. 用管理员权限重新注册每个包的AppXManifest.xml文件，并禁用开发模式。

```shell
Get-AppxPackage -AllUsers| Foreach {Add-AppxPackage -DisableDevelopmentMode -Register “( ((_.InstallLocation)\AppXManifest.xml”}
```

# windows 10 11 删除微软账号

> 主要通过删除注册表项实现

尝试删除以下两个位置的注册表项：

![](./images/1.png)

![](./images/2.png)

注销账户重新登录,查看账户设置里电子邮件和账户里是否有删除微软账号的选项, 如果没有,继续删除以下两个位置的注册表项：

![](./images/3.jpg)

![](./images/4.png)

再次注销账户重新登录,查看账户设置里电子邮件和账户应该已经有删除微软账号的选项：

![](./images/5.png)
