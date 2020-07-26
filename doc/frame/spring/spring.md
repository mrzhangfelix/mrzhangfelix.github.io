

使用 Spring 框架来创建性能好、易于测试、可重用的代码。

# Spring框架的优点

1.方便解耦，简化开发
通过Spring提供的IoC容器，我们可以将对象之间的依赖关系交由Spring进行控制，避免硬编码所造成的过度程序耦合。有了Spring，用户不必再为单实例模式类、属性文件解析等这些很底层的需求编写代码，可以更专注于上层的应用。

2.AOP编程的支持
通过Spring提供的AOP功能，方便进行面向切面的编程，许多不容易用传统OOP实现的功能可以通过AOP轻松应付。

3.声明事物的支持
在Spring中，我们可以从单调烦闷的事务管理代码中解脱出来，通过声明式方式灵活地进行事务的管理，提高开发效率和质量。 支持 POJO(Plain Old Java Object) 编程，从而具备持续集成和可测试性。

4.方便程序的测试
可以用非容器依赖的编程方式进行几乎所有的测试工作，在Spring里，测试不再是昂贵的操作，而是随手可做的事情。例如：Spring对Junit4支持，可以通过注解方便的测试Spring程序。

5.方便集成各种优秀框架
Spring不排斥各种优秀的开源框架，相反，Spring可以降低各种框架的使用难度，Spring提供了对各种优秀框架（如Struts,Hibernate、Hessian、Quartz）等的直接支持。它是开源免费的。

# Spring Framework 有哪些不同的功能？

- **轻量级** - Spring 在代码量和透明度方面都很轻便。
- **IOC** - 控制反转
- **AOP** - 面向切面编程可以将应用业务逻辑和系统服务分离，以实现高内聚。
- **容器** - Spring 负责创建和管理对象（Bean）的生命周期和配置。
- **MVC** - 对 web 应用提供了高度可配置性，其他框架的集成也十分方便。
- **事务管理** - 提供了用于事务管理的通用抽象层。Spring 的事务支持也可用于容器较少的环境。
- **JDBC 异常** - Spring 的 JDBC 抽象层提供了一个异常层次结构，简化了错误处理策略。

## 依赖注入（DI）

IoC（Inverse of Control:控制反转）是一种**设计思想**，就是 **将原本在程序中手动创建对象的控制权，交由Spring框架来管理。**  IoC 在其他语言中也有应用，并非 Spirng 特有。 **IoC 容器是 Spring 用来实现 IoC 的载体，  IoC 容器实际上就是个Map（key，value）,Map 中存放的是各种对象。**

将对象之间的相互依赖关系交给 IOC 容器来管理，并由 IOC 容器完成对象的注入。这样可以很大程度上简化应用的开发，把应用从复杂的依赖关系中解放出来。  **IOC 容器就像是一个工厂一样，当我们需要创建一个对象的时候，只需要配置好配置文件/注解即可，完全不用考虑对象是如何被创建出来的。** 在实际项目中一个 Service 类可能有几百甚至上千个类作为它的底层，假如我们需要实例化这个 Service，你可能要每次都要搞清这个 Service 所有底层类的构造函数，这可能会把人逼疯。如果利用 IOC 的话，你只需要配置好，然后在需要的地方引用就行了，这大大增加了项目的可维护性且降低了开发难度



在依赖注入中，您不必创建对象，但必须描述如何创建它们。您不是直接在代码中将组件和服务连接在一起，而是描述配置文件中哪些组件需要哪些服务。由 IoC 容器将它们装配在一起。

### 依赖注入可以通过三种方式完成

- 构造函数注入

- setter 注入

- 接口注入

  创建对象和注入属性

  实现两种方式：

  1. XML文件方式，

  在spring配置文件中使用bean标签

  \- id属性，对象的标识，用于获取对象

  \- class属性，类的全路径

  默认执行无参方法

  DI：依赖注入，就是注入属性，是IOC的一个实现，创建对象时使用

  \- set方法注入

  使用property标签注入，ref属性可以注入外部bean

  <array><list><map><set>支持集合类型注入

### IoC 的一些好处是：

