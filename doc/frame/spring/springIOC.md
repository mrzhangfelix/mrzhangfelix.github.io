# Spring IOC容器初始化

以下源码分析是根据spring-context：5.1.8：RELEASE版本进行分析的，欢迎参考学习

## this（）：

调用父类GenericApplicationContext构造函数，构建beanFactory （newDefaultListableBeanFactory（））

```java
this.reader = new AnnotatedBeanDefinitionReader(this);
this.scanner = new ClassPathBeanDefinitionScanner(this);
```

### 初始化：注解bean的定义读取器

获取bean工厂  

```java
DefaultListableBeanFactory beanFactory = unwrapDefaultListableBeanFactory(registry);
```

bean的定义Set集合

```java
Set<BeanDefinitionHolder> beanDefs = new LinkedHashSet<>(8);
```

 检查各个组件是否已经注册过了，没有注册就将其放入beandefs中去。

- org.springframework.context.annotation.ConfigurationClassPostProcessor//解析@Configuration配置bean定义后置处理器，在bean定义完成之后执行，因为实现的是BeanDefinitionRegistryPostProcessor
- org.springframework.context.event.DefaultEventListenerFactory//注册事件监听器工厂
- org.springframework.context.event.EventListenerMethodProcessor//处理监听方法的注解解析器
- org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor//解析Autowired
- org.springframework.context.annotation.CommonAnnotationBeanPostProcessor//解析JSR规范的

### 初始化：classPath的定义扫描器 用于扫描注解的一些bean

```java
this.includeFilters.add(new AnnotationTypeFilter(Component.class));//让我们的compent能够被识别
this.includeFilters.add(new AnnotationTypeFilter(
					((Class<? extends Annotation>) ClassUtils.forName("javax.annotation.ManagedBean", cl)), false));//JSR-250的注解规范
    this.includeFilters.add(new AnnotationTypeFilter(
					((Class<? extends Annotation>) ClassUtils.forName("javax.inject.Named", cl)), false));//JSR-330的注解规范
```



## register(annotatedClasses);

注册配置类：将配置类注册到容器中去，可以传入多个配置类依次进行遍历注册。

```
AnnotatedGenericBeanDefinition abd = new AnnotatedGenericBeanDefinition(annotatedClass);
```

设置bean定义的属性：是否懒加载，Bean初始化销毁的顺序、是否首选、设置依赖、描述等等

```
BeanDefinitionHolder definitionHolder = new BeanDefinitionHolder(abd, beanName);
```

最后将definitionHolder注册到容器中去。



## refresh()【创建刷新】（重点工作）;

源码如下：

```java
// Prepare this context for refreshing.
prepareRefresh();

// Tell the subclass to refresh the internal bean factory.
ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

// Prepare the bean factory for use in this context.
prepareBeanFactory(beanFactory);

try {
   // Allows post-processing of the bean factory in context subclasses.
   postProcessBeanFactory(beanFactory);

   // Invoke factory processors registered as beans in the context.
   invokeBeanFactoryPostProcessors(beanFactory);

   // Register bean processors that intercept bean creation.
   registerBeanPostProcessors(beanFactory);

   // Initialize message source for this context.
   initMessageSource();

   // Initialize event multicaster for this context.
   initApplicationEventMulticaster();

   // Initialize other special beans in specific context subclasses.
   onRefresh();

   // Check for listener beans and register them.
   registerListeners();

   // Instantiate all remaining (non-lazy-init) singletons.
   finishBeanFactoryInitialization(beanFactory);

   // Last step: publish corresponding event.
   finishRefresh();
}
```



### 1、prepareRefresh()

刷新前的预处理;

1）、initPropertySources()

​		初始化一些属性设置;子类自定义个性化的属性设置方法；空方法需要子类继承实现。
2）、getEnvironment().validateRequiredProperties();

​		检验属性的合法等
3）、earlyApplicationEvents= new LinkedHashSet<ApplicationEvent>();

​		保存容器中的一些早期的事件；

