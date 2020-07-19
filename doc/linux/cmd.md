#linux

把用的命令在网上查过得命令进行整理，深度记忆

uname -a 显示系统的关键信息，内核名称主机名、内核版本、处理机类型

top 命令会默认按照CPU的占用情况，显示占用量较大的进程,可以使用top -u 查看某个用户的CPU使用排名情况。

ls -l | wc -l 计数当前目录下的文件数量

        wc -l
    
        统计输出信息的行数，统计结果就是输出信息的行数，一行信息对应一个文件，所以就是文件的个数。



解包：tar xvf FileName.tar

　　打包：tar cvf FileName.tar DirName

解压：unzip FileName.zip

　　压缩：zip FileName.zip DirName

nohup java -jar XXX.jar >temp.txt &  java后台运行程序 jobs查看后台任务 fg {id}前台运行

chmod 777 file  更改文件权限：r 表示可读取，w 表示可写入，x 表示可执行 

三种角色：该档案的拥有者，该档案的拥有者属于同一个群体(group)者，其他以外的人

kill -9 PID 强杀进程  ps -ef | grep 查看进程id

find . -name "*.txt" 将目前目录及其子目录下所有后缀名是txt的文件列出来

tail -f notes.log 实时刷新日志打印

# 文件管理命令

## cat 命令

cat 命令用于连接文件并打印到标准输出设备上。

1. 一次显示整个文件:

2. 从键盘创建一个文件:

3. 将几个文件合并为一个文件:

## hmod 命令

Linux/Unix 的文件调用权限分为三级 : 文件拥有者、群组、其他。利用 chmod 可以控制文件如何被他人所调用。

用于改变 linux 系统文件或目录的访问权限。用它控制文件或目录的访问权限。该命令有两种用法。一种是包含字母和操作符表达式的文字设定法；另一种是包含数字的数字设定法。

权限范围：

u ：目录或者文件的当前的用户

g ：目录或者文件的当前的群组

o ：除了目录或者文件的当前用户或群组之外的用户或者群组

a ：所有的用户及群组

权限代号：

r ：读权限，用数字4表示

w ：写权限，用数字2表示

x ：执行权限，用数字1表示

## chown 命令

chown 将指定文件的拥有者改为指定的用户或组，用户可以是用户名或者用户 ID；组可以是组名或者组 ID；文件是以空格分开的要改变权限的文件列表，支持通配符

## cp 命令

将源文件复制至目标文件，或将多个源文件复制至目标目录。

-i 提示

-r 复制目录及目录内所有项目

-a 复制的文件与原文件时间一样

cp -ai a.txt test

## find 命令

用于在文件树中查找文件，并作出相应的处理。

（1）查找 48 小时内修改过的文件

find -atime -2

（2）在当前目录查找 以 .log 结尾的文件。 . 代表当前目录

find ./ -name '*.log'

（3）查找 /opt 目录下 权限为 777 的文件

find /opt -perm 777

（4）查找大于 1K 的文件

find -size +1000c

## head 命令

head 用来显示档案的开头至标准输出中，默认 head 命令打印其相应文件的开头 10 行。

**实例**：

（1）显示 1.log 文件中前 20 行

```shell

head 1.log -n 20

```

（2）显示 1.log 文件前 20 字节

```shell

head -c 20 log2014.log

```

（3）显示 t.log最后 10 行

```shell

head -n -10 t.log

```

## less 命令

less 与 more 类似，但使用 less 可以随意浏览文件，而 more 仅能向前移动，却不能向后移动，而且 less 在查看之前不会加载整个文件。

## ln 命令

功能是为文件在另外一个位置建立一个同步的链接，当在不同目录需要该问题时，就不需要为每一个目录创建同样的文件，通过 ln 创建的链接（link）减少磁盘占用量。

软链接：

1.软链接，以路径的形式存在。类似于Windows操作系统中的快捷方式

