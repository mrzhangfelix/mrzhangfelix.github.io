#  SpringWebflux

   **Spring WebFlux 是一个异步非阻塞式的 Web 框架，它能够充分利用多核 CPU 的硬件资源去处理大量的并发请求。**

   WebFlux 内部使用的是响应式编程（Reactive Programming），以 Reactor 库为基础, 基于异步和事件驱动，可以让我们在不扩充硬件资源的前提下，提升系统的吞吐量和伸缩性。

   **WebFlux 并不能使接口的响应时间缩短，它仅仅能够提升吞吐量和伸缩性**。

   上面说到了， Spring WebFlux 是一个异步非阻塞式的 Web 框架，所以，它特别适合应用在 IO 密集型的服务中，比如微服务网关这样的应用中。

​    

   PS: IO 密集型包括：**磁盘IO密集型**, **网络IO密集型**，微服务网关就属于网络 IO 密集型，使用异步非阻塞式编程模型，能够显著地提升网关对下游服务转发的吞吐量。

​    

   **在合适的场景中，选型最合适的技术**。

   - Spring MVC     因为是使用的同步阻塞式，更方便开发人员编写功能代码，Debug 测试等，一般来说，如果 Spring MVC 能够满足的场景，就尽量不要用     WebFlux;   

   - WebFlux     默认情况下使用 Netty 作为服务器;
   - WebFlux 不支持     MySql;

   1、新增的模块，用于web开发的，类似于SpringMVC，Webflux使用比较流行的响应式编程。

   2、传统web框架，SpringMvc，这些事基于servlet容器，Webflux是一种异步非阻塞的框架，一部非阻塞在Servlet3.1以后才支持，核心是基于Reactor的相关API实现的

   3、同步和异步，调用者发送请求等待对方的回应才去做其他事情就是同步

   阻塞和非阻塞  针对被调用者，被调用者收到请求后先反馈在做事情就是非阻塞。

   4、Webflux特点：异步非阻塞在有限的资源下，提高系统的吞吐量和伸缩性，实现响应式编程

   使用java8的函数式编程实现路由请求

   5、和SpringMvc的比较

   1、都可以使用注解方式，都运行在Tomcat等容器中。

   2、SpringMvc使用命令式编程

​    

## 响应式编程和实现方式

   面向数据流和变化传播的编程范式，自动将变化的值通过数据流进行传播

   Java8：提供的观察者模式，Observer和Observable，响应式编程是观察者模式的一种

   Reactor实现：

   	两个核心类Mono和Flux，这两个类都实现接口Publisher，提供丰富的操作符，Flux对象实现发布者，返回N个元素；Mono实现发布者，返回0个活1个元素，都可以发出三种数据信号，元素值，错误信号，完成信号

   	错误和完成信号不能共存

   	需要订阅才能出发数据流

   	操作符：map和flatMap

   ## Webflux的执行流程和核心API

   1、基于Reactor，默认使用容器是Netty，Netty是高性能的NIO框架，异步非阻塞的框架。

   2、执行过程与MVC相似。Webflux核心控制器是DispatchHandler，实现接口WebHandler，负责请求的处理

   - HandlerMapping 请求查询到处理的方法
   - HandlerAdapter 真正负责请求处理
   - HandlerReultHandler     响应结果处理

   实现函数式编程的两个接口

   - RouterFunction 路由处理
   - HandlerFunction 处理函数

   两种实现方式

   1、使用注解编程模式方式，和之前的SpringMVC使用相似的，只需要把相关依赖配置导入项目中，SpringBoot自动配置相关容器，默认情况下使用Netty服务器。

   	SpringMVC方式：同步阻塞的方式，基于SpringMVC+Servlet+Tomcat

   	SpringWebflux方式实现，异步非阻塞的方式，基于SpringWebflux+Reactor+Netty

   2、函数式编程模型：

   	需要自己初始化服务器

   	核心任务是定义两个函数式接口的实现并启动需要的服务器（RouterFunction和HandlerFunction）

   	SpringWebflux请求和响应不在是ServletRequest和ServletResponse，而是ServerRequest和ServerResponse