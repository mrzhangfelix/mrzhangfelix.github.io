# 创建bean实例的过程

 1）、创建Bean的实例
 2）、populateBean；给bean的各种属性赋值
 3）、initializeBean：初始化bean；
            1）、invokeAwareMethods()：处理Aware接口的方法回调
            2）、applyBeanPostProcessorsBeforeInitialization()：应用后置处理器的postProcessBeforeInitialization（）
            3）、invokeInitMethods()；执行自定义的初始化方法
            4）、applyBeanPostProcessorsAfterInitialization()；执行后置处理器的postProcessAfterInitialization（）；
4）、Bean创建成功

![Spring Bean生命周期](C:\Users\z00475199\Downloads\Spring Bean生命周期.png)

# 动态代理



指在程序运行期间动态的将某段代码切入到指定方法指定位置进行运行的编程方式；

 1、导入aop模块；Spring AOP：(spring-aspects)
  2、定义一个业务逻辑类；在业务逻辑运行的时候将日志进行打印（方法之前、方法运行结束、方法出现异常，xxx）
  3、定义一个日志切面类：切面类里面的方法需要动态感知MathCalculator.div运行到哪里然后执行；
         通知方法：
            前置通知(@Before)：logStart：在目标方法(div)运行之前运行
            后置通知(@After)：logEnd：在目标方法(div)运行结束之后运行（无论方法正常结束还是异常结束）
            返回通知(@AfterReturning)：logReturn：在目标方法(div)正常返回之后运行
            异常通知(@AfterThrowing)：logException：在目标方法(div)出现异常以后运行
            环绕通知(@Around)：动态代理，手动推进目标方法运行（joinPoint.procced()）
  4、给切面类的目标方法标注何时何地运行（通知注解）；
  5、将切面类和业务逻辑类（目标方法所在类）都加入到容器中;
  6、必须告诉Spring哪个类是切面类(给切面类上加一个注解：@Aspect)
  7、给配置类中加 @EnableAspectJAutoProxy 【开启基于注解的aop模式】
         在Spring中很多的 @EnableXXX;

  三步：
      1）、将业务逻辑组件和切面类都加入到容器中；告诉Spring哪个是切面类（@Aspect）
      2）、在切面类上的每一个通知方法上标注通知注解，告诉Spring何时何地运行（切入点表达式）
   3）、开启基于注解的aop模式；@EnableAspectJAutoProxy



## AOP原理

 通过@EnableAspectJAutoProxy；注解，【看给容器中注册了什么组件，这个组件什么时候工作，这个组件的功能是什么？】

###        1、@EnableAspectJAutoProxy作用

         @Import(AspectJAutoProxyRegistrar.class)：

给容器中导入AspectJAutoProxyRegistrar
            利用AspectJAutoProxyRegistrar自定义给容器中注册bean；BeanDefinetion
            internalAutoProxyCreator=AnnotationAwareAspectJAutoProxyCreator

		给容器中注册一个AnnotationAwareAspectJAutoProxyCreator；

###   2、 AnnotationAwareAspectJAutoProxyCreator继承关系：

         AnnotationAwareAspectJAutoProxyCreator（注解感知AspectJ自动代理创建器）
            ->AspectJAwareAdvisorAutoProxyCreator
               ->AbstractAdvisorAutoProxyCreator
                  ->AbstractAutoProxyCreator

实现了两个接口，                   

 implements SmartInstantiationAwareBeanPostProcessor（智能实例化感知bean后置处理器）, BeanFactoryAware(Bean工厂感知)

一个是后置处理器在bean初始化完成前后做事情，

一个是bean工厂的Aware  ，自动装配BeanFactory                  



  AbstractAutoProxyCreator.setBeanFactory()
  AbstractAutoProxyCreator.有后置处理器的逻辑；

  AbstractAdvisorAutoProxyCreator.setBeanFactory()-》initBeanFactory()

  AnnotationAwareAspectJAutoProxyCreator.initBeanFactory()

###   流程：

#### 一、传入配置类，创建ioc容器

#### 二、注册配置类，调用refresh（）刷新容器；

#### 三、registerBeanPostProcessors(beanFactory);

注册bean的后置处理器来方便拦截bean的创建；
           1）、先获取ioc容器已经定义了的需要创建对象的所有BeanPostProcessor
           2）、给容器中加别的BeanPostProcessor
           3）、优先注册实现了PriorityOrdered接口的BeanPostProcessor；
           4）、再给容器中注册实现了Ordered接口的BeanPostProcessor；
           5）、注册没实现优先级接口的BeanPostProcessor；
           6）、注册BeanPostProcessor，实际上就是创建BeanPostProcessor对象，保存在容器中；
             创建internalAutoProxyCreator的BeanPostProcessor【AnnotationAwareAspectJAutoProxyCreator】aspectJAdvisorsBuilder 
           7）、把BeanPostProcessor注册到BeanFactory中；
            beanFactory.addBeanPostProcessor(postProcessor);

