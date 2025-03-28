# Linux 简介

Linux 内核最初只是由芬兰人林纳斯·托瓦兹（Linus Torvalds）在赫尔辛基大学上学时出于个人爱好而编写的。

Linux 能运行主要的 UNIX 工具软件、应用程序和网络协议。它支持 32 位和 64 位硬件。Linux 继承了 Unix 以网络为核心的设计思想，是一个性能稳定的多用户网络操作系统。

Linux 是一套免费使用和自由传播的类 Unix 操作系统，是一个基于 POSIX 和 UNIX 的多用户、多任务、支持多线程和多 CPU 的操作系统。它能运行主要的UNIX工具软件、应用程序和网络协议。支持32位和64位硬件。Linux继承了Unix以网络为核心的设计思想，是一个性能稳定的多用户网络操作系统。

Linux存在着许多不同的Linux版本，但它们都使用了Linux内核。Linux可安装在各种计算机硬件设备中，比如手机、平板电脑、路由器、视频游戏控制台、台式计算机、大型机和超级计算机。

严格来讲，Linux操作系统指的是“linux内核+各种软件”，Linux这个词只表示Linux内核，但实际上人们已经习惯了用Linux来形容整个基于Linux内核，并且使用GNU 工程各种工具和数据库的操作系统。

**参考文档**

