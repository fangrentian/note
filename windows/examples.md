# CMD常用命令使用示例

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


| `\<computername\>` | 指定远程计算机。 如果省略此参数，则 at 将计划本地计算机上的命令和程序。                                                                                                                                                                                                            |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `<id>`             | 指定分配给已计划命令的标识号。                                                                                                                                                                                                                                                     |
| /delete            | 取消已计划的命令。 如果省略*ID* ，将取消计算机上的所有已计划的命令。                                                                                                                                                                                                               |
| /yes               | 删除计划事件时，对来自系统的所有查询回答“是”。                                                                                                                                                                                                                                   |
| `<time>`           | 指定要运行命令的时间。 时间 24 小时表示法表示（即从 00：00（午夜）到 23：59），格式为“小时:分钟”。                                                                                                                                                                               |
| 交互式             | 允许命令与在命令运行时登录的用户的桌面进行交互。                                                                                                                                                                                                                                   |
| 间隔：             | 在每周或每月的每个指定日（例如，每个星期四或每月的第三天）运行命令。                                                                                                                                                                                                               |
| `<date>`           | 指定要运行命令的日期。 你可以指定一周中的一天或多天（即，键入 M、T、W、Th、F、S、Su），也可以指定一个月中的一天或多天（即，键入 1 到 31）。 <br />用逗号分隔多个日期条目。 如果省略 date，则 at 使用该月的当前日期。                                                               |
| next:              | 在下一个当天日期（例如，下个星期四）运行命令。                                                                                                                                                                                                                                     |
| `<command>`        | 指定要运行的 Windows 命令、程序（即.exe 或.com 文件）或批处理程序（即.bat 或.cmd 文件）。 <br />当命令需要路径作为参数时，请使用绝对路径（即以驱动器号开头的整个路径）。 <br />如果该命令位于远程计算机上，请为服务器和共享名称指定通用命名约定 (UNC) 表示法，而不是远程驱动器号。 |
| /?                 | 在命令提示符下显示帮助。                                                                                                                                                                                                                                                           |

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
