# Spring Boot

简化新 Spring 应用的初始搭建以及开发过程。使开发人员不再需要定义样板化的配置。Spring Boot 整合了所有的框架。

- 简化Spring应用开发的一个框架；

- 整个Spring技术栈的一个大整合；

- J2EE开发的一站式解决方案；

方便、快速搭建项目，使我们不用关心框架之间的兼容性，适用版本等各种问题，我们想使用任何东西，仅仅添加一个配置就可以，所以使用 Spring Boot 非常适合构建微服务



. 内嵌servlet容器。内嵌容器，使得我们可以执行运行项目的主程序main函数，使得项目可以快速运行。

. 自动配置：针对很多Spring应用程序常见的应用功能，Spring Boot能自动提供相关配置

. 提供starter简化Maven配置。springboot提供了一系列的start用来简化maven依赖。如：常用的spring-boot-starter-web、spring-boot-starter-tomcat、spring-boot-starter-actuator等

. 命令行界面：这是Spring Boot的可选特性，借此你只需写代码就能完成完整的应用程序，无需传统项目构建。

. Actuator：让你能够深入运行中的Spring Boot应用程序，一探究竟。用于监控统计。springboot提供了actuator组件，只需要在配置中加入spring-boot-starter-actuator依赖，通过继承AbstractHealthIndicator这个抽象类，然后在doHealthCheck()方法中检测服务健康的方法，就可以实现一个简单的监控。



# 微服务

2014，martin fowler

微服务：架构风格（服务微化）

一个应用应该是一组小型服务；可以通过HTTP的方式进行互通；

单体应用：ALL IN ONE

微服务：每一个功能元素最终都是一个可独立替换和独立升级的软件单元；



## 入门

简单、快速、方便

环境约束

– jdk1.8：Spring Boot 推荐jdk1.7及以上；java version "1.8.0_112"

– maven3.x：maven 3.3以上版本；Apache Maven 3.3.9

– IntelliJIDEA2017：IntelliJ IDEA 2017.2.2 x64、STS

– SpringBoot 1.5.9.RELEASE：1.5.9；

统一环境；

给maven 的settings.xml配置文件的profiles标签添加

```xml
<profile>
  <id>jdk-1.8</id>
  <activation>
    <activeByDefault>true</activeByDefault>
    <jdk>1.8</jdk>
  </activation>
  <properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
  </properties>
</profile>
```





引入 web 模块

1、pom.xml中添加支持web的模块：

```xml

<dependency>

    <groupId>org.springframework.boot</groupId>

    <artifactId>spring-boot-starter-web</artifactId>

</dependency>

```

- spring-boot-starter ：核心模块，包括自动配置支持、日志和 YAML，
- 如果引入了 spring-boot-starter-web web 模块可以去掉此配置，因为 spring-boot-starter-web 自动依赖了 spring-boot-starter。spring-boot场景启动器；帮我们导入了web模块正常运行所依赖的组件；

- spring-boot-starter-test ：测试模块，包括 JUnit、Hamcrest、Mockito。

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>1.5.9.RELEASE</version>
</parent>
```

Spring Boot的版本仲裁中心；

以后我们导入依赖默认是不需要写版本；（没有在dependencies里面管理的依赖自然需要声明版本号）



Spring Boot将所有的功能场景都抽取出来，做成一个个的starters（启动器），只需要在项目里面引入这些starter相关场景的所有依赖都会导入进来。要用什么功能就导入什么场景的启动器

### 编写一个主程序；启动Spring Boot应用

```java
/**
 *  @SpringBootApplication 来标注一个主程序类，说明这是一个Spring Boot应用
 */
@SpringBootApplication
public class HelloWorldMainApplication {