=======以上是创建和注册AnnotationAwareAspectJAutoProxyCreator的过程========
            AnnotationAwareAspectJAutoProxyCreator => InstantiationAwareBeanPostProcessor

####     四、finishBeanFactoryInitialization(beanFactory);

完成BeanFactory初始化工作；创建剩下的单实例bean
            1）、遍历获取容器中所有的Bean，依次创建对象getBean(beanName);
               getBean->doGetBean()->getSingleton()->
            2）、创建bean
               【AnnotationAwareAspectJAutoProxyCreator在所有bean创建之前会有一个拦截，InstantiationAwareBeanPostProcessor，会调用postProcessBeforeInstantiation()】
               1）、先从缓存中获取当前bean，如果能获取到，说明bean是之前被创建过的，直接使用，否则再创建；
                  只要创建好的Bean都会被缓存起来
               2）、createBean（）;创建bean；如果需要代理对象则用后置处理器创建对象，
                  AnnotationAwareAspectJAutoProxyCreator 会在任何bean创建之前先尝试返回bean的实例
                  【BeanPostProcessor是在Bean对象创建完成初始化前后调用的】
                  【InstantiationAwareBeanPostProcessor是在创建Bean实例之前先尝试用后置处理器返回对象的】
                  1）、resolveBeforeInstantiation(beanName, mbdToUse);解析BeforeInstantiation
                     希望后置处理器在此能返回一个代理对象；如果能返回代理对象就使用，如果不能就继续
                     1）、后置处理器先尝试返回对象；       拿到所有后置处理器，如果是InstantiationAwareBeanPostProcessor;
                           就执行postProcessBeforeInstantiation           

 

```java
bean = applyBeanPostProcessorsBeforeInstantiation(targetType, beanName);//执行实例化之前的后置处理方法
if (bean != null) {
    bean = applyBeanPostProcessorsAfterInitialization(bean, beanName);//执行初始化之后的后置处理方法
}
```

                    2）、doCreateBean(beanName, mbdToUse, args);真正的去创建一个bean实例；不使用代理方式创建对象。




### AnnotationAwareAspectJAutoProxyCreator【InstantiationAwareBeanPostProcessor】 的作用：

 

####  1）、每一个bean创建之前，调用postProcessBeforeInstantiation()；

         关心MathCalculator和LogAspect的创建
         1）、判断当前bean是否在advisedBeans中（保存了所有需要增强bean）
         2）、判断当前bean是否是基础类型的Advice、Pointcut、Advisor、AopInfrastructureBean，
            或者是否是切面（@Aspect）
         3）、是否需要跳过
            1）、获取候选的增强器（切面里面的通知方法）【List<Advisor> candidateAdvisors】
               每一个封装的通知方法的增强器是 InstantiationModelAwarePointcutAdvisor；
               判断每一个增强器是否是 AspectJPointcutAdvisor 类型的；返回true
            2）、永远返回false

 


####  2）、创建对象

  postProcessAfterInitialization；
         return wrapIfNecessary(bean, beanName, cacheKey);//包装如果需要的情况下
         1）、获取当前bean的所有增强器（通知方法）  Object[]  specificInterceptors
            1、找到候选的所有的增强器（找哪些通知方法是需要切入当前bean方法的）
            2、获取到能在bean使用的增强器。
            3、给增强器排序
         2）、保存当前bean在advisedBeans中；
         3）、如果当前bean需要增强，创建当前bean的代理对象；
            1）、获取所有增强器（通知方法）
            2）、保存到proxyFactory
            3）、创建代理对象：Spring自动决定
               JdkDynamicAopProxy(config);jdk动态代理；
               ObjenesisCglibAopProxy(config);cglib的动态代理；
         4）、给容器中返回当前组件使用cglib增强了的代理对象；
         5）、以后容器中获取到的就是这个组件的代理对象，执行目标方法的时候，代理对象就会执行通知方法的流程；
     