- 它将最小化应用程序中的代码量。
- 它将使您的应用程序易于测试，因为它不需要单元测试用例中的任何单例或 JNDI 查找机制。
- 它以最小的影响和最少的侵入机制促进松耦合。
- 它支持即时的实例化和延迟加载服务。

Spring 中的 IoC 的实现原理就是工厂模式加反射机制。

1、xml解析

2、工厂模式，降低耦合度，需要什么对象通过工厂的方法去创建，从而不需要和对象的类耦合

3、反射

## 面向方面的程序设计（AOP）：

AOP(Aspect-Oriented Programming:面向切面编程)能够将那些与业务无关，**却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来**，便于**减少系统的重复代码**，**降低模块间的耦合度**，并**有利于未来的可拓展性和可维护性**。

AOP：利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用行，同时提高开发效率

不通过修改源代码的方式，在主干功能里面添加新功能

底层使用动态代理方式

1. 有接口的情况，JDK动态代理

创建一个接口实现类的代理对象，增强类的方法

java.lang.reflect.Proxy

调用newProxyInstance方法

三个参数1、当前的类加载器	2、增强方法所在的类实现的接口，支持多个接口  3、实现这个接口InvacationHander，创建代理对象，写增强的方法。

```java

public class JDKProxy {

    public static void main(String[] args) {

        //创建接口实现类代理对象

        Class[] interfaces = {UserDao.class};

//        Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces, new InvocationHandler() {

//            @Override

//            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

//                return null;

//            }

//        });

        UserDaoImpl userDao = new UserDaoImpl();

        UserDao dao = (UserDao)Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces, new UserDaoProxy(userDao));

        int result = dao.add(1, 2);

        System.out.println("result:"+result);

    }

}

//创建代理对象代码

class UserDaoProxy implements InvocationHandler {

    //1 把创建的是谁的代理对象，把谁传递过来

    //有参数构造传递

    private Object obj;

    public UserDaoProxy(Object obj) {

        this.obj = obj;

    }

    //增强的逻辑

    @Override

    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

        //方法之前

        System.out.println("方法之前执行...."+method.getName()+" :传递的参数..."+ Arrays.toString(args));

        //被增强的方法执行

        Object res = method.invoke(obj, args);

        //方法之后

        System.out.println("方法之后执行...."+obj);

        return res;

    }

}

```

2.没有接口的情况，使用CGLIB动态代理

创建当前类子类的代理对象，增强类的方法





**Spring AOP就是基于动态代理的**

在不修改源代码的情况下，可以给目标类打补丁，让其执行补丁中的代码。

AOP(Aspect-Oriented Programming), 即 **面向切面编程**, 它与 OOP( Object-Oriented Programming, 面向对象编程) 相辅相成, 提供了与 OOP 不同的抽象软件结构的视角.
在 OOP 中, 我们以类(class)作为我们的基本单元, 而 AOP 中的基本单元是 **Aspect(切面)**



使用 AOP 之后我们可以把一些通用功能抽象出来，在需要用到的地方直接使用即可，这样大大简化了代码量。我们需要增加新功能时也方便，这样也提高了系统扩展性。日志功能、事务管理等等场景都用到了 AOP 。



当切面太多的话，最好选择 AspectJ ，它比Spring AOP 快很多。

### 什么是 切面Aspect？

`aspect` 由 `pointcount` 和 `advice` 组成, 它既包含了横切逻辑的定义, 也包括了连接点的定义. Spring AOP 就是负责实施切面的框架, 它将切面所定义的横切逻辑编织到切面所指定的连接点中.

 把通知应用到切入点的过程

AOP 的工作重心在于如何将增强编织目标对象的连接点上, 这里包含两个工作:

1. 如何通过 pointcut 和 advice 定位到特定的 joinpoint 上
2. 如何在 advice 中编写切面代码.

**可以简单地认为, 使用 @Aspect 注解的类就是切面.**

### 什么是切点（JoinPoint）

程序运行中的一些时间点, 例如一个方法的执行, 或者是一个异常的处理.

在 Spring AOP 中, join point 总是方法的执行点。

### 什么是通知（Advice）？

