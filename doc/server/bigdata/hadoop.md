# 大数据

指无法在一定时间范围内用常规工具进行捕捉，管理和处理的数据。

特点：

- 大量：存储的数据量很大
- 多样：广范的数据来源，结构化不明显
- 高速：大数据产生非常迅速，主要通过互联网传输。
- 价值，核心特征，通过从大量不想管的各种类型中，找出有价值的数据

应用

- 保险：海量数据挖掘
- 金融：防范欺诈风险
- 房产：推荐和是的楼和地，精准营销
- 广告：给用户推荐广告
- 网站用户行为分析
- 零售行业购物模式分析

发展前景

- 大数据自身能创造出更多的价值
- 推动科技领域的发展
- 大数据产业链主键形成
- 产业互联网推动大数据

# Hadoop

分布式系统基础架构，充分利用集群进行高速运算和存储

实现了一个分布式文件系统（Hadoop Distributed File System）HDFS，

- 具有高容错型的特点：其中最为重要的概念就是所有的数据默认保存3份
- 低廉硬件上：大数据集群设计，可以使用廉价的PC实现

提供高吞吐量来访问应用程序的数据

Hadoop最核心的设计就是HDFS和MapReduce，HDFS提供数据存储，

MapReduce为海量数据提供了分布式计算

Hadooop YARN：一个资源管理平台，资源调度

其他生态组件：HBase、Hive、Zookeeper

## 优缺点

优点

- 高可靠性：维护多个工作副本
- 高扩展性
- 高效性
- 高容错性

缺点

- 不适用于低延迟的数据访问
- 不能高效存储大量小文件
- 不支持多用户写入并任意修改文件

## Hadoop架构组成

HDFS架构：

- NameNode：存储文件的元数据，如文件名称，文件目录结构，文件属性（生成时间，副本数，文件权限）以及每个文件的块列表和块所在的DataNote等，存储在内存中，操作日志edits，磁盘存储fsimage
- DataNode：在本地文件系统存储文件块数据，以及数据块的校验
- Secondary NameNode：同步元数据和日志。

MapReduce架构原理：

- map阶段并行处理输入数据
- reduce阶段对map结果进行汇总

YARN架构原理：

-  ResourceManager：负责整个集群的资源管理和调度。ResourceManager 将各个资源部分（计算、内存、带宽等）精心安排给基础NodeManager（YARN 的每节点代理）。ResourceManager还与 ApplicationMaster 一起分配资源，与NodeManager 一起启动和监视它们的基础应用程序
- ApplicationMaster：负责应用程序相关的事务，比如任务调度、任务监控和容错等。ApplicationMaster管理一个在YARN内运行的应用程序的每个实例。ApplicationMaster 负责协调来自 ResourceManager 的资源，并通过 NodeManager 监视容器的执行和资源使用（CPU、内存等的资源分配）。
- NodeManager：NodeManager管理一个YARN集群中的每个节点。NodeManager提供针对集群中每个节点的服务，从监督对一个容器的终生管理到监视资源和跟踪节点健康。
- Container 对任务运行环境进行抽象，封装CPU、内存等多维度的资源以及环境变量、启动命令等任务运行相关的信息。比如内存、CPU、磁盘、网络等，当AM向RM申请资源时，RM为AM返回的资源便是用Container表示的

## 大数据技术生态体系

- Sqoop：Sqoop是一款开源的工具，主要用于在Hadoop、Hive与传统的数据库(MySql)间进行数据的传递，可以将一个关系型数据库（例如 ：MySQL，Oracle 等）中的数据导进到Hadoop的HDFS中，也可以将HDFS的数据导进到关系型数据库中。

- Flume：Flume是Cloudera提供的一个高可用的，高可靠的，分布式的海量日志采集、聚合和传输的系统，Flume支持在日志系统中定制各类数据发送方，用于收集数据；同时，Flume提供对数据进行简单处理，并写到各种数据接受方（可定制）的能力。

