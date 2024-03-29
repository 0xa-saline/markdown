linux mkdir 命令用来创建指定的名称的目录，要求创建目录的用户在当前目录中具有写权限，并且指定的目录名不能是当前目录中已有的目录。



1．命令格式：



    mkdir [选项] 目录…



2．命令功能：



    通过 mkdir 命令可以实现在指定位置创建以 DirName(指定的文件名)命名的文件夹或目录。要创建文件夹或目录的用户必须对所创建的文件夹的父文件夹具有写权限。并且，所创建的文件夹(目录)不能与其父目录(即父文件夹)中的文件名重名，即同一个目录下不能有同名的(区分大小写)。



3．命令参数：



    -m, --mode=模式，设定权限<模式> (类似 chmod)，而不是 rwxrwxrwx 减 umask

     

    -p, --parents  可以是一个路径名称。此时若路径中的某些目录尚不存在,加上此选项后,系统将自动建立好那些尚不存在的目录,即一次可以建立多个目录;

     

    -v, --verbose  每次创建新目录都显示信息

     

    --help   显示此帮助信息并退出

     

    --version  输出版本信息并退出



4．命令实例：



实例1：创建一个空目录 



命令：



    mkdir test1



输出：



    [root@localhost soft]# cd test

    [root@localhost test]# mkdir test1

    [root@localhost test]# ll

    总计 4drwxr-xr-x 2 root root 4096 10-25 17:42 test1

    [root@localhost test]#



实例2：递归创建多个目录 



命令：



    mkdir -p test2/test22



输出：



    [root@localhost test]# mkdir -p test2/test22

    [root@localhost test]# ll

    总计 8drwxr-xr-x 2 root root 4096 10-25 17:42 test1

    drwxr-xr-x 3 root root 4096 10-25 17:44 test2

    [root@localhost test]# cd test2/

    [root@localhost test2]# ll

    总计 4drwxr-xr-x 2 root root 4096 10-25 17:44 test22

    [root@localhost test2]#



实例3：创建权限为777的目录 



命令：



    mkdir -m 777 test3



输出：



    [root@localhost test]# mkdir -m 777 test3

    [root@localhost test]# ll

    总计 12drwxr-xr-x 2 root root 4096 10-25 17:42 test1

    drwxr-xr-x 3 root root 4096 10-25 17:44 test2

    drwxrwxrwx 2 root root 4096 10-25 17:46 test3

    [root@localhost test]#



说明：



    test3 的权限为rwxrwxrwx



实例4：创建新目录都显示信息



命令：



    mkdir -v test4



输出：



    [root@localhost test]# mkdir -v test4

    mkdir: 已创建目录 “test4”

    [root@localhost test]# mkdir -vp test5/test5-1

    mkdir: 已创建目录 “test5”

    mkdir: 已创建目录 “test5/test5-1”

    [root@localhost test]#



实例五：一个命令创建项目的目录结构



参考：http://www.ibm.com/developerworks/cn/aix/library/au-badunixhabits.html



命令：



    mkdir -vp scf/{lib/,bin/,doc/{info,product},logs/{info,product},service/deploy/{info,product}}



输出：



    [root@localhost test]# mkdir -vp scf/{lib/,bin/,doc/{info,product},logs/{info,product},service/deploy/{info,product}}

    mkdir: 已创建目录 “scf”

    mkdir: 已创建目录 “scf/lib”

    mkdir: 已创建目录 “scf/bin”

    mkdir: 已创建目录 “scf/doc”

    mkdir: 已创建目录 “scf/doc/info”

    mkdir: 已创建目录 “scf/doc/product”

    mkdir: 已创建目录 “scf/logs”

    mkdir: 已创建目录 “scf/logs/info”

    mkdir: 已创建目录 “scf/logs/product”

    mkdir: 已创建目录 “scf/service”

    mkdir: 已创建目录 “scf/service/deploy”

    mkdir: 已创建目录 “scf/service/deploy/info”

    mkdir: 已创建目录 “scf/service/deploy/product”

    [root@localhost test]# tree scf/

    scf/

    |-- bin

    |-- doc

    |   |-- info

    |   `-- product

    |-- lib

    |-- logs

    |   |-- info

    |   `-- product

    `-- service

       `-- deploy

                |-- info

                `-- product

    12 directories, 0 files

    [root@localhost test]#
