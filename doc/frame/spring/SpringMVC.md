# MVC

Spring MVC 下我们一般把后端项目分为 Service层（处理业务）、Dao层（数据库操作）、Entity层（实体类）、Controller层(控制层，返回数据给前台页面)。

### Spring MVC 框架有什么用？

Spring Web MVC 框架提供 **模型-视图-控制器** 架构和随时可用的组件，用于开发灵活且松散耦合的 Web 应用程序。 MVC 模式有助于分离应用程序的不同方面，如输入逻辑，业务逻辑和 UI 逻辑，同时在所有这些元素之间提供松散耦合。

### 介绍一下 WebApplicationContext

WebApplicationContext 是 ApplicationContext 的扩展。它具有 Web 应用程序所需的一些额外功能。它与普通的 ApplicationContext 在解析主题和决定与哪个 servlet 关联的能力方面有所不同。



## 五大核心组件

> DispatcherServlet 请求的入口
> HandlerMapping 请求的派发 负责让请求 和 控制器建立一一对应的关联
> Controller（Handler） 真正的处理器
>
> HandlerAdapter：处理器适配器
>
> ModelAndView 封装模型信息和视图信息的
>
> ViewResolver 视图处理器 最终定位页面的



> DispatcherServlet 中的九大核心属性
>
> - MultipartResolver	内容类型( `Content-Type` )为 `multipart/*` 的请求的解析器接口
> - LocaleResolver	**本地化( 国际化 )解析器接口**。
> - ThemeResolver	主题解析器接口 因为现在的前端，基本和后端做了分离，所以这个功能已经越来越少用了。
> - HandlerMapping	处理器匹配接口，**根据请求( `handler` )获得其的处理器( `handler`)和拦截器们( HandlerInterceptor 数组 )。**
> - HandlerAdapter	处理器适配器接口	因为，处理器 `handler` 的类型是 Object 类型，需要有一个调用者来实现 `handler` 是怎么被使用，怎么被执行。而 HandlerAdapter 的用途就在于此
> - HandlerExceptionResolver    **处理器异常解析器接口，将处理器( `handler` )执行时发生的异常，解析( 转换 )成对应的 ModelAndView 结果。**
> - RequestToViewNameTranslator  请求到视图名的转换器接口
> - ViewResolver    **实体解析器接口，根据视图名和国际化，获得最终的视图 View 对象**
> - FlashMapManager    F**lashMap 管理器接口，负责重定向时，保存参数到临时存储中**。实际场景下，使用的非常少，特别是前后端分离之后



![img](img\SpringMVC的运行流程图.jpg)



第一步：用户发起request请求，请求至DispatcherServlet前端控制器。

第二步：DispatcherServlet前端控制器请求HandlerMapping处理器映射器查找Handler。

              DispatcherServlet：前端控制器，相当于中央调度器，各各组件都和前端控制器进行交互，降低了各组件之间耦合度。

第三步：HandlerMapping处理器映射器，根据url及一些配置规则（xml配置、注解配置）查找Handler，将Handler返回给                          DispatcherServlet前端控制器。

第四步：DispatcherServlet前端控制器调用适配器执行Handler，有了适配器通过适配器去扩展对不同Handler执行方式（比如：                原始servlet开发，注解开发）

第五步：适配器执行Handler。Handler是后端控制器，当成模型。

第六步：Handler执行完成返回ModelAndView

              ModelAndView是springmvc的一个对象，对Model和view进行封装。

第七步：适配器将ModelAndView返回给DispatcherServlet

第八步：DispatcherServlet调用视图解析器进行视图解析，解析后生成view。视图解析器根据逻辑视图名解析出真正的视图。

              View：springmvc视图封装对象，提供了很多view，比如：jsp、freemarker、pdf、excel。

第九步：ViewResolver视图解析器给前端控制器返回view

第十步：DispatcherServlet调用view的渲染视图的方法，将模型数据填充到request域 。

第十一步：DispatcherServlet向用户响应结果(jsp页面、json数据。)

## SpringMVC 0xml原理

使用javaConfig初始化项目：

实现WebApplicationInitializer接口，web容器启动的时候会调用onStartup方法。用java注解的方式去初始化spring上下文环境，AnnotationConfigWebApplicationContext ac=new AnnotationConfigWebApplicationContext（）；

ac.register(AppConfig.class);

DispatcherServlet servlet = new DispatcherServlet(ac);

spring帮我们实现了servlet的接口，把我们实现WebApplicationInitializer接口的类反射，并调用其onstartup方法并启动。



1、web容器在启动的时候，会扫描每个jar包下的META-INF/services/javax.servlet.ServletContainerInitializer
2、加载这个文件指定的类SpringServletContainerInitializer
3、spring的应用一启动会加载感兴趣的WebApplicationInitializer接口的下的所有组件；
4、并且为WebApplicationInitializer组件创建对象（组件不是接口，不是抽象类）
	1）、AbstractContextLoaderInitializer：创建根容器；createRootApplicationContext()；
	2）、AbstractDispatcherServletInitializer：
			创建一个web的ioc容器；createServletApplicationContext();
			创建了DispatcherServlet；createDispatcherServlet()；
			将创建的DispatcherServlet添加到ServletContext中；
				getServletMappings();
	3）、AbstractAnnotationConfigDispatcherServletInitializer：注解方式配置的DispatcherServlet初始化器
			创建根容器：createRootApplicationContext()
					getRootConfigClasses();传入一个配置类
			创建web的ioc容器： createServletApplicationContext();
					获取配置类；getServletConfigClasses();
	
