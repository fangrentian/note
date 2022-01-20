# 什么是springboot

Spring Boot是由Pivotal团队提供的全新框架，其设计目的是用来简化Spring应用的创建、运行、调试、部署等。使用Spring Boot可以做到专注于Spring应用的开发，而无需过多关注XML的配置。Spring Boot使用“习惯优于配置”的理念，简单来说，它提供了一堆依赖打包，并已经按照使用习惯解决了依赖问题。使用Spring Boot可以不用或者只需要很少的Spring配置就可以让企业项目快速运行起来。

Spring Boot是开发者和Spring 本身框架的中间层，帮助开发者统筹管理应用的配置，提供基于实际开发中常见配置的默认处理（即习惯优于配置），简化应用的开发，简化应用的运维；总的来说，其目的Spring Boot就是为了对Java web 的开发进行“简化”和加“快”速度，简化开发过程中引入或启动相关Spring 功能的配置。这样带来的好处就是降低开发人员对于框架的关注点，可以把更多的精力放在自己的业务代码上。

同时随着微服务概念的推广和实践，Spring Boot的精简理念又使其成为Java微服务开发的不二之选，也可以说，Spring Boot其实就是为了微服务而生的Java web框架。

2013年，Pivotal团队开始研发SpringBoot。

2014年4月，发布全新开源的轻量级框架的第一个SpringBoot版本。

**参考文档**

