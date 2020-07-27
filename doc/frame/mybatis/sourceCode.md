# Mybatis

ORM框架：数据持久化 Object Relational Mapping 用于实现面向对象编程语言里不同类型系统的数据之间的转换。

JDBC链接数据库的问题：

1. 没有使用到连接池
2. sql写在代码中
3. 硬编码

# 技术本质

- 数据库源 Driver、URL、username、password
- 执行语句 增删改查
- 操作 Connection、Parparestatement、ResultSet

一、获取数据库源：

> #### SqlSessionFactoryBuilder().build(inputStream)
>
> XMLConfigBuilder.parse()    调用    parseConfiguration(parser.evalNode("/configuration"));
>
> XMLConfigBuilder.environmentsElement
>
> Configuration.setEnvironment
>

二、获取数据库执行语句

> mybatis加载mapper的方式有几种？
>
> XMLConfigBuilder
>
> Mapper
>

三、mybatis是如何操作的

mybatis的执行器有几种？Simple、Reuse、 Batch

> 获取数据库源
>
> 获取执行语句
>
> 通过jdbc执行。
>
> 反射到对象中返回结果集

## MyBatis的工作原理？

关于MyBatis的工作原理，JDBC有四个核心对象：
（1）DriverManager，用于注册数据库连接
（2）Connection，与数据库连接对象
（3）Statement/PrepareStatement，操作数据库SQL语句的对象
（4）ResultSet，结果集或一张虚拟表

而MyBatis也有四大核心对象：
（1）SqlSession对象，该对象中包含了执行SQL语句的所有方法【1】。类似于JDBC里面的Connection 【2】。
（2）Executor接口，它将根据SqlSession传递的参数动态地生成需要执行的SQL语句，同时负责查询缓存的维护。类似于JDBC里面的Statement/PrepareStatement。
（3）MappedStatement对象，该对象是对映射SQL的封装，用于存储要映射的SQL语句的id、参数等信息。
（4）ResultHandler对象，用于对返回的结果进行处理，最终得到自己想要的数据格式或类型。可以自定义返回类型。



![mybatis.png](img\mybatis2.png)

> 上面中流程就是MyBatis内部核心流程，每一步流程的详细说明如下文所述：
>
> （1）读取MyBatis的配置文件。mybatis-config.xml为MyBatis的全局配置文件，用于配置数据库连接信息。
>
> （2）加载映射文件。映射文件即SQL映射文件，该文件中配置了操作数据库的SQL语句，需要在MyBatis配置文件mybatis-config.xml中加载。mybatis-config.xml 文件可以加载多个映射文件，每个文件对应数据库中的一张表。
>
> （3）构造会话工厂。通过MyBatis的环境配置信息构建会话工厂SqlSessionFactory。
>
> （4）创建会话对象。由会话工厂创建SqlSession对象，该对象中包含了执行SQL语句的所有方法。
>
> （5）Executor执行器。MyBatis底层定义了一个Executor接口来操作数据库，它将根据SqlSession传递的参数动态地生成需要执行的SQL语句，同时负责查询缓存的维护。
>
> （6）MappedStatement对象。在Executor接口的执行方法中有一个MappedStatement类型的参数，该参数是对映射信息的封装，用于存储要映射的SQL语句的id、参数等信息。
>
> （7）输入参数映射。输入参数类型可以是Map、List等集合类型，也可以是基本数据类型和POJO类型。输入参数映射过程类似于JDBC对preparedStatement对象设置参数的过程。
>
> （8）输出结果映射。输出结果类型可以是Map、List等集合类型，也可以是基本数据类型和POJO类型。输出结果映射过程类似于JDBC对结果集的解析过程。

 

### 一、mybatis 的核心组件：

- SqlSessionFactoryBuilder	它会根据配置或者代码来生成SqlSessionFactory，采用的是分步构建的Builder模式
- SqlSessionFactory  依靠它生成SqlSession，使用的是工厂模式。
- SqlSession      ①可以发送SQL执行返回结构，②获取Mapper的接口。而我们现在一般会让其在业务代码中消失，使用Mapper接口编程
- SQL Mapper   接口和XML文件（或注解）构成，给出对应的SQL和映射规则，去执行并返回结果。