总结：
	以注解方式来启动SpringMVC；继承AbstractAnnotationConfigDispatcherServletInitializer；
实现抽象方法指定DispatcherServlet的配置信息；

===========================
定制SpringMVC；
1）、@EnableWebMvc:开启SpringMVC定制配置功能；
	<mvc:annotation-driven/>；

2）、配置组件（视图解析器、视图映射、静态资源映射、拦截器。。。）
	extends WebMvcConfigurerAdapter

## SpringMVC源码分析

核心功能是请求分发

请求分发的核心类：DispatcherServlet 继承HTTPServlet

核心方法：doDispatch（HttpServletRequest request，HttpServletResponse response）处理分发逻辑。

HandlerMapping组件用于寻找获取到我们的Controller，实现类为RequestMappingHandlerMapping.getHandlerInternal

方法是getHandler(processedRequest) 获取到我们的Controller 

HandlerAdapter 适配器，完成调用方法

HandlerMethodArgumentResolver 参数处理器，进行参数处理

	1.  supportsParameter判断你的参数是否需要有当前处理器来处理
 	2.  resolveArgument 给这个参数赋值

除了加Controller注解来实现一个controller以外还有两种方式来实现

	1. 实现HttpRequestHandler接口
 	2. 实现Controller接口

## 从发请求到Controller中间的过程

1. 扫描整个项目，定义一个map集合
2. 拿到所有加了Controller注解的类
3. 遍历类里面的所有方法对象
4. 判断方法是否加了RequestMapping注解
5. 把@RequestMapping注解的value作为map集合的Key给put进去，把method对象作为value放入map集合中
6. 根据用户请求拿到请求中的uri
7. 使用请求中的uri去map中匹配，匹配不到就404，匹配到了就得到了方法对象

# 相关注解

## @Controller
@Controller 注解，它将一个类标记为 Spring Web MVC 控制器 Controller 。

## @RestController 和 @Controller 的区别
@RestController 注解，在 @Controller 基础上，增加了 @ResponseBody 注解，更加适合目前前后端分离的架构下，提供 Restful API ，返回例如 JSON 数据格式。当然，返回什么样的数据格式，根据客户端的 "ACCEPT" 请求头来决定。

## @RequestMapping
@RequestMapping 注解，用于将特定 HTTP 请求方法映射到将处理相应请求的控制器中的特定类/方法。此注释可应用于两个级别：

类级别：映射请求的 URL。

方法级别：映射 URL 以及 HTTP 请求方法。

## @RequestMapping 和 @GetMapping 区别
@RequestMapping 可注解在类和方法上；@GetMapping 仅可注册在方法上。

@RequestMapping 可进行 GET、POST、PUT、DELETE 等请求方法；@GetMapping 是 @RequestMapping 的 GET 请求方法的特例，目的是为了提高清晰度。

## 返回 JSON 格式使用的注解：
可以使用 @ResponseBody 注解，或者使用包含 @ResponseBody 注解的 @RestController 注解。

当然，还是需要配合相应的支持 JSON 格式化的 HttpMessageConverter 实现类。例如，Spring MVC 默认使用 MappingJackson2HttpMessageConverter 。

## @PathVariable 注解与@RequestParam的区别
@PathVariable 注解，是 Spring MVC 中有用的注解之一，它允许您从URI 读取值，比如查询参数。它在使用 Spring 创建 RESTful Web 服务时特别有用，因为在 REST 中，资源标识符是 URI 的一部分。

@RequestParam注解和@PathVariable注解的区别，从字面上可以看出前者是获取请求里边携带的参数；后者是获取请求路径里边的变量参数。

(例如：127.0.0.1/user/{userId}?userName=zhangshan,userId是路径上的变量，userName才是请求参数信息)

> 1.@RequestParam注解
>
> @RequestParam有三个参数：
>
> value：参数名；
>
> required：是否必需，默认为true，表示请求参数中必须包含该参数，如果不包含抛出异常。
>
> defaultValue：默认参数值，如果设置了该值自动将required设置为false，如果参数中没有包含该参数则使用默认值。
>
> 示例：@RequestParam(value = "userId", required = false, defaultValue = "1")
>
> 2.@PathVariable注解
>
> 当使用@RequestMapping URI占位符映射时，Url中可以通过一个或多个{xxxx}占位符映射，通过@PathVariable可以绑定占位符参数到方法参数中。
>
> 例如：@PathVariable("userId") Long userId,@PathVariable("userName") String userName
>
> (注：Long类型可以根据需求自己改变String或int，Spring会自动做转换)
>
> @RequestMapping(“/user/{userId}/{userName}/query")

## @RequestBody
@requestBody注解常用来处理content-type不是默认的application/x-www-form-urlcoded编码的内容，比如说：application/json或者是application/xml等。一般情况下来说常用其来处理application/json类型。

通过@requestBody可以将请求体中的JSON字符串绑定到相应的bean上，当然，也可以将其分别绑定到对应的字符串上。

前端通过post传数据过来,要用RequestBody注解。