2.软链接可以 跨文件系统 ，硬链接不可以

3.软链接可以对一个不存在的文件名进行链接

4.软链接可以对目录进行链接

硬链接:

1.硬链接，以文件副本的形式存在。但不占用实际空间。

2.不允许给目录创建硬链接

3.硬链接只有在同一个文件系统中才能创建

（1）给文件创建软链接，并显示操作信息	ln -sv source.log link.log

（2）给文件创建硬链接，并显示操作信息	ln -v source.log link1.log

（3）给目录创建软链接	ln -sv /opt/soft/test/test3 /opt/soft/test/test5

## more 命令

功能类似于 cat, more 会以一页一页的显示方便使用者逐页阅读，而最基本的指令就是按空白键（space）就往下一页显示，按 b 键就会往回（back）一页显示。

命令参数：

+n      从笫 n 行开始显示

-n      定义屏幕大小为n行

+/pattern 在每个档案显示前搜寻该字串（pattern），然后从该字串前两行之后开始显示

-c      从顶部清屏，然后显示

-d      提示“Press space to continue，’q’ to quit（按空格键继续，按q键退出）”，禁用响铃功能

-l        忽略Ctrl+l（换页）字符

-p      通过清除窗口而不是滚屏来对文件进行换页，与-c选项相似

-s      把连续的多个空行显示为一行

-u      把文件内容中的下画线去掉

常用操作命令：

Enter    向下 n 行，需要定义。默认为 1 行

Ctrl+F  向下滚动一屏

空格键  向下滚动一屏

Ctrl+B  返回上一屏

=      输出当前行的行号

:f    输出文件名和当前行的行号

V      调用vi编辑器

!命令  调用Shell，并执行命令

q      退出more

## mv 命令

移动文件或修改文件名，根据第二参数类型（如目录，则移动文件；如为文件则重命令该文件）。

（1）将文件 test.log 重命名为 test1.txt

mv test.log test1.txt

1

（2）将文件 log1.txt,log2.txt,log3.txt 移动到根的 test3 目录中

mv llog1.txt log2.txt log3.txt /test3

1

（3）将文件 file1 改名为 file2，如果 file2 已经存在，则询问是否覆盖

mv -i log1.txt log2.txt

1

（4）移动当前文件夹下的所有文件到上一级目录

mv * ../

## rm 命令

删除一个目录中的一个或多个文件或目录，如果没有使用 -r 选项，则 rm 不会删除目录。如果使用 rm 来删除文件，通常仍可以将该文件恢复原状。

**实例**：

（1）删除任何 .log 文件，删除前逐一询问确认：

```shell

rm -i *.log

```

（2）删除 test 子目录及子目录中所有档案删除，并且不用一一确认：

```shell

rm -rf test

```

（3）删除以 -f 开头的文件

```shell

rm -- -f*

```

## tail 命令

用于显示指定文件末尾内容，不指定文件时，作为输入信息进行处理。常用查看日志文件。

**常用参数**：

```

-f 循环读取（常用于查看递增的日志文件）

-n<行数> 显示行数（从后向前）

```

## touch 命令

Linux touch命令用于修改文件或者目录的时间属性，包括存取时间和更改时间。

实例

使用指令"touch"修改文件"testfile"的时间属性为当前系统时间，输入如下命令：

$ touch testfile                #修改文件的时间属性

首先，使用ls命令查看testfile文件的属性，如下所示：

$ ls -l testfile                #查看文件的时间属性 

执行指令"touch"修改文件属性以后，并再次查看该文件的时间属性，如下所示：

touch testfile                #修改文件时间属性为当前系统时间 

ls -l testfile                #查看文件的时间属性 

修改后文件的时间属性为当前系统时间 

使用指令"touch"时，如果指定的文件不存在，则将创建一个新的空白文件。例如，在当前目录下，使用该指令创建一个空白文件"file"，输入如下命令：

$ touch file 

## vim 命令