### 2、obtainFreshBeanFactory();

获取BeanFactory；

1）、refreshBeanFactory();刷新	只允许bean工厂刷新一次，设置id；
2）、getBeanFactory();返回刚才GenericApplicationContext创建的BeanFactory对象；
3）、将创建的BeanFactory【DefaultListableBeanFactory】返回；

### 3、prepareBeanFactory(beanFactory);

BeanFactory的预准备工作（BeanFactory进行一些设置）；

1）、设置BeanFactory的类加载器、支持表达式解析器...
2）、添加感知回调的后置处理器，BeanPostProcessor【ApplicationContextAwareProcessor】
3）、设置忽略的感知自动装配的接口
		EnvironmentAware、EmbeddedValueResolverAware、ResourceLoaderAware、ApplicationEventPublisherAware、MessageSourceAware、ApplicationContextAware；
4）、注册可以解析的自动装配；我们能直接在任何组件中自动注入：
		BeanFactory、ResourceLoader、ApplicationEventPublisher、ApplicationContext
5）、添加早期BeanPostProcessor，用于发现内部bean
		ApplicationListenerDetector
6）、添加编译时的AspectJ切面代码织入；后置处理器
		LoadTimeWeaverAwareProcessor
7）、给BeanFactory中注册一些环境的组件；
	environment【ConfigurableEnvironment】、
	systemProperties【Map<String, Object>】、
	systemEnvironment【Map<String, Object>】

### 4、postProcessBeanFactory(beanFactory);

BeanFactory准备工作完成后进行的后置处理工作；

1）、子类通过重写这个方法来在BeanFactory创建并预准备完成以后做进一步的设置，可以给beanfactory添加一些自己想要实现的bean工厂后置处理器。

### 5、invokeBeanFactoryPostProcessors(beanFactory);

执行BeanFactoryPostProcessor的方法；执行bean工厂的后置处理器

BeanFactoryPostProcessor：BeanFactory的后置处理器。在BeanFactory标准初始化之后执行的；
两个接口：BeanFactoryPostProcessor、BeanDefinitionRegistryPostProcessor

#### 5.1先执行BeanDefinitionRegistryPostProcessor

1）、获取所有的BeanDefinitionRegistryPostProcessor；

```java
String[] postProcessorNames =
      beanFactory.getBeanNamesForType(BeanDefinitionRegistryPostProcessor.class, true, false);
```

2）、看先执行实现了PriorityOrdered优先级接口的BeanDefinitionRegistryPostProcessor、

```java
// First, invoke the BeanDefinitionRegistryPostProcessors that implement PriorityOrdered.
for (String ppName : postProcessorNames) {
   if (beanFactory.isTypeMatch(ppName, PriorityOrdered.class)) {
      currentRegistryProcessors.add(beanFactory.getBean(ppName, BeanDefinitionRegistryPostProcessor.class));
      processedBeans.add(ppName);
   }
}
sortPostProcessors(currentRegistryProcessors, beanFactory);
registryProcessors.addAll(currentRegistryProcessors);
invokeBeanDefinitionRegistryPostProcessors(currentRegistryProcessors, registry);
currentRegistryProcessors.clear();
```

3）、在执行实现了Ordered顺序接口的BeanDefinitionRegistryPostProcessor；

```java
postProcessorNames = beanFactory.getBeanNamesForType(BeanDefinitionRegistryPostProcessor.class, true, false);
for (String ppName : postProcessorNames) {
	if (!processedBeans.contains(ppName) && beanFactory.isTypeMatch(ppName, Ordered.class)) {
	   currentRegistryProcessors.add(beanFactory.getBean(ppName,BeanDefinitionRegistryPostProcessor.class));
		processedBeans.add(ppName);
	}
}
sortPostProcessors(currentRegistryProcessors, beanFactory);
registryProcessors.addAll(currentRegistryProcessors);
invokeBeanDefinitionRegistryPostProcessors(currentRegistryProcessors, registry);
currentRegistryProcessors.clear();
```

