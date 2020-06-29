# java虚拟机

屏蔽底层操作系统平台的不同并且减少基于原生语言开发的复杂性，使java能够跨各种平台 

在机器和编译程序之间加入了一层抽象的虚拟机器。这台虚拟的机器在任何平台上都提供给编译程序一个的共同的接口。编译程序只需要面向虚拟机，生成虚拟机能够理解的代码，然后由解释器来将虚拟机代码转换为特定系统的机器码执行。在Java中，这种供虚拟机理解的代码叫做字节码（即扩展为.class的文件），它不面向任何特定的处理器，只面向虚拟机。每一种平台的解释器是不同的，但是实现的虚拟机是相同的。Java源程序经过编译器编译后变成字节码，字节码由虚拟机解释执行，虚拟机将每一条要执行的字节码送给解释器，解释器将其翻译成特定机器上的机器码，然后在特定的机器上运行，这就是上面提到的Java的特点的编译与解释并存的解释。


**内存中的栈(stack)、堆(heap)和方法区(method area)** 

定义一个基本数据类型的变量，一个对象的引用，还有就是函数调用的现场保存都使用JVM中的栈空间；

通过new关键字和构造器创建的对象则放在堆空间，堆是垃圾收集器管理的主要区域。

由于现在的垃圾收集器都采用分代收集算法，所以堆空间还可以细分为新生代和老生代，
再具体一点可以分为Eden、Survivor（又可分为From Survivor和To Survivor）、Tenured；

方法区和堆都是各个线程共享的内存区域，用于存储已经被JVM加载的类信息、常量、静态变量、JIT编译器编译后的代码等数据；
程序中的字面量（literal）如直接书写的100、"hello"和常量都是放在常量池中，常量池是方法区的一部分，。

栈空间操作起来最快但是栈很小，通常大量的对象都是放在堆空间，栈和堆的大小都可以通过JVM的启动参数来进行调整，
栈空间用光了会引发StackOverflowError，而堆和常量池空间不足则会引发OutOfMemoryError。

垃圾回收机制有很多种，包括：
分代复制垃圾回收、标记垃圾回收、增量垃圾回收等方式。标准的Java进程既有栈又有堆。
栈保存了原始型局部变量，堆保存了要创建的对象。
Java平台对堆内存回收和再利用的基本算法被称为标记和清除，但是Java对其进行了改进，采用“分代式垃圾收集”。
这种方法会跟Java对象的生命周期将堆内存划分为不同的区域，在垃圾收集过程中，可能会将对象移动到不同区域：
- 伊甸园（Eden）：这是对象最初诞生的区域，并且对大多数对象来说，这里是它们唯一存在过的区域。
- 幸存者乐园（Survivor）：从伊甸园幸存下来的对象会被挪到这里。
- 终身颐养园（Tenured）：这是足够老的幸存对象的归宿。年轻代收集（Minor-GC）过程是不会触及这个地方的。当年轻代收集不能把对象放进终身颐养园时，就会触发一次完全收集（Major-GC），这里可能还会牵扯到压缩，以便为大对象腾出足够的空间。

 可以检查死锁的工具有？
A： jconsole、jstack



检查内存泄露的工具有？
A： jmap生成dump转储文件，jhat可视化查看。

某进程CPU使用率一直占满，用什么工具可以排查？
A：
top -Hp pid找到最占CPU的线程
然后jstack来查找那个线程此时所处的堆栈，确定问题发生位置

### jstack

全称： JVM Stack Trance
作用： 查看某个java进程的堆栈情况， 可用于确认死锁、IO等待、死循环等问题。



###  jstat

全称：
作用：
查看进程中内存使用情况，但只能给出一些简单统计数据

- 统计加载了多少类以及占用空间 jstat -class pid
- 统计编译了多少文件 jstat -compiler 10

### [§](http://3ms.huawei.com/km/blogs/details/7923091#jmap) jmap

全称： JVM Memory Map
作用：生成进程的内存堆快照
当需要看一下进程里是什么东西占用了过多内存时， 可以用jmap打印一下堆快照。
命令用法：

- 打印堆快照： jmap -dump:file=./dumpfile.dump 进程pid
- 查看特定类所占用的情况： jmap -histo:live 进程pid | grep 类名

### [§](http://3ms.huawei.com/km/blogs/details/7923091#jhat) jhat

