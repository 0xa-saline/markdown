##ubuntu下解决wireshark权限问题

wireshark要监控eth0，但是必须要root权限才行。但是，直接用root运行程序是相当危险，也是非常不方便的。



#解决方法如下： 

1.添加wireshark用户组 



    sudo groupadd wireshark 



2.将dumpcap更改为wireshark用户组 



    sudo chgrp wireshark /usr/bin/dumpcap 



3.让wireshark用户组有root权限使用dumpcap 



    sudo chmod 4755 /usr/bin/dumpcap 



4.将需要使用的用户名加入wireshark用户组，我的用户名是craftor 



    sudo gpasswd -a craftor wireshark
