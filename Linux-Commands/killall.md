Linux系统中的killall命令用于杀死指定名字的进程（kill processes by name）。



我们可以使用kill命令杀死指定进程PID的进程，如果要找到我们需要杀死的进程，



我们还需要在之前使用ps等命令再配合grep来查找进程，而killall把这两个过程合二为一，是一个很好用的命令。



1．命令格式：



    killall[参数][进程名]



2．命令功能：



    用来结束同名的的所有进程



3．命令参数：



    -Z 只杀死拥有scontext 的进程

    -e 要求匹配进程名称

    -I 忽略小写

    -g 杀死进程组而不是进程

    -i 交互模式，杀死进程前先询问用户

    -l 列出所有的已知信号名称

    -q 不输出警告信息

    -s 发送指定的信号

    -v 报告信号是否成功发送

    -w 等待进程死亡

    --help 显示帮助信息

    --version 显示版本显示



4．使用实例：



实例1：杀死所有同名进程



    命令：



    killall vi



    输出：



    [root@localhost ~]# ps -ef|grep vi

    root     17581 17398  0 17:51 pts/0    00:00:00 vi test.txt

    root     17611 17582  0 17:51 pts/1    00:00:00 grep vi

    [root@localhost ~]# ps -ef|grep vi

    root     17581 17398  0 17:51 pts/0    00:00:00 vi test.txt

    root     17640 17612  0 17:51 pts/2    00:00:00 vi test.log

    root     17642 17582  0 17:51 pts/1    00:00:00 grep vi

    [root@localhost ~]# killall vi

    [root@localhost ~]# ps -ef|grep vi

    root     17645 17582  0 17:52 pts/1    00:00:00 grep vi



实例2：向进程发送指定信号



    命令：

    后台运行程序：vi &

    杀死 vi进程：killall -TERM vi  或者  killall -KILL vi

    

    输出：

    

    [root@localhost ~]# vi & 

    [1] 17646[root@localhost ~]# killall -TERM vi

    [1]+  Stopped                 vi

    [root@localhost ~]# vi & 

    [2] 17648[root@localhost ~]# ps -ef|grep vi

    root     17646 17582  0 17:54 pts/1    00:00:00 vi

    root     17648 17582  0 17:54 pts/1    00:00:00 vi

    root     17650 17582  0 17:55 pts/1    00:00:00 grep vi

    

    [2]+  Stopped                 vi

    [root@localhost ~]# killall -TERM vi

    [root@localhost ~]# ps -ef|grep vi

    root     17646 17582  0 17:54 pts/1    00:00:00 vi

    root     17648 17582  0 17:54 pts/1    00:00:00 vi

    root     17653 17582  0 17:55 pts/1    00:00:00 grep vi

    [root@localhost ~]# killall -KILL vi

    [1]-  已杀死               vi

    [2]+  已杀死               vi

    [root@localhost ~]# ps -ef|grep vi

    root     17656 17582  0 17:56 pts/1    00:00:00 grep vi

    [root@localhost ~]#



实例3：把所有的登录后的shell给杀掉



    命令：

    killall -9 bash

    

    输出：

    

    [root@localhost ~]# w

     18:01:03 up 41 days, 18:53,  3 users,  load average: 0.00, 0.00, 0.00USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT

    root     pts/0    10.2.0.68        14:58    9:52   0.10s  0.10s -bash

    root     pts/1    10.2.0.68        17:51    0.00s  0.02s  0.00s w

    root     pts/2    10.2.0.68        17:51    9:24   0.01s  0.01s -bash

    [root@localhost ~]# killall -9 bash

    [root@localhost ~]# w

     18:01:48 up 41 days, 18:54,  1 user,  load average: 0.07, 0.02, 0.00USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT

    root     pts/0    10.2.0.68        18:01    0.00s  0.01s  0.00s w

    [root@localhost ~]#



说明：



运行命令：killall -9 bash 后，所有bash都会被卡掉了，所以当前所有连接丢失了。需要重新连接并登录。