####  3）、目标方法执行  ；

         容器中保存了组件的代理对象（cglib增强后的对象），这个对象里面保存了详细信息（比如增强器，目标对象，xxx）；
         1）、CglibAopProxy.intercept();拦截目标方法的执行
         2）、根据ProxyFactory对象获取将要执行的目标方法拦截器链；
            List<Object> chain = this.advised.getInterceptorsAndDynamicInterceptionAdvice(method, targetClass);
            1）、List<Object> interceptorList保存所有拦截器 5
               一个默认的ExposeInvocationInterceptor 和 4个增强器；
            2）、遍历所有的增强器，将其转为Interceptor；
               registry.getInterceptors(advisor);
            3）、将增强器转为List<MethodInterceptor>；
               如果是MethodInterceptor，直接加入到集合中
               如果不是，使用AdvisorAdapter将增强器转为MethodInterceptor；
               转换完成返回MethodInterceptor数组；
      	3）、如果没有拦截器链，直接执行目标方法;
            拦截器链（每一个通知方法又被包装为方法拦截器，利用MethodInterceptor机制）
         4）、如果有拦截器链，把需要执行的目标对象，目标方法，
            拦截器链等信息传入创建一个 CglibMethodInvocation 对象，
            并调用 Object retVal =  mi.proceed();
         5）、拦截器链的触发过程;
            1)、如果没有拦截器执行执行目标方法，或者拦截器的索引和拦截器数组-1大小一样（指定到了最后一个拦截器）执行目标方法；
            2)、链式获取每一个拦截器，拦截器执行invoke方法，每一个拦截器等待下一个拦截器执行完成返回以后再来执行；
               拦截器链的机制，保证通知方法与目标方法的执行顺序；


## 总结

```
*     1）、  @EnableAspectJAutoProxy 开启AOP功能
*     2）、 @EnableAspectJAutoProxy 会给容器中注册一个组件 AnnotationAwareAspectJAutoProxyCreator
*     3）、AnnotationAwareAspectJAutoProxyCreator是一个后置处理器；
*     4）、容器的创建流程：
*        1）、registerBeanPostProcessors（）注册后置处理器；创建AnnotationAwareAspectJAutoProxyCreator对象
*        2）、finishBeanFactoryInitialization（）初始化剩下的单实例bean
*           1）、创建业务逻辑组件和切面组件
*           2）、AnnotationAwareAspectJAutoProxyCreator拦截组件的创建过程
*           3）、组件创建完之后，判断组件是否需要增强
*              是：切面的通知方法，包装成增强器（Advisor）;给业务逻辑组件创建一个代理对象（cglib）；
*     5）、执行目标方法：
*        1）、代理对象执行目标方法
*        2）、CglibAopProxy.intercept()；
*           1）、得到目标方法的拦截器链（增强器包装成拦截器MethodInterceptor）
*           2）、利用拦截器的链式机制，依次进入每一个拦截器进行执行；
*           3）、效果：
*              正常执行：前置通知-》目标方法-》后置通知-》返回通知
*              出现异常：前置通知-》目标方法-》后置通知-》异常通知
```





# bean的生命周期：

      bean创建---初始化----销毁的过程
容器管理bean的生命周期；

我们可以自定义初始化和销毁方法；容器在bean进行到当前生命周期的时候来调用我们自定义的初始化和销毁方法
*

构造（对象创建）

单实例：在容器启动的时候创建对象

多实例：在每次获取的时候创建对象\

> BeanPostProcessor.postProcessBeforeInitialization
>
> 初始化：
>
> 对象创建完成，并赋值好，调用初始化方法。。。
>
> BeanPostProcessor.postProcessAfterInitialization

销毁：

单实例：容器关闭的时候

多实例：容器不会管理这个bean；容器不会调用销毁方法；



遍历得到容器中所有的BeanPostProcessor；挨个执行beforeInitialization，

一但返回null，跳出for循环，不会执行后面的BeanPostProcessor.postProcessorsBeforeInitialization


## BeanPostProcessor原理

```java
populateBean(beanName, mbd, instanceWrapper);//给bean进行属性赋值
initializeBean(){
applyBeanPostProcessorsBeforeInitialization(wrappedBean, beanName);
invokeInitMethods(beanName, wrappedBean, mbd);执行自定义初始化
applyBeanPostProcessorsAfterInitialization(wrappedBean, beanName);
}
```



```
/**
 * 1）、指定初始化和销毁方法；
 *        通过@Bean指定init-method和destroy-method；
 * 2）、通过让Bean实现InitializingBean（定义初始化逻辑），
 *              DisposableBean（定义销毁逻辑）;
 * 3）、可以使用JSR250；
 *        @PostConstruct：在bean创建完成并且属性赋值完成；来执行初始化方法
 *        @PreDestroy：在容器销毁bean之前通知我们进行清理工作
 * 4）、BeanPostProcessor【interface】：bean的后置处理器；
 *        在bean初始化前后进行一些处理工作；
 *        postProcessBeforeInitialization:在初始化之前工作
 *        postProcessAfterInitialization:在初始化之后工作
 *
 * Spring底层对 BeanPostProcessor 的使用；
 *        bean赋值，注入其他组件，@Autowired，生命周期注解功能，@Async,xxx BeanPostProcessor;
 *
 * @author lfy
 *
 */
```



