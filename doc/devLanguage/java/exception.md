# 异常

Java异常是Java提供的一种识别及响应错误的一致性机制。

Throwable 是 Java 语言中所有错误与异常的超类。

Throwable 包含两个子类：Error（错误）和 Exception（异常），它们通常用于指示发生了异常情况。

检查性异常：最具代表的检查性异常是用户错误或问题引起的异常，这是程序员无法预见的。例如要打开一个不存在文件时，一个异常就发生了，这些异常在编译时不能被简单地忽略。

运行时异常： 运行时异常是可能被程序员避免的异常。与检查性异常相反，运行时异常可以在编译时被忽略。

错误： 错误不是异常，而是脱离程序员控制的问题。错误在代码中通常被忽略。例如，当栈溢出时，一个错误就发生了，它们在编译也检查不到的。

常见的检查性异常

## Error（错误）

定义：Error 类及其子类。程序中无法处理的错误，表示运行应用程序中出现了严重的错误。

特点：此类错误一般表示代码运行时 JVM 出现问题。通常有 Virtual MachineError（虚拟机运行错误）、NoClassDefFoundError（类定义错误）等。比如 OutOfMemoryError：内存不足错误；StackOverflowError：栈溢出错误。此类错误发生时，JVM 将终止线程。

这些错误是不受检异常，非代码性错误。因此，当此类错误发生时，应用程序不应该去处理此类错误。按照Java惯例，我们是不应该实现任何新的Error子类的！

程序本身可以捕获并且可以处理的异常。Exception 这种异常又分为两类：运行时异常和编译时异常。

## 运行时异常和编译时异常

定义：RuntimeException 类及其子类，表示 JVM 在运行期间可能出现的异常。

编译时异常

定义: Exception 中除 RuntimeException 及其子类之外的异常。

## 受检异常与非受检异常

### 受检异常

编译器要求必须处理的异常。除 RuntimeException 及其子类外，其他的 Exception 异常都属于受检异常。

### 非受检异常

编译器不会进行检查并且不要求必须处理的异常，也就说当程序中出现此类异常时，即使我们没有try-catch捕获它，也没有使用throws抛出该异常，编译也会正常通过。该类异常包括运行时异常（RuntimeException极其子类）和错误（Error）。

## 常见的受检异常

| 异常                      | 描述                                                        |

| -------------------------- | ------------------------------------------------------------ |

| ClassNotFoundException    | 应用程序试图加载类时，找不到相应的类，抛出该异常。          |

| CloneNotSupportedException | 当调用 Object 类中的 clone 方法克隆对象，但该对象的类无法实现 Cloneable 接口时，抛出该异常。 |

| IllegalAccessException    | 拒绝访问一个类的时候，抛出该异常。                          |

| InstantiationException    | 当试图使用 Class 类中的 newInstance 方法创建一个类的实例，而指定的类对象因为是一个接口或是一个抽象类而无法实例化时，抛出该异常。 |

| InterruptedException      | 一个线程被另一个线程中断，抛出该异常。                      |

| NoSuchFieldException      | 请求的变量不存在                                            |

| NoSuchMethodException      | 请求的方法不存在                                            |

## 常见的运行时异常

| 异常                            | 描述                                                        |

| ------------------------------- | ------------------------------------------------------------ |

| ArithmeticException            | 当出现异常的运算条件时，抛出此异常。例如，一个整数"除以零"时，抛出此类的一个实例。 |

| ArrayIndexOutOfBoundsException  | 用非法索引访问数组时抛出的异常。如果索引为负或大于等于数组大小，则该索引为非法索引。 |

| ArrayStoreException            | 试图将错误类型的对象存储到一个对象数组时抛出的异常。        |

| ClassCastException              | 当试图将对象强制转换为不是实例的子类时，抛出该异常。        |

| IllegalArgumentException        | 抛出的异常表明向方法传递了一个不合法或不正确的参数。        |

| IllegalMonitorStateException    | 抛出的异常表明某一线程已经试图等待对象的监视器，或者试图通知其他正在等待对象的监视器而本身没有指定监视器的线程。 |

