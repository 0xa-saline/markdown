###MySQL会出现中文乱码的原因不外乎下列几点：
    1.server本身设定问题，例如还停留在latin1
    2.table的语系设定问题(包含character与collation)
    3.客户端程式(例如php)的连线语系设定问题
##强烈建议使用utf8!!!!
####utf8可以兼容世界上所有字符!!!!
###一、避免创建数据库及表出现中文乱码和查看编码方法
####1、创建数据库的时候：

    
    CREATE DATABASE `test`  
    CHARACTER SET 'utf8'  
    COLLATE 'utf8_general_ci';  
####2、建表的时候 

    CREATE TABLE `database_user` (  
    `ID` varchar(40) NOT NULL default '',  
    `UserID` varchar(40) NOT NULL default '',  
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8;  

>这3个设置好了，基本就不会出问题了,即建库和建表时都使用相同的编码格式。
>但是如果你已经建了库和表可以通过以下方式进行查询。
####1.查看默认的编码格式:


    mysql> show variables like "%char%";  
    +--------------------------+---------------+  
    | Variable_name | Value |  
    +--------------------------+---------------+  
    | character_set_client | gbk |  
    | character_set_connection | gbk |  
    | character_set_database | utf8 |  
    | character_set_filesystem | binary |  
    | character_set_results | gbk |  
    | character_set_server | utf8 |  
    | character_set_system | utf8 |  
    +--------------------------+-------------+  
    
>注：以前2个来确定,可以使用set names utf8,set names gbk设置默认的编码格式;
>执行SET NAMES utf8的效果等同于同时设定如下：


    SET character_set_client='utf8';  
    SET character_set_connection='utf8';  
    SET character_set_results='utf8';  
    

####2.查看test数据库的编码格式:

 
    mysql> show create database test;  
    +------------+------------------------------------------------------------------------------------------------+  
    | Database | Create Database |  
    +------------+------------------------------------------------------------------------------------------------+  
    | test | CREATE DATABASE `test` /*!40100 DEFAULT CHARACTER SET gbk */ |  
    +------------+------------------------------------------------------------------------------------------------+  

####3.查看yjdb数据表的编码格式:
  
    mysql> show create table yjdb;  
    | yjdb | CREATE TABLE `yjdb` (  
    `sn` int(5) NOT NULL AUTO_INCREMENT,  
    `type` varchar(10) NOT NULL,  
    `brc` varchar(6) NOT NULL,  
    `teller` int(6) NOT NULL,  
    `telname` varchar(10) NOT NULL,  
    `date` int(10) NOT NULL,  
    `count` int(6) NOT NULL,  
    `back` int(10) NOT NULL,  
    PRIMARY KEY (`sn`),  
    UNIQUE KEY `sn` (`sn`),  
    UNIQUE KEY `sn_2` (`sn`)  
    ) ENGINE=MyISAM AUTO_INCREMENT=1826 DEFAULT CHARSET=gbk ROW_FORMAT=DYNAMIC |  
    

####二、避免导入数据有中文乱码的问题
#####1:将数据编码格式保存为utf-8
#####设置默认编码为utf8：
    set names utf8;
#####设置数据库db_name默认为utf8:

  
    ALTER DATABASE `db_name` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;  
#####设置表tb_name默认编码为utf8:
  
    ALTER TABLE `tb_name` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;  
##导入：
  
    LOAD DATA LOCAL INFILE 'C:\\utf8.txt' INTO TABLE yjdb;  
####2:将数据编码格式保存为ansi(即GBK或GB2312)
#####设置默认编码为gbk：
    set names gbk;
#####设置数据库db_name默认编码为gbk:
  
    ALTER DATABASE `db_name` DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;  
#####设置表tb_name默认编码为gbk:
  
    ALTER TABLE `tb_name` DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;  
###导入：
  
    LOAD DATA LOCAL INFILE 'C:\\gbk.txt' INTO TABLE yjdb;  
>注：1.UTF8不要导入gbk，gbk不要导入UTF8;
>2.dos下不支持UTF8的显示;
###三、解决网页中乱码的问题
 
>将网站编码设为 utf-8,这样可以兼容世界上所有字符。
>　　如果网站已经运作了好久,已有很多旧数据,不能再更改简体中文的设定,那么建议将页面的编码设为 GBK, GBK与GB2312的区别就在于:GBK能比GB2312显示更多的字符,要显示简体码的繁体字,就只能用GBK。
>1.编辑/etc/my.cnf　,在[mysql]段加入default_character_set=utf8;
>2.在编写Connection URL时，加上?useUnicode=true&characterEncoding=utf-8参;
>3.在网页代码中加上一个"set names utf8"或者"set names gbk"的指令，告诉MySQL连线内容都要使用
>utf8或者gbk;
