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

SqlSessionFactoryBuilder.build

XMLConfigBuilder.parseConfiguration

XMLConfigBuilder.environmentsElement

Configuration.setEnvironment

二、获取数据库执行语句

mybatis加载mapper的方式有几种？

XMLConfigBuilder

Mapper

三、mybatis是如何操作的

mybatis的执行器有几种？Simple、Reuse、 Batch

获取数据库源

获取执行语句

通过jdbc执行。

反射到对象中返回结果集

 