## Mysql 使用总结

1、添加用户并赋权：
>grant 权限 on 数据库 to 用户@主机 identified by '密码' ;

2、设置用户权限：
>grant 权限 on 数据库 to 用户@主机 ;

3、查看用户权限：
>show grants for 用户@主机 ;

4、切换数据库：
>use database_name ;

5、查询：
>select ( filed1,filed2 | * ) from table_name where 条件;

6、删除数据：
>delete from table_name where 条件;

7、更新数据：
>update table_name set filed1=XX where 条件;

8、回收权限：
>revoke 权限 on 数据库 from 用户@主机;

9、查看进程信息：
>show processlist ;

10、查看当前设置信息：
>show variables like '%变量%' ;

11、更改全局设置（重启失效）；
>set global  变量=XX ;

12、重载权限信息到内存：
>flush privileges ;

13、更改用户密码：
>update user set password=PASSWORD('密码') where 条件;

>flush privileges ;