Vim是从 vi 发展出来的一个文本编辑器。代码补完、编译及错误跳转等方便编程的功能特别丰富，在程序员中被广泛使用。

基本上 vi/vim 共分为三种模式，分别是**命令模式（Command mode）**，**输入模式（Insert mode）\**和\**底线命令模式（Last line mode）**。

## whereis 命令

whereis 命令只能用于程序名的搜索，而且只搜索二进制文件（参数-b）、man说明文件（参数-m）和源代码文件（参数-s）。如果省略参数，则返回所有信息。whereis 及 locate 都是基于系统内建的数据库进行搜索，因此效率很高，而find则是遍历硬盘查找文件。

## which 命令

在 linux 要查找某个文件，但不知道放在哪里了，可以使用下面的一些命令来搜索：

which    查看可执行文件的位置。

whereis 查看文件的位置。

locate  配合数据库查看文件位置。

find        实际搜寻硬盘查询文件名称。

which 是在 PATH 就是指定的路径中，搜索某个系统命令的位置，并返回第一个搜索结果。使用 which 命令，就可以看到某个系统命令是否存在，以及执行的到底是哪一个位置的命令。

# 文档编辑命令

## grep 命令

强大的文本搜索命令，grep(Global Regular Expression Print) 全局正则表达式搜索。

（1）查找指定进程

ps -ef | grep svn

（2）查找指定进程个数

ps -ef | grep svn -c

（3）从文件中读取关键词

cat test1.txt | grep -f key.log

（4）从文件夹中递归查找以grep开头的行，并只列出文件

grep -lR '^grep' /tmp

（5）查找非x开关的行内容

grep '^[^x]' test.txt

（6）显示包含 ed 或者 at 字符的内容行

grep -E 'ed|at' test.txt

## wc 命令

wc(word count)功能为统计指定的文件中字节数、字数、行数，并将统计结果输出

**命令参数**：

```

-c 统计字节数

-l 统计行数

-m 统计字符数

-w 统计词数，一个字被定义为由空白、跳格或换行字符分隔的字符串

```

# 磁盘管理命令

## cd 命令

cd(changeDirectory) 命令  说明：切换当前目录至 dirName。

## df 命令（常用）

显示磁盘空间使用情况。获取硬盘被占用了多少空间，目前还剩下多少空间等信息，如果没有文件名被指定，则所有当前被挂载的文件系统的可用空间将被显示。默认情况下，磁盘空间将以 1KB 为单位进行显示，除非环境变量 POSIXLY_CORRECT 被指定，那样将以512字节为单位进行显示：

-a 全部文件系统列表

-h 以方便阅读的方式显示信息（常用参数）

-i 显示inode信息

-k 区块为1024字节

-l 只显示本地磁盘

-T 列出文件系统类型

## du 命令

du 命令也是查看使用空间的，但是与 df 命令不同的是 Linux du 命令是对文件和目录磁盘使用的空间的查看：

-a 显示目录中所有文件大小

-k 以KB为单位显示文件大小

-m 以MB为单位显示文件大小

-g 以GB为单位显示文件大小

-h 以易读方式显示文件大小

-s 仅显示总计

-c或--total  除了显示个别目录或文件的大小外，同时也显示所有目录或文件的总和

## ls命令

就是 list 的缩写，通过 ls 命令不仅可以查看 linux 文件夹包含的文件，而且可以查看文件权限(包括目录、文件夹、文件权限)查看目录信息等等。

常用参数搭配：

ls -a 列出目录所有文件，包含以.开始的隐藏文件

ls -A 列出除.及..的其它文件

ls -r 反序排列

ls -t 以文件修改时间排序

ls -S 以文件大小排序

ls -h 以易读大小显示

ls -l 除了文件名之外，还将文件的权限、所有者、文件大小等信息详细列出来

## mkdir 命令

mkdir 命令用于创建文件夹。

可用选项：