    public static void main(String[] args) {

        // Spring应用启动起来
        SpringApplication.run(HelloWorldMainApplication.class,args);
    }
}
```

@**SpringBootApplication**:    Spring Boot应用标注在某个类上说明这个类是SpringBoot的主配置类，SpringBoot就应该运行这个类的main方法来启动

```java
SpringBoot应用；

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = {
      @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
      @Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {
```



### 开发环境的调试

热启动 需要添加以下的配置：

```xml

<dependencies>

    <dependency>

        <groupId>org.springframework.boot</groupId>

        <artifactId>spring-boot-devtools</artifactId>

        <optional>true</optional>

    </dependency>

</dependencies>

<build>

    <plugins>

        <plugin>

            <groupId>org.springframework.boot</groupId>

            <artifactId>spring-boot-maven-plugin</artifactId>

            <configuration>

                <fork>true</fork>

            </configuration>

        </plugin>

</plugins>

</build>




```

### 简化部署

```xml
 <!-- 这个插件，可以将应用打包成一个可执行的jar包；-->
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```

将这个应用打成jar包，直接使用java -jar的命令进行执行；



# 二、配置文件

## 1、配置文件

SpringBoot使用一个全局的配置文件，配置文件名是固定的；

•application.properties

•application.yml



配置文件的作用：修改SpringBoot自动配置的默认值；SpringBoot在底层都给我们自动配置好；



YAML（YAML Ain't Markup Language）

​	YAML  A Markup Language：是一个标记语言

​	YAML   isn't Markup Language：不是一个标记语言；

标记语言：

​	以前的配置文件；大多都使用的是  **xxxx.xml**文件；

​	YAML：**以数据为中心**，比json、xml等更适合做配置文件；

## 2、YAML语法：

1、基本语法

k:(空格)v：表示一对键值对（空格必须有）；

以**空格**的缩进来控制层级关系；只要是左对齐的一列数据，都是同一个层级的

```yaml
server:
    port: 8081
    path: /hello
```

属性和值也是大小写敏感；



2、值的写法

字面量：普通的值（数字，字符串，布尔）

​	k: v：字面直接来写；

​		字符串默认不用加上单引号或者双引号；

​		""：双引号；不会转义字符串里面的特殊字符；特殊字符会作为本身想表示的意思

​				name:   "zhangsan \n lisi"：输出；zhangsan 换行  lisi

​		''：单引号；会转义特殊字符，特殊字符最终只是一个普通的字符串数据

​				name:   ‘zhangsan \n lisi’：输出；zhangsan \n  lisi



对象、Map（属性和值）（键值对）：

​	k: v：在下一行来写对象的属性和值的关系；注意缩进

​		对象还是k: v的方式

```yaml
friends:
		lastName: zhangsan
		age: 20
```

行内写法：

```yaml
friends: {lastName: zhangsan,age: 18}
```



数组（List、Set）：

用- 值表示数组中的一个元素

```yaml
pets:
 - cat
 - dog
 - pig
```

行内写法

```yaml
pets: [cat,dog,pig]
```



## 3、配置文件值注入

配置文件

```yaml
person:
    lastName: hello
    age: 18
    boss: false
    birth: 2017/12/12
    maps: {k1: v1,k2: 12}
    lists:
      - lisi
      - zhaoliu
    dog:
      name: 小狗
      age: 12
```

javaBean：

```java
/**
 * 将配置文件中配置的每一个属性的值，映射到这个组件中
 * @ConfigurationProperties：告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定；
 *      prefix = "person"：配置文件中哪个下面的所有属性进行一一映射
 *
 * 只有这个组件是容器中的组件，才能容器提供的@ConfigurationProperties功能；
 *
 */
@Component
@ConfigurationProperties(prefix = "person")
public class Person {

    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;

    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;

```



我们可以导入配置文件处理器，以后编写配置就有提示了

```xml
<!--导入配置文件处理器，配置文件进行绑定就会有提示-->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-configuration-processor</artifactId>
			<optional>true</optional>
		</dependency>
```

## @Value比较

|                      | @ConfigurationProperties | @Value     |
| -------------------- | ------------------------ | ---------- |
| 功能                 | 批量注入配置文件中的属性 | 一个个指定 |
| 松散绑定（松散语法） | 支持                     | 不支持     |
| SpEL                 | 不支持                   | 支持       |
| JSR303数据校验       | 支持                     | 不支持     |
| 复杂类型封装         | 支持                     | 不支持     |

配置文件yml还是properties他们都能获取到值；

如果说，我们只是在某个业务逻辑中需要获取一下配置文件中的某项值，使用@Value；

如果说，我们专门编写了一个javaBean来和配置文件进行映射，我们就直接使用@ConfigurationProperties；

## 自定义 Property

配置在 application.properties 中

```properties

com.felix.title=felix

com.felix.description=good

```

自定义配置类

```java

@Component

public class NeoProperties {

@Value("${com.neo.title}")

private String title;

@Value("${com.neo.description}")

private String description;

//省略getter settet方法

}

```

## 使用自定义配置文件

  有时候我们不希望把所有配置都放在application.properties里面，这时候我们可以另外定义一个，这里我明取名为test.properties,路径跟也放在src/main/resources下面。

```properties

    com.md.name="哟西~"

  com.md.want="祝大家鸡年,大吉吧"

```

  我们新建一个bean类,如下：

```java

@Configuration

    @ConfigurationProperties(prefix = "com.md")

    @PropertySource("classpath:test.properties")

    public class ConfigTestBean {

        private String name;

        private String want;

        // 省略getter和setter

    }

```

## @PropertySource&@ImportResource&@Bean

@**PropertySource**：加载指定的配置文件；

将配置文件中配置的每一个属性的值，映射到这个组件中
 * @ConfigurationProperties：告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定；

 * prefix = "person"：配置文件中哪个下面的所有属性进行一一映射

 * 只有这个组件是容器中的组件，才能容器提供的@ConfigurationProperties功能；

 * @ConfigurationProperties(prefix = "person")默认从全局配置文件中获取值；

 * SpringBoot推荐给容器中添加组件的方式；推荐使用全注解的方式

   1、配置类**@Configuration**------>Spring配置文件

   2、使用**@Bean**给容器中添加组件

   ```java
   /**
    * @Configuration：指明当前类是一个配置类；就是来替代之前的Spring配置文件
    *
    * 在配置文件中用<bean><bean/>标签添加组件
    *
    */
   @Configuration
   public class MyAppConfig {
   
       //将方法的返回值添加到容器中；容器中这个组件默认的id就是方法名
       @Bean
       public HelloService helloService02(){
           System.out.println("配置类@Bean给容器中添加组件了...");
           return new HelloService();
       }
   }
   ```

   

   ## 激活指定profile

   ​	1、在配置文件中指定  spring.profiles.active=dev

   ​	2、命令行：

   ​		java -jar spring-boot-02-config-0.0.1-SNAPSHOT.jar --spring.profiles.active=dev；

   ​		可以直接在测试的时候，配置传入命令行参数

   ​	3、虚拟机参数；

   ​		-Dspring.profiles.active=dev

   

## 配置文件的优先级

application.properties和application.yml文件可以放在以下四个位置：

- 外置，在相对于应用程序运行目录的/congfig子目录里。
- 外置，在应用程序运行的目录里
- 内置，在config包内
- 内置，在Classpath根目录

优先级由高到底，高优先级的配置会覆盖低优先级的配置；

SpringBoot会从这四个位置全部加载主配置文件；**互补配置**；



==我们还可以通过spring.config.location来改变默认的配置文件位置==

**项目打包好以后，我们可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；指定配置文件和默认加载的这些配置文件共同起作用形成互补配置；**

java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --spring.config.location=G:/application.properties



# Springboot其他整合

## log配置

假如maven依赖中添加了spring-boot-starter-logging：

<dependency>

    <groupId>org.springframework.boot</groupId>
    
    <artifactId>spring-boot-starter-logging</artifactId>

</dependency>

配置输出的地址和输出级别

```properties

logging.path=/user/local/log

logging.level.com.favorites=DEBUG

logging.level.org.springframework.web=INFO

logging.level.org.hibernate=ERROR

```

path 为本机的 log 地址，logging.level 后面可以根据包路径配置不同资源的 log 级别



## 整合Swagger2

接口文档维护

```xml

<dependency>

    <groupId>io.springfox</groupId>

    <artifactId>springfox-swagger2</artifactId>

    <version>2.9.2</version>

</dependency>

<dependency>

    <groupId>io.springfox</groupId>

    <artifactId>springfox-swagger-ui</artifactId>

    <version>2.9.2</version>

</dependency>

```

```java

@Configuration

@EnableSwagger2

public class SwaggerConfig {

    @Bean

    public Docket createRestApi() {

        return new Docket(DocumentationType.SWAGGER_2)

                .pathMapping("/")

                .select()

                .apis(RequestHandlerSelectors.basePackage("com.nvn.controller"))

                .paths(PathSelectors.any())

                .build().apiInfo(new ApiInfoBuilder()

                        .title("SpringBoot整合Swagger")

                        .description("SpringBoot整合Swagger，详细信息......")

                        .version("9.0")

                        .contact(new Contact("啊啊啊啊","blog.csdn.net","aaa@gmail.com"))

                        .license("The Apache License")

                        .licenseUrl("http://www.baidu.com")

                        .build());

    }

}

```

http://localhost:8080/swagger-ui.html

controller中的注解

- @Api注解可以用来标记当前Controller的功能。

- @ApiOperation注解用来标记一个方法的作用。

- @ApiImplicitParam注解用来描述一个参数，可以配置参数的中文含义，也可以给参数设置默认值，这样在接口测试的时候可以避免手动输入。

- 如果有多个参数，则需要使用多个@ApiImplicitParam注解来描述，多个@ApiImplicitParam注解需要放在一个@ApiImplicitParams注解中。

- 需要注意的是，@ApiImplicitParam注解中虽然可以指定参数是必填的，但是却不能代替@RequestParam(required = true)，前者的必填只是在Swagger2框架内必填，抛弃了Swagger2，这个限制就没用了，所以假如开发者需要指定一个参数必填，@RequestParam(required = true)注解还是不能省略。

- 如果参数是一个对象（例如上文的更新接口），对于参数的描述也可以放在实体类中。

@ApiImplicitParam：

作用在方法上，表示单独的请求参数

参数：

1. name ：参数名。

2. value ： 参数的具体意义，作用。

3. required ： 参数是否必填。

4. dataType ：参数的数据类型。

5. paramType ：查询参数类型，这里有几种形式：

类型 作用

path 以地址的形式提交数据

query 直接跟参数完成自动映射赋值

body 以流的形式提交 仅支持POST

header 参数在request headers 里边提交

form 以form表单的形式提交 仅支持POST

## 整合security

```xml

<dependency>

            <groupId>org.springframework.boot</groupId>

            <artifactId>spring-boot-starter-security</artifactId>

        </dependency>

```

```xml

<dependency>

            <groupId>io.jsonwebtoken</groupId>

            <artifactId>jjwt</artifactId>

            <version>0.9.1</version>

        </dependency>

```

## JWT原理

JSON Web Token（JWT）是目前最流行的跨域身份验证解决方案。

其原理是将用户信息的JSON字符串加密生成唯一的token返回给前端，后端通过解析token来验证是否已登录。

##  Spring Security 和 Shiro 各自的优缺点

1. Spring Security 是一个重量级的安全管理框架；Shiro 则是一个轻量级的安全管理框架

2. Spring Security 概念复杂，配置繁琐；Shiro 概念简单、配置简单

3. Spring Security 功能强大；Shiro 功能简单

## lombok

```xml

<dependency>

            <groupId>org.projectlombok</groupId>

            <artifactId>lombok</artifactId>

            <optional>true</optional>

        </dependency>

```

## StringUtils

```xml

<dependency>

            <groupId>org.apache.commons</groupId>

            <artifactId>commons-lang3</artifactId>

            <version>3.5</version>

        </dependency>

```

## 微服务中如何实现 session 共享 ?

在微服务中，一个完整的项目被拆分成多个不相同的独立的服务，各个服务独立部署在不同的服务器上，各自的 session 被从物理空间上隔离开了，但是经常，我们需要在不同微服务之间共享 session ，常见的方案就是 Spring Session + Redis 来实现 session 共享。将所有微服务的 session 统一保存在 Redis 上，当各个微服务对 session 有相关的读写操作时，都去操作 Redis 上的 session 。这样就实现了 session 共享，Spring Session 基于 Spring 中的代理过滤器实现，使得 session 的同步操作对开发人员而言是透明的，非常简便。

## Spring Boot 中如何实现定时任务 ?

定时任务也是一个常见的需求，Spring Boot 中对于定时任务的支持主要还是来自 Spring 框架。

在 Spring Boot 中使用定时任务主要有两种不同的方式，一个就是使用 Spring 中的 @Scheduled 注解，另一个则是使用第三方框架 Quartz。

使用 Spring 中的 @Scheduled 的方式主要通过 @Scheduled 注解来实现。

使用 Quartz ，则按照 Quartz 的方式，定义 Job 和 Trigger 即可。