4）、最后执行没有实现任何优先级或者是顺序接口的BeanDefinitionRegistryPostProcessors；

```java
// Finally, invoke all other BeanDefinitionRegistryPostProcessors until no further ones appear.
			boolean reiterate = true;
			while (reiterate) {
				reiterate = false;
				postProcessorNames = beanFactory.getBeanNamesForType(BeanDefinitionRegistryPostProcessor.class, true, false);
				for (String ppName : postProcessorNames) {
					if (!processedBeans.contains(ppName)) {
						currentRegistryProcessors.add(beanFactory.getBean(ppName, BeanDefinitionRegistryPostProcessor.class));
						processedBeans.add(ppName);
						reiterate = true;
					}
				}
				sortPostProcessors(currentRegistryProcessors, beanFactory);
				registryProcessors.addAll(currentRegistryProcessors);
				invokeBeanDefinitionRegistryPostProcessors(currentRegistryProcessors, registry);
				currentRegistryProcessors.clear();
			}
```



#### 5.2再执行BeanFactoryPostProcessor的方法

1）、获取所有的BeanFactoryPostProcessor

```java
String[] postProcessorNames =
      beanFactory.getBeanNamesForType(BeanFactoryPostProcessor.class, true, false);

// Separate between BeanFactoryPostProcessors that implement PriorityOrdered,
// Ordered, and the rest.
List<BeanFactoryPostProcessor> priorityOrderedPostProcessors = new ArrayList<>();
List<String> orderedPostProcessorNames = new ArrayList<>();
List<String> nonOrderedPostProcessorNames = new ArrayList<>();
for (String ppName : postProcessorNames) {
   if (processedBeans.contains(ppName)) {
      // skip - already processed in first phase above
   }
   else if (beanFactory.isTypeMatch(ppName, PriorityOrdered.class)) {
      priorityOrderedPostProcessors.add(beanFactory.getBean(ppName, BeanFactoryPostProcessor.class));
   }
   else if (beanFactory.isTypeMatch(ppName, Ordered.class)) {
      orderedPostProcessorNames.add(ppName);
   }
   else {
      nonOrderedPostProcessorNames.add(ppName);
   }
}
```

2）、看先执行实现了PriorityOrdered优先级接口的BeanFactoryPostProcessor、

```java
// First, invoke the BeanFactoryPostProcessors that implement PriorityOrdered.
sortPostProcessors(priorityOrderedPostProcessors, beanFactory);
invokeBeanFactoryPostProcessors(priorityOrderedPostProcessors, beanFactory);
```

​		

3）、在执行实现了Ordered顺序接口的BeanFactoryPostProcessor；

```java
// Next, invoke the BeanFactoryPostProcessors that implement Ordered.
List<BeanFactoryPostProcessor> orderedPostProcessors = new ArrayList<>();
for (String postProcessorName : orderedPostProcessorNames) {
   orderedPostProcessors.add(beanFactory.getBean(postProcessorName, BeanFactoryPostProcessor.class));
}
sortPostProcessors(orderedPostProcessors, beanFactory);
invokeBeanFactoryPostProcessors(orderedPostProcessors, beanFactory);
```

4）、最后执行没有实现任何优先级或者是顺序接口的BeanFactoryPostProcessor；

```java
// Finally, invoke all other BeanFactoryPostProcessors.
List<BeanFactoryPostProcessor> nonOrderedPostProcessors = new ArrayList<>();
for (String postProcessorName : nonOrderedPostProcessorNames) {
   nonOrderedPostProcessors.add(beanFactory.getBean(postProcessorName, BeanFactoryPostProcessor.class));
}
invokeBeanFactoryPostProcessors(nonOrderedPostProcessors, beanFactory);
```



检测LoadTimeWeaver，并准备代码织入，注册一个LoadTimeWeaver的bean后置处理器

