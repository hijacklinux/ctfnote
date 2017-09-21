---
layout: default
---
# ![](../img/hj.jpg)伪造ip+用户登录+验证码绕过+浏览器同源策略
>小贱提示： 这里只是说说方法，至于原理我也懒得说了，毕竟这是笔记嘛，笔记就是要简单方便查阅

## ![](../img/github13.png)伪造ip
```html
client-ip
x-forwarded-for
x-remote-ip
x-orginating-ip
x-remote-addr
```
## ![](../img/github14.png)用户登录

#### 暴力破解
需要条件：
用户名和密码错误次数无限制
ip登录错误次数无限制

#### api登录
类似于CSRF，前提是没验证token


## ![](../img/github15.png)验证码绕过
#### 不刷新直接绕过
有的登陆页面验证码能够多次使用，原因是验证码跟Session绑定一起了，所以会把验证码明文或密文放在Cookie或POST数据包里面，所以每次只要同一个数据包里面两个验证码一致就可绕过
#### 暴力破解
注册或找回密码等操作是手机或邮箱验证码能够爆破，因为没有次数和超时限制
#### 机器识别
#### 打码平台


## ![](../img/github16.png)浏览器的同源策略
同源策略规定：不同域的客户端脚本在没明确授权的情况下，不能读写对方的资源
同源策略有以下几个关键词
#### 不同域或同域
下面列出与http://www.foo.com是否同域

|          站点           | 是否同域 |            原因            |
| ----------------------- | -------- | -------------------------- |
| https://www.foo.com     | 否       | 协议不同                   |
| http://hello.foo.com    | 否       | hello子域与www子域不同     |
| http://foo.com          | 否       | 顶级域与子域不是同一个概念 |
| http://www.foo.com:8080 | 否       | 端口不同                   |
| http://www.foo.com/a/                        |  是        |  满足同协议，同域名，同端口                          |

#### 客户端脚本
主要是指JavaScript和ActionScript，及二者都遵循的ECMAScript标准
#### 授权
客户端也存在授权现象

比如：

HTML5新标准提到关于AJAX跨域访问的情况：默认情况下是不允许跨域访问的，
只有目标站点（www.foo.com）明确返回HTTP响应头（Access-Control-Allow-Origin:www.8pwn.com），
www.8pwn.com站点上的客户端才有权通过AJAX技术对www.foo.com上的数据进行读写操作
#### 读写权限
HTTP请求头里的Referer（请求来源）：只可读

document.cookie：具备读写权限
#### 资源
- HTTP消息头
- 整个DOM树
- 浏览器存储,如：Cookies，Flash Cookies，localStorage等


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
