# 什么是Maven

Apache Maven是一个项目管理工具，它基于项目对象模型(POM)的概念，通过一小段描述信息来管理项目的构建、报告和文档。

## Maven的作用

Maven是跨平台的项目管理工具。主要服务于基于Java平台的项目构建，依赖管理和项目信息管理。

项目构建：

项目构建包括清理，···，编译，测试，报告，打包，部署···等步骤

理想的项目构建

高度自动化，跨平台，可重用的组件，标准化

传统方式管理jar包依赖的问题：

1. jar包冲突

2. jar包依赖

3. jar包体积过大

4. jar包在不同阶段无法个性化配置

使用maven方式管理jar包依赖的好处：

1. 解决jar包冲突

2. 解决jar包依赖问题

3. jar包不用再每个项目保存，只需要放在仓库即可

4. maven可以指定jar包的依赖范围

# Maven的几个核心概念

## POM

POM(Project Object Model)项目对象模型，一个项目所有的配置都放在POM文件中：定义项目的类型、名字、管理依赖关系，定制插件的行为等等。

Maven通过pom.xml文件来管理依赖和管理项目的构建生命周期，而项目构建的生命周期是依靠一个个的插件完成的。

## Maven仓库

Maven管理资源的位置。仓库里面包含依赖（jar包）和插件（plug-in）。Maven仓库分为本地仓库和远程仓库，而远程仓库又包括私服和中央仓库。

Maven仓库

----本地仓库

----远程仓库

--------私服

私服是一种特殊的远程仓库，搭建在局域网内的仓库，私服代理广域网的仓库，提供给局域网内的用户使用，可用减少局域网内的用户与外界仓库的传输，每一个jar包只需要拉取一次就可以提供给局域网内所有的用户使用，并且也更加稳定。

--------中央仓库

Maven官方提供的远程仓库，里面拥有最全的jar包资源，Maven首先从本地仓库中寻找项目所需的jar包，若本地仓库没有，再到Maven的中央仓库下载所需jar包。地址是：http://repo1.maven.org/maven2/。

## 坐标

在Maven中，坐标是jar包的唯一标识，Maven通过坐标在仓库中找到项目所需的jar包。

- groupId：公司或组织域名倒序

- artifactId：模块名

- version：版本号

- packaging：项目的打包方式(pom/jar/war，默认jar)

groupId、artifactId、versioin简称GAV(Maven坐标)，是用来唯一标识jar包的。

最新最全的Maven依赖项版本查询网站：

> http://mvnrepository.com/

Maven工程的坐标与仓库中路径的关系：

Maven坐标和仓库对应的映射关系：

{groupId}{artifactId}{version}{artifactId}-{version}.jar

对应本地仓库目录：

org\springframework\spring-core\4.3.4.RELEASE\spring-core-4.3.4.RELEASE.jar

## 依赖范围

依赖范围就是控制依赖在不同阶段的作用。**不同的依赖会使用不同的classpath**，在Maven中依赖的域有这几个：import、provided、runtime、compile、system、test。默认取值为compile。

## 可选依赖

1. < optional > true/false 是否可选，也可以理解为是否向下传递

2. 在依赖中添加optional选项决定此依赖是否向下传递，如果是true则不传递，如果是false就传递，默认为false

## 排除依赖

1. 若 A 依赖 B，但是对于依赖 B 本身的部分依赖，依赖 A 并不需要

2. 此时，就可以在依赖 A 中做出声明，依赖 B 的同时，排除依赖 B 中不需要的部分

3. 注意exclusions是写在dependency中

# Maven常用命令

| 命令 | 说明 |

| ---- | ---- |

|mvn clean	|	清除原来的编译结果|

|mvn compile	|	编译|

|mvn test	|	运行测试代码，mvn test -Dtest=类名 //单独运行测试类|

|mvn package	|	打包项目，mvn package -Dmaven.test.skip=true //打包时不执行测试|

|mvn install	|	将项目打包并安装到本地仓库|

|mvn deploy	|	发布到本地仓库或者服务器|

# Maven常用插件

Maven本质上是一个插件框架，它的核心并不执行任何具体的构建任务，所有这些任务都交给插件来完成。下面说几个常用的插件：

maven-compiler-plugin(编译插件)

用来编译Java代码，在对Java代码进行编译的时候，可以指定使用哪个JDK版本来进行编译，配置如下所示：

<plugin>

    <groupId>org.apache.maven.plugins</groupId>

    <artifactId>maven-compiler-plugin</artifactId>

    <version>3.8.1</version>

    <configuration>

    <!-- 源代码使用jdk1.8支持的特性 -->

        <source>1.8</source>

        <!-- 使用jdk1.8编译目标代码 -->

        <target>1.8</target>

          <!-- 传递参数 -->

              <compilerArgs>

                    <arg>-parameters</arg>

                    <arg>-Xlint:unchecked</arg>

                    <arg>-Xlint:deprecation </arg>

              </compilerArgs>

    </configuration>

</plugin>

maven-clean-plugin(清除插件)

主要作用就是清理构建目录下的全部内容，有些项目，构建时需要清理构建目录以外的文件，比如指定的库文件，这时候就需要配置来实现了，配置如下所示：

<plugin>

    <groupId>org.apache.maven.plugins</groupId>

    <artifactId>maven-clean-plugin</artifactId>

    <version>3.1.0</version>

    <configuration>

        <!--<skip>true</skip>-->

        <!--<failOnError>false</failOnError>-->

        <!--当配置true时,只清理filesets里的文件,构建目录中得文件不被清理.默认是flase.-->

        <excludeDefaultDirectories>false</excludeDefaultDirectories>

        <filesets>

            <fileset>

                <!--要清理的目录位置-->

                <directory>${basedir}/logs</directory>

                <!--是否跟随符号链接 (symbolic links)-->

                <followSymlinks>false</followSymlinks>

            </fileset>

        </filesets>

    </configuration>

</plugin>