| IllegalStateException          | 在非法或不适当的时间调用方法时产生的信号。换句话说，即 Java 环境或 Java 应用程序没有处于请求操作所要求的适当状态下。 |

| IllegalThreadStateException    | 线程没有处于请求操作所要求的适当状态时抛出的异常。          |

| IndexOutOfBoundsException      | 指示某排序索引（例如对数组、字符串或向量的排序）超出范围时抛出。 |

| NegativeArraySizeException      | 如果应用程序试图创建大小为负的数组，则抛出该异常。          |

| NullPointerException            | 当应用程序试图在需要对象的地方使用 null 时，抛出该异常      |

| NumberFormatException          | 当应用程序试图将字符串转换成一种数值类型，但该字符串不能转换为适当格式时，抛出该异常。 |

| SecurityException              | 由安全管理器抛出的异常，指示存在安全侵犯。                  |

| StringIndexOutOfBoundsException | 此异常由 String 方法抛出，指示索引或者为负，或者超出字符串的大小。 |

| UnsupportedOperationException  | 当不支持请求的操作时，抛出该异常。                          |

# Java异常常见面试题、

##  Error 和 Exception 区别是什么？

Error 类型的错误通常为虚拟机相关错误，如系统崩溃，内存不足，堆栈溢出等，编译器不会对这类错误进行检测，JAVA 应用程序也不应对这类错误进行捕获，一旦这类错误发生，通常应用程序会被终止，仅靠应用程序本身无法恢复；

Exception 类的错误是可以在应用程序中进行捕获并处理的，通常遇到这种错误，应对其进行处理，使应用程序可以继续正常运行。

## JVM 是如何处理异常的？

在一个方法中如果发生异常，这个方法会创建一个异常对象，并转交给 JVM，该异常对象包含异常名称，异常描述以及异常发生时应用程序的状态。创建异常对象并转交给 JVM 的过程称为抛出异常。可能有一系列的方法调用，最终才进入抛出异常的方法，这一系列方法调用的有序列表叫做调用栈。

JVM 会顺着调用栈去查找看是否有可以处理异常的代码，如果有，则调用异常处理代码。当 JVM 发现可以处理异常的代码时，会把发生的异常传递给它。如果 JVM 没有找到可以处理该异常的代码块，JVM 就会将该异常转交给默认的异常处理器（默认处理器为 JVM 的一部分），默认异常处理器打印出异常信息并终止应用程序。

## NoClassDefFoundError 和 ClassNotFoundException 区别？

NoClassDefFoundError 是一个 Error 类型的异常，是由 JVM 引起的，不应该尝试捕获这个异常。

引起该异常的原因是 JVM 或 ClassLoader 尝试加载某类时在内存中找不到该类的定义，该动作发生在运行期间，即编译时该类存在，但是在运行时却找不到了，可能是变异后被删除了等原因导致；

ClassNotFoundException 是一个受查异常，需要显式地使用 try-catch 对其进行捕获和处理，或在方法签名中用 throws 关键字进行声明。当使用 Class.forName, ClassLoader.loadClass 或 ClassLoader.findSystemClass 动态加载类到内存的时候，通过传入的类路径参数没有找到该类，就会抛出该异常；另一种抛出该异常的可能原因是某个类已经由一个类加载器加载至内存中，另一个加载器又尝试去加载它。

# Java异常处理最佳实践

1. 在 finally 块中清理资源或者使用 try-with-resource 语句

2. 优先明确的异常

3. 对异常进行文档说明

4. 使用描述性消息抛出异常

5. 优先捕获最具体的异常

6. 不要捕获 Throwable 类 JVM 抛出错误，指出不应该由应用程序处理的严重问题。

7. 不要忽略异常	合理的做法是至少要记录异常的信息。

8. 不要记录并抛出异常	这经常会给同一个异常输出多条日志

9. 包装异常时不要抛弃原始的异常	否则，你将会丢失堆栈跟踪和原始异常的消息，这将会使分析导致异常的异常事件变得困难。

10. 不要使用异常控制程序的流程	会严重影响应用的性能

11. 使用标准异常	内建的异常可以解决问题，就不要定义自己的异常

12. 异常会影响性能	成本非常高