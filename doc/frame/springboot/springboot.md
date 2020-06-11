# Spring Boot
简化新 Spring 应用的初始搭建以及开发过程。使开发人员不再需要定义样板化的配置。Spring Boot 整合了所有的框架。

方便、快速搭建项目，使我们不用关心框架之间的兼容性，适用版本等各种问题，我们想使用任何东西，仅仅添加一个配置就可以，所以使用 Spring Boot 非常适合构建微服务

自动配置：针对很多Spring应用程序常见的应用功能，Spring Boot能自动提供相关配置

起步依赖：告诉Spring Boot需要什么功能，它就能引入需要的库。

命令行界面：这是Spring Boot的可选特性，借此你只需写代码就能完成完整的应用程序，无需传统项目构建。

Actuator：让你能够深入运行中的Spring Boot应用程序，一探究竟。

## 入门
简单、快速、方便

引入 web 模块

1、pom.xml中添加支持web的模块：
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

spring-boot-starter ：核心模块，包括自动配置支持、日志和 YAML，如果引入了 spring-boot-starter-web web 模块可以去掉此配置，因为 spring-boot-starter-web 自动依赖了 spring-boot-starter。
spring-boot-starter-test ：测试模块，包括 JUnit、Hamcrest、Mockito。

开发环境的调试

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
  
  

## 配置文件的优先级
application.properties和application.yml文件可以放在以下四个位置：

- 外置，在相对于应用程序运行目录的/congfig子目录里。
- 外置，在应用程序运行的目录里
- 内置，在config包内
- 内置，在Classpath根目录

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
JWT原理

JSON Web Token（JWT）是目前最流行的跨域身份验证解决方案。

其原理是将用户信息的JSON字符串加密生成唯一的token返回给前端，后端通过解析token来验证是否已登录。

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