```java
if (beanFactory.getTempClassLoader() == null && beanFactory.containsBean(LOAD_TIME_WEAVER_BEAN_NAME)) {
   beanFactory.addBeanPostProcessor(new LoadTimeWeaverAwareProcessor(beanFactory));
   beanFactory.setTempClassLoader(new ContextTypeMatchClassLoader(beanFactory.getBeanClassLoader()));
}
```



### 6、registerBeanPostProcessors(beanFactory);

注册BeanPostProcessor（Bean的后置处理器）【 intercept bean creation】

​	不同接口类型的BeanPostProcessor；在Bean创建前后的执行时机是不一样的
​	BeanPostProcessor、
​	DestructionAwareBeanPostProcessor、
​	InstantiationAwareBeanPostProcessor、
​	SmartInstantiationAwareBeanPostProcessor、
​	MergedBeanDefinitionPostProcessor【internalPostProcessors】、


1）、获取所有的 BeanPostProcessor;后置处理器都默认可以通过PriorityOrdered、Ordered接口来执行优先级

```java
String[] postProcessorNames = beanFactory.getBeanNamesForType(BeanPostProcessor.class, true, false);
```

2）、注册一个BeanPostProcessorChecker

BeanPostProcessorChecker在一个Bean不能被所有BeanPostProcessor处理时，会打印信息。	

3）、对所有的bean后置处理器进行分类排序

```java
List<BeanPostProcessor> priorityOrderedPostProcessors = new ArrayList<>();
List<BeanPostProcessor> internalPostProcessors = new ArrayList<>();
List<String> orderedPostProcessorNames = new ArrayList<>();
List<String> nonOrderedPostProcessorNames = new ArrayList<>();
for (String ppName : postProcessorNames) {
   if (beanFactory.isTypeMatch(ppName, PriorityOrdered.class)) {
      BeanPostProcessor pp = beanFactory.getBean(ppName, BeanPostProcessor.class);
      priorityOrderedPostProcessors.add(pp);
      if (pp instanceof MergedBeanDefinitionPostProcessor) {
         internalPostProcessors.add(pp);
      }
   }
   else if (beanFactory.isTypeMatch(ppName, Ordered.class)) {
      orderedPostProcessorNames.add(ppName);
   }
   else {
      nonOrderedPostProcessorNames.add(ppName);
   }
}
```

2）、先注册PriorityOrdered优先级接口的BeanPostProcessor；

```java
sortPostProcessors(priorityOrderedPostProcessors, beanFactory);
registerBeanPostProcessors(beanFactory, priorityOrderedPostProcessors);
```

​	把每一个BeanPostProcessor；添加到BeanFactory中
​	beanFactory.addBeanPostProcessor(postProcessor);
3）、再注册Ordered接口的

```java
// Next, register the BeanPostProcessors that implement Ordered.
List<BeanPostProcessor> orderedPostProcessors = new ArrayList<>();
for (String ppName : orderedPostProcessorNames) {
   BeanPostProcessor pp = beanFactory.getBean(ppName, BeanPostProcessor.class);
   orderedPostProcessors.add(pp);
   if (pp instanceof MergedBeanDefinitionPostProcessor) {
      internalPostProcessors.add(pp);
   }
}
sortPostProcessors(orderedPostProcessors, beanFactory);
registerBeanPostProcessors(beanFactory, orderedPostProcessors);
```

4）、最后注册没有实现任何优先级接口的

```java
List<BeanPostProcessor> nonOrderedPostProcessors = new ArrayList<>();
for (String ppName : nonOrderedPostProcessorNames) {
   BeanPostProcessor pp = beanFactory.getBean(ppName, BeanPostProcessor.class);
   nonOrderedPostProcessors.add(pp);
   if (pp instanceof MergedBeanDefinitionPostProcessor) {
      internalPostProcessors.add(pp);
   }
}
registerBeanPostProcessors(beanFactory, nonOrderedPostProcessors);
```

5）、最终注册重新注册所有的内部bean后置处理器；

