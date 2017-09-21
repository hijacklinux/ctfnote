---
layout: default
---
# ![](../img/hj.jpg)SQL注入格式
>小贱提示：替换掉[查询条件]

## ![](../img/github4.png)数字型
ID=49 这类注入的参数是数字型，SQL 语句原貌大致如下：
```sql
Select * from 表名 where 字段=49
```
注入的参数为 ID=-49 And [查询条件],即是生成语句：
```sql
Select * from 表名 where 字段=-49 And [查询条件]
```
## ![](../img/github5.png)字符型
需要单引号或双引号闭合

Class=连续剧 这类注入的参数是字符型，SQL 语句原貌大致概如下：
```sql
Select * from 表名 where 字段=‟连续剧‟
```
注入的参数为 ：
```sql
Class=连续剧‘ and [查询条件] and ’‘=’
```
即是生成语句：
```sql
Select * from 表名 where 字段=‘连续剧’ and [查询条件] and ‘’=‘’
```
## ![](../img/github6.png) keyword=关键字
搜索时没过滤参数的，如 keyword=关键字，SQL 语句原貌大致如下：
```sql
Select * from 表名 where 字段 like ‘%关键字%’
```
注入的参数为
```sql
 keyword=‘ and [查询条件] and ’%25‘=’
```
 即是生成语句：
 ```sql
Select * from 表名 where 字段 like ‘%’ and [查询条件] and ‘%’=‘%’
```
__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