特定 JoinPoint 处的 Aspect 所采取的动作称为 Advice。Spring AOP 使用一个 Advice 作为拦截器，在 JoinPoint “周围”维护一系列的拦截器。

实际增强的逻辑部分被称为通知。

###  切入点

   实际被真正增强的方法，成为切入点

  execution（[权限修饰符][返回类型][类全路径][方法名称][参数列表]）

### 有哪些类型的通知（Advice）？

- **Before** - 这些类型的 Advice 在 joinpoint 方法之前执行，并使用 @Before 注解标记进行配置。
- **After Returning** - 这些类型的 Advice 在连接点方法正常执行后执行，并使用@AfterReturning 注解标记进行配置。
- **After Throwing** - 这些类型的 Advice 仅在 joinpoint 方法通过抛出异常退出并使用 @AfterThrowing 注解标记配置时执行。
- **After (finally)** - 这些类型的 Advice 在连接点方法之后执行，无论方法退出是正常还是异常返回，并使用 @After 注解标记进行配置。
- **Around** - 这些类型的 Advice 在连接点之前和之后执行，并使用 @Around 注解标记进行配置。

   相同的切入点可以进行抽取

   @Order 设置切面的优先级，值越小越先执行。

# 核心组件

核心容器由**spring-core，spring-beans，spring-context，spring-context-support和spring-expression**（SpEL，Spring表达式语言，Spring Expression Language）等模块组成，它们的细节如下：

- **spring-core**模块提供了框架的基本组成部分，包括 IoC 和依赖注入功能。

- **spring-beans** 模块提供 BeanFactory，工厂模式的微妙实现，它移除了编码式单例的需要，并且可以把配置和依赖从实际编码逻辑中解耦。

- **context**模块建立在由**core**和 **beans** 模块的基础上建立起来的，它以一种类似于JNDI注册的方式访问对象。Context模块继承自Bean模块，并且添加了国际化（比如，使用资源束）、事件传播、资源加载和透明地创建上下文（比如，通过Servelet容器）等功能。Context模块也支持Java EE的功能，比如EJB、JMX和远程调用等。**ApplicationContext**接口是Context模块的焦点。**spring-context-support**提供了对第三方库集成到Spring上下文的支持，比如缓存（EhCache, Guava, JCache）、邮件（JavaMail）、调度（CommonJ, Quartz）、模板引擎（FreeMarker, JasperReports, Velocity）等。

- **spring-expression**模块提供了强大的表达式语言，用于在运行时查询和操作对象图。它是JSP2.1规范中定义的统一表达式语言的扩展，支持set和get属性值、属性赋值、方法调用、访问数组集合及索引的内容、逻辑算术运算、命名变量、通过名字从Spring IoC容器检索对象，还支持列表的投影、选择以及聚合等。

- 数据访问/集成

   

  – 该层提供与数据库交互的支持。它包含以下模块：

  - JDBC (Java DataBase Connectivity)
  - ORM (Object Relational Mapping)用于支持Hibernate等ORM工具。
  - OXM (Object XML Mappers)
  - JMS (Java Messaging Service)Java消息服务。
  - Transaction

- Web

   

  – 该层提供了创建 Web 应用程序的支持。它包含以下模块：

  - Web
  - Web – Servlet
  - Web – Socket
  - Web – Portlet

- **AOP** – 该层支持面向切面编程

- **Instrumentation** – 该层为类检测和类加载器实现提供支持。

- **Test** – 该层为使用 JUnit 和 TestNG 进行测试提供支持。

- 几个杂项模块:

  - Messaging – 该模块为 STOMP 提供支持。它还支持注解编程模型，该模型用于从 WebSocket 客户端路由和处理 STOMP 消息。
  - Aspects – 该模块为与 AspectJ 的集成提供支持。

# Spring IoC 容器

IOC的思想基于IOC容器来实现。IOC容器的本质就是所有对象的工厂，需要什么对象就从工厂中取

Spring提供了IOC容器的两种方式

（1）BeanFactory 是Spring内部使用的接口，一般不提供给开发人员，

\* 加载配置文件的时候不会创建对象，获取对象的时候才会创建对象。