[https://www.linuxcool.com/](https://www.linuxcool.com/)

[https://www.w3cschool.cn/linux/dict](https://www.w3cschool.cn/linux/dict)

# Linux 的发行版

Linux 的发行版说简单点就是将 Linux 内核与应用软件做一个打包。

![](./images/1511849829609658.jpg)

目前市面上较知名的发行版有：Ubuntu、RedHat、CentOS、Debian、Fedora、SuSE、OpenSUSE、Arch Linux、SolusOS 等。

![](./images/wKioL1bvVPWAu7hqAAEyirVUn3c446.jpg-wh_651x-s_3197843091.jpg)

# Linux 应用领域

今天各种场合都有使用各种 Linux 发行版，从嵌入式设备到超级计算机，并且在服务器领域确定了地位，通常服务器使用 LAMP（Linux + Apache + MySQL + PHP）或 LNMP（Linux + Nginx+ MySQL + PHP）组合。

目前 Linux 不仅在家庭与企业中使用，并且在政府中也很受欢迎。

* 巴西联邦政府由于支持 Linux 而世界闻名。
* 有新闻报道俄罗斯军队自己制造的 Linux 发布版的，做为 G.H.ost 项目已经取得成果。
* 印度的 Kerala 联邦计划在向全联邦的高中推广使用 Linux。
* 中华人民共和国为取得技术独立，在龙芯处理器中排他性地使用 Linux。
* 在西班牙的一些地区开发了自己的 Linux 发布版，并且在政府与教育领域广泛使用，如 Extremadura 地区的 gnuLinEx 和 Andalusia 地区的 Guadalinex。
* 葡萄牙同样使用自己的 Linux 发布版 Caixa Mágica，用于 Magalh?es 笔记本电脑和 e-escola 政府软件。
* 法国和德国同样开始逐步采用 Linux。

# Linux vs Windows

目前国内 Linux 更多的是应用于服务器上，而桌面操作系统更多使用的是 Windows。主要区别如下


| 比较     | Windows                                                                                                                                                                                                                                                               | Linux                                                                                                                                                                                                                                                                                                                                                         |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 界面     | 界面统一，外壳程序固定所有 Windows 程序菜单几乎一致，快捷键也几乎相同                                                                                                                                                                                                 | 图形界面风格依发布版不同而不同，可能互不兼容。GNU/Linux 的终端机是从 UNIX 传承下来，<br />基本命令和操作方法也几乎一致。                                                                                                                                                                                                                                      |
| 驱动程序 | 驱动程序丰富，版本更新频繁。默认安装程序里面一般包含有该版本发布时流行<br />的硬件驱动程序，之后所出的新硬件驱动依赖于硬件厂商提供。对于一些老硬件，<br />如果没有了原配的驱动有时很难支持。另外，有时硬件厂商未提供所需版本的 <br />Windows 下的驱动，也会比较头痛。 | 由志愿者开发，由 Linux 核心开发小组发布，很多硬件厂商基于版权考虑并未提供驱动程序，<br />尽管多数无需手动安装，但是涉及安装则相对复杂，使得新用户面对驱动程序问题<br />（是否存在和安装方法）会一筹莫展。但是在开源开发模式下，许多老硬件尽管在Windows下很难<br />支持的也容易找到驱动。HP、Intel、AMD 等硬件厂商逐步不同程度支持开源驱动，问题正在得到缓解。 |
| 使用     | 使用比较简单，容易入门。图形化界面对没有计算机背景知识的用户使用十分有利。                                                                                                                                                                                            | 图形界面使用简单，容易入门。文字界面，需要学习才能掌握。                                                                                                                                                                                                                                                                                                      |
| 学习     | 系统构造复杂、变化频繁，且知识、技能淘汰快，深入学习困难。                                                                                                                                                                                                            | 系统构造简单、稳定，且知识、技能传承性好，深入学习相对容易。                                                                                                                                                                                                                                                                                                  |
| 软件     | 每一种特定功能可能都需要商业软件的支持，需要购买相应的授权。                                                                                                                                                                                                          | 大部分软件都可以自由获取，同样功能的软件选择较少。                                                                                                                                                                                                                                                                                                            |


# 历史背景

众所周知，linux内核是林纳斯·托瓦兹（Linus Torvalds）最先设计的。在1988年，托瓦兹进入了赫尔辛基大学，选读了计算机科学系。在学校期间，因为学业的需要与自己的兴趣，接触到了unix这个操作系统。当时整个赫尔辛基只有一部最新的UNIX系统,但是仍然很难满足托瓦兹的需求。为此,他就想到自己写一个类unix操作系统。不久之后，托瓦兹又了解到，谭宁邦教授为了教育需要而编写的Minix系统。让自己的想法更加坚定。

1991年初，林纳斯·托瓦兹购买了最新的intel 386的个人计算机，安装了Minix系统，开始学习minix操作系统。经由Minix系统的源码学习到了很多的内核程序设计的设计概念。1991年4月，林纳斯·托瓦兹开始酝酿并着手编制自己的操作系统。

当他发现自己的写的东西真的可以在386上面顺利运行，并且可以读取Minix的文件系统后，他将这个内核程序放置在当时芬兰最大的ftp网络上供大家下载，同时在BBS上面贴了一则消息，希望能获得大家的一些修改建议。因此当用户在安装使用时，发生问题，会反馈给托瓦兹。在托瓦兹能够解决的问题范围内，他能很快速的进行linux内核的更新和排错。

同时为了让自己的Linux能够兼容于UNIX系统，并让所有的软件都可以在Linux上执行，于是托瓦兹开始参考标准的POSIX规范。

> POSIX是可携式操作系统接口(（Portable Operating System Interface)）的缩写，重点在于规范核心内核与应用程序之间的接口，这是由美国电气与电子工程师学会(（IEEE)）所发布的一项标准喔！

一个人的力量毕竟是有限的。有些硬件托瓦兹无法取得，因此无法编写相关驱动程序，这个时候，就有志愿者站出来帮忙写驱动程序。而且托瓦兹这个人还特别务实。只要经测试可以运行后，就会将这些程序代码加入到内核中。因为这样的状况，linux的开发逐渐模块化，可维护性也大大增加。

后来，因为Linux内核加入了太多的功能，光靠托瓦兹一个人进行内核的实际测试并加入内核源程序实在太费力，于是，就有很多的朋友跳出来帮忙。例如考克斯（Alan Cox）、与崔迪（Stephen Tweedie）等，这些重要的副手会先将志愿者的修补程序或者新功能的程序代码进行测试，并且将结果上传给托瓦兹看，让托瓦兹做最后内核加入的源码的选择与整并！这个分层负责的结果让Linux的开发更加容易！

特别值得注意的是，这些托瓦兹的Linux开发副手，以及自愿传送修补程序的黑客志愿者，其实都没有见过面，而且彼此分布在地球的各个角落，大家群策群力共同开发出现今的Linux。

这群素未谋面的虚拟团队，在1994年终于完成了Linux的内核正式版Version 1.0。代码量达17万行。这一版同时还加入了X Window System的支持！此外，托瓦兹指明了企鹅为Linux的吉祥物。

1996年6月，Linux 2.0内核发布，此内核有大约40万行代码，并可以支持多个处理器。此时的Linux 已经进入了实用阶段，全球大约有350万人使用。

# Linux开枝散叶

因为linux加入了gnu计划，所以，任何人均可取得源码和可执行这个内核程序，并且可以修改。并且由于GNU的GPL授权并非不能从事商业行为，因此很多商业公司和团队便专门来开发自己的linux distribution。比较出名的有Red Hat，SuSE,Ubuntu,Fedora,Debian,CentOS等。

很多人不免担心，这么多distribution会不会都不相同呢？我可以告诉大家，无需担心，因为这些版本使用的都是同一个内核，而且还有Linux Standard Base（LSB）以及目录架构的File system Hierarchy Standard（FHS）标准规范等一些标准来规范开发者，它们的唯一差别可能就是该开发者自家所开发出来的管理工具以及套件管理的模式。

所以说，基本上，每个Linux distributions除了架构的严谨度与选择的套件内容外，其实差异并不太大。大家可以选择自己喜好的distribution来安装即可！
