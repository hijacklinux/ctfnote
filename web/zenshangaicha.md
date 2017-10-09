---
layout: default
---
# ![](../img/hj.jpg)增删改查


## ![](../img/github11.png)插入数据语句
1.基本插入
```
>INSERT INTO customers(cust_name,cust_address) VALUES(‘Pep’, ‘100 main street’)
```
2.插入多行
```
>INSERT INTO customers(cust_name,cust_address) VALUES(‘Pep’, ‘100 main street’),(‘Tim’, ‘200 main Street’);
```
3.插入检索出来的数据
```
>INSERT INTO customers(cust_name,cust_address) SELECT cust_name, custaddress FROM custnew;
```

## ![](../img/github10.png)删除数据语句
DELETE FROM customers WHERE cust_id = 10006
## ![](../img/github12.png)更新数据语句
1.更新行
```
>UPDATE customers SET cust_email = ‘a@fudd.com’ WHERE cust_id = 10005
```
2.即使发生错误也继续进行而不是退出
```
>UPDATE IGNORE customers

```
## ![](../img/github13.png)查询数据语句
#### SELECT子句顺序
```
SELECT
FROM
WHERE
GROUP BY
HAVING
ORDER BY
LIMIT
```
#### SELECT
检索单个列
```
>SELECT col FROM tb_name;
```
多个列
```
>SELECT col1, col2 FROM tb_name;
```
检索所有列
```
>SELECT * FROM tb_name;
#除非确认要用到所有列
```
检索去重
```
>SELECT DISTINCT col FROM tb_name;
```
限制结果数
```
>SELECT col1 FROM tb_name LIMIT 5;
```
返回不多于五行
```
>SELECT col1 FROM tb_name LIMIT 5, 5;
```
 第一个为开始位置，初始为0.第二个为显示个数
等价于LIMIT 5 OFFSET 5
#### Order by
按某个字段排序
```
>SELECT col1 FROM tb_name ORDER BY col1;
```
按多列排序
```
>SELECT col1, col2, col3 FROM tb_name ORDER BY col1, col2;
```
指定排序方向（升序降序）
```
>SELECT col1, col2 FROM tb_name ORDER BY col1 DESC;
```
【默认ASC升序】

注意：如果想在多个列上排序，必须对每个列使用DESC

注意：ORDER BY必须放在LIMIT之前
#### Where
过滤
```
>SELECT col1, col2 FROM tb_name WHERE col1 = 2.5;
```
过滤不匹配
```
>SELECT col1, col2 FROM tb_name WHERE col1 <> 1000;
```
范围检查
```
>SELECT col1, col2 FROM tb_name WHERE col1 BETWEEN 5 AND 10;
```
空值检查
```
>SELECT col1 FROM tb_name WHERE col2 IS NULL;
NULL, 无值，它与字段包含0，空字符串或仅仅包含空格不同
```
多条件，组合and
```
>SELECT col1 FROM tb_name WHERE col1=100 AND col2 <= 10;
```
多条件, 组合or
```
>SELECT col1 FROM tb_name WHERE col1=100 OR col2 <= 10;
```
优先级 and 大于 or, 先处理的and,所以应该适当使用括号
select prod_id from products where (prod_price < 2.5 or vend_id = 1000) and prod_price > 1

指定查询范围, in操作符
```
>SELECT col1 FROM tb_name WHERE col1 IN (1001,1002);
```
取反，not操作符
```
>SELECT col1 FROM tb_name WHERE col1 NOT IN (1001,1002);
````
操作符
```
= <> != < <= > >= between A and B
```
#### like
通配
```
>SELECT col1 FROM tb_name WHERE col1 LIKE ‘jet%’;
%匹配0个或多个字符
```
单个字符
```
>SELECT col1 FROM tb_name WHERE col1 LIKE ‘_ ton anvil’;
```
#### regexp
正则搜索
```
>SELECT col1 FROM tb_name WHERE col1 REGEXP ‘1000’;
```
 REGEXP ‘.000’
REGEXP对列值匹配

进行or匹配
```
>SELECT col1 FROM tb_name WHERE col1 REGEXP ‘1000|2000’;
```
几个之一
```
select prod_id from products where prod_name regexp '[1|2]000';
```
匹配范围
```
select prod_id from products where prod_name regexp '[1-5]000';
```
```
匹配特殊字符，\ 进行转义

必须使用\\为前导。 \\-
>SELECT col1 FROM tb_name WHERE col1 REGEXP ‘\\.’
```
like和 regexp
like整列匹配
regexp 列值内匹配
#### concat
拼接字符
```
>SELECT Concat(name, ‘ ----‘, age) FROM tb_name
```
去除空白
```
>SELECT Rtrim(name) FROM tb_name
Ltrim() Trim()
```
使用列名
```
>SELECT Concat(name, ‘---‘, age) AS info FROM tb_name
```
算术计算
```
>SELECT quantity * item_price AS total_price FROM tb_name
支持+ - * /
```
#### group
group
```
>SELECT id, COUNT(*) AS num_prods FROM tb_name GROUP BY id
```
注意：

1.group by 可以包含任意数目的列

2.group by 中每个列都必须是检索列或有效的表达式（但不能是聚集函数）

3.除聚集函数外，select语句中的每个列都必须在group by子句中出现

4.如果分组列有Null值，Null将作为一个分组返回

5.group by 子句必须出现在where子句之后, order by 之前

过滤分组
```
>SELECT cust_id, COUNT(*) AS orders FROM orders GROUP BY cust_id HAVING COUNT(*) > 2
```
where和having区别
where在分组前过滤，having在分组后过滤
#### 子查询
1.用于过滤
```
>SELECT cust_id FROM orders WHERE order_num IN (SELECT order_num FROM orderitems)
```
2.作为字段
```
>SELECT cust_name,cust_state,(SELECT COUNT(*) FROM orders WHERE orders.cust_id = customers.cust_id) AS orders FROM customers ORDER BY cust_name

```
#### 组合查询
1.UNION
```
>SELECT vend_id, prod_id, prod_price FROM products WHERE prod_price <=5 UNION SELECT vend_id, prod_id, prod_price FROM products WHERE vend_id IN (1001,1002)
```
UNION自动去除重复行
UNION ALL 保留

2.放在UNION后的排序语句对所有SELECT生效


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
