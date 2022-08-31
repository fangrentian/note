# 什么是nodejs

Node.js 是一个开源和跨平台的 JavaScript 运行时环境。 它几乎是任何类型项目的流行工具！

Node.js 在浏览器之外运行 V8 JavaScript 引擎（Google Chrome 的内核）。 这使得 Node.js 的性能非常好。

Node.js 应用程序在单个进程中运行，无需为每个请求创建新的线程。 Node.js 在其标准库中提供了一组异步的 I/O 原语，以防止 JavaScript 代码阻塞，通常，Node.js 中的库是使用非阻塞范式编写的，使得阻塞行为成为异常而不是常态。

当 Node.js 执行 I/O 操作时（比如从网络读取、访问数据库或文件系统），Node.js 将在响应返回时恢复操作（而不是阻塞线程和浪费 CPU 周期等待）。

这允许 Node.js 使用单个服务器处理数千个并发连接，而​​不会引入管理线程并发（这可能是错误的重要来源）的负担。

Node.js 具有独特的优势，因为数百万为浏览器编写 JavaScript 的前端开发者现在无需学习完全不同的语言，就可以编写除客户端代码之外的服务器端代码。

在 Node.js 中，可以毫无问题地使用新的 ECMAScript 标准，因为你不必等待所有用户更新他们的浏览器，你负责通过更改 Node.js 版本来决定使用哪个 ECMAScript 版本，你还可以通过运行带有标志的 Node.js 来启用特定的实验性功能。

**参考文档**

