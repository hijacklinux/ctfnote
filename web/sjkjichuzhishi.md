---
layout: default
---
# ![](../img/hj.jpg)myql数据库基础知识

## ![](../img/github4.png)环境配置
ubuntu安装MySQL
```
sudo apt-get install mysql-server mysql-client
netstat -nltp | grep mysql
配置文件 /etc/mysql/my.conf
```
## ![](../img/github5.png)进入
输入： mysql

或者

mysql -u ken

mysql -u ken -p -h myserver -P 9999 【用户名，主机名，端口】

获取帮助: mysql --help

1.命令必须；或\\g结束，仅Enter不执行命令

2.help 或\\h获得帮助

3.quit或exit退出

可以用GUI工具

MySQL Administrator

MySQL Query Browser
## ![](../img/github6.png)use
创建库:
```
>CREATE DATABASE MYSQLDATA
```
使用某个库
```
use db_name
```
## ![](../img/github7.png)show
查看所有数据库
```
show databases;
```
列出库中所有表
```
use db_name;
show tables;
```

列出表的所有列信息
```
show columns from table_name;
或
desc table_name;
```
显示创建的sql语句
```
show create database db_name;
show create table table_name;
```
其他
```
show status  服务器状态信息
show grants  显示授权用户
show errors/show warnings 显示服务器错误或警告信息
```
## ![](../img/github8.png)导入导出
1.导入

用文本形式插入数据
```
>LOAD DATA LOCAL INFILE 'd:/mysql.txt' INTO TABLE mytable;
```
导入.sql
```
>use database;
>source d:/mysql.sql
```
从另外一张表往这张表插入
INSERT INTO tab1(f1,f2) SELECT a.f1, a.f2 FROM a WHERE a.f1='a'

2.备份
导出要用到MySQL的mysqldump工具，基本用法是：
```
mysqldump [OPTIONS] database [tables]
```
备份MySQL数据库的命令
```
mysqldump -hhostname -uusername -ppassword databasename > backupfile.sql
```
备份MySQL数据库为带删除表的格式，能够让该备份覆盖已有数据库而不需要手动删除原有数据库。
```
mysqldump -–add-drop-table -uusername -ppassword databasename > backupfile.sql
```
直接将MySQL数据库压缩备份
```
mysqldump -hhostname -uusername -ppassword databasename | gzip > backupfile.sql.gz
```
备份MySQL数据库某个(些)表
```
mysqldump -hhostname -uusername -ppassword databasename specific_table1 specific_table2 > backupfile.sql
```
同时备份多个MySQL数据库
```
mysqldump -hhostname -uusername -ppassword –databases databasename1 databasename2 databasename3 > multibackupfile.sql
```
仅仅备份数据库结构
```
mysqldump –no-data –databases databasename1 databasename2 databasename3 > structurebackupfile.sql
```
备份服务器上所有数据库
```
mysqldump –all-databases > allbackupfile.sql
```
3.还原
还原MySQL数据库的命令
```
mysql -hhostname -uusername -ppassword databasename < backupfile.sql
mysql -hhostname -ppassword databasename tablename < backuptablefile.sql
```
还原压缩的MySQL数据库
```
gunzip < backupfile.sql.gz | mysql -uusername -ppassword databasename
```
将数据库转移到新服务器
```
mysqldump -uusername -ppassword databasename | mysql –host=*.*.*.* -C databasename
```
4.将查询结果导入外部文件
```
SELECT a,b,a+b
FROM test_table
INTO OUTFILE '/tmp/result.txt'
FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"'
LINES TERMINATED BY '\n'
```
或者
```
mysql -u you -p -e "SELECT ..." >  file_name
```
## ![](../img/github9.png)实时监控
查看mysql数据库的当前连接数

命令： show processlist;

或者
```
# mysqladmin -uroot -p密码 processlist
```
当前状态

命令： show status;

或者 # mysqladmin -uroot -p密码 status

日志监控：
```
1、找到/etc目录下的my.cnf文件
2、找到[mysqlId]，添加如下代码：log =/tmp/mysqls.log，
如果需要监控慢查询，可以添加如下内容：
log-slow-requeries = /tmp/mysqlslowquedery.log
long_query_time = 1
3、重启服务：
service mysqld restart
4、监控SQL语句
tail -f /tmp/mysqls.log
```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
