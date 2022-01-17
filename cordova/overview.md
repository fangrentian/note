## 什么是cordova

Cordova 是使用HTML，CSS和JavaScript构建混合移动应用程序的平台。Cordova是PhoneGap贡献给Apache后的开源项目，是从PhoneGap中抽出的核心代码，是驱动PhoneGap的核心引擎。你可以把它们的关系想象成类似于Webkit和Google Chrome的关系 。官方文档给了我们Cordova的定义:

> Apache Cordova是一个开源移动开发框架，它允许您使用标准的Web技术，如HTML5，CSS3和JavaScript进行跨平台开发，避免每个移动平台本机开发语言。应用程序在针对每个平台的包装内执行，并依靠符合标准的API绑定来访问每个设备的传感器，数据和网络状态。

Cordova提供了一组设备相关的API，通过这组API，移动应用能够以JavaScript访问原生的设备功能，如摄像头、麦克风等。

Cordova还提供了一组统一的JavaScript类库，以及为这些类库所用的设备相关的原生后台代码。

Cordova支持如下移动操作系统:iOS, Android,ubuntu phone os, Blackberry, Windows Phone, Palm WebOS, Bada 和 Symbian。

**官网** [https://cordova.apache.org/](https://cordova.apache.org/)

**中文网** [http://cordova.axuer.com/](http://cordova.axuer.com/)

## 历史背景

Adobe将会继续以Cordova加上PhoneGap Build和Adobe Shadow的组合提供PhoneGap。 早在2011年10月，Adobe收购了Nitobi Software和它的PhoneGap产品，然后宣布这个移动开发框架将会继续开源，并把它提交到Apache Incubator，以便完全接受ASF的管治。我们想知道为什么Adobe会收购Nitobi并开源PhoneGap，尤其是为什么PhoneGap还会继续，如果另一个项目应该完成它的工作?

最近Adobe出现了一系列的沟通问题，包括处理Flash、Flex、AIR和PhoneGap的过渡问题。几个月之后，Adobe终于搞清楚他们对Flash和Flex的规划了，发帖澄清围绕着PhoneGap的一些谜团。

PhoneGap的项目主管Brian LeRoux指出开源PhoneGap的决定在Adobe收购Nitobi之前就做出了，由于Adobe拥有PhoneGap商标，他们不得不换个名字。第一个选中的名字是Callback，毫无创意，因此再改一次，产品叫Apache Cordova。

虽然很多人认为PhoneGap这个名字不会再用，因为代码已在一个不同的名字下面，但现实的情况是，Adobe想继续在PhoneGap品牌下提供Cordova。在不久的将来，Adobe会把Cordova、PhoneGap Build(一个在线应用程序构建服务)和Adobe Shadow(一个检查和预览工具)打包起来，将来很可能还会向PhoneGap包添加更多移动开发工具。

还不清楚Adobe是否会巩固PhoneGap品牌，虽然开发者对它已经耳熟能详，或者是否换成另一个名字。此外，也不清楚他们是否会在Cordova代码之上构建私有代码，但LeRoux的帖子留下了线索:"(PhoneGap和Apache Cordova之间的)唯一区别是下载的包的名字，这会持续一段时间(加重语气)。"

在微软2014年11月12日发布的Visual Studio 2015预览版本中已经集成了Cordova开发的相关工具，支持Apache Cordova 4.0.0，可以很方便的直接在上面开发Android, iOS以及Windows Phone的应用。同时该预览版本中还集成了Android模拟器以配合Android的开发调试。

## Cordova特征

### 命令行界面(Cordova CLI)

这是可用于启动项目，构建不同平台的进程，安装插件和许多其他有用的东西，使开发过程更容易的工具。您将在以后的章节中学习如何使用。

### Cordova核心组件

Cordova提供了每个移动应用程序所需的核心组件。这些组件将用于创建应用程序的基础，所以我们可以花更多的时间来实现我们自己的逻辑。

### Cordova外挂程式

Cordova提供的API将用于实现本地移动功能到我们的JavaScript应用程序。

### 许可证

Cordova按照Apache许可证2.0版授权。 Apache和Apache羽毛标志是Apache软件基金会的商标。

## Cordova的优势

* Cordova为构建混合移动应用程序提供了一个平台，因此我们可以开发一个应用程序，将在不同的移动平台IOS，Android，Windows Phone，Amazon-fireos，黑莓，Firefox OS，Ubuntu和tizien上使用。

* 开发混合应用程序然后原生应用程序更快，所以Cordova可以节省大量的开发时间。

* 由于我们在使用Cordova时使用JavaScript，我们不需要学习平台特定的编程语言。

* 有大量的社区插件可以与Cordova一起使用。许多库和框架都经过优化以便使用它。

## Cordova限制

* 混合应用程序比本地应用程序慢，因此对于需要大量数据和功能的大型应用程序使用Cordova不是最佳选择。

* 跨浏览器兼容性可能会产生很多问题。大多数时候，我们为不同的平台构建应用程序，所以测试和优化可能需要很多时间，因为我们需要覆盖大量的设备和操作系统。

* 某些插件与不同的设备和平台存在兼容性问题。还有一些Cordova尚不支持的本机API。