- Kafka：Kafka是一种高吞吐量的分布式发布订阅消息系统，有如下特性：

  （1）通过O(1)的磁盘数据结构提供消息的持久化，这种结构对于即使数以TB的消息存储也能够保持长时间的稳定性能。

  （2）高吞吐量：即使是非常普通的硬件Kafka也可以支持每秒数百万的消息。

  （3）支持通过Kafka服务器和消费机集群来分区消息。

  （4）支持Hadoop并行数据加载。

- Storm：Storm用于“连续计算”，对数据流做连续查询，在计算时就将结果以流的形式输出给用户。

- Spark：Spark是当前最流行的开源大数据内存计算框架。可以基于Hadoop上存储的大数据进行计算。

- Oozie：Oozie是一个管理Hdoop作业（job）的工作流程调度管理系统。

- Hbase：HBase是一个分布式的、面向列的开源数据库。HBase不同于一般的关系数据库，它是一个适合于非结构化数据存储的数据库。

- Hive：Hive是基于Hadoop的一个数据仓库工具，可以将结构化的数据文件映射为一张数据库表，并提供简单的SQL查询功能，可以将SQL语句转换为MapReduce任务进行运行。 其优点是学习成本低，可以通过类SQL语句快速实现简单的MapReduce统计，不必开发专门的MapReduce应用，十分适合数据仓库的统计分析。

- Mahout：Apache Mahout是个可扩展的机器学习和数据挖掘库。

- ZooKeeper：Zookeeper是Google的Chubby一个开源的实现。它是一个针对大型分布式系统的可靠协调系统，提供的功能包括：配置维护、名字服务、 分布式同步、组服务等。ZooKeeper的目标就是封装好复杂易出错的关键服务，将简单易用的接口和性能高效、功能稳定的系统提供给用户。

  

  # Hadoop安装

  hadoop-env.sh 配置文件

  运行官方案例：bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar

  0. 关闭防火墙

  **执行：**service iptables stop 这个指令关闭完防火墙后，如果重启，防火墙会重新建立，所以，如果想重启后防火墙还关闭，

  **需额外执行：**chkconfig iptables off

  1. 配置主机名

  **执行：vim /etc/sysconfig/network**

2. 配置hosts文件

执行：vim /etc/hosts

5. 安装和配置jdk

7. 配置hadoop-env.sh


这个文件里写的是hadoop的环境变量,主要修改hadoop的java_home路径

8. 修改core-site.xml

在etc/hadoop目录下

**执行：vim core-site.xml**

```xml
<configuration>
    <!--用来指定hdfs的老大，namenode的地址-->
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://hadoop01:9000</value>
    </property>
    <!--用来指定hadoop运行时产生文件的存放目录-->
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/home/software/hadoop-2.7.1/tmp</value>
    </property>
</configuration>
```

9.修改 hdfs-site .xml

```xml
<configuration>
    <!--指定hdfs保存数据副本的数量，包括自己，默认值是3-->
    <!--如果是伪分布模式，此值是1-->
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <!--设置hdfs的操作权限，false表示任何用户都可以在hdfs上操作文件-->
    <property>
        <name>dfs.permissions</name>
        <value>false</value>
    </property>
</configuration>
```

10.修改 mapred-site.xml

这个文件初始时是没有的，有的是模板文件，mapred-site.xml.template

所以需要拷贝一份，并重命名为mapred-site.xml

**执行：cp mapred-site.xml.template mapred-site.xml**

```xml
<configuration>
    <property>
        <!--指定mapreduce运行在yarn上-->
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```

11.修改yarn-site.xml

```xml
<configuration>
    <!-- Site specific YARN configuration properties -->
    <property>
        <!--指定yarn的老大 resoucemanager的地址-->
        <name>yarn.resourcemanager.hostname</name>
        <value>hadoop01</value>
    </property>
    <property>
        <!--NodeManager获取数据的方式-->
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
```

14.格式化namenode

15. hadoop的启动

切换到sbin目录下, 执行start-dfs.sh, 启动hadoop相关服务; 执行start-yarn.sh, 启动yarn相关服务。

yarn-daemon.sh启动resourcemanager，启动nodemanager