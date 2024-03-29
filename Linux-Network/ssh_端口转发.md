##详解SSH三种端口转发

###SSH端口转发的好处：

    1. 利用SSH通道天然的加密特性
    2. 通过具备访问权限的第三者，突破防火墙对自己的限制，或者隐身

###角色定义：
    A. 本地服务器，想通过中间服务器B间接访问目标服务器C
    B. 中间服务器，类似于代理，A以B的名义去访问C
    C. 目标服务器，C看到的都是中间服务器B在访问自己

    A Home_Computer_IP: 10.10.1.200
    B SSH_Forward_Server_IP1: 10.10.1.201
      SSH_Forward_Server_IP2: 192.168.1.201
    C The_Tag_Server_IP: 192.168.1.224

###1. 本地端口转发
####命令：
    ssh -Nf -L [local_A_address]:local_A_port:target_C_server:target_C_port via_B_server
    ssh -Nf -p B_SSH_PORT -L 10.10.1.200:8080:192.168.1.224:80 B_User@10.10.1.201
####参数：
    -N,不执行命令
    -f,后台执行
    -L,local本地端口转发
    local_A_address:
        127.0.0.1 - 默认，只能本机使用这个端口转发
        也可以是本机的IP地址，同时其他人可以使用这个IP来使用这个端口转发
    via_B_server:中间服务器
####应用：
    A---能访问------>B-------能访问------>C
    A---不能访问----------------------------->C
    A---通过本机端口,以B的名义访问-->C
####关闭：
    直接kill -9 建立的SSH连接，下同

###2. 远程端口转发 在B上执行
####命令：
    ssh -Nf -R [local_A_address]:local_A_port:target_C_server:target_C_port local_A_address
    ssh -Nf -p A_SSH_PORT 10.10.1.201:8080:192.168.1.224:80 A_User@10.10.1.200
####参数：
    -R,remote,远程端口转发
    local_A_address,这个地址为A的IP
####应用：
    环境和目的 与 本地端口转发是一样的，这里只是不在本地服务器A上执行命令，而是在中间服务器B上执行；
    为什么不直接在服务器A自己身上执行命令呢？这个场景有别于本地端口转发的地方在于A不能主动连接B但反之可以，比如A在外网，B在内网；
    而A去访问的时候，同样都是通过自己的IP和端口，同样首先建立AB之间的SSH通道，以服务器B的名义来访问目标服务器C。

###3. 动态端口转发
####命令：
    ssh -Nf -D [local_A_address]:local_A_port via_B_server
    ssh -Nf -p B_SSH_PORT 10.10.1.200:8080 B_User@10.10.1.201
####参数：
    -D,dynamic,动态端口转发
####应用：
    本地和远程端口转发，都限定了目标服务器以及目标服务器的端口；
    而动态端口转发，A把B作为了自己的全权代理，不限定目标服务器及其端口；
    这里要求在A上，做下代理设置，比如浏览器的代理设定为自己的IP:PORT；
    本地端口转发和远程端口转发，其实都可看着是动态端口转发(代理)的子集；
    三者和一般代理的目的和场景都一致，区别在于这里自己A和代理服务器B之前的所有连接都是基于加密的SSH。
