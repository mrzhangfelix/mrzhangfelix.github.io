JDK 中常用的包有哪些
java.lang：这个是系统的基础类；
java.io：这里面是所有输入输出有关的类，比如文件操作等；
java.nio：为了完善 io 包中的功能，提高 io 包中性能而写的一个新包；
java.net：这里面是与网络有关的类；
java.util：这个是系统辅助类，特别是集合类；
java.sql：这个是数据库操作的类。

# 常见类

## String

1. char charAt(int index) 返回指定索引处的 char 值。
1. boolean endsWith(String suffix) 测试此字符串是否以指定的后缀结束。
1. boolean equals(Object anObject) 将此字符串与指定的对象比较。
1. boolean equalsIgnoreCase(String anotherString) 将此 String 与另一个 String 比较，不考虑大小写。
1. int indexOf(String str)  返回指定子字符串在此字符串中第一次出现处的索引。
1. int indexOf(String str, int fromIndex) 返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始。
1. int lastIndexOf(int ch)  返回指定字符在此字符串中最后一次出现处的索引。
1. int length() 返回此字符串的长度。
1. boolean matches(String regex) 告知此字符串是否匹配给定的正则表达式。
1. String replace(char oldChar, char newChar) 返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所有 oldChar 得到的。
1. String[] split(String regex) 根据给定正则表达式的匹配拆分此字符串。
1. boolean startsWith(String prefix) 测试此字符串是否以指定的前缀开始。
1. boolean startsWith(String prefix, int toffset) 测试此字符串从指定索引开始的子字符串是否以指定前缀开始。
1. String substring(int beginIndex, int endIndex) 返回一个新字符串，它是此字符串的一个子字符串。
1. char[] toCharArray() 将此字符串转换为一个新的字符数组
1. String toLowerCase() 使用默认语言环境的规则将此 String 中的所有字符都转换为小写。
1. String toUpperCase() 使用默认语言环境的规则将此 String 中的所有字符都转换为大写。
1. String trim() 返回字符串的副本，忽略前导空白和尾部空白。

## StringBuilder&StringBuffer
1.	public StringBuffer append(String s) 将指定的字符串追加到此字符序列。
2.	public StringBuffer reverse()  将此字符序列用其反转形式取代。
3.	public delete(int start, int end) 移除此序列的子字符串中的字符。
4.	public insert(int offset, int i) 将 int 参数的字符串表示形式插入此序列中。
5.	replace(int start, int end, String str) 使用给定 String 中的字符替换此序列的子字符串中的字符。

##  java.lang.Math

1. Math.abs(12.3);		12.3 返回这个数的绝对值
2. Math.sqrt(x);			√(x) x的二次方根
3. Math.max(x, y);		返回x、y中较大的那个数
4. Math.min(x, y);		返回x、y中较小的那个数
5. Math.pow(x, y);					
6. Math.pow(2, 3);		即2³ 即返回：8
7. Math.random();					//随机返回[0,1)之间的无符号double值			
8. Math.sin(α);					//sin（α）的值
9. Math.cos(α);					//cos（α）的值
10. Math.tan(α);			tan（α）的值

## Character 

1. isLetter() 是否是一个字母 
2. isDigit() 是否是一个数字字符 
3. isWhitespace() 是否是一个空白字符 
4. isUpperCase() 是否是大写字母 
5. isLowerCase() 是否是小写字母 
6. toUpperCase() 指定字母的大写形式 
7. toLowerCase() 指定字母的小写形式 
8. toString 返回字符的字符串形式，字符串的长度仅为1 

## Date
1.	boolean after(Date date)  若当调用此方法的Date对象在指定日期之后返回true,否则返回false。
2.	boolean before(Date date)  若当调用此方法的Date对象在指定日期之前返回true,否则返回false。
3.	Object clone( )  返回此对象的副本。
4.	int compareTo(Date date)  比较当调用此方法的Date对象和指定日期。两者相等时候返回0。调用对象在指定日期之前则返回负数。调用对象在指定日期之后则返回正数。
5.	boolean equals(Object date)  当调用此方法的Date对象和指定日期相等时候返回true,否则返回false。
6.	long getTime( )  返回自 1970 年 1 月 1 日 00:00:00 GMT 以来此 Date 对象表示的毫秒数。
7.	void setTime(long time)  用自1970年1月1日00:00:00 GMT以后time毫秒数设置时间和日期。
8.	String toString( )  把此 Date 对象转换为以下形式的 String： dow mon dd hh:mm:ss zzz yyyy 其中： dow 是一周中的某一天 (Sun, Mon, Tue, Wed, Thu, Fri, Sat)。

## Matcher
1. public int start()  返回以前匹配的初始索引。
2.	public int start(int group)  返回在以前的匹配操作期间，由给定组所捕获的子序列的初始索引
3.	public int end() 返回最后匹配字符之后的偏移量。
4.	public int end(int group) 返回在以前的匹配操作期间，由给定组所捕获子序列的最后字符之后的偏移量。
1.	public boolean lookingAt()  尝试将从区域开头开始的输入序列与该模式匹配。
2.	public boolean find() 尝试查找与该模式匹配的输入序列的下一个子序列。
3.	public boolean find(int start） 重置此匹配器，然后尝试查找匹配该模式、从指定索引开始的输入序列的下一个子序列。
4.	public boolean matches() 尝试将整个区域与模式匹配。
1.	public Matcher appendReplacement(StringBuffer sb, String replacement) 实现非终端添加和替换步骤。
2.	public StringBuffer appendTail(StringBuffer sb) 实现终端添加和替换步骤。
3.	public String replaceAll(String replacement)  替换模式与给定替换字符串相匹配的输入序列的每个子序列。
4.	public String replaceFirst(String replacement)  替换模式与给定替换字符串匹配的输入序列的第一个子序列。
5.	public static String quoteReplacement(String s) 返回指定字符串的字面替换字符串。这个方法返回一个字符串，就像传递给Matcher类的appendReplacement 方法一个字面字符串一样工作。

## Stack 
1	boolean empty() 测试堆栈是否为空。
2	Object peek( ) 查看堆栈顶部的对象，但不从堆栈中移除它。
3	Object pop( ) 移除堆栈顶部的对象，并作为此函数的值返回该对象。
4	Object push(Object element) 把项压入堆栈顶部。
5	int search(Object element) 返回对象在堆栈中的位置，以 1 为基数。

## Thread
1.	public void start() 使该线程开始执行；Java 虚拟机调用该线程的 run 方法。
2.	public void run() 如果该线程是使用独立的 Runnable 运行对象构造的，则调用该 Runnable 对象的 run 方法；否则，该方法不执行任何操作并返回。
3.	public final void setName(String name) 改变线程名称，使之与参数 name 相同。
4.	public final void setPriority(int priority)  更改线程的优先级。
5.	public final void setDaemon(boolean on) 将该线程标记为守护线程或用户线程。
6.	public final void join(long millisec) 等待该线程终止的时间最长为 millis 毫秒。
7.	public void interrupt() 中断线程。
8.	public final boolean isAlive() 测试线程是否处于活动状态。



## Files的常用方法
Files. exists()：检测文件路径是否存在。
Files. createFile()：创建文件。
Files. createDirectory()：创建文件夹。
Files. delete()：删除一个文件或目录。
Files. copy()：复制文件。
Files. move()：移动文件。
Files. size()：查看文件个数。
Files. read()：读取文件。
Files. write()：写入文件。













