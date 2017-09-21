---
layout: default
---
# ![](../img/hj.jpg)SQL判断类型


## ![](../img/github23.png)?id=49 and user>0
重点在 and user>0，

user 是 SQLServer 的一个内置变量，它的值是当前连接的用户名，类型为 nvarchar。

拿一个 nvarchar 的值跟 int 的数 0 比较，系统会先试图将 nvarchar 的值转成 int 型，当然，转的过程中肯定会出错，SQLServer 的出错提示是：将 nvarchar 值 ”abc” 转换数据类型为 int 的列时发生语法错误，abc 正是变量 user 的值，这样，就拿到了数据库的用户名。

>小贱提示：
>
>上面的方法可以很方便的测试出是否是用 sa 登录，注意：如果是 sa 登录，提示是将”dbo”转换成 int 的列发生错误，而不是”sa”。

## ![](../img/github24.png)GET
## ![](../img/github25.png)POST
>小贱提示：如果登陆框有字数限制，很有可能是javascript作怪，可以自己构造个html再访问提交
## ![](../img/github26.png)Cookie
## ![](../img/github27.png)Http Header
user-agent/client-ip/x-forwarded-for/referer

这些都是$_SERVER变量，不受GPC转义的影响
## ![](../img/github28.png)hash注入
## ![](../img/github2.png)登陆界面
## ![](../img/github1.png)订单处理
## ![](../img/github3.png)搜索框


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