[http://nodejs.cn/](http://nodejs.cn/)

[https://www.w3cschool.cn/nodejs/dict](https://www.w3cschool.cn/nodejs/dict)

# 仓库

npm 以其简单的结构帮助 Node.js 生态系统蓬勃发展，现在 npm 仓库托管了超过 1,000,000 个开源包，你可以自由使用。

# 历史背景

Ryan Dahl是一名资深的C/C++程序员，在创造出Node之前，他的主要工作都是围绕高性能 Web服务器进行的。经历过一些尝试和失败之后，他找到了设计高性能，Web服务器的几个要点： 事件驱动、非阻塞I/O，而这也正是nodejs的两大特点。
所以Ryan Dahl最初的目标是写一个基于事件驱动、非阻塞I/O的Web服务器，以达到更高的性能，提供Apache等服务器之外的选择。写Node的时候，Ryan Dahl曾经评估过C、Lua、Haskell、 Ruby等语言作为备选实现，结论为：C的开发门槛高，可以预见不会有太多的开发者能将它用于日常的业务开发，所以舍弃它；Ryan Dahl觉得自己还不足够玩转Haskell，所以舍弃它；Lua自身已经含有很多阻塞I/O库，为其构建非阻塞I/O库也不能改变人们继续使用阻塞I/O库的习惯，所以也舍弃它；而Ruby的虚拟机由于性能不好而落选。
相比之下，JavaScript比C的开发门槛要低，比Lua的历史包袱要少。尽管服务器端JavaScript存在已经很多年了，但是后端部分一直没有市场，可以说历史包袱为零，为其导入非阻塞I/O库没有额外阻力。另外，JavaScript在浏览器中有广泛的事件驱动方面的应用，暗合Ryan Dahl喜好基于事件驱动的需求。当时，第二次浏览器大战也渐渐分出高下，Chrome浏览器的JavaScript引擎V8摘得性能第一的桂冠。考虑到高性能、符合事件驱动、没有历史包袱这3个主要原因，JavaScript成为了Node的实现语言。
起初，Ryan Dahl称他的项目为web.js，就是一个Web服务器，但是项目的发展超过了他最初单纯开发一个Web服务器的想法，变成了构建网络应用的一个基础框架，这样可以在它的基础上构建更多的东西，诸如服务器、客户端、命令行工具等。Node发展为一个强制不共享任何资源的单线程、单进程系统，包含十分适宜网络的库，为构建大型分布式应用程序提供基础设施，其目标也是成为一个构建快速、可伸缩的网络应用平台。它自身非常简单，通过通信协议来组织许多Node，非常容易通过扩展来达成构建大型网络应用的目的。每一个Node进程都构成这个网络应用中的一个节点，这是它名字所含意义的真谛。

# 发展历程

* 2009年2月，Ryan Dahl在博客上宣布准备基于V8创建一个轻量级的Web服务器并提供一套库。

* 2009年5月，Ryan Dahl在GitHub上发布了最初版本的部分Node包，随后几个月里，有人开始使用Node开发应用。

* 2009年11月和2010年4月，两届JSConf大会都安排了Node.js的讲座。

* 2010年年底，Node获得云计算服务商Joyent资助，创始人Ryan Dahl加入Joyent全职负责Node的发展。

* 2011年7月，Node在微软的支持下发布Windows版本

# 主要功能

V8引擎本身使用了一些最新的编译技术。这使得用Javascript这类脚本语言编写出来的代码运行速度获得了极大提升，又节省了开发成本。对性能的苛求是Node的一个关键因素。 Javascript是一个事件驱动语言，Node利用了这个优点，编写出可扩展性高的服务器。Node采用了一个称为“事件循环(event loop）”的架构，使得编写可扩展性高的服务器变得既容易又安全。提高服务器性能的技巧有多种多样。Node选择了一种既能提高性能，又能减低开发复杂度的架构。这是一个非常重要的特性。并发编程通常很复杂且布满地雷。Node绕过了这些，但仍提供很好的性能。
Node采用一系列“非阻塞”库来支持事件循环的方式。本质上就是为文件系统、数据库之类的资源提供接口。向文件系统发送一个请求时，无需等待硬盘（寻址并检索文件），硬盘准备好的时候非阻塞接口会通知Node。该模型以可扩展的方式简化了对慢资源的访问， 直观，易懂。尤其是对于熟悉onmouseover、onclick等DOM事件的用户，更有一种似曾相识的感觉。
虽然让Javascript运行于服务器端不是Node的独特之处，但却是其一强大功能。不得不承认，浏览器环境限制了我们选择编程语言的自由。任何服务器与日益复杂的浏览器客户端应用程序间共享代码的愿望只能通过Javascript来实现。虽然还存在其他一些支持Javascript在服务器端 运行的平台，但因为上述特性，Node发展迅猛，成为事实上的平台。

# 特点

作为后端JavaScript的运行平台，Node保留了前端浏览器JavaScript中那些熟悉的接口，没有改写语言本身的任何特性，依旧基于作用域和原型链，区别在于它将前端中广泛运用的思想迁移到了服务器端。Node相较于其他语言的特点如下所示：

1. 异步I/O
在Node中，绝大多数的操作都以异步的方式进行调用。Ryan Dahl排除万难，在底层构建了很多异步I/O的API，从文件读取到网络请求等，均是如此。这样的意义在于，在Node中，我们可 以从语言层面很自然地进行并行I/O操作。每个调用之间无须等待之前的I/O调用结束。在编程模型上可以极大提升效率。
以同时执行两个文件读取任务为例，异步I/O取决于最慢的那个文件读取的耗时，而同步I/O的耗时是两个任务的耗时之和。这里异步带来的优势是显而易见的。

2. 事件
随着Web 2.0时代的到来，JavaScript在前端担任了更多的职责，事件也得到了广泛的应用。 Node不像Rhino那样受Java的影响很大，而是将前端浏览器中应用广泛且成熟的事件引入后端， 配合异步I/O，将事件点暴露给业务逻辑。
事件的编程方式具有轻量级、松耦合、只关注事务点等优势，但是在多个异步任务的场景下，事件与事件之间各自独立，如何协作是一个问题。

3. 回调函数
与其他的Web后端编程语言相比，Node除了异步和事件外，回调函数是一大特色。纵观下来，回调函数也是最好的接受异步调用返回数据的方式。但是这种编程方式对于很多习惯同步思路编程的人来说，也许是十分不习惯的。代码的编写顺序与执行顺序并无关系，这对他们可能造成阅读上的障碍。在流程控制方面，因为穿插了异步方法和回调函数，与常规的同步方式相比，变得不那么一目了然了。

4. 单线程
JavaScript语言的一大特点就是单线程，也就是说，同一个时间只能做一件事。JavaScript的单线程，与它的用途有关。作为浏览器脚本语言，JavaScript的主要用途是与用户互动，以及操作DOM。这决定了它只能是单线程，否则会带来很复杂的同步问题。比如，假定JavaScript同时有两个线程，一个线程在某个DOM节点上添加内容，另一个线程删除了这个节点，这时浏览器应该以哪个线程为准？所以，为了避免复杂性，从一诞生，JavaScript就是单线程，这已经成了这门语言的核心特征。

Node保持了JavaScript在浏览器中单线程的特点。而且在Node中，JavaScript与其余线程是无法共享任何状态的。单线程的最大好处是不用像多线程编程那样处处在意状态的同步问题，这里没有死锁的存在，也没有线程上下文交换所带来的性能上的开销。

同样，单线程也有它自身的弱点，具体有以下3方面：无法利用多核CPU；错误会引起整个应用退出，应用的健壮性值得考验；大量计算占用CPU导致无法继续调用异步I/O。

像浏览器中JavaScript与UI共用一个线程一样，JavaScript长时间执行会导致UI的渲染和响应被中断。在Node中，长时间的CPU占用也会导致后续的异步I/O发不出调用，已完成的异步I/O的回调函数也会得不到及时执行。
HTML5定制了Web Workers的标准，Web Workers能够创建工作线程来进行计算，以解决JavaScript大计算阻塞UI渲染的问题。工作线程为了不阻塞主线程，通过消息传递的方式来传递运行结果，这也使得工作线程不能访问到主线程中的UI。
Node采用了与Web Workers相同的思路来解决单线程中大计算量的问题：child_process。 子进程的出现，意味着Node可以从容地应对单线程在健壮性和无法利用多核CPU方面的问题。通过将计算分发到各个子进程，可以将大量计算分解掉，然后再通过进程之间的事件消息来传递结果，这可以很好地保持应用模型的简单和低依赖。通过Master-Worker的管理方式，也可以很好地管理各个工作进程，以达到更高的健壮性。

# 应用场景

在进行技术选型之前，需要了解一项新技术具体适合什么样的场景，毕竟合适的技术用在合适的场景可以起到意想不到的效果。关于Node，探讨得较多的主要有I/O密集型和CPU密集型。

1. I/O密集型
如果将所有的脚本语言拿到一处来评判，那么从单线程的角度来说，Node处理I/O的能力是值得竖起拇指称赞的。通常， 说Node擅长I/O密集型的应用场景基本上是没人反对的。Node面向网络且擅长并行I/O，能够有效地组织起更多的硬件资源，从而提供更多好的服务。
I/O密集的优势主要在于Node利用事件循环的处理能力，而不是启动每一个线程为每一个请求服务，资源占用极少

2. CPU密集型
换一个角度，在CPU密集的应用场景中，Node是否能胜任呢？实际上，V8的执行效率是十分高的。单以执行效率来做评判，V8的执行效率是毋庸置疑的。
CPU密集型应用给Node带来的挑战主要是：由于JavaScript单线程的原因，如果有长时间运行的计算（比如大循环），将会导致CPU时间片不能释放，使得后续I/O无法发起。但是适当调整和分解大型运算任务为多个小任务，使得运算能够适时释放，不阻塞I/O调用的发起，这样既可同时享受到并行异步I/O的好处，又能充分利用CPU。

# 应用程序的示例

Node.js 中最常见的 Hello World 示例是 Web 服务器：

```js
const http = require('http')

const hostname = '127.0.0.1'
const port = 3000

const server = http.createServer((req, res) => {
    res.statusCode = 200
    res.setHeader('Content-Type', 'text/plain')
    res.end('Hello World\n')
})

server.listen(port, hostname, () => {
    console.log(`Server running at http://${hostname}:${port}/`)
})
```
要运行此代码片段，则将其另存为 server.js 文件并在终端中执行以下命令: 
```shell
node server.js
```

# 框架和工具

* [AdonisJS](https://adonisjs.com/)：基于 TypeScript 的全功能框架，高度关注开发者的效率、稳定和信任。Adonis 是最快的 Node.js Web 框架之一。

* [Egg.js](https://eggjs.org/en/)：使用 Node.js 和 Koa 构建更好的企业级框架和应用程序的框架。

* [Express](https://expressjs.com/)：提供了最简单而强大的方式来创建 Web 服务器。它的极简主义方法、没有偏见、专注于服务器的核心功能，是其成功的关键。

* [Fastify](https://fastify.io/)：高度专注于以最少的开销和强大的插件架构提供最佳开发者体验的 Web 框架。Fastify 是最快的 Node.js Web 之一 构架。

* [FeatherJS](https://feathersjs.com/)：Feathers 是轻量级的网络框架，用于使用 JavaScript 或 TypeScript 创建实时应用程序和 REST API。在几分钟内构建原型，在几天内构建可用于生产的应用程序。

* [Gatsby](https://www.gatsbyjs.com/)：基于 React、由 GraphQL 驱动的静态网站生成器，具有非常丰富的插件和启动器生态系统。

* [hapi](https://hapijs.com/)：用于构建应用程序和服务的富框架，使开发者能够专注于编写可重用的应用程序逻辑，而不是花时间搭建基础设施。

* [koa](http://koajs.com/)：由 Express 背后的同一个团队构建，旨在更简单、更小，建立在多年知识的基础上。新项目的诞生是为了在不破坏现有社区的情况下创建不兼容的更改。

* [Loopback.io](https://loopback.io/)：使构建需要复杂集成的现代应用程序变得容易。

* [Meteor](https://meteor.com/)：非常强大的全栈框架，为您提供同构的方法来使用 JavaScript 构建应用程序，在客户端和服务器上共享代码。曾经是提供所有功能的现成工具，现在可以与前端库 React、Vue 和 Angular 集成。也可用于创建移动应用程序。

* [Micro](https://github.com/zeit/micro)：提供了非常轻量级的服务器来创建异步的 HTTP 微服务。

* [NestJS](https://nestjs.com/)：基于 TypeScript 的渐进式 Node.js 框架，用于构建企业级的高效、可靠和可扩展的服务器端应用程序。

* [Next.js](https://nextjs.org/)：React 框架，为您提供最佳的开发者体验，包括生产所需的所有功能：混合静态和服务器渲染、TypeScript 支持、智能捆绑、路由预取等。

* [Nx](https://nx.dev/)：使用 NestJS、Express、React、Angular 等进行全栈大仓开发的工具包！Nx 有助于将您的开发从构建单个应用程序的团队扩展到多个团队协作开发多个应用程序！

* [Remix](https://remix.run/)：Remix 是一个全栈 Web 框架，用于为 web 构建出色的用户体验。它开箱即用，提供构建现代 web 应用程序所需的一切（包括前端和后端）并将其部署到任何基于 JavaScript 的运行时环境（包括 Node.js）。

* [Sapper](https://sapper.svelte.dev/)：Sapper 是用于构建各种规模的 Web 应用程序的框架，具有优美的开发体验和灵活的基于文件系统的路由。提供 SSR 等等！

* [Socket.io](https://socket.io/): 构建网络应用的实时通信引擎。

* [Strapi](https://strapi.io/)：Strapi 是灵活开源的 Headless CMS，让开发者可以自由选择他们喜欢的工具和框架，同时还允许编辑人员轻松管理和分发他们的内容。通过插件系统使管理面板和 API 可扩展，Strapi 使世界上最大的公司能够在构建精美的数字体验的同时加速内容交付。

* [thinkjs](https://thinkjs.org/zh-cn/doc/3.0/create_project.html): hinkJS 是一款面向未来开发的 Node.js 框架，整合了大量的项目最佳实践，让企业级开发变得更简单、高效。从 3.0 开始，框架底层基于 Koa 2.x 实现，兼容 Koa 的所有功能。