# Profile：

```
/**
 * Profile：
 *        Spring为我们提供的可以根据当前环境，动态的激活和切换一系列组件的功能；
 *
 * 开发环境、测试环境、生产环境；
 * 数据源：(/A)(/B)(/C)；
 *
 *
 * @Profile：指定组件在哪个环境的情况下才能被注册到容器中，不指定，任何环境下都能注册这个组件
 *
 * 1）、加了环境标识的bean，只有这个环境被激活的时候才能注册到容器中。默认是default环境
 * 2）、写在配置类上，只有是指定的环境的时候，整个配置类里面的所有配置才能开始生效
 * 3）、没有标注环境标识的bean在，任何环境下都是加载的；
 */
```



#  扩展原理：

##  BeanPostProcessor：

接口：bean后置处理器，bean创建对象初始化前后进行拦截工作的
 *

1、BeanFactoryPostProcessor：beanFactory的后置处理器；

	在BeanFactory标准初始化之后调用，来定制和修改BeanFactory的内容；
	
	所有的bean定义已经保存加载到beanFactory，但是bean的实例还未创建


## BeanFactoryPostProcessor

接口bean工厂的后置处理器	

	1)、ioc容器创建对象
	
	2)、invokeBeanFactoryPostProcessors(beanFactory);
	
	如何找到所有的BeanFactoryPostProcessor并执行他们的方法；
	
		1）、直接在BeanFactory中找到所有类型是BeanFactoryPostProcessor的组件，并执行他们的方法
	
		2）、在初始化创建其他组件前面执行



## BeanDefinitionRegistryPostrocessor 

继承于 BeanFactoryPostProcessor

		postProcessBeanDefinitionRegistry();
	
	在所有bean定义信息将要被加载，bean实例还未创建的；
	
	优先于BeanFactoryPostProcessor执行；
	
	利用BeanDefinitionRegistryPostProcessor给容器中再额外添加一些组件；


原理：

1）、ioc创建对象

2）、refresh()-》invokeBeanFactoryPostProcessors(beanFactory);

3）、从容器中获取到所有的BeanDefinitionRegistryPostProcessor组件。

		1、依次触发所有的postProcessBeanDefinitionRegistry()方法
	
		2、再来触发postProcessBeanFactory()方法BeanFactoryPostProcessor；

4）、再来从容器中找到BeanFactoryPostProcessor组件；然后依次触发postProcessBeanFactory()方法



## ApplicationListener：

监听容器中发布的事件。事件驱动模型开发；

public interface ApplicationListener<E extends ApplicationEvent>

监听 ApplicationEvent 及其下面的子事件；


### 步骤：

1）、写一个监听器（ApplicationListener实现类）来监听某个事件（ApplicationEvent及其子类）

		@EventListener;
	
		原理：使用EventListenerMethodProcessor处理器来解析方法上的@EventListener；

2）、把监听器加入到容器；

3）、只要容器中有相关事件的发布，我们就能监听到这个事件；

	ContextRefreshedEvent：容器刷新完成（所有bean都完全创建）会发布这个事件；
	
	ContextClosedEvent：关闭容器会发布这个事件；

4）、发布一个事件：

	applicationContext.publishEvent()；


### 原理：

ContextRefreshedEvent、IOCTest_Ext$1[source=我发布的时间]、ContextClosedEvent；

1）、ContextRefreshedEvent事件：

1）、容器创建对象：refresh()；

2）、finishRefresh();容器刷新完成会发布ContextRefreshedEvent事件

2）、自己发布事件；

3）、容器关闭会发布ContextClosedEvent；
*

### 【事件发布流程】：

3）、publishEvent(new ContextRefreshedEvent(this));

1）、获取事件的多播器（派发器）：getApplicationEventMulticaster()

2）、multicastEvent派发事件：

3）、获取到所有的ApplicationListener；

