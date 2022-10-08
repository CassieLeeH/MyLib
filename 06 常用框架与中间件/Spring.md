# 基础概念
Spring 框架是很多模块的集合，使用这些模块可以很方便地协助我们进行开发，比如说 Spring 支持 IoC（Inverse of Control:控制反转） 和 AOP(Aspect-Oriented Programming:面向切面编程)、可以很方便地对数据库进行访问、可以很方便地集成第三方组件（电子邮件，任务，调度，缓存等等）、对单元测试支持比较好、支持 RESTful Java 应用程序的开发。
Spring 最核心的思想就是不重新造轮子，开箱即用，提高开发效率。
语言的流行通常需要一个杀手级的应用，Spring 就是 Java 生态的一个杀手级的应用框架。
![](../assets/Pasted%20image%2020220901171255.png)
## Core Container
Spring 框架的核心模块，也可以说是基础模块，主要提供 IoC 依赖注入功能的支持。Spring 其他所有的功能基本都需要依赖于该模块，我们从上面那张 Spring 各个模块的依赖关系图就可以看出来。
-   **spring-core** ：Spring 框架基本的核心工具类。
-   **spring-beans** ：提供对 bean 的创建、配置和管理等功能的支持。
-   **spring-context** ：提供对国际化、事件传播、资源加载等功能的支持。
-   **spring-expression** ：提供对表达式语言（Spring Expression Language） SpEL 的支持，只依赖于 core 模块，不依赖于其他模块，可以单独使用。

## AOP
-   **spring-aspects** ：该模块为与 AspectJ 的集成提供支持。
-   **spring-aop** ：提供了面向切面的编程实现。
-   **spring-instrument** ：提供了为 JVM 添加代理（agent）的功能。 具体来讲，它为 Tomcat 提供了一个织入代理，能够为 Tomcat 传递类文 件，就像这些文件是被类加载器加载的一样。没有理解也没关系，这个模块的使用场景非常有限。

## Data Access/Integration
-   **spring-jdbc** ：提供了对数据库访问的抽象 JDBC。不同的数据库都有自己独立的 API 用于操作数据库，而 Java 程序只需要和 JDBC API 交互，这样就屏蔽了数据库的影响。
-   **spring-tx** ：提供对事务的支持。
-   **spring-orm** ： 提供对 Hibernate、JPA 、iBatis 等 ORM 框架的支持。
-   **spring-oxm** ：提供一个抽象层支撑 OXM(Object-to-XML-Mapping)，例如：JAXB、Castor、XMLBeans、JiBX 和 XStream 等。
-   **spring-jms** : 消息服务。自 Spring Framework 4.1 以后，它还提供了对 spring-messaging 模块的继承。

## Spring Web
-   **spring-web** ：对 Web 功能的实现提供一些最基础的支持。
-   **spring-webmvc** ： 提供对 Spring MVC 的实现。
-   **spring-websocket** ： 提供了对 WebSocket 的支持，WebSocket 可以让客户端和服务端进行双向通信。
-   **spring-webflux** ：提供对 WebFlux 的支持。WebFlux 是 Spring Framework 5.0 中引入的新的响应式框架。与 Spring MVC 不同，它不需要 Servlet API，是完全异步。

## Messaging
**spring-messaging** 是从 Spring4.0 开始新加入的一个模块，主要职责是为 Spring 框架集成一些基础的报文传送应用。

## Spring Test
Spring 团队提倡测试驱动开发（TDD）。有了控制反转 (IoC)的帮助，单元测试和集成测试变得更简单。
Spring 的测试模块对 JUnit（单元测试框架）、TestNG（类似 JUnit）、Mockito（主要用来 Mock 对象）、PowerMock（解决 Mockito 的问题比如无法模拟 final, static， private 方法）等等常用的测试框架支持的都比较好

# Ioc


# AoP


# MVC


# 事务


# 设计模式