（2）ApplicationContext	是BeanFactory的一个子接口，提供更多更强大的功能，面向开发人员设计使用的

\* 加载配置文件的时候就会把配置文件对象是进行创建

 ApplicationContext的实现类：FileSystemXmlApplicationContext，ClassPathXmlApplicationContext

Spring 容器是 Spring 框架的核心。容器将创建对象，把它们连接在一起，配置它们，并管理他们的整个生命周期从创建到销毁。**IOC 容器**具有依赖注入功能的容器，它可以创建对象，IOC 容器负责实例化、定位、配置应用程序中的对象及建立这些对象间的依赖

Spring 框架的核心是 Spring 容器。容器创建对象，将它们装配在一起，配置它们并管理它们的完整生命周期。Spring 容器使用依赖注入来管理组成应用程序的组件。容器通过读取提供的配置元数据来接收对象进行实例化，配置和组装的指令。该元数据可以通过 XML，Java 注解或 Java 代码提供。

| 序号 | 容器 & 描述                                                  |
| ---- | ------------------------------------------------------------ |
| 1    | [Spring BeanFactory 容器](https://www.w3cschool.cn/wkspring/j3181mm3.html)它是最简单的容器，给 DI 提供了基本的支持，它用 org.springframework.beans.factory.BeanFactory 接口来定义。BeanFactory 或者相关的接口，如 BeanFactoryAware，InitializingBean，DisposableBean，在 Spring 中仍然存在具有大量的与 Spring 整合的第三方框架的反向兼容性的目的。最常被使用的是 **XmlBeanFactory** 类 |
| 2    | [Spring ApplicationContext 容器](https://www.w3cschool.cn/wkspring/yqdx1mm5.html)该容器添加了更多的企业特定的功能，例如从一个属性文件中解析文本信息的能力，发布应用程序事件给感兴趣的事件监听器的能力。该容器是由 org.springframework.context.ApplicationContext 接口定义。 |

- **FileSystemXmlApplicationContext**：该容器从 XML 文件中加载已被定义的 bean。在这里，你需要提供给构造器 XML 文件的完整路径。
- **ClassPathXmlApplicationContext**：该容器从 XML 文件中加载已被定义的 bean。在这里，你不需要提供 XML 文件的完整路径，只需正确配置 CLASSPATH 环境变量即可，因为，容器会从 CLASSPATH 中搜索 bean 配置文件。
- **WebXmlApplicationContext**：该容器会在一个 web 应用程序的范围内加载在 XML 文件中已被定义的 bean。



# Spring Bean 



### 生命周期

Bean的生命周期可以表达为：Bean的定义——Bean的初始化——Bean的使用——Bean的销毁

### 什么是 spring bean？

- 它们是构成用户应用程序主干的对象。
- Bean 由 Spring IoC 容器管理。
- 它们由 Spring IoC 容器实例化，配置，装配和管理。
- Bean 是基于用户提供给容器的配置元数据创建。

###  spring 注入方式

- 基于 xml 配置

  bean 所需的依赖项和服务在 XML 格式的配置文件中指定

  <context:compont-scan base-package="com.xxx.xxx">

  use-default-filters=“false”，表示不使用默认过滤器

  <context:include-filter >只扫描的注解配置

  <context:exclude-filter >不扫描的注解配置

- 基于注解配置

您可以通过在相关的类，方法或字段声明上使用注解，将 bean 配置为组件类本身，而不是使用 XML 来描述 bean 装配

### 

```java
@Autowired  //根据类型进行注入

@Qualifier(value = "userDaoImpl1")

@Resource(name = "userDaoImpl1")  //根据名称进行注入

@Value(value = "abc")

```

完全注解开发：

AnnotationConfigApplicationContext ，使用配置类代替XML文件

- 基于 Java API 配置

Spring 的 Java 配置是通过使用 @Bean 和 @Configuration 来实现。

1. @Bean 注解扮演与 `<bean />` 元素相同的角色。
2. @Configuration 类允许通过简单地调用同一个类中的其他 @Bean 方法来定义 bean 间依赖关系。

### Spring Bean 作用域

| 作用域         | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| singleton      | 在spring IoC容器仅存在一个Bean实例，Bean以单例方式存在，默认值,默认的作用域，在类中定义一个ThreadLocal成员变量，将需要的可变成员变量保存在 ThreadLocal 中（推荐的一种解决线程安全方式）。 |
| prototype      | 每次从容器中调用Bean时，都返回一个新的实例，即每次调用getBean()时，相当于执行newXxxBean() |
| request        | 每次HTTP请求都会创建一个新的Bean，该作用域仅适用于WebApplicationContext环境 |
| session        | 同一个HTTP Session共享一个Bean，不同Session使用不同的Bean，仅适用于WebApplicationContext环境 |
| global-session | 一般用于Portlet应用环境，该运用域仅适用于WebApplicationContext环境 |

### spring bean 容器的生命周期

 spring bean 容器的生命周期流程如下：

1. Spring 容器根据配置中的 bean 定义中实例化 bean。

2. Spring 使用依赖注入填充所有属性，如 bean 中所定义的配置。

   - 如果 bean 实现 BeanNameAware 接口，则工厂通过传递 bean 的 ID 来调用 setBeanName()。

   - 如果 bean 实现 BeanFactoryAware 接口，工厂通过传递自身的实例来调用 setBeanFactory()。

3. 如果存在与 bean 关联的任何 BeanPostProcessors，则调用 preProcessBeforeInitialization() 方法。

4. 如果为 bean 指定了 init 方法（`<bean>` 的 init-method 属性），那么将调用它。

5. 最后，如果存在与 bean 关联的任何 BeanPostProcessors，则将调用 postProcessAfterInitialization() 方法。

6. bean的获取使用

   ​	- 如果 bean 实现 DisposableBean 接口，当 spring 容器关闭时，会调用 destory()。

7. 如果为 bean 指定了 destroy 方法（`<bean>` 的 destroy-method 属性），那么将调用它

###  spring 装配

当 bean 在 Spring 容器中组合在一起时，它被称为装配或 bean 装配。 Spring 容器需要知道需要什么 bean 以及容器应该如何使用依赖注入来将 bean 绑定在一起，同时装配 bean。

bean的自动装配

```xml
<bean id="emp" class="com.atguigu.spring5.autowire.Emp" autowire="byName">

  <!--<property name="dept" ref="dept"></property>-->

</bean>

<bean id="dept" class="com.atguigu.spring5.autowire.Dept"></bean>
```



bean的外部配置文件引入

```xml
<!--引入外部属性文件-->

<context:property-placeholder location="classpath:jdbc.properties"/>

<!--配置连接池-->

<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">

  <property name="driverClassName" value="${prop.driverClass}"></property>

  <property name="url" value="${prop.url}"></property>

  <property name="username" value="${prop.userName}"></property>

  <property name="password" value="${prop.password}"></property>

</bean>
```





### 自动装配有哪些方式？

Spring 容器能够自动装配 bean。也就是说，可以通过检查 BeanFactory 的内容让 Spring 自动解析 bean 的协作者。

自动装配的不同模式：

- **no** - 这是默认设置，表示没有自动装配。应使用显式 bean 引用进行装配。
- **byName** - 它根据 bean 的名称注入对象依赖项。它匹配并装配其属性与 XML 文件中由相同名称定义的 bean。
- **byType** - 它根据类型注入对象依赖项。如果属性的类型与 XML 文件中的一个 bean 名称匹配，则匹配并装配属性。
- **构造函数** - 它通过调用类的构造函数来注入依赖项。它有大量的参数。
- **autodetect** - 首先容器尝试通过构造函数使用 autowire 装配，如果不能，则尝试通过 byType 自动装配。

### 

# spring事务

## Spring事务的配置方式

Spring支持编程式事务管理以及声明式事务管理两种方式。

是数据库操作最基本的单元，一组操作，要么都成功要么都失败：四个特性ACID

   一般使用声明式事务管理。

   底层使用的是AOP原理。

### 1. 编程式事务管理

不推荐使用，编程式事务管理是侵入性事务管理，使用TransactionTemplate或者直接使用PlatformTransactionManager，对于编程式事务管理，Spring推荐使用TransactionTemplate。

### 2. 声明式事务管理

声明式事务管理建立在AOP之上，其本质是对方法前后进行拦截，然后在目标方法开始之前创建或者加入一个事务，执行完目标方法之后根据执行的情况提交或者回滚。
编程式事务每次实现都要单独实现，但业务量大功能复杂时，使用编程式事务无疑是痛苦的，而声明式事务不同，声明式事务属于无侵入式，不会影响业务逻辑的实现，只需要在配置文件中做相关的事务规则声明或者通过注解的方式，便可以将事务规则应用到业务逻辑中。
显然声明式事务管理要优于编程式事务管理，这正是Spring倡导的非侵入式的编程方式。唯一不足的地方就是声明式事务管理的粒度是方法级别，而编程式事务管理是可以到代码块的，但是可以通过提取方法的方式完成声明式事务管理的配置。

## 事务的传播机制

事务的传播性一般用在事务嵌套的场景，比如一个事务方法里面调用了另外一个事务方法，那么两个方法是各自作为独立的方法提交还是内层的事务合并到外层的事务一起提交，这就是需要事务传播机制的配置来确定怎么样执行。
常用的事务传播机制如下：

- PROPAGATION_REQUIRED
  Spring默认的传播机制，能满足绝大部分业务需求，如果外层有事务，则当前事务加入到外层事务，一块提交，一块回滚。如果外层没有事务，新建一个事务执行
- PROPAGATION_REQUES_NEW
  该事务传播机制是每次都会新开启一个事务，同时把外层事务挂起，当当前事务执行完毕，恢复上层事务的执行。如果外层没有事务，执行当前新开启的事务即可
- PROPAGATION_SUPPORT
  如果外层有事务，则加入外层事务，如果外层没有事务，则直接使用非事务方式执行。完全依赖外层的事务
- PROPAGATION_NOT_SUPPORT
  该传播机制不支持事务，如果外层存在事务则挂起，执行完当前代码，则恢复外层事务，无论是否异常都不会回滚当前的代码
- PROPAGATION_NEVER
  该传播机制不支持外层事务，即如果外层有事务就抛出异常
- PROPAGATION_MANDATORY
  与NEVER相反，如果外层没有事务，则抛出异常
- PROPAGATION_NESTED
  该传播机制的特点是可以保存状态保存点，当前事务回滚到某一个点，从而避免所有的嵌套事务都回滚，即各自回滚各自的，如果子事务没有把异常吃掉，基本还是会引起全部回滚的。

> 传播规则回答了这样一个问题：一个新的事务应该被启动还是被挂起，或者是一个方法是否应该在事务性上下文中运行。

## 事务的隔离级别

事务的隔离级别定义一个事务可能受其他并发务活动活动影响的程度，可以把事务的隔离级别想象为这个事务对于事物处理数据的自私程度。

在一个典型的应用程序中，多个事务同时运行，经常会为了完成他们的工作而操作同一个数据。并发虽然是必需的，但是会导致以下问题：

1. 脏读（Dirty read）
   
   一个未提交事务读取到另一个未提交事务的数据：：：脏读发生在一个事务读取了被另一个事务改写但尚未提交的数据时。如果这些改变在稍后被回滚了，那么第一个事务读取的数据就会是无效的。
   
2. 不可重复读（Nonrepeatable read）
   一个未提交事务读到了一个提交事务修改的数据：：：不可重复读发生在一个事务执行相同的查询两次或两次以上，但每次查询结果都不相同时。这通常是由于另一个并发事务在两次查询之间更新了数据。

> 不可重复读重点在修改。

1. 幻读（Phantom reads）
   一个未提交事务读到了一个提交事务的添加的数据：：：幻读和不可重复读相似。当一个事务（T1）读取几行记录后，另一个并发事务（T2）插入了一些记录时，幻读就发生了。在后来的查询中，第一个事务（T1）就会发现一些原来没有的额外记录。

> 幻读重点在新增或删除。

在理想状态下，事务之间将完全隔离，从而可以防止这些问题发生。然而，完全隔离会影响性能，因为隔离经常涉及到锁定在数据库中的记录（甚至有时是锁表）。完全隔离要求事务相互等待来完成工作，会阻碍并发。因此，可以根据业务场景选择不同的隔离级别。

| 隔离级别                           | 含义                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| ISOLATION_DEFAULT                  | 使用后端数据库默认的隔离级别                                 |
| ISOLATION_READ_UNCOMMITTED读未提交 | 允许读取尚未提交的更改。可能导致脏读、幻读或不可重复读。     |
| ISOLATION_READ_COMMITTED读已提交   | （Oracle 默认级别）允许从已经提交的并发事务读取。可防止脏读，但幻读和不可重复读仍可能会发生。 |
| ISOLATION_REPEATABLE_READ可重复读  | （MYSQL默认级别）对相同字段的多次读取的结果是一致的，除非数据被当前事务本身改变。可防止脏读和不可重复读，但幻读仍可能发生。 |
| ISOLATION_SERIALIZABLE串行化       | 完全服从ACID的隔离级别，确保不发生脏读、不可重复读和幻影读。这在所有隔离级别中也是最慢的，因为它通常是通过完全锁定当前事务所涉及的数据表来完成的。 |

## Spring声明式事务配置参考

事物配置中有哪些属性可以配置?以下只是简单的使用参考

1. 事务的传播性：
   @Transactional(propagation=Propagation.REQUIRED)
2. 事务的隔离级别：
   @Transactional(isolation = Isolation.READ_UNCOMMITTED)

> 读取未提交数据(会出现脏读, 不可重复读) 基本不使用

1. 只读：
   @Transactional(readOnly=true)
   该属性用于设置当前事务是否为只读事务，设置为true表示只读，false则表示可读写，默认值为false。
2. 事务的超时性：
   @Transactional(timeout=30)
3. 回滚：
   指定单一异常类：@Transactional(rollbackFor=RuntimeException.class)
   指定多个异常类：@Transactional(rollbackFor={RuntimeException.class, Exception.class})

# 注解

###  Component, Controller, Repository, Service 有何区别？

- @Component：这将 java 类标记为 bean。它是任何 Spring 管理组件的通用构造型。spring 的组件扫描机制现在可以将其拾取并将其拉入应用程序环境中。

- @Controller：这将一个类标记为 Spring Web MVC 控制器。标有它的 Bean 会自动导入到 IoC 容器中。

- @Service：此注解是组件注解的特化。它不会对 @Component 注解提供任何其他行为。您可以在服务层类中使用 @Service 而不是 @Component，因为它以更好的方式指定了意图。

- @Repository：这个注解是具有类似用途和功能的 @Component 注解的特化。它为 DAO 提供了额外的好处。它将 DAO 导入 IoC 容器，并使未经检查的异常有资格转换为 Spring DataAccessException。

- `@Component` ：通用的注解，可标注任意类为 `Spring` 组件。如果一个Bean不知道属于拿个层，可以使用`@Component` 注解标注。

  `@Repository` : 对应持久层即 Dao 层，主要用于数据库相关操作。

  `@Service` : 对应服务层，主要涉及一些复杂的逻辑，需要用到 Dao层。

  `@Controller` : 对应 Spring MVC 控制层，主要用户接受用户请求并调用 Service 层返回数据给前端页面。

# Spring5新功能：

   1、基于java8开发，运行时兼容jdk9

   2、自带了通用的日志封装，

   已经移除了log4jCongigListener，官方建议使用log4j2    

   支持@Nullable

   函数式风格创建对象。

   集成Junit5；





# 设计模式

**工厂设计模式** : Spring使用工厂模式通过 `BeanFactory`、`ApplicationContext` 创建 bean 对象。

**代理设计模式** : Spring AOP 功能的实现。

**单例设计模式** : Spring 中的 Bean 默认都是单例的。

**模板方法模式** : Spring 中 `jdbcTemplate`、`hibernateTemplate` 等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。

**包装器设计模式** : 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。

**观察者模式:** Spring 事件驱动模型就是观察者模式很经典的一个应用。

**适配器模式** :Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配`Controller`。
