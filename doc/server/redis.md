# Redis概述

## redis的应用

Redis是一个key-value存储系统，大部分情况下是因为其高性能的特性，被当做缓存使用。

- 读写性能优异
- 持久化
- 数据类型丰富
- 单线程
- 数据自动过期
- 发布订阅
- 分布式

- Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
- Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
- Redis支持数据的备份，即master-slave模式的数据备份。

## Redis 优势

- 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
- 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
- 原子 – Redis的所有操作都是原子性的，意思就是要么成功执行要么失败完全不执行。单个操作是原子性的。多个操作也支持事务，即原子性，通过MULTI和EXEC指令包起来。
- 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。

# Redis 数据类型

Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。

# Redis 字符串(String)

Redis 字符串数据类型的相关命令用于管理 redis 字符串值，基本语法如下：

### 语法

```
redis 127.0.0.1:6379> COMMAND KEY_NAME
```

### 实例

```shell
redis 127.0.0.1:6379> SET key redis
OK
redis 127.0.0.1:6379> GET key
"redis"
```

# Redis 哈希(Hash)

Redis hash 是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于存储对象。

Redis 中每个 hash 可以存储 232 - 1 键值对（40多亿）。

### 实例

```shell
127.0.0.1:6379>  HMSET key name "redis tutorial" description "redis basic commands for caching" likes 20 visitors 23000
OK
127.0.0.1:6379>  HGETALL key
1) "name"
2) "redis tutorial"
3) "description"
4) "redis basic commands for caching"
5) "likes"
6) "20"
7) "visitors"
8) "23000"
```

# Redis 列表(List)

Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）

一个列表最多可以包含 232 - 1 个元素 (4294967295, 每个列表超过40亿个元素)。

### 实例

```shell
redis 127.0.0.1:6379> LPUSH key redis
(integer) 1
redis 127.0.0.1:6379> LPUSH key mongodb
(integer) 2
redis 127.0.0.1:6379> LPUSH key mysql
(integer) 3
redis 127.0.0.1:6379> LRANGE key 0 10

1) "mysql"
2) "mongodb"
3) "redis"
```

# Redis 集合(Set)

Redis 的 Set 是 String 类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据。

Redis 中集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。

集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)。

### 实例

```shell
redis 127.0.0.1:6379> SADD key redis
(integer) 1
redis 127.0.0.1:6379> SADD key mongodb
(integer) 1
redis 127.0.0.1:6379> SADD key mysql
(integer) 1
redis 127.0.0.1:6379> SADD key mysql
(integer) 0
redis 127.0.0.1:6379> SMEMBERS key

1) "mysql"
2) "mongodb"
3) "redis"
```

# Redis 有序集合(sorted set)

Redis 有序集合和集合一样也是string类型元素的集合,且不允许重复的成员。

不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

有序集合的成员是唯一的,但分数(score)却可以重复。

集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。 集合中最大的成员数为 232 - 1 (4294967295, 每个集合可存储40多亿个成员)。

### 实例

```shell
redis 127.0.0.1:6379> ZADD key 1 redis
(integer) 1
redis 127.0.0.1:6379> ZADD key 2 mongodb
(integer) 1
redis 127.0.0.1:6379> ZADD key 3 mysql
(integer) 1
redis 127.0.0.1:6379> ZADD key 3 mysql
(integer) 0
redis 127.0.0.1:6379> ZADD key 4 mysql
(integer) 0
redis 127.0.0.1:6379> ZRANGE key 0 10 WITHSCORES

1) "redis"
2) "1"
3) "mongodb"
4) "2"
5) "mysql"
6) "4"
```