- **-m**: 对新建目录设置存取权限，也可以用 chmod 命令设置;

- **-p**: 可以是一个路径名称。此时若路径中的某些目录尚不存在,加上此选项后，系统将自动建立好那些尚不在的目录，即一次可以建立多个目录。

## pwd 命令

pwd 命令用于查看当前工作目录路径。

## rmdir 命令

从一个目录中删除一个或多个子目录项，删除某目录时也必须具有对其父目录的写权限。

# 网络通讯命令

## ifconfig 命令

- ifconfig 用于查看和配置 Linux 系统的网络接口。

- 查看所有网络接口及其状态：`ifconfig -a` 。

- 使用 up 和 down 命令启动或停止某个接口：`ifconfig eth0 up` 和 `ifconfig eth0 down` 。

## iptables 命令

iptables ，是一个配置 Linux 内核防火墙的命令行工具。功能非常强大，对于我们开发来说，主要掌握如何开放端口即可。

## netstat 命令

Linux netstat命令用于显示网络状态。

利用netstat指令可让你得知整个Linux系统的网络情况

## ping 命令

Linux ping命令用于检测主机。

执行ping指令会使用ICMP传输协议，发出要求回应的信息，若远端主机的网络功能没有问题，就会回应该信息，因而得知该主机运作正常。

## telnet 命令

Linux telnet命令用于远端登入。

执行telnet指令开启终端机阶段作业，并登入远端主机。

# 系统管理命令

## date 命令

显示或设定系统的日期与时间。

（1）显示下一天

date +%Y%m%d --date="+1 day"  //显示下一天的日期

（2）-d参数使用

date -d "nov 22"  今年的 11 月 22 日是星期三

date -d '2 weeks' 2周后的日期

date -d 'next monday' (下周一的日期)

date -d next-day +%Y%m%d（明天的日期）或者：date -d tomorrow +%Y%m%d

date -d last-day +%Y%m%d(昨天的日期) 或者：date -d yesterday +%Y%m%d

date -d last-month +%Y%m(上个月是几月)

date -d next-month +%Y%m(下个月是几月)

## free 命令（常用）

显示系统内存使用情况，包括物理内存、交互区内存(swap)和内核缓冲区内存。

常用-m参数

命令参数：

-b 以Byte显示内存使用情况

-k 以kb为单位显示内存使用情况

-m 以mb为单位显示内存使用情况

-g 以gb为单位显示内存使用情况

-s<间隔秒数> 持续显示内存

-t 显示内存使用总合

## kill 命令

发送指定的信号到相应进程。不指定型号将发送SIGTERM（15）终止指定进程。如果任无法终止该程序可用"-KILL" 参数，其发送的信号为SIGKILL(9) ，将强制结束进程，使用ps命令或者jobs 命令可以查看进程号。root用户将影响用户的进程，非root用户只能影响自己的进程。

## ps 命令

ps(process status)，用来查看当前运行的进程状态，一次性查看，如果需要动态连续结果使用 top

linux上进程有5种状态:

1. 运行(正在运行或在运行队列中等待)

2. 中断(休眠中, 受阻, 在等待某个条件的形成或接受到信号)

3. 不可中断(收到信号不唤醒和不可运行, 进程必须等待直到有中断发生)

4. 僵死(进程已终止, 但进程描述符存在, 直到父进程调用wait4()系统调用后释放)

5. 停止(进程收到SIGSTOP, SIGSTP, SIGTIN, SIGTOU信号后停止运行运行)

**命令参数**：

```

-A 显示所有进程

a 显示所有进程

-a 显示同一终端下所有进程

c 显示进程真实名称

e 显示环境变量

f 显示进程间的关系

r 显示当前终端运行的进程

-aux 显示所有包含其它使用的进程

```

## rpm 命令

Linux rpm 命令用于管理套件。

## top 命令（常用）

显示当前系统正在执行的进程的相关信息，包括进程 ID、内存占用率、CPU 占用率等，