```java
// Finally, re-register all internal BeanPostProcessors.
sortPostProcessors(internalPostProcessors, beanFactory);
registerBeanPostProcessors(beanFactory, internalPostProcessors);
```

6）、重新注册一个ApplicationListenerDetector；

用于发现内部bean是否是一个ApplicationListener

```java
// Re-register post-processor for detecting inner beans as ApplicationListeners,
// moving it to the end of the processor chain (for picking up proxies etc).
beanFactory.addBeanPostProcessor(new ApplicationListenerDetector(applicationContext));
```



### 7、initMessageSource();

初始化MessageSource组件（做国际化功能；消息绑定，消息解析）；MessageSource：取出国际化配置文件中的某个key的值；能按照区域信息获取；

1）、获取BeanFactory
2）、看容器中是否有id为messageSource的，类型是MessageSource的组件
		如果有赋值给messageSource，如果没有自己创建一个DelegatingMessageSource；
3）、把创建好的MessageSource注册在容器中，以后获取国际化配置文件的值的时候，可以自动注入MessageSource；

```java
// Use empty MessageSource to be able to accept getMessage calls.
DelegatingMessageSource dms = new DelegatingMessageSource();
dms.setParentMessageSource(getInternalParentMessageSource());
this.messageSource = dms;
beanFactory.registerSingleton(MESSAGE_SOURCE_BEAN_NAME, this.messageSource);
MessageSource.getMessage(String code, Object[] args, String defaultMessage, Locale locale);
```



### 8、initApplicationEventMulticaster();

初始化应用事件派发器（多播器）；

1）、获取BeanFactory
2）、从BeanFactory中获取applicationEventMulticaster的ApplicationEventMulticaster；
3）、如果上一步没有配置；创建一个SimpleApplicationEventMulticaster

```java
this.applicationEventMulticaster = new SimpleApplicationEventMulticaster(beanFactory);
beanFactory.registerSingleton(APPLICATION_EVENT_MULTICASTER_BEAN_NAME, this.applicationEventMulticaster);
```

4）、将创建的ApplicationEventMulticaster添加到BeanFactory中，以后其他组件直接自动注入

### 9、onRefresh();

> 留给子类扩展，子类重写这个方法，在单例bean被初始化之前调用；
>

### 10、registerListeners();

> 检查侦听器bean并注册它们
>
> 1、首先注册静态指定的侦听器。
>
> 2、从容器中拿到所有的ApplicationListener，不要在这里初始化factorybean:我们需要保留所有常规bean未初始化，让后处理器应用到它们!将每个监听器添加到事件派发器中；
>
> ```java
> getApplicationEventMulticaster().addApplicationListenerBean(listenerBeanName);
> ```
>
> 3、派发之前步骤产生的事件；

### 11、finishBeanFactoryInitialization(beanFactory);【重点步骤】

初始化所有剩下的（非懒加载）单实例bean；

1、bean设置类型转换器（conversion service）、添加默认的值解析器（EmbeddedValueResolver）冻结所有的bean定义，不让修改了，等前置操作不进行细致分析了

2、beanFactory.preInstantiateSingletons();初始化后剩下的单实例bean
	1）、获取容器中的所有beanNames，依次进行初始化和创建对象

List<String> beanNames = new ArrayList<>(this.beanDefinitionNames);

​	2）、遍历获取Bean的定义信息；RootBeanDefinition

RootBeanDefinition bd = getMergedLocalBeanDefinition(beanName);

​	3）、不是抽象的，是单实例的，不是懒加载的bean进入下面步骤；
​				判断是否是工厂Bean；是否是实现FactoryBean接口的Bean；

利用getBean(beanName);创建对象，以下是调用doGetBean(name, null, null, false);方法
			
			3.1、先获取缓存中保存的单实例Bean。如果能获取到说明这个Bean之前被创建过（所有创建过的单实例Bean都会被缓存起来）

```java
Object sharedInstance = getSingleton(beanName);
```

如果获取到的实例不为空但是args为空，说明正在该bean还没有初始化完，存在循环引用，调用getObjectForBeanInstance方法获取bean；从factoryBeanObjectCache中返回一个bean出去

