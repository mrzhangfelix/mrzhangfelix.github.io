# Spark

Spark是加州大学伯克利分校AMP实验室（Algorithms, Machines, and People Lab）开发的通用内存并行计算框架

Spark使用Scala语言进行实现，它是一种面向对象、函数式编程语言，能够像操作本地集合对象一样轻松地操作分布式数据集，具有以下特点。


1. 运行速度快：Spark拥有DAG执行引擎，支持在内存中对数据进行迭代计算。官方提供的数据表明，如果数据由磁盘读取，速度是Hadoop MapReduce的10倍以上，如果数据从内存中读取，速度可以高达100多倍。
2. 易用性好：Spark不仅支持Scala编写应用程序，而且支持Java和Python等语言进行编写，特别是Scala是一种高效、可拓展的语言，能够用简洁的代码处理较为复杂的处理工作。
3. 通用性强：Spark生态圈即BDAS（伯克利数据分析栈）包含了Spark Core、Spark SQL、Spark Streaming、MLLib和GraphX等组件，这些组件分别处理Spark Core提供内存计算框架、SparkStreaming的实时处理应用、Spark SQL的即席查询、MLlib或MLbase的机器学习和GraphX的图处理。
4. 随处运行：Spark具有很强的适应性，能够读取HDFS、Cassandra、HBase、S3和Techyon为持久层读写原生数据，能够以Mesos、YARN和自身携带的Standalone作为资源管理器调度job，来完成Spark应用程序的计算

## spark与hadoop:

------

- Hadoop有两个核心模块，分布式存储模块HDFS和分布式计算模块Mapreduce
- spark本身并没有提供分布式文件系统，因此spark的分析大多依赖于Hadoop的分布式文件系统HDFS
- Hadoop的Mapreduce与spark都可以进行数据计算，而相比于Mapreduce，spark的速度更快并且提供的功能更加丰富

## 和mapreduce对比

mapreduce 完成数据的处理需要写多个mr程序并且需要重复读取磁盘中的文件 后台计算

spark 对数据的处理写入内存中，使用多个连续的job在内存中处理，性能更好 实时处理

## Spark框架体系

- Cluster Manager：在standalone模式中即为Master主节点，控制整个集群，监控worker。在YARN模式中为资源管理器
- Worker节点：从节点，负责控制计算节点，启动Executor或者Driver。
- Driver： 运行Application 的main()函数
- Executor：执行器，是为某个Application运行在worker node上的一个进程

## 运行流程及特点：

1. 构建Spark Application的运行环境，启动SparkContext
2. SparkContext向资源管理器（可以是Standalone，Mesos，Yarn）申请运行Executor资源，并启动StandaloneExecutorbackend，
3. Executor向SparkContext申请Task
4. SparkContext将应用程序分发给Executor
5. SparkContext构建成DAG图，将DAG图分解成Stage、将Taskset发送给Task Scheduler，最后由Task Scheduler将Task发送给Executor运行
6. Task在Executor上运行，运行完释放所有资源

Spark运行特点：

1. 每个Application获取专属的executor进程，该进程在Application期间一直驻留，并以多线程方式运行Task。这种Application隔离机制是有优势的，无论是从调度角度看（每个Driver调度他自己的任务），还是从运行角度看（来自不同Application的Task运行在不同JVM中），当然这样意味着Spark Application不能跨应用程序共享数据，除非将数据写入外部存储系统
2. Spark与资源管理器无关，只要能够获取executor进程，并能保持相互通信就可以了
3. 提交SparkContext的Client应该靠近Worker节点（运行Executor的节点），最好是在同一个Rack里，因为Spark Application运行过程中SparkContext和Executor之间有大量的信息交换
4. Task采用了数据本地性和推测执行的优化机制

# Spark常用术语

|   术语   |  描述    |
| ---- | ---- |
|   Application   |  Spark的应用程序，包含一个Driver program和若干Executor  |
|	SparkContext	|Spark应用程序的入口，负责调度各个运算资源，协调各个Worker Node上的Executor|
|	 Driver Program	 | 	运行Application的main()函数并且创建SparkContext	|
|	Executor	|是为Application运行在Worker node上的一个进程，该进程负责运行Task，并且负责将数据存在内存或者磁盘上。每个Application都会申请各自的Executor来处理任务	|
|	Cluster Manager	|	在集群上获取资源的外部服务(例如：Standalone、Mesos、Yarn)	|
|	Worker Node	|	集群中任何可以运行Application代码的节点，运行一个或多个Executor进程	|
|	Task	|	运行在Executor上的工作单元	|
|	Job	|	SparkContext提交的具体Action操作，常和Action对应	|
|	Stage	|	每个Job会被拆分很多组task，每组任务被称为Stage，也称TaskSet	|
|	RDD	|	是Resilient distributed datasets的简称，中文为弹性分布式数据集;是Spark最核心的模块和类	|
|	DAGScheduler	|	根据Job构建基于Stage的DAG，并提交Stage给TaskScheduler	|
|	TaskScheduler	|	将Taskset提交给Worker node集群运行并返回结果	|
|	Transformations	|	是Spark API的一种类型，Transformation返回值还是一个RDD，所有的Transformation采用的都是懒策略，如果只是将Transformation提交是不会执行计算的	|
|	Action	|	是Spark API的一种类型，Action返回值不是一个RDD，而是一个scala集合；计算只有在Action被提交的时候计算才被触发。	|

# RDD

分布式弹性数据集