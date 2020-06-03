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
