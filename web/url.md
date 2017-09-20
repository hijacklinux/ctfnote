---
layout: default
---
# ![](../img/hj.jpg)url与HTML协议

## ![](../img/github3.png)url
#### 格式
协议://用户名:密码@子域名.域名.顶级域名:端口号/目录/文件名.文件后缀?参数=值#标志

#### 协议有：

|  协议  |                说明                |
| ------ | ---------------------------------- |
| http   | 超文本传输协议资源                 |
| https  | 用安全套接字层传送的超文本传输协议 |
| ftp    | 文件传输协议                       |
| mailto | 电子邮件地址                       |
| ldap   | 轻型目录访问协议搜索               |
| file   | 当地电脑或网上分享的文件           |
| news   | Usenet新闻组                       |
| gopher | Gopher协议                         |
| telnet | Telnet协议                         |

## ![](../img/github4.png)HTTP协议
#### 请求头
GET http://www.foo.com/   HTTP/1.1
```
  这一行必不可少，常见的方法有GET/POST，最后的HTTP/1.1表示HTTP的版本是1.1
```
Host:www.foo.com
```
  这一行也必不可少，表示请求的主机是什么
```
User-Agent

  ```
  User-Agent：Mozilla/5.0 (Windows NT 10.0; WOW64; rv:55.0) Gecko/20100101 Firefox/55.0

  很重要，用于表明身份（我是谁）。从这里可以看到操作系统，浏览器，浏览器内核以及对应版本号等信息
  ```
Referer

  ```
  Referer：http://www.baidu.com/
  很重要，表明从哪里来，比如从http://www.baidu.com/页面点击而来
  ```
Cookie
#### 请求体
一般出现在POST方法中，比如表单的键值对
#### 响应头

HTTP/1.1  200  OK
```
必须有，200是状态码，OK是状态描述
```
Server：Apache/2.2.8 (Win32) PHP/5.2.6
```
透露了服务端的一些信息：Web容器，操作系统，服务端语言及版本等
```
X-Powered-By：PHP/5.2.6
```
同样透露了服务端语言信息
```
Content-Length：3635
```
响应体长度
```
Content-Type：text/html;charset=gbk
```
响应资源的类型与字符集
```

Set-Cookie

Set-Cookie : PTOKEN = ; expires = Mon, 01 Jan 1970 00:00:00 GMT ; Path = / ; domain = .foo.com ; HttpOnly ; Secure

Set-Cookie : USERID = c89d97c13d7a9b ; expires = Tue, 01 Jan 2030 00:00:00 GMT ; Path = / ; domain = .foo.com

每个Set-Cookie都设置一个Cookie

|   参数   |                                   说明                                   |
| -------- | ------------------------------------------------------------------------ |
| expires  | 过期时间，如果过期，cookie被删除                                         |
| path     | 相对路径，只有这个路径下的资源可以访问Cookie                             |
| domain   | 域名                                                                     |
| HttpOnly | 标志（默认无），有的话，表明Cookie只存在于HTTP层面，不能被客户端脚本读取 |
| Secure   | 标志（默认无），有的话，表明Cookie仅通过HTTPS协议安全传输                |


#### 响应体



__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
