2006，MySQL server has gone away

By default, MySQL-5.0 does not automatically reconnect. 

mysql连接如果长时间idle的话，（时间：默认为8小时），会自动断开，而且不会为原连接自动恢复。 

在python中，如果应用程序某个模块使用了持久化的db链接，则失效后，继续使用，会报错： 2006，MySQL server has gone away 

解决办法： 比较ugly 

在每次连接之前，判断该链接是否有效。 MySQLdb提供的接口是 Connection.ping(), 
(奇怪，ping() 这个方法在 MySQLdb 的文档中居然没有文档化, 害我找了好久) 

采用 类似: 
    try: 
       conn.ping() 
    except Excption,e:      #实际对应的  MySQLdb.OperationalError 这个异常 
       conn = new conn 

的方法解决 

为了测试出 ping()的效果， 
我在代码中先关闭了 conn， 示意如下： 

    try: 
       conn.close() 
       conn.ping() 
    except Excption,e:      #实际对应的  MySQLdb.OperationalError 这个异常 
       print e 
       conn = new conn 

结果爆出了另外一个错误： 
InterfaceError: (0, '') 

google了一大圈都没找到正确原因，一度还以为是： 
  File "/usr/lib/pymodules/python2.6/MySQLdb/cursors.py", line 147, in execute charset = db.character_set_name() 
的问题， （因为错误的traceback 指向了此处...) 

实际上： 对任何已经close的conn进行 db相关 操作，包括ping()都会爆出这个错误。

    def executeSQL(self,sql=""):
            try:
                self.conn.ping()
            except Exception,e:
                self.log.error("Msql出了问题")
                self.log.error(str(e))
                while True:
                    try:
                        self.conn = MySQLdb.connect(self.config.get('mysql_server'),self.config.get('mysql_user'),self.config.get('mysql_pass'),self.config.get('mysql_db_name'),connect_timeout=60,compress=True,charset="UTF8")
                        break
                    except Exception,e:
                        self.log.error("尝试重连接失败")
                        time.sleep(2)
                        continue
                self.cursor=self.conn.cursor()
            try:
                self.cursor.execute(sql)
                self.conn.commit()
                return 1
            except Exception,e:
                self.log.error(str(e))
                return 0
