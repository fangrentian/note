## 什么 是docker

Docker 是一种运行于 Linux 和 Windows 上的软件，用于创建、管理和编排容器。

Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的镜像中，然后发布到任何流行的 Linux或Windows操作系统的机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口。

Docker 是在 GitHub 上开发的 Moby 开源项目的一部分。

Docker 公司，位于旧金山，是整个 Moby 开源项目的维护者。Docker 公司还提供包含支持服务的商业版本的 Docker。

**一个完整的Docker有以下几个部分组成:**

* DockerClient客户端

* Docker Daemon守护进程

* Docker Image镜像

* DockerContainer容器

**参考文档** 

[https://www.w3cschool.cn/docker/dict](https://www.w3cschool.cn/docker/dict)

[http://www.dockerinfo.net/document](http://www.dockerinfo.net/document)

[https://www.docker.org.cn/docker/docker-206.html](https://www.docker.org.cn/docker/docker-206.html)

**docker仓库** [https://hub.docker.com/](https://hub.docker.com/)

## 起源

Docker 是 PaaS 提供商 dotCloud 开源的一个基于 LXC 的高级容器引擎，源代码托管在 Github 上, 基于go语言并遵从Apache2.0协议开源。

Docker自2013年以来非常火热，无论是从 github 上的代码活跃度，还是Redhat在RHEL6.5中集成对Docker的支持, 就连 Google 的 Compute Engine 也支持 docker 在其之上运行。

一款开源软件能否在商业上成功，很大程度上依赖三件事 - 成功的 user case(用例), 活跃的社区和一个好故事。 dotCloud 之家的 PaaS 产品建立在docker之上，长期维护且有大量的用户，社区也十分活跃，接下来我们看看docker的故事。

环境管理复杂 - 从各种OS到各种中间件到各种app, 一款产品能够成功作为开发者需要关心的东西太多，且难于管理，这个问题几乎在所有现代IT相关行业都需要面对。
云计算时代的到来 - AWS的成功, 引导开发者将应用转移到 cloud 上, 解决了硬件管理的问题，然而中间件相关的问题依然存在 (所以openstack HEAT和 AWS cloudformation 都着力解决这个问题)。开发者思路变化提供了可能性。
虚拟化手段的变化 - cloud 时代采用标配硬件来降低成本，采用虚拟化手段来满足用户按需使用的需求以及保证可用性和隔离性。然而无论是KVM还是Xen在 docker 看来,都在浪费资源，因为用户需要的是高效运行环境而非OS, GuestOS既浪费资源又难于管理, 更加轻量级的LXC更加灵活和快速
LXC的移动性 - LXC在 linux 2.6 的 kernel 里就已经存在了，但是其设计之初并非为云计算考虑的，缺少标准化的描述手段和容器的可迁移性，决定其构建出的环境难于迁移和标准化管理(相对于KVM之类image和snapshot的概念)。docker 就在这个问题上做出实质性的革新。这是docker最独特的地方。
面对上述几个问题，docker设想是交付运行环境如同海运，OS如同一个货轮，每一个在OS基础上的软件都如同一个集装箱，用户可以通过标准化手段自由组装运行环境，同时集装箱的内容可以由用户自定义，也可以由专业人员制造。这样，交付一个软件，就是一系列标准化组件的集合的交付，如同乐高积木，用户只需要选择合适的积木组合，并且在最顶端署上自己的名字(最后一个标准化组件是用户的app)。这也就是基于docker的PaaS产品的原型。

## 折叠Docker架构

Docker 使用客户端-服务器 (C/S) 架构模式，使用远程API来管理和创建Docker容器。Docker 容器通过 Docker 镜像来创建。容器与镜像的关系类似于面向对象编程中的对象与类。

Docker采用 C/S架构 Docker daemon 作为服务端接受来自客户的请求，并处理这些请求(创建、运行、分发容器)。 客户端和服务端既可以运行在一个机器上，也可通过 socket 或者RESTful API 来进行通信。

Docker daemon 一般在宿主主机后台运行，等待接收来自客户端的消息。 Docker 客户端则为用户提供一系列可执行命令，用户用这些命令实现跟 Docker daemon 交互。

## 特性

在docker的网站上提到了docker的典型场景:

* Automating the packaging and deployment of applications(使应用的打包与部署自动化)

* Creation of lightweight, private PAAS environments(创建轻量、私密的PAAS环境)

* Automated testing and continuous integration/deployment(实现自动化测试和持续的集成/部署)

* Deploying and scaling web apps, databases and backend services(部署与扩展webapp、数据库和后台服务)

由于其基于LXC的轻量级虚拟化的特点，docker相比KVM之类最明显的特点就是启动快，资源占用小。因此对于构建隔离的标准化的运行环境，轻量级的PaaS(如dokku), 构建自动化测试和持续集成环境，以及一切可以横向扩展的应用(尤其是需要快速启停来应对峰谷的web应用)。

* 构建标准化的运行环境，现有的方案大多是在一个baseOS上运行一套puppet/chef，或者一个image文件，其缺点是前者需要base OS许多前提条件，后者几乎不可以修改(因为copy on write 的文件格式在运行时rootfs是read only的)。并且后者文件体积大，环境管理和版本控制本身也是一个问题。

* PaaS环境是不言而喻的，其设计之初和dotcloud的案例都是将其作为PaaS产品的环境基础

* 因为其标准化构建方法(buildfile)和良好的REST API，自动化测试和持续集成/部署能够很好的集成进来

* 因为LXC轻量级的特点，其启动快，而且docker能够只加载每个container变化的部分，这样资源占用小，能够在单机环境下与KVM之类的虚拟化方案相比能够更加快速和占用更少资源

## 局限

Docker并不是全能的，设计之初也不是KVM之类虚拟化手段的替代品，简单总结几点:

1、Docker是基于Linux 64bit的，无法在32bit的linux/Windows/unix环境下使用

2、LXC是基于cgroup等linux kernel功能的，因此container的guest系统只能是linux base的

3、隔离性相比KVM之类的虚拟化方案还是有些欠缺，所有container公用一部分的运行库

4、网络管理相对简单，主要是基于namespace隔离

5、cgroup的cpu和cpuset提供的cpu功能相比KVM的等虚拟化方案相比难以度量(所以dotcloud主要是按内存收费)

6、Docker对disk的管理比较有限

7、container随着用户进程的停止而销毁，container中的log等用户数据不便收集

针对1-2，有windows base应用的需求的基本可以pass了; 3-5主要是看用户的需求，到底是需要一个container还是一个VM, 同时也决定了docker作为 IaaS 不太可行。

针对6,7虽然是docker本身不支持的功能，但是可以通过其他手段解决(disk quota, mount --bind)。总之，选用container还是vm, 就是在隔离性和资源复用性上做权衡。

另外即便docker 0.7能够支持非AUFS的文件系统，但是由于其功能还不稳定，商业应用或许会存在问题，而AUFS的稳定版需要kernel 3.8, 所以如果想复制dotcloud的成功案例，可能需要考虑升级kernel或者换用ubuntu的server版本(后者提供deb更新)。这也是为什么开源界更倾向于支持ubuntu的原因(kernel版本)

Docker并非适合所有应用场景，Docker只能虚拟基于Linux的服务。Windows Azure 服务能够运行Docker实例，但到目前为止Windows服务还不能被虚拟化。

可能最大的障碍在于管理实例之间的交互。由于所有应用组件被拆分到不同的容器中，所有的服务器需要以一致的方式彼此通信。这意味着任何人如果选择复杂的基础设施，那么必须掌握应用编程接口管理以及集群工具，比如Swarm、Mesos或者Kubernets以确保机器按照预期运转并支持故障切换。

Docker在本质上是一个附加系统。使用文件系统的不同层构建一个应用是有可能的。每个组件被添加到之前已经创建的组件之上，可以比作为一个文件系统更明智。分层架构带来另一方面的效率提升，当你重建存在变化的Docker镜像时，不需要重建整个Docker镜像，只需要重建变化的部分。

可能更为重要的是，Docker旨在用于弹性计算。每个Docker实例的运营生命周期有限，实例数量根据需求增减。在一个管理适度的系统中，这些实例生而平等，不再需要时便各自消亡了。

针对Docker环境存在的不足，意味着在开始部署Docker前需要考虑如下几个问题。首先，Docker实例是无状态的。这意味着它们不应该承载任何交易数据，所有数据应该保存在数据库服务器中。

其次，开发Docker实例并不像创建一台虚拟机、添加应用然后克隆那样简单。为成功创建并使用Docker基础设施，管理员需要对系统管理的各个方面有一个全面的理解，包括Linux管理、编排及配置工具比如Puppet、Chef以及Salt。这些工具生来就基于命令行以及脚本。

## 原理

Docker核心解决的问题是利用LXC来实现类似VM的功能，从而利用更加节省的硬件资源提供给用户更多的计算资源。同VM的方式不同, LXC 其并不是一套硬件虚拟化方法 - 无法归属到全虚拟化、部分虚拟化和半虚拟化中的任意一个，而是一个操作系统级虚拟化方法, 理解起来可能并不像VM那样直观。所以我们从虚拟化到docker要解决的问题出发，看看他是怎么满足用户虚拟化需求的。

用户需要考虑虚拟化方法，尤其是硬件虚拟化方法，需要借助其解决的主要是以下4个问题:

* 隔离性 - 每个用户实例之间相互隔离, 互不影响。 硬件虚拟化方法给出的方法是VM, LXC给出的方法是container，更细一点是kernel namespace

* 可配额/可度量 - 每个用户实例可以按需提供其计算资源，所使用的资源可以被计量。硬件虚拟化方法因为虚拟了CPU, memory可以方便实现, LXC则主要是利用cgroups来控制资源

* 移动性 - 用户的实例可以很方便地复制、移动和重建。硬件虚拟化方法提供snapshot和image来实现，docker(主要)利用AUFS实现

* 安全性 - 这个话题比较大，这里强调是host主机的角度尽量保护container。硬件虚拟化的方法因为虚拟化的水平比较高，用户进程都是在KVM等虚拟机容器中翻译运行的, 然而对于LXC, 用户的进程是lxc-start进程的子进程, 只是在Kernel的namespace中隔离的, 因此需要一些kernel的patch来保证用户的运行环境不会受到来自host主机的恶意入侵, dotcloud(主要是)利用kernel grsec patch解决的.

## Docker公司

Docker 公司位于旧金山，由法裔美籍开发者和企业家 Solumon Hykes 创立，其标志如下图所示。

![](./images/4-1Z415145F32A.gif)

有意思的是，Docker 公司起初是一家名为 dotCloud 的平台即服务（Platform-as-a-Service, PaaS）提供商。

底层技术上，dotCloud 平台利用了 Linux 容器技术。为了方便创建和管理这些容器，dotCloud 开发了一套内部工具，之后被命名为“Docker”。Docker就是这样诞生的！

2013年，dotCloud 的 PaaS 业务并不景气，公司需要寻求新的突破。于是他们聘请了 Ben Golub 作为新的 CEO，将公司重命名为“Docker”，放弃dotCloud PaaS 平台，怀揣着“将 Docker 和容器技术推向全世界”的使命，开启了一段新的征程。

如今 Docker 公司被普遍认为是一家创新型科技公司，据说其市场价值约为 10 亿美元。Docker 公司已经通过多轮融资，吸纳了来自硅谷的几家风投公司的累计超过 2.4 亿美元的投资。

几乎所有的融资都发生在公司更名为“Docker”之后。
> 提示：“Docker”一词来自英国口语，意为码头工人（Dock Worker），即从船上装卸货物的人。

## Docker 运行时与编排引擎

多数技术人员在谈到 Docker 时，主要是指 Docker 引擎。

Docker 引擎是用于运行和编排容器的基础设施工具。有 VMware 管理经验的读者可以将其类比为 ESXi。

ESXi 是运行虚拟机的核心管理程序，而 Docker 引擎是运行容器的核心容器运行时。

其他 Docker 公司或第三方的产品都是围绕 Docker 引擎进行开发和集成的。

如下图所示，Docker 引擎位于中心，其他产品基于 Docker 引擎的核心功能进行集成。

![](./images/4-1Z415145K55S.gif)

Docker 引擎可以从 Docker 网站下载，也可以基于 GitHub 上的源码进行构建。无论是开源版本还是商业版本，都有 Linux 和 Windows 版本。

Docker 引擎主要有两个版本：企业版（EE）和社区版（CE）。

每个季度，企业版和社区版都会发布一个稳定版本。社区版本会提供 4 个月的支持，而企业版本会提供 12 个月的支持。

社区版还会通过 Edge 方式发布月度版。

从 2017 年第一季度开始，Docker 版本号遵循 YY.MM-xx 格式，类似于 Ubuntu 等项目。例如，2018 年 6 月第一次发布的社区版本为 18.06.0-ce。
注：2017 年第一季度以前，Docker 版本号遵循大版本号.小版本号的格式。采用新格式前的最后一个版本是 Docker 1.13。

## Docker开源项目（Moby）

“Docker”一词也会用于指代开源 Docker 项目。其中包含一系列可以从 Docker 官网下载和安装的工具，比如 Docker 服务端和 Docker 客户端。

不过，该项目在 2017 年于 Austin 举办的 DockerCon 上正式命名为 Moby 项目。

由于这次改名，GitHub 上的 docker/docker 库也被转移到了 moby/moby，并且拥有了项目自己的 Logo，如下图所示。

![](./images/4-1Z415145SWJ.gif)

Moby 项目的目标是基于开源的方式，发展成为 Docker 上游，并将 Docker 拆分为更多的模块化组件。

Moby 项目托管于 GitHub 的 Moby 代码库，包括子项目和工具列表。核心的 Docker 引擎项目位于 GitHub 的 moby/moby，但是引擎中的代码正持续被拆分和模块化。

作为一个开源项目，其源码是公开可得的，在遵循 Apache 协议 2.0 的情况下，任何人都可以自由地下载、贡献、调整和使用。

如果查看项目的提交历史，可以发现其中包含来自如下公司的基础技术：红帽、微软、IBM、思科，以及 HPE。此外，还可以看到一些并非来自大公司的贡献者。

多数项目及其工具都是基于 Golang 编写的，这是谷歌推出的一种新的系统级编程语言，又叫 Go 语言。使用 Go 语言的读者，将更容易为该项目贡献代码。

Mody/Docker 作为开源项目的好处在于其所有的设计和开发都是开放的，并摒弃了私有代码闭源开发模式下的陈旧方法。

因此发布过程也是公开进行的，不会再出现某个秘密的版本提前几个月就宣布要召开发布会和庆功会的荒唐情况。

Moby/Docker 不是这样运作的，项目中多数内容都是开放并欢迎任何人查看和作出贡献的。

Moby 项目以及更广泛的 Docker 运动一时间掀起了一波热潮。GitHub 上已经有数以千计的提交请求（pull request），以及数以万计的基于容器化技术的项目了，更不用说 Docker Hub 上数十亿的镜像下载。

Moby 项目已经给软件产业带来了翻天覆地的变化。

这并非妄想，Docker 已经得到了广泛的应用！

## 容器生态

Docker 公司的一个核心哲学通常被称为“含电池，但可拆卸”（Batteries included but removable）。

意思是许多 Docker 内置的组件都可以替换为第三方的组件，网络技术栈就是一个很好的例子。

Docker 核心产品内置有网络解决方案。但是网络技术栈是可插拔的，这意味着 Docker 内置的网络方案可以被替换为第三方的方案。许多人都会这样使用。

早期的时候，经常出现第三方插件比 Docker 提供的内置组件更好的情况。然而这会对 Docker 公司的商业模式造成冲击。毕竟，Docker 公司需要依靠盈利来维持基业长青。

因此，“内置的电池”变得越来越好用了。这也导致了生态内部的紧张关系和竞争的加剧。

简单来说，Docker 内置的“电池”仍然是可插拔的，然而越来越不需要将它们移除了。

尽管如此，容器生态在一种良性的合作与竞争的平衡中还是得以繁荣发展。

在谈及容器生态时，人们经常使用到诸如“co-opetition”（意即合作与竞争，英文中 co-operation 与 competition 合并的词）与“frenemy”（英文中朋友 friend 与敌人 enemy 合并的词）这样的字眼。这是一个好现象！因为良性的竞争是创新之母。

## 开放容器计划

如果不谈及开放容器计划（The Open Container Initiative, OCI）的话，对 Docker 和容器生态的探讨总是不完整的。下图所示为 OCI 的Logo。

![](./images/4-1Z41514595K39.gif)

OCI 是一个旨在对容器基础架构中的基础组件（如镜像格式与容器运行时）进行标准化的管理委员会。

同样，如果不谈历史的话，对 OCI 的探讨也是不完整的。

一个名为 CoreOS 的公司不喜欢 Docker 的某些行事方式。因此它就创建了一个新的开源标准，称作“appc”，该标准涉及诸如镜像格式和容器运行时等方面。

此外它还开发了一个名为 rkt（发音“rocket”）的实现。

两个处于竞争状态的标准将容器生态置于一种尴尬的境地。

这使容器生态陷入了分裂的危险中，同时也令用户和消费者陷入两难。虽然竞争是一件好事，但是标准的竞争通常不是。因为它会导致困扰，降低用户接受度，对谁都无益。

考虑到这一点，所有相关方都尽力用成熟的方式处理此事，共同成立了 OCI ——一个旨在管理容器标准的轻量级的、敏捷型的委员会。

OCI 已经发布了两份规范（标准）：镜像规范和运行时规范。

提到这两项标准时，经常用到的比喻就是铁轨。它们就像对铁轨的尺寸和相关属性达成一致，让所有人都能自由地建造更好的火车、更好的车厢、更好的信号系统、更好的车站等。

只要各方都遵循标准就是安全的。没人会希望在铁轨尺寸问题上存在两个相互竞争的标准！

公平地说，这两个 OCI 规范对 Docker 的架构和核心产品设计产生了显著影响。Docker 1.11 版本中，Docker 引擎架构已经遵循 OCI 运行时规范了。

到目前为止，OCI 已经取得了不错的成效，将容器生态团结起来。然而，标准总是会减慢创新的步伐！尤其是对于超快速发展的新技术来说更是如此。

这在容器社区引起了热烈的讨论。这应该算是好事！容器技术正在重塑世界，走在技术前列的人们有热情、有想法，这很正常。

OCI 在 Linux 基金会的支持下运作，Docker 公司和 CoreOS 公司都是主要贡献者。