全称： JVM Heap Analysis Tool
和jmap配合， 可以解析jmap生成的堆快照， 支持生成1个web进程供我们分析和查看。
命令用法：

- jhat -J-Xmx515M dumpfile.dump
  此时就会启动1个webServer，然后我们去访问就行了

### [§](http://3ms.huawei.com/km/blogs/details/7923091#jdb) jdb

全称：Java Debugger
作用：用来对core文件和正在运行的Java进程进行实时地调试，类似于c++里的gdb
常见用法：

- 启动进程并调试: jdb -classpath . Test
- 至二级调试某进程: jdb -attach 8000 -sourcepath /Users/wefit/Development/study/java/jtest/src/

### [§](http://3ms.huawei.com/km/blogs/details/7923091#jcmd) jcmd

作用：多功能的工具，可以用它来导出堆、查看Java进程、导出线程信息、执行GC、还可以进行采样分析，可以理解为1个性能调优时用的工具。
常见命令：

- 查看 当前机器上所有的 jvm 进程信息: jcmd -l
- 查看指定进程的性能统计信息: jcmd pid PerfCounter.print
- 列出当前运行的 java 进程可以执行的操作: jcmd PID help
- 查看线程堆栈信息: jcmd PID Thread.print
- 查看堆内存信息： jcmd PID GC.heap_dump FILE_NAME

### [§](http://3ms.huawei.com/km/blogs/details/7923091#jps) jps

简单记法： JVM process status
全名：Java Virtual Machine Process Status Tool
作用： 显示 ***当前系统用户*** 的 ***所有*** Java进程情况及其进程号
常用命令：

- 查看进程jvm参数： jps -v
- 输出程序main class的完整package名或程序的jar文件完整路径名： jps -l
- 输出传递给main方法的参数: jps -m

## [§](http://3ms.huawei.com/km/blogs/details/7923091#jinfo) jinfo

jvm infomation
作用：和jps功能类似， 但是支持根据指定pis查看指定进程

- 可以查看JVM参数、系统参数、调整jvm参数
- 但不支持查看java程序的内存使用情况

## [§](http://3ms.huawei.com/km/blogs/details/7923091#javap) javap

把java字节码文件反汇编为Java源码文件。

## 垃圾收集

Java虚拟机规范将JVM虚拟机所管理的内存分为几部分？如果是多选题，估计会给一些不在里面的，例如直接内存。
A：程序计数器、java虚拟机栈、本地方法栈、方法区、堆。

java使用根搜索算法来确定对象是否存货，哪些对象可以作为GC Roots？
A：

- 虚拟机栈（栈帧中的本地变量表）中的引用的对象
- 方法区中的类静态属性引用的对象
- 方法区中的常量引用的对象
- 本地方法栈中JNI（Native方法）的引用对象

 标记清除、标记整理、复制算法哪个块？
A： 复制算法较快。

Q： SerialOld用的是什么算法？
A： 标记整理算法，属于处理老年代算法。
各收集器的变化图如下，主要关注一下变化和区别，感觉serial和par可能会考：

fullGC 会发生在老年代区还是新生代区？
A: 会发生在老年代区。 相反，minorGC一般发送在新生代区。
新生代、老生代以及minorGC、fullGC的发生流程如下：

Q： 方法区里的class对象（即类对象）什么时候会被回收？
A： 所有实例都被回收、对应classLoader也被回收、class对象不会再被引用或者反射

## [§](http://3ms.huawei.com/km/blogs/details/7983891#finalized与gc) finalized与GC

Q： 什么时候会调用对象的finalized方法
A： JVM启动垃圾回收，且该对象要被回收时。

finalized应该更多是规范吧，很多规范里都要求我们不要自己实现finalized了，毕竟不确定性太大。

（-clien）启动较快
（-server）性能和内存管理效率高
桌面应用一般使用（-clien）， 服务器一般使用（-server）

用于配置java初始堆内存的是（）
A：
-Xms。

用于配置java堆的最大值的是（）
A:
-Xmx。

如果不设置，-Xms和-Xmx的大小分别默认是多少？
A：
不设置的话，二者相等，默认是 物理内存/64（小于1G）

Q： 如何根据上面的参数计算老年代内存大小？
A：
Xmx的值（堆最大值）- Xmn的值（新生代内存）

用于配置线程栈内存的是（）？ 替代的还有哪个参数？
A：
-Xss。 另一个是-XX:ThreadStackSize