![img](img\mybatis.png)

#### 1. SqlSessionFactory(工厂接口)

使用MyBatis首先就是去配置或者代码去生成SqlSessionFactory。

创建SqlSessionFactory,需要用到SqlSessionFactoryBuilder和一个类org.apache.itatis.session.Configuraton，采用Builder模式，具体分步在Configuration中完成。

#### 2. SqlSession

是MyBatis的核心接口，类似于JDBC中的Connection对象，代表着一个链接资源的启用。具体作用有三个：

- 获取Mapper接口
- 发送SQL给数据库
- 控制数据库事务

**创建一个SqlSession：**

```java
SqlSession sqlSession = SqlSessionFactory.openSession();
```

注意：SqlSession是个门面接口，但实际上底层工作的是Executor

**SqlSession事务：**

```java
sqlSession.commit();//提交事务
sqlSession.rollback();//回滚事务
sqlSession.close();
```

#### 3. 映射器

映射器是MyBatis中最重要、最复杂的组件，由一个接口和对应的XML文件（或注解组成）,他可以配置：

- 描述映射规则
- 提供SQL语句，并可以配置SQL参数类型、返回类型、缓存刷新等信息
- 配置缓存
- 提供动态SQL

**用XML实现映射器与注解实现映射器**

xml与pojo是自动映射的（如：role_name <==> roleName）,只要列名一一对应即可。

与注解映射比较：

- XML和注解同时使用，XML会覆盖到注解的映射
- XML可读性高，动态SQL更容易使用
- XML可以相互引入，而注解不可以

## 其他类

> Configuration
>  存储了MyBatis的所有配置项和解析后的信息，极为重要的一个类。MyBatis初始化时，将解析出的信息一股脑的扔给Configuration进行存储。执行sql语句或者其它操作时，从此类里取出相关的配置信息进行操作。
>  两个重要属性：
>
> - MapperRegistry mapperRegistry; 映射注册机实例。
> - Map<String, MappedStatement> mappedStatements，存储了 dao接口完全限定名+方法名 - MappedStatement实例的键值对。
>
> XMLConfigBuilder
>  XML配置构建器，建造者模式。解析MyBatis的xml配置文件并以Document的形式保存下来，方便其内部解析方法解析出MyBatis的具体配置信息。
>
> MapperRegistry
>  映射注册机，其属性 `Map<Class<?>, MapperProxyFactory<?>> knownMappers` 存储了 Class对象 - 映射器代理工厂实例 的键值对，MyBatis初始化时存入，使用时取出
>
> MapperProxyFactory
>  映射器代理工厂，映射器代理类的工厂方法，生产一个dao接口的代理实例提供给用户。
>
> MapperProxy
>  映射器代理类，利用jdk动态代理返回给用户代理类实例，除Object通用方法（toString()、hashcode()等等）、接口默认方法（java8 新增默认方法）外，皆走代理方法，在代理方法内部执行数据库的crud。
>
> MappedStatement
>  映射语句，存储了解析注解、xml后的sql语句相关信息。
>
> MapperMethod
>  映射器方法。负责sql语句参数填充及语句的分类执行。
>  两个极为重要的属性：
>  SqlCommand一个内部类 封装了SQL标签的类型 insert delete update select
>  MethodSignature一个内部类 封装了方法的参数信息 返回类型信息等
>
> 一个execute方法，负责执行sql语句，并将结果处理好后返回给用户。





1、知识点：
 在刚刚梳理代码中，涉及到了单例模式、工厂模式、建造者模式，代理模式等设计模式，带来的好处就是代码层级和架构清晰，类职责明确。从实现来看，基本符合高内聚、低耦合。

MyBatis的核心就在于配置文件解析和sql语句映射这一块，也是精髓所在，尤其是利用jdk的代理，达到crud的操作，堪称画龙点睛之笔。

