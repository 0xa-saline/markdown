- ##Windows:
        - 1.以系统管理员身份登陆系统。
        - 2.打开cmd-----net start 查看mysql是否启动。启动的话就停止net stop mysql.
        - 3.我的mysql安装在d:\usr\local\mysql4\bin下。
        - 4.跳过权限检查启动mysql.
        - d:\usr\local\mysql\bin\mysqld-nt --skip-grant-tables
        - 5.重新打开cmd。进到d:\usr\local\mysql4\bin下：
        -   d:\usr\local\mysql\bin\mysqladmin -u root flush-privileges password "newpassword"
        -   d:\usr\local\mysql\bin\mysqladmin -u root -p shutdown  这句提示你重新输密码。
        - 6.在cmd里net start mysql
        - 7.搞定了。
- ##Linux:
    - ###MySQL root密码的恢复方法之一

            -1.KILL掉系统里的MySQL进程；
            -    killall -TERM MySQLd 
            -2.用以下命令启动MySQL，以不检查权限的方式启动；
            -    safe_MySQLd --skip-grant-tables & 
            -3.然后用空密码方式使用root用户登录 MySQL；
            -    MySQL -u root 
            -4.修改root用户的密码；
            -    MySQL> update MySQL.user set password=PASSWORD('新密码') where User='root';  
            -    MySQL> flush privileges;  
            -    MySQL> quit 
            - 重新启动MySQL，就可以使用新密码登录了。
    - ###MySQLroot密码的恢复方法二
>有可能你的系统没有 safe_MySQLd 程序(比如我现在用的 ubuntu操作系统, apt-get安装的MySQL) , 下面方法可以恢复
        
        - 1.停止MySQLd；
        -     sudo /etc/init.d/MySQL stop
        - (您可能有其它的方法,总之停止MySQLd的运行就可以了)
        - 2.用以下命令启动MySQL，以不检查权限的方式启动；
        -     MySQLd --skip-grant-tables &
        - 3.然后用空密码方式使用root用户登录 MySQL；
        -     MySQL -u root
        - 4.修改root用户的密码；
        -     MySQL> update MySQL.user set password=PASSWORD('newpassword') where User='root';  
        -     MySQL> flush privileges;  
        -     MySQL> quit 
        - 重新启动MySQL
        -     /etc/init.d/MySQL restart
        - 就可以使用新密码 newpassword 登录了。