```java
private final Map<String, Object> singletonObjects = new ConcurrentHashMap<>(256);//一级缓存，存放已经创建好的bean，先从这里获取
private final Map<String, Object> earlySingletonObjects = new HashMap<>(16);//早期的单实例对象，提前曝光的单例对象的cache，存放原始的 bean 对象（尚未填充属性），用于解决循环依赖，正在创建中的，再从这里获取
private final Map<String, ObjectFactory<?>> singletonFactories = new HashMap<>(16);//单实例工厂的缓存，最后从这里获取，单例对象工厂的cache，存放 bean 工厂对象，用于解决循环依赖，如果获取到了就把对象移动到二级缓存中去。
//以上三个缓存中都获取不到说明bean没有被创建
private final Set<String> singletonsCurrentlyInCreation =
			Collections.newSetFromMap(new ConcurrentHashMap<>(16));//正在创建中的bean都会存在这个map里面
private final Set<String> registeredSingletons = new LinkedHashSet<>(256);//已经注册好的单实例会放到其中去
//FactoryBeanRegistrySupport类中的缓存
private final Map<String, Object> factoryBeanObjectCache = new ConcurrentHashMap<>(16);//被工厂bean创建的单实例对象，Spring注册的 bean是 FactoryBean的话、这个 FactoryBean存放在三级缓存中、FactoryBean产生的 bean缓存在这里
```

​			3.2、缓存中获取不到，开始Bean的创建对象流程；判断当前bean是否正在创建中，

```java
// Fail if we're already creating this bean instance:
// We're assumably within a circular reference.
if (isPrototypeCurrentlyInCreation(beanName)) {
   throw new BeanCurrentlyInCreationException(beanName);//重复创建会抛出异常，
}

if (!typeCheckOnly) {
	markBeanAsCreated(beanName);//标记这个bean正在创建中
}
```

​			3.3、获取Bean的定义信息；

```java
final RootBeanDefinition mbd = getMergedLocalBeanDefinition(beanName);
```

​			3.4、获取当前Bean依赖的其他Bean;如果有按照getBean()把依赖的Bean先创建出来；

```
String[] dependsOn = mbd.getDependsOn();
```

​			3.5、判断是否单实例bean，启动单实例Bean的创建流程；（我这里只分析一下单实例bean，多实例的bean是不被ioc容器管理的）
​				执行getSingleton（），里面执行createBean(beanName, mbd, args);方法，

​				3.5.1、复制bean的定义信息和准备方法覆盖
​				3.5.2、给BeanPostProcessors一个返回目标实例代理对象的机会

```java
// Give BeanPostProcessors a chance to return a proxy instead of the target bean instance.
			Object bean = resolveBeforeInstantiation(beanName, mbdToUse);
			if (bean != null) {
				return bean;
			}
```

​				3.5.2.1、让BeanPostProcessor先拦截返回代理对象；InstantiationAwareBeanPostProcessor：提前执行；
​					先触发：postProcessBeforeInstantiation()；
​					如果有返回值：触发postProcessAfterInitialization()；

```java
bean = applyBeanPostProcessorsBeforeInstantiation(targetType, beanName);
if (bean != null) {
   bean = applyBeanPostProcessorsAfterInitialization(bean, beanName);
}
```


​				3.5.3、如果获取到的代理对象为null；以下是执行doCreateBean（）方法，

```java
Object beanInstance = doCreateBean(beanName, mbdToUse, args);//创建Bean
```

=========================================================================================================================			

​					3.5.3.1、获取实例包装对象，先从factoryBeanInstanceCache中删除该bean，未完成的FactoryBean实例的缓存。获取instanceWrapper

```java
		if (mbd.isSingleton()) {
			instanceWrapper = this.factoryBeanInstanceCache.remove(beanName);
		}
		if (instanceWrapper == null) {
			instanceWrapper = createBeanInstance(beanName, mbd, args);
		}
```

