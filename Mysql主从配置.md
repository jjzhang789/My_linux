##Mysql主从配置

####一、mysql安装
mysql安装已经有文章描述，本文只讲解主从配置。

####二、主库配置
<pre>
编辑/etc/my.cnf：
#开启binlog日志
log_bin = mysql_test
#设置server_id
server_id = 1
</pre>
设置完毕，重启mysql。

####三、从库配置
<pre>
编辑/etc/my.cnf：
#开启relaylog日志
relay-log = relay-log
#开启只读模式
read-only = 1
#设置server_id,不可与主库一样
server_id = 2
</pre>
设置完毕，重启mysql。

####四、设置主从复制

#####1、在主库运行命令：
在主库新建用于从库复制日志的用户：
>GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'username'@'从库IP' IDENTIFIED BY 'password';

在主库运行命令进行锁表，不可退出命令行，否则锁表失效：
>flush tables with read lock;

查看主数据库binlog信息，以备之后使用：
>show master status;

导出主库数据：
>/usr/local/mysql/bin/mysqldump --single-transaction -uroot -pPASSWORD -A > mysql_all.sql



解锁主库：
>unlock tables;

#####2、在从库运行命令：

把主库数据导入到从库：
>/usr/local/mysql/bin/mysql <mysql_all.sql

设置复制信息：
<pre>
#MASTER_LOG_FILE为在主库执行show master status时获得的文件命名；MASTER_LOG_POS为在主库获得的POS信息。

CHANGE MASTER TO MASTER_HOST='IP',\
MASTER_USER='username',MASTER_PORT=3306, \
MASTER_PASSWORD='password', \
MASTER_LOG_FILE='mysql_test.000001', \
MASTER_LOG_POS=46122, ;
</pre>

开启从库
>start slave;

查看复制状态
>SHOW SLAVE STATUS\G;

当Slave\_IO\_Running、Slave\_SQL\_Running都为Yes时表明配置成功。