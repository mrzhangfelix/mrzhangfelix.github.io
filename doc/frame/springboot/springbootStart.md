# Springboot启动流程

源码使用Springboot:2.3.2.RELEASE版本，仅供学习参考

启动应用的入口

```
SpringApplication.run(MainApplication.class,args);
```

返回一个ConfigurableApplicationContext

```
return new SpringApplication(primarySources).run(args);
```

## 1、创建SpringApplication对象

源码如下：东西很简单查看注解

```java
this(null, primarySources);//primarySources我们传入的主配置类

public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
   this.resourceLoader = resourceLoader;
   Assert.notNull(primarySources, "PrimarySources must not be null");
   this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
    //判断当前是否一个web应用
   this.webApplicationType = WebApplicationType.deduceFromClasspath();
     //从类路径下找到META-INF/spring.factories配置的所有ApplicationContextInitializer；然后保存起来
   setInitializers((Collection) getSpringFactoriesInstances(ApplicationContextInitializer.class));
    //从类路径下找到ETA-INF/spring.factories配置的所有ApplicationListener
   setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
    //从调用链中找到有main方法的主配置类
   this.mainApplicationClass = deduceMainApplicationClass();
}
```







## 2、运行SpringApplication对象的run方法

源码如下：

```java
public ConfigurableApplicationContext run(String... args) {
   StopWatch stopWatch = new StopWatch();
   stopWatch.start();
   ConfigurableApplicationContext context = null;
   Collection<SpringBootExceptionReporter> exceptionReporters = new ArrayList<>();
   configureHeadlessProperty();
 	//获取SpringApplicationRunListeners；从类路径下META-INF/spring.factories
   SpringApplicationRunListeners listeners = getRunListeners(args);
    //回调所有的获取SpringApplicationRunListener.starting()方法
   listeners.starting();
   try {
       //封装命令行参数ApplicationArguments
      ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
       //1、准备环境
      ConfigurableEnvironment environment = prepareEnvironment(listeners, applicationArguments);
      configureIgnoreBeanInfo(environment);
       //打印banner
      Banner printedBanner = printBanner(environment);
       //创建容器，三种AnnotationConfigServletWebServerApplicationContext；AnnotationConfigReactiveWebServerApplicationContext；AnnotationConfigApplicationContext
      context = createApplicationContext();
       //获取一个FailureAnalyzers
      exceptionReporters = getSpringFactoriesInstances(SpringBootExceptionReporter.class,
            new Class[] { ConfigurableApplicationContext.class }, context);
       //2、准备容器
      prepareContext(context, environment, listeners, applicationArguments, printedBanner);
       //3、刷新容器，调用容器刷新方法，扫描，创建，加载所有组件的地方；这里不进行细致分析，可以参考IOC容器的启动流程的文章
      refreshContext(context);
       //4、刷新完成
      afterRefresh(context, applicationArguments);
      stopWatch.stop();
      if (this.logStartupInfo) {
         new StartupInfoLogger(this.mainApplicationClass).logStarted(getApplicationLog(), stopWatch);
      }
       //所有的SpringApplicationRunListener回调started方法
      listeners.started(context);
       //5、从ioc容器中获取所有的ApplicationRunner和CommandLineRunner进行回调
      callRunners(context, applicationArguments);
   }
   catch (Throwable ex) {
      handleRunFailure(context, ex, exceptionReporters, listeners);
      throw new IllegalStateException(ex);
   }

   try {
       //所有的SpringApplicationRunListener回调running方法
      listeners.running(context);
   }
   catch (Throwable ex) {
      handleRunFailure(context, ex, exceptionReporters, null);
      throw new IllegalStateException(ex);
   }
    //整个SpringBoot应用启动完成以后返回启动的ioc容器；
   return context;
}
```

详细说明的方法

1、prepareEnvironment(listeners, applicationArguments);

```java
// Create and configure the environment
//创建环境时会判断是否时wed环境，servlet和reactive或者标准的环境
ConfigurableEnvironment environment = getOrCreateEnvironment();
//添加删除环境的相关配置，配置此应用程序的哪些配置是被激活的（profiles）
configureEnvironment(environment, applicationArguments.getSourceArgs());
ConfigurationPropertySources.attach(environment);
//调用监听器的environmentPrepared方法
listeners.environmentPrepared(environment);
//绑定环境信息到应用上
bindToSpringApplication(environment);
if (!this.isCustomEnvironment) {
   environment = new EnvironmentConverter(getClassLoader()).convertEnvironmentIfNecessary(environment,
         deduceEnvironmentClass());
}
ConfigurationPropertySources.attach(environment);
return environment;
```

2、prepareContext(context, environment, listeners, applicationArguments, printedBanner);

```java
//设置环境信息，将environment保存到ioc中
context.setEnvironment(environment);
//调用后置处理器
postProcessApplicationContext(context);
//回调之前保存的所有的ApplicationContextInitializer的initialize方法完成容器中Initializer的初始化操作
applyInitializers(context);
//回调所有的SpringApplicationRunListener的contextPrepared()；
listeners.contextPrepared(context);
if (this.logStartupInfo) {
   logStartupInfo(context.getParent() == null);
   logStartupProfileInfo(context);
}
// Add boot specific singleton beans
ConfigurableListableBeanFactory beanFactory = context.getBeanFactory();
beanFactory.registerSingleton("springApplicationArguments", applicationArguments);
if (printedBanner != null) {
   beanFactory.registerSingleton("springBootBanner", printedBanner);
}
if (beanFactory instanceof DefaultListableBeanFactory) {
   ((DefaultListableBeanFactory) beanFactory)
         .setAllowBeanDefinitionOverriding(this.allowBeanDefinitionOverriding);
}
if (this.lazyInitialization) {
   context.addBeanFactoryPostProcessor(new LazyInitializationBeanFactoryPostProcessor());
}
// Load the sources
Set<Object> sources = getAllSources();
Assert.notEmpty(sources, "Sources must not be empty");
load(context, sources.toArray(new Object[0]));
//prepareContext运行完成以后回调所有的SpringApplicationRunListener的contextLoaded（）；
listeners.contextLoaded(context);
```

5、callRunners(context, applicationArguments);

```java
List<Object> runners = new ArrayList<>();
//ApplicationRunner先回调，CommandLineRunner再回调
runners.addAll(context.getBeansOfType(ApplicationRunner.class).values());
runners.addAll(context.getBeansOfType(CommandLineRunner.class).values());
AnnotationAwareOrderComparator.sort(runners);
for (Object runner : new LinkedHashSet<>(runners)) {
   if (runner instanceof ApplicationRunner) {
      callRunner((ApplicationRunner) runner, args);
   }
   if (runner instanceof CommandLineRunner) {
      callRunner((CommandLineRunner) runner, args);
   }
}
```



# 回调机制

配置在META-INF/spring.factories

**ApplicationContextInitializer**

**SpringApplicationRunListener**



只需要放在ioc容器中

**ApplicationRunner**

**CommandLineRunner**