​				3.5.3.2、applyMergedBeanDefinitionPostProcessors(mbd, beanType, beanName);

​							后置处理器处理bean定义信息	调用MergedBeanDefinitionPostProcessor的postProcessMergedBeanDefinition(mbd, beanType, beanName);

​				3.5.3.3、快速缓存单例，以便能够解决循环引用	

```java
if (!this.singletonObjects.containsKey(beanName)) {
   this.singletonFactories.put(beanName, singletonFactory);
   this.earlySingletonObjects.remove(beanName);
   this.registeredSingletons.add(beanName);
}
```

​				3.5.3.4、【Bean属性赋值】

```java
populateBean(beanName, mbd, instanceWrapper);
```

​					 	1）、拿到InstantiationAwareBeanPostProcessor后置处理器；
​					 		postProcessAfterInstantiation()；
​					 	2）、拿到InstantiationAwareBeanPostProcessor后置处理器；
​					 		postProcessPropertyValues()；
​					 	3）、应用Bean属性的值；为属性利用setter方法等进行赋值；
​					 		applyPropertyValues(beanName, mbd, bw, pvs);
​				3.5.3.5、【Bean初始化】initializeBean(beanName, exposedObject, mbd);
​					 	1）、【执行Aware接口方法】invokeAwareMethods(beanName, bean);执行xxxAware接口的方法
​					 		BeanNameAware\BeanClassLoaderAware\BeanFactoryAware
​					 	2）、【执行后置处理器初始化之前】applyBeanPostProcessorsBeforeInitialization(wrappedBean, beanName);
​					 		BeanPostProcessor.postProcessBeforeInitialization（）;
​					 	3）、【执行初始化方法】invokeInitMethods(beanName, wrappedBean, mbd);
​					 		1）、是否是InitializingBean接口的实现；执行接口规定的初始化；
​					 		2）、是否自定义初始化方法；
​						4）、【执行后置处理器初始化之后】applyBeanPostProcessorsAfterInitialization
​					 		BeanPostProcessor.postProcessAfterInitialization()；

​						5）、返回包装weappedBean

​				3.5.3.6、注册Bean的销毁方法；

```java
registerDisposableBeanIfNecessary(beanName, bean, mbd);
```

=========================================================================================================================				



3.6、将创建的Bean添加到缓存中singletonObjects；

```java
synchronized (this.singletonObjects) {
   this.singletonObjects.put(beanName, singletonObject);//单例对象的缓存  Map<String, Object>
   this.singletonFactories.remove(beanName);//单例工厂的缓存，从三级缓存中删除。
   this.earlySingletonObjects.remove(beanName);//早期单例对象的缓存，二级缓存中也删除。
   this.registeredSingletons.add(beanName);//一组已注册的单例对象，按注册顺序包含bean名称。Set<String>
}
```


​		4）、所有Bean都利用getBean创建完成以后；触发初始化后回调
​			检查所有的Bean是否是SmartInitializingSingleton接口的；如果是；就执行afterSingletonsInstantiated()；

### 12、finishRefresh();

完成BeanFactory的初始化创建工作；IOC容器就创建完成；

1）、clearResourceCaches();清除资源缓存

2）、initLifecycleProcessor();初始化和生命周期有关的后置处理器；LifecycleProcessor

	默认从容器中找是否有lifecycleProcessor的组件【LifecycleProcessor】；
	
	如果没有new DefaultLifecycleProcessor();加入到容器；
	
	DefaultLifecycleProcessor defaultProcessor = new DefaultLifecycleProcessor();
	defaultProcessor.setBeanFactory(beanFactory);
	this.lifecycleProcessor = defaultProcessor;

3）、	getLifecycleProcessor().onRefresh();
		拿到前面定义的生命周期处理器（BeanFactory）；回调onRefresh()；
4）、publishEvent(new ContextRefreshedEvent(this));发布容器刷新完成事件；
5）、liveBeansView.registerApplicationContext(this);java管理拓展；




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