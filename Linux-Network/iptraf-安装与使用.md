##iptraf 安装与使用
>iptraf是一个基于ncurses开发的IP局域网监控工具，它可以实时地监视网卡流量，可以生成各种网络统计数据，包括TCP信息、UDP统计、ICMP和OSPF信息、以太网负载信息、节点统计、IP校验和错误和其它一些信息。

###iptraf的参数列表

>iptraf后面加上不同的参数，可以起到不同的作用，下面是iptraf的参数命令列表：

    参数命令	作用
    -i iface	网络接口：立即在指定网络接口上开启IP流量监视,iface为all指监视所有的网络接口，iface指相应的interface
    -g	立即开始生成网络接口的概要状态信息
    -d iface	网络接口：在指定网络接口上立即开始监视明细的网络流量信息,iface指相应的interface
    -s iface	网络接口：在指定网络接口上立即开始监视TCP和UDP网络流量信息,iface指相应的interface
    -z iface	网络接口：在指定网络接口上显示包计数,iface指相应的interface
    -l iface	网络接口：在指定网络接口上立即开始监视局域网工作站信息,iface指相应的interface
    -t timeout	时间：指定iptraf指令监视的时间，timeout指监视时间的minute数
    -B	将标注输出重新定向到“/dev/null”，关闭标注输入，将程序作为后台进程运行
    -L logfile	指定一个文件用于记录所有命令行的log，默认文件是地址：/var/log/iptraf
    -I interval	指定记录log的时间间隔（单位是minute），不包括IP traffic monitor
    -u	允许使用不支持的接口作为以太网设备
    -f	清空所有计数器
    -h	显示帮助信息



### 按任意键进入
![00.jpg](http://upload-images.jianshu.io/upload_images/5969055-0c3ea3d650a7d2d5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![01.jpg](http://upload-images.jianshu.io/upload_images/5969055-3f45a57f2b82638f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![02.jpg](http://upload-images.jianshu.io/upload_images/5969055-5c2ea4fd75f4656e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![03.jpg](http://upload-images.jianshu.io/upload_images/5969055-07d821210652c58c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![11.jpg](http://upload-images.jianshu.io/upload_images/5969055-68a254fe971427b8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![21.jpg](http://upload-images.jianshu.io/upload_images/5969055-35d403a23a61df8d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![31.jpg](http://upload-images.jianshu.io/upload_images/5969055-314cf03d158d2a1b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![32.jpg](http://upload-images.jianshu.io/upload_images/5969055-13a0a6e9376a3316.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![41.jpg](http://upload-images.jianshu.io/upload_images/5969055-66e2d1d02199e71e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