[http://felord.cn/_doc/_springboot/2.1.5.RELEASE/_book/](http://felord.cn/_doc/_springboot/2.1.5.RELEASE/_book/)

[https://www.docs4dev.com/docs/zh/spring-boot/2.1.1.RELEASE/reference/](https://www.docs4dev.com/docs/zh/spring-boot/2.1.1.RELEASE/reference/)

# Spring框架介绍

Spring框架是Java平台上的一种开源应用框架，提供具有控制反转特性的容器。尽管Spring框架自身对编程模型没有限制，但其在Java应用中的频繁使用让它备受青睐，以至于后来让它作为EJB（EnterpriseJavaBeans）模型的补充，甚至是替补。Spring框架为开发提供了一系列的解决方案，比如利用控制反转的核心特性，并通过依赖注入实现控制反转来实现管理对象生命周期容器化，利用面向切面编程进行声明式的事务管理，整合多种持久化技术管理数据访问，提供大量优秀的Web框架方便开发等等。Spring框架具有控制反转（IOC）特性，IOC旨在方便项目维护和测试，它提供了一种通过Java的反射机制对Java对象进行统一的配置和管理的方法。Spring框架利用容器管理对象的生命周期，容器可以通过扫描XML文件或类上特定Java注解来配置对象，开发者可以通过依赖查找或依赖注入来获得对象。Spring框架具有面向切面编程（AOP）框架，SpringAOP框架基于代理模式，同时运行时可配置；AOP框架主要针对模块之间的交叉关注点进行模块化。Spring框架的AOP框架仅提供基本的AOP特性，虽无法与AspectJ框架相比，但通过与AspectJ的集成，也可以满足基本需求。Spring框架下的事务管理、远程访问等功能均可以通过使用SpringAOP技术实现。Spring的事务管理框架为Java平台带来了一种抽象机制，使本地和全局事务以及嵌套事务能够与保存点一起工作，并且几乎可以在Java平台的任何环境中工作。Spring集成多种事务模板，系统可以通过事务模板、XML或Java注解进行事务配置，并且事务框架集成了消息传递和缓存等功能。Spring的数据访问框架解决了开发人员在应用程序中使用数据库时遇到的常见困难。它不仅对Java:JDBC、iBATS/MyBATIs、Hibernate、Java数据对象（JDO）、ApacheOJB和ApacheCayne等所有流行的数据访问框架中提供支持，同时还可以与Spring的事务管理一起使用，为数据访问提供了灵活的抽象。Spring框架最初是没有打算构建一个自己的WebMVC框架，其开发人员在开发过程中认为现有的StrutsWeb框架的呈现层和请求处理层之间以及请求处理层和模型之间的分离不够，于是创建了SpringMVC。

# 特点

SpringBoot基于Spring4.0设计，不仅继承了Spring框架原有的优秀特性，而且还通过简化配置来进一步简化了Spring应用的整个搭建和开发过程。另外SpringBoot通过集成大量的框架使得依赖包的版本冲突，以及引用的不稳定性等问题得到了很好的解决。
SpringBoot所具备的特征有：

1. 可以创建独立的Spring应用程序，并且基于其Maven或Gradle插件，可以创建可执行的JARs和WARs；
2. 内嵌Tomcat或Jetty等Servlet容器；
3. 提供自动配置的“starter”项目对象模型（POMS）以简化Maven配置；
4. 尽可能自动配置Spring容器；
5. 提供准备好的特性，如指标、健康检查和外部化配置；
6. 绝对没有代码生成，不需要XML配置。

# 重要策略

SpringBoot框架中还有两个非常重要的策略：开箱即用和约定优于配置。开箱即用，Outofbox，是指在开发过程中，通过在MAVEN项目的pom文件中添加相关依赖包，然后使用对应注解来代替繁琐的XML配置文件以管理对象的生命周期。这个特点使得开发人员摆脱了复杂的配置工作以及依赖的管理工作，更加专注于业务逻辑。约定优于配置，Convention over configuration，是一种由SpringBoot本身来配置目标结构，由开发者在结构中添加信息的软件设计范式。这一特点虽降低了部分灵活性，增加了BUG定位的复杂性，但减少了开发人员需要做出决定的数量，同时减少了大量的XML配置，并且可以将代码编译、测试和打包等工作自动化。

SpringBoot应用系统开发模板的基本架构设计从前端到后台进行说明：前端常使用模板引擎，主要有FreeMarker和Thymeleaf，它们都是用Java语言编写的，渲染模板并输出相应文本，使得界面的设计与应用的逻辑分离，同时前端开发还会使用到Bootstrap、AngularJS、JQuery等；在浏览器的数据传输格式上采用Json，非xml，同时提供RESTfulAPI；SpringMVC框架用于数据到达服务器后处理请求；到数据访问层主要有Hibernate、MyBatis、JPA等持久层框架；数据库常用MySQL；开发工具推荐IntelliJIDEA。

# 核心功能

1. 可独立运行的Spring项目：Spring Boot可以以jar包的形式独立运行。
2. 内嵌的Servlet容器：Spring Boot可以选择内嵌Tomcat、Jetty或者Undertow，无须以war包形式部署项目。
3. 简化的Maven配置：Spring提供推荐的基础 POM 文件来简化Maven 配置。
4. 自动配置Spring：Spring Boot会根据项目依赖来自动配置Spring 框架，极大地减少项目要使用的配置。
5. 提供生产就绪型功能：提供可以直接在生产环境中使用的功能，如性能指标、应用信息和应用健康检查。
6. 无代码生成和xml配置：Spring Boot不生成代码。完全不需要任何xml配置即可实现Spring的所有配置。

# SpringBoot与SpringCloud 的区别

1. SpringBoot快速开发框架，快速整合第三方框架（Maven依赖关系###Maven继承），完全采用注解化，简化XML配置，最终以java应用程序进行执行。
2. SpringCloud一套目前完整的微服务解决框架，功能非常强大，注册中心，客户端调用工具，服务治理（负载均衡，断路器，分布式配置中心，网管，消息总线等）。
3. 关系：微服务通讯技术Http+json（restfull）轻量级，SpringBoot Web组件默认集成SpringMVC,SpringCloud依赖于SpringBoot实现微服务，使用SpringMVC编写微服务接口。

# SpringBoot与SpringMVC 的区别

SpringBoot Web组件默认集成SpringMVC框架,SpringMVC3.0以后支持注解方式使用java代码启动SpringMVC。

# springboot优点

## 强大的基因

因为SpringBoot是伴随着Spring 4.0而生的，boot是引导的意思，也就是它的作用其实就是在于帮助开发者快速的搭建Spring框架，因此SpringBoot继承了Spring优秀的基因，在Spring中开发更为方便快捷。

## 简化编码

比如我们要创建一个 web 项目，使用 Spring 的朋友都知道，在使用 Spring 的时候，需要在 pom 文件中添加多个依赖，而 Spring Boot 则会帮助开发着快速启动一个 web 容器，在 Spring Boot 中，我们只需要在 pom 文件中添加如下一个 starter-web 依赖即可。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

我们点击进入该依赖后可以看到，Spring Boot 这个 starter-web 已经包含了多个依赖，包括之前在 Spring 工程中需要导入的依赖，我们看一下其中的一部分，如下：

```xml
<!-- .....省略其他依赖 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.0.7.RELEASE</version>
    <scope>compile</scope>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.0.7.RELEASE</version>
    <scope>compile</scope>
</dependency>
```

由此可以看出，Spring Boot 大大简化了我们的编码，我们不用一个个导入依赖，直接一个依赖即可。

## 简化配置

Spring 虽然使Java EE轻量级框架，但由于其繁琐的配置，一度被人认为是“配置地狱”。各种XML、Annotation配置会让人眼花缭乱，而且配置多的话，如果出错了也很难找出原因。Spring Boot更多的是采用 Java Config 的方式，对 Spring 进行配置。举个例子：

我新建一个类，但是我不用 @Service注解，也就是说，它是个普通的类，那么我们如何使它也成为一个 Bean 让 Spring 去管理呢？只需要@Configuration 和@Bean两个注解即可，如下：

```java
public class TestService {
    public String sayHello () {
        return "Hello Spring Boot!";
    }
}
 
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
 
@Configuration
public class JavaConfig {
    @Bean
    public TestService getTestService() {
        return new TestService();
    }
}
```

@Configuration表示该类是个配置类，@Bean表示该方法返回一个 Bean。这样就把TestService作为 Bean 让 Spring 去管理了，在其他地方，我们如果需要使用该 Bean，和原来一样，直接使用@Resource注解注入进来即可使用，非常方便。

@Resource private TestService testService;

另外，部署配置方面，原来 Spring 有多个 xml 和 properties配置，在 Spring Boot 中只需要个 application.yml即可。

## 简化部署

在使用 Spring 时，项目部署时需要我们在服务器上部署 tomcat，然后把项目打成 war 包扔到 tomcat里，在使用 Spring Boot 后，我们不需要在服务器上去部署 tomcat，因为 Spring Boot 内嵌了 tomcat，我们只需要将项目打成 jar 包，使用 java -jar xxx.jar一键式启动项目。
另外，也降低对运行环境的基本要求，环境变量中有JDK即可。

## 简化监控

我们可以引入 spring-boot-start-actuator 依赖，直接使用 REST 方式来获取进程的运行期性能参数，从而达到监控的目的，比较方便。但是 Spring Boot 只是个微框架，没有提供相应的服务发现与注册的配套功能，没有外围监控集成方案，没有外围安全管理方案，所以在微服务架构中，还需要 Spring Cloud 来配合一起使用。