for (final ApplicationListener<?> listener : getApplicationListeners(event, type)) {

1）、如果有Executor，可以支持使用Executor进行异步派发；

Executor executor = getTaskExecutor();

2）、否则，同步的方式直接执行listener方法；invokeListener(listener, event);

拿到listener回调onApplicationEvent方法；
*

### 【事件多播器（派发器）】

1）、容器创建对象：refresh();

2）、initApplicationEventMulticaster();初始化ApplicationEventMulticaster；

1）、先去容器中找有没有id=“applicationEventMulticaster”的组件；

2）、如果没有this.applicationEventMulticaster = new SimpleApplicationEventMulticaster(beanFactory);

并且加入到容器中，我们就可以在其他组件要派发事件，自动注入这个applicationEventMulticaster；

### 【容器中有哪些监听器】

1）、容器创建对象：refresh();

2）、registerListeners();

从容器中拿到所有的监听器，把他们注册到applicationEventMulticaster中；

```java
String[] listenerBeanNames = getBeanNamesForType(ApplicationListener.class, true, false);

//将listener注册到ApplicationEventMulticaster中

getApplicationEventMulticaster().addApplicationListenerBean(listenerBeanName);
```



## SmartInitializingSingleton 原理：

在spring容器管理的所有单例对象（非懒加载对象）初始化完成之后调用的回调接口。智能初始化单例

afterSingletonsInstantiated();

	1）、ioc容器创建对象并refresh()；
	
	2）、finishBeanFactoryInitialization(beanFactory);初始化剩下的单实例bean；
	
			1）、先创建所有的单实例bean；getBean();
	
			2）、获取所有创建好的单实例bean，判断是否是SmartInitializingSingleton类型的；

如果是就调用afterSingletonsInstantiated();





# 自动装配;

```
/**
 * 
 *        Spring利用依赖注入（DI），完成对IOC容器中中各个组件的依赖关系赋值；
 *
 * 1）、@Autowired：自动注入：
 *        1）、默认优先按照类型去容器中找对应的组件:applicationContext.getBean(BookDao.class);找到就赋值
 *        2）、如果找到多个相同类型的组件，再将属性的名称作为组件的id去容器中查找
 *                       applicationContext.getBean("bookDao")
 *        3）、@Qualifier("bookDao")：使用@Qualifier指定需要装配的组件的id，而不是使用属性名
 *        4）、自动装配默认一定要将属性赋值好，没有就会报错；
 *           可以使用@Autowired(required=false);
 *        5）、@Primary：让Spring进行自动装配的时候，默认使用首选的bean；
 *              也可以继续使用@Qualifier指定需要装配的bean的名字
 *        BookService{
 *           @Autowired
 *           BookDao  bookDao;
 *        }
 *
 * 2）、Spring还支持使用@Resource(JSR250)和@Inject(JSR330)[java规范的注解]
 *        @Resource:
 *           可以和@Autowired一样实现自动装配功能；默认是按照组件名称进行装配的；
 *           没有能支持@Primary功能没有支持@Autowired（reqiured=false）;
 *        @Inject:
 *           需要导入javax.inject的包，和Autowired的功能一样。没有required=false的功能；
 *  @Autowired:Spring定义的； @Resource、@Inject都是java规范
 *
 * AutowiredAnnotationBeanPostProcessor:解析完成自动装配功能；
 *
 * 3）、 @Autowired:构造器，参数，方法，属性；都是从容器中获取参数组件的值
 *        1）、[标注在方法位置]：@Bean+方法参数；参数从容器中获取;默认不写@Autowired效果是一样的；都能自动装配
 *        2）、[标在构造器上]：如果组件只有一个有参构造器，这个有参构造器的@Autowired可以省略，参数位置的组件还是可以自动从容器中获取
 *        3）、放在参数位置：
 *
 * 4）、自定义组件想要使用Spring容器底层的一些组件（ApplicationContext，BeanFactory，xxx）；
 *        自定义组件实现xxxAware；在创建对象的时候，会调用接口规定的方法注入相关组件；Aware；
 *        把Spring底层一些组件注入到自定义的Bean中；
 *        xxxAware：功能使用xxxProcessor；
 *           ApplicationContextAware==》ApplicationContextAwareProcessor；
 *
 *
 * @author lfy
 *
 */
```



#  声明式事务：

```
/**
 * 声明式事务：
 * 
 * 环境搭建：
 * 1、导入相关依赖
 *        数据源、数据库驱动、Spring-jdbc模块
 * 2、配置数据源、JdbcTemplate（Spring提供的简化数据库操作的工具）操作数据
 * 3、给方法上标注 @Transactional 表示当前方法是一个事务方法；
 * 4、 @EnableTransactionManagement 开启基于注解的事务管理功能；
 *        @EnableXXX
 * 5、配置事务管理器来控制事务;
 *        @Bean
 *        public PlatformTransactionManager transactionManager()
 * 
 * 
 * 原理：
 * 1）、@EnableTransactionManagement
 *           利用TransactionManagementConfigurationSelector给容器中会导入组件
 *           导入两个组件
 *           AutoProxyRegistrar
 *           ProxyTransactionManagementConfiguration
 * 2）、AutoProxyRegistrar：
 *           给容器中注册一个 InfrastructureAdvisorAutoProxyCreator 组件；
 *           InfrastructureAdvisorAutoProxyCreator：
 *           利用后置处理器机制在对象创建以后，包装对象，返回一个代理对象（增强器），代理对象执行方法利用拦截器链进行调用；
 * 
 * 3）、ProxyTransactionManagementConfiguration 做了什么？
 *           1、给容器中注册事务增强器；
 *              1）、事务增强器要用事务注解的信息，AnnotationTransactionAttributeSource解析事务注解
 *              2）、事务拦截器：
 *                 TransactionInterceptor；保存了事务属性信息，事务管理器；
 *                 他是一个 MethodInterceptor；
 *                 在目标方法执行的时候；
 *                    执行拦截器链；
 *                    事务拦截器：
 *                       1）、先获取事务相关的属性
 *                       2）、再获取PlatformTransactionManager，如果事先没有添加指定任何transactionmanger
 *                          最终会从容器中按照类型获取一个PlatformTransactionManager；
 *                       3）、执行目标方法
 *                          如果异常，获取到事务管理器，利用事务管理回滚操作；
 *                          如果正常，利用事务管理器，提交事务
 *           
 */
```



# this（）：

调用父类GenericApplicationContext构造函数，构建beanFactory （newDefaultListableBeanFactory（））

```
this.reader = new AnnotatedBeanDefinitionReader(this);
this.scanner = new ClassPathBeanDefinitionScanner(this);
```

初始化：注解的bean的定义读取器

初始化：classPath的定义扫描器 用于扫描注解的一些bean



# register(annotatedClasses);

注册配置类：将配置类注册到容器中去



# Spring容器的refresh()【创建刷新】;

## 1、prepareRefresh()

刷新前的预处理;

	1）、initPropertySources()初始化一些属性设置;子类自定义个性化的属性设置方法；
	2）、getEnvironment().validateRequiredProperties();检验属性的合法等
	3）、earlyApplicationEvents= new LinkedHashSet<ApplicationEvent>();保存容器中的一些早期的事件；
## 2、obtainFreshBeanFactory();

获取BeanFactory；

	1）、refreshBeanFactory();刷新【创建】BeanFactory；
			创建了一个this.beanFactory = new DefaultListableBeanFactory();
			设置id；
	2）、getBeanFactory();返回刚才GenericApplicationContext创建的BeanFactory对象；
	3）、将创建的BeanFactory【DefaultListableBeanFactory】返回；
## 3、prepareBeanFactory(beanFactory);

BeanFactory的预准备工作（BeanFactory进行一些设置）；

	1）、设置BeanFactory的类加载器、支持表达式解析器...
	2）、添加部分BeanPostProcessor【ApplicationContextAwareProcessor】
	3）、设置忽略的自动装配的接口EnvironmentAware、EmbeddedValueResolverAware、xxx；
	4）、注册可以解析的自动装配；我们能直接在任何组件中自动注入：
			BeanFactory、ResourceLoader、ApplicationEventPublisher、ApplicationContext
	5）、添加BeanPostProcessor【ApplicationListenerDetector】
	6）、添加编译时的AspectJ；
	7）、给BeanFactory中注册一些能用的组件；
		environment【ConfigurableEnvironment】、
		systemProperties【Map<String, Object>】、
		systemEnvironment【Map<String, Object>】

## 4、postProcessBeanFactory(beanFactory);

BeanFactory准备工作完成后进行的后置处理工作；

	1）、子类通过重写这个方法来在BeanFactory创建并预准备完成以后做进一步的设置
======================以上是BeanFactory的创建及预准备工作==================================
## 5、invokeBeanFactoryPostProcessors(beanFactory);

执行BeanFactoryPostProcessor的方法；执行bean工厂的后置处理器

	BeanFactoryPostProcessor：BeanFactory的后置处理器。在BeanFactory标准初始化之后执行的；
	两个接口：BeanFactoryPostProcessor、BeanDefinitionRegistryPostProcessor
	1）、执行BeanFactoryPostProcessor的方法；
		先执行BeanDefinitionRegistryPostProcessor
		1）、获取所有的BeanDefinitionRegistryPostProcessor；
		2）、看先执行实现了PriorityOrdered优先级接口的BeanDefinitionRegistryPostProcessor、
			postProcessor.postProcessBeanDefinitionRegistry(registry)
		3）、在执行实现了Ordered顺序接口的BeanDefinitionRegistryPostProcessor；
			postProcessor.postProcessBeanDefinitionRegistry(registry)
		4）、最后执行没有实现任何优先级或者是顺序接口的BeanDefinitionRegistryPostProcessors；
			postProcessor.postProcessBeanDefinitionRegistry(registry)


​		

		再执行BeanFactoryPostProcessor的方法
		1）、获取所有的BeanFactoryPostProcessor
		2）、看先执行实现了PriorityOrdered优先级接口的BeanFactoryPostProcessor、
			postProcessor.postProcessBeanFactory()
		3）、在执行实现了Ordered顺序接口的BeanFactoryPostProcessor；
			postProcessor.postProcessBeanFactory()
		4）、最后执行没有实现任何优先级或者是顺序接口的BeanFactoryPostProcessor；
			postProcessor.postProcessBeanFactory()
## 6、registerBeanPostProcessors(beanFactory);

注册BeanPostProcessor（Bean的后置处理器）【 intercept bean creation】

		不同接口类型的BeanPostProcessor；在Bean创建前后的执行时机是不一样的
		BeanPostProcessor、
		DestructionAwareBeanPostProcessor、
		InstantiationAwareBeanPostProcessor、
		SmartInstantiationAwareBeanPostProcessor、
		MergedBeanDefinitionPostProcessor【internalPostProcessors】、


		1）、获取所有的 BeanPostProcessor;后置处理器都默认可以通过PriorityOrdered、Ordered接口来执行优先级
		2）、先注册PriorityOrdered优先级接口的BeanPostProcessor；
			把每一个BeanPostProcessor；添加到BeanFactory中
			beanFactory.addBeanPostProcessor(postProcessor);
		3）、再注册Ordered接口的
		4）、最后注册没有实现任何优先级接口的
		5）、最终注册MergedBeanDefinitionPostProcessor；
		6）、注册一个ApplicationListenerDetector；来在Bean创建完成后检查是否是ApplicationListener，如果是
			applicationContext.addApplicationListener((ApplicationListener<?>) bean);
## 7、initMessageSource();

初始化MessageSource组件（做国际化功能；消息绑定，消息解析）；

		1）、获取BeanFactory
		2）、看容器中是否有id为messageSource的，类型是MessageSource的组件
			如果有赋值给messageSource，如果没有自己创建一个DelegatingMessageSource；
				MessageSource：取出国际化配置文件中的某个key的值；能按照区域信息获取；
		3）、把创建好的MessageSource注册在容器中，以后获取国际化配置文件的值的时候，可以自动注入MessageSource；
			beanFactory.registerSingleton(MESSAGE_SOURCE_BEAN_NAME, this.messageSource);	
			MessageSource.getMessage(String code, Object[] args, String defaultMessage, Locale locale);
## 8、initApplicationEventMulticaster();

初始化事件派发器；

		1）、获取BeanFactory
		2）、从BeanFactory中获取applicationEventMulticaster的ApplicationEventMulticaster；
		3）、如果上一步没有配置；创建一个SimpleApplicationEventMulticaster
		4）、将创建的ApplicationEventMulticaster添加到BeanFactory中，以后其他组件直接自动注入
## 9、onRefresh();

留给子容器（子类）

		1、子类重写这个方法，在容器刷新的时候可以自定义逻辑；
		web容器就重写了这个方法来创建tomcat容器。
## 10、registerListeners();

给容器中将所有项目里面的ApplicationListener注册进来；

		1、从容器中拿到所有的ApplicationListener
		2、将每个监听器添加到事件派发器中；
			getApplicationEventMulticaster().addApplicationListenerBean(listenerBeanName);
		3、派发之前步骤产生的事件；
## 11、finishBeanFactoryInitialization(beanFactory);

初始化所有剩下的单实例bean；

	1、beanFactory.preInstantiateSingletons();初始化后剩下的单实例bean
		1）、获取容器中的所有Bean，依次进行初始化和创建对象
		2）、获取Bean的定义信息；RootBeanDefinition
		3）、Bean不是抽象的，是单实例的，是懒加载；
			1）、判断是否是FactoryBean；是否是实现FactoryBean接口的Bean；
			2）、不是工厂Bean。利用getBean(beanName);创建对象
				0、getBean(beanName)； ioc.getBean();
				1、doGetBean(name, null, null, false);
				2、先获取缓存中保存的单实例Bean。如果能获取到说明这个Bean之前被创建过（所有创建过的单实例Bean都会被缓存起来）
					从private final Map<String, Object> singletonObjects = new ConcurrentHashMap<String, Object>(256);获取的
				3、缓存中获取不到，开始Bean的创建对象流程；
				4、标记当前bean已经被创建
				5、获取Bean的定义信息；
				6、【获取当前Bean依赖的其他Bean;如果有按照getBean()把依赖的Bean先创建出来；】
				7、启动单实例Bean的创建流程；
					1）、createBean(beanName, mbd, args);
					2）、Object bean = resolveBeforeInstantiation(beanName, mbdToUse);让BeanPostProcessor先拦截返回代理对象；
						【InstantiationAwareBeanPostProcessor】：提前执行；
						先触发：postProcessBeforeInstantiation()；
						如果有返回值：触发postProcessAfterInitialization()；
					3）、如果前面的InstantiationAwareBeanPostProcessor没有返回代理对象；调用4）
					4）、Object beanInstance = doCreateBean(beanName, mbdToUse, args);创建Bean
						 1）、【创建Bean实例】；createBeanInstance(beanName, mbd, args);
						 	利用工厂方法或者对象的构造器创建出Bean实例；
						 2）、applyMergedBeanDefinitionPostProcessors(mbd, beanType, beanName);
						 	调用MergedBeanDefinitionPostProcessor的postProcessMergedBeanDefinition(mbd, beanType, beanName);
						 3）、【Bean属性赋值】populateBean(beanName, mbd, instanceWrapper);
						 	赋值之前：
						 	1）、拿到InstantiationAwareBeanPostProcessor后置处理器；
						 		postProcessAfterInstantiation()；
						 	2）、拿到InstantiationAwareBeanPostProcessor后置处理器；
						 		postProcessPropertyValues()；
						 	=====赋值之前：===
						 	3）、应用Bean属性的值；为属性利用setter方法等进行赋值；
						 		applyPropertyValues(beanName, mbd, bw, pvs);
						 4）、【Bean初始化】initializeBean(beanName, exposedObject, mbd);
						 	1）、【执行Aware接口方法】invokeAwareMethods(beanName, bean);执行xxxAware接口的方法
						 		BeanNameAware\BeanClassLoaderAware\BeanFactoryAware
						 	2）、【执行后置处理器初始化之前】applyBeanPostProcessorsBeforeInitialization(wrappedBean, beanName);
						 		BeanPostProcessor.postProcessBeforeInitialization（）;
						 	3）、【执行初始化方法】invokeInitMethods(beanName, wrappedBean, mbd);
						 		1）、是否是InitializingBean接口的实现；执行接口规定的初始化；
						 		2）、是否自定义初始化方法；
						 	4）、【执行后置处理器初始化之后】applyBeanPostProcessorsAfterInitialization
						 		BeanPostProcessor.postProcessAfterInitialization()；
						 5）、注册Bean的销毁方法；
					5）、将创建的Bean添加到缓存中singletonObjects；
				ioc容器就是这些Map；很多的Map里面保存了单实例Bean，环境信息。。。。；
		所有Bean都利用getBean创建完成以后；
			检查所有的Bean是否是SmartInitializingSingleton接口的；如果是；就执行afterSingletonsInstantiated()；
## 12、finishRefresh();

完成BeanFactory的初始化创建工作；IOC容器就创建完成；

		1）、initLifecycleProcessor();初始化和生命周期有关的后置处理器；LifecycleProcessor
			默认从容器中找是否有lifecycleProcessor的组件【LifecycleProcessor】；如果没有new DefaultLifecycleProcessor();
			加入到容器；


			写一个LifecycleProcessor的实现类，可以在BeanFactory
				void onRefresh();
				void onClose();	
		2）、	getLifecycleProcessor().onRefresh();
			拿到前面定义的生命周期处理器（BeanFactory）；回调onRefresh()；
		3）、publishEvent(new ContextRefreshedEvent(this));发布容器刷新完成事件；
		4）、liveBeansView.registerApplicationContext(this);



## 总结

	1）、Spring容器在启动的时候，先会保存所有注册进来的Bean的定义信息；
		1）、xml注册bean；<bean>
		2）、注解注册Bean；@Service、@Component、@Bean、xxx
	2）、Spring容器会合适的时机创建这些Bean
		1）、用到这个bean的时候；利用getBean创建bean；创建好以后保存在容器中；
		2）、统一创建剩下所有的bean的时候；finishBeanFactoryInitialization()；
	3）、后置处理器；BeanPostProcessor
		1）、每一个bean创建完成，都会使用各种后置处理器进行处理；来增强bean的功能；
			AutowiredAnnotationBeanPostProcessor:处理自动注入
			AnnotationAwareAspectJAutoProxyCreator:来做AOP功能；
			xxx....
			增强的功能注解：
			AsyncAnnotationBeanPostProcessor
			....
	4）、事件驱动模型；
		ApplicationListener；事件监听；
		ApplicationEventMulticaster；事件派发：



# springboot自动装配原理