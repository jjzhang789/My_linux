##源码安装mysql5.7数据库

安装依赖
>yum -y install gcc gcc-c++ ncurses ncurses-devel cmake bison

下载相应源码
>wget http://downloads.sourceforge.net/project/boost/boost/1.59.0/boost_1_59_0.tar.gz

>wget http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.9.tar.gz

新建用户和用户组
>groupadd -r mysql && useradd -r -g mysql -s /sbin/nologin -M mysql

>mkdir /web/mysql_3306

编译安装
<pre>
cmake . -DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DMYSQL_DATADIR=/web/mysql_3306 \
-DWITH_BOOST=../boost_1_59_0 \
-DSYSCONFDIR=/etc \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \
-DWITH_FEDERATED_STORAGE_ENGINE=1 \
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
-DWITH_MYISAM_STORAGE_ENGINE=1 \
-DENABLED_LOCAL_INFILE=1 \
-DENABLE_DTRACE=0 \
-DDEFAULT_CHARSET=utf8mb4 \
-DDEFAULT_COLLATION=utf8mb4_general_ci \
-DWITH_EMBEDDED_SERVER=1
</pre>

>make -j \`grep processor /proc/cpuinfo | wc -l\`

>make install

设置开机自启动
>cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld

>chmod +x /etc/init.d/mysqld

设置配置文件
<pre>
cat > /etc/my.cnf << EOF
[client]
port = 3306
socket = /tmp/mysql.sock
default-character-set = utf8mb4
[mysqld]
port = 3306
socket = /tmp/mysql.sock
basedir = /usr/local/mysql
datadir = /web/mysql_3306
pid-file = /web/mysql_3306/mysql.pid
user = mysql
bind-address = 0.0.0.0
server-id = 1
init-connect = 'SET NAMES utf8mb4'
character-set-server = utf8mb4
back_log = 300
max_connections = 1000
max_connect_errors = 6000
open_files_limit = 65535
table_open_cache = 128
max_allowed_packet = 4M
binlog_cache_size = 1M
max_heap_table_size = 8M
tmp_table_size = 16M
read_buffer_size = 2M
read_rnd_buffer_size = 8M
sort_buffer_size = 8M
join_buffer_size = 8M
key_buffer_size = 4M
thread_cache_size = 8
query_cache_type = 1
query_cache_size = 8M
query_cache_limit = 2M
ft_min_word_len = 4
log_bin = mysql-bin
binlog_format = mixed
expire_logs_days = 30
log_error = /web/mysql_3306/mysql-error.log
slow_query_log = 1
long_query_time = 1
slow_query_log_file = /web/mysql_3306/mysql-slow.log
performance_schema = 0
explicit_defaults_for_timestamp
skip-external-locking
default_storage_engine = InnoDB
innodb_file_per_table = 1
innodb_open_files = 500
innodb_buffer_pool_size = 64M
innodb_write_io_threads = 4
innodb_read_io_threads = 4
innodb_thread_concurrency = 0
innodb_purge_threads = 1
innodb_flush_log_at_trx_commit = 2
innodb_log_buffer_size = 2M
innodb_log_file_size = 32M
innodb_log_files_in_group = 3
innodb_max_dirty_pages_pct = 90
innodb_lock_wait_timeout = 120
bulk_insert_buffer_size = 8M
myisam_sort_buffer_size = 8M
myisam_max_sort_file_size = 10G
myisam_repair_threads = 1
interactive_timeout = 28800
wait_timeout = 28800
[mysqldump]
quick
max_allowed_packet = 16M
[myisamchk]
key_buffer_size = 8M
sort_buffer_size = 8M
read_buffer = 4M
write_buffer = 4M
EOF
</pre>

添加环境变量
>echo -e '\n\nexport PATH=/usr/local/mysql/bin:$PATH\n' >> /etc/profile && source /etc/profile

初始化数据库
>mysqld --initialize-insecure --user=mysql --basedir=/usr/local/mysql --datadir=/web/mysql_3306

启动数据库
>/etc/init.d/mysqld start

我们在设置好MySQL数据库的安全配置后初始化root用户的密码。配制过程中，一路输入 y 就行了。这里只说明下MySQL5.7.13版本中，用户密码策略分成低级 LOW 、中等 MEDIUM 和超强 STRONG 三种，推荐使用中等 MEDIUM 级别！

>mysql\_secure\_installation