99.9%id：空闲cpu百分比

 load average: 0.00, 0.02, 0.05系统负载，即任务队列的平均长度。
三个数值分别为 1分钟、5分钟、15分钟前到现在的平均值。 

**常用参数**：

```

-c 显示完整的进程命令

-s 保密模式

-p <进程号> 指定进程显示

-n <次数>循环显示次数

```

## yum 命令

yum（ Yellow dog Updater, Modified）是一个在Fedora和RedHat以及SUSE中的Shell前端软件包管理器。

##  vmstat 命令 

 vmstat 5 5 【在5秒时间内进行5次采样】 

**字段说明：**

Procs（进程）：

>  r: 运行队列中进程数量
>
>  b： 等待IO的进程数量

CPU（以百分比表示）：

>  us: 用户进程执行时间(user time)
>
>  sy: 系统进程执行时间(system time)
>
>  id: 空闲时间(包括IO等待时间),中央处理器的空闲时间 。以百分比表示。
>
>  wa: 等待IO时间

## iostat命令

iostat -xdk 2 3  查看设备使用率（%util）、响应时间（await 

rrqm/s： 每秒进行 merge 的读操作数目.即 delta(rmerge)/s

wrqm/s： 每秒进行 merge 的写操作数目.即 delta(wmerge)/s

%util： 一秒中有百分之多少的时间用于 I/O

如果%util接近100%，说明产生的I/O请求太多，I/O系统已经满负荷

  idle小于70% IO压力就较大了，一般读取速度有较多的wait。

# 备份压缩命令

## bzip2 命令

- 创建 `*.bz2` 压缩文件：`bzip2 test.txt` 。

- 解压 `*.bz2` 文件：`bzip2 -d test.txt.bz2` 。

## gzip 命令

- 创建一个 `*.gz` 的压缩文件：`gzip test.txt` 。

- 解压 `*.gz` 文件：`gzip -d test.txt.gz` 。

- 显示压缩的比率：`gzip -l *.gz` 。

## tar 命令

用来压缩和解压文件。tar 本身不具有压缩功能，只具有打包功能，有关压缩及解压是调用其它的功能来完成。

弄清两个概念：打包和压缩。打包是指将一大堆文件或目录变成一个总的文件；压缩则是将一个大的文件通过一些压缩算法变成一个小文件

常用参数：

-c 建立新的压缩文件

-f 指定压缩文件

-r 添加文件到已经压缩文件包中

-u 添加改了和现有的文件到压缩包中

-x 从压缩包中抽取文件

-t 显示压缩文件中的内容

-z 支持gzip压缩

-j 支持bzip2压缩

-Z 支持compress解压文件

-v 显示操作过程

## unzip 命令

- 解压 `*.zip` 文件：`unzip test.zip` 。

- 查看 `*.zip` 文件的内容：`unzip -l jasper.zip` 。

# 服务类命令

## service(centos6)
注册在系统中的标准化层序

有方便统一的管理方式（常用的方法）

service 服务名 start
service 服务名 stop
service 服务名 restart
service 服务名 reload
service 服务名 status
查看服务的方法 /etc/init.d/服务名

通过chkconfig 命令设置自启动

查看服务 chkconfig --list|grep xxx

chkconfig --level 5 服务名 on

(on 是启动，off是关闭)

## systemctl(centos7)
注册在系统中的标准化程序
有方便统一的管理方式（常用的方法）
systemctl start 服务名(xxxx.service)
systemctl restart 服务名(xxxx.service)
systemctl stop 服务名(xxxx.service)
systemctl reload 服务名(xxxx.service)
systemctl status 服务名(xxxx.service)
查看服务的方法 /usr/lib/systemd/system
查看服务的命令
systemctl list-unit-files
systemctl --type service
通过systemctl命令设置自启动
自启动systemctl enable service_name
不自启动systemctl disable serivice_name

