---
layout: default
---
# ![](../img/hj.jpg)Linux常用命令
```

pwd:查询当前目录

‘~’代表自己的home目录

‘/’代表根目录，/root并不是根目录。'.'代表当前目录。'..'代表上层目录

cd:change directory,切换目录。cd / :切换到根目录。 cd ..:返回上一级目录。tab自动补全。

ls:查看当前目录下的文件或目录。

ls -l:查看下面详细信息：drwxr-xr-x  2 cyborg  cyborg  4096  Jun  1 10:45  Desktop

drwxr-xr-x（d:类型为目录。rwx:权限,1组：属主；2组：属组；3组：其他用户） 2（文件硬链接数目）

cyborg（属主） cyborg（属组） 4096（大小，单位为byte字节） Jun  1 10:45（修改时间） Desktop。

ls -lh:h=human，人性化展示列表。

ls -a:查看所有（包括隐藏）ls -l 文件夹/：不用进入查看文件夹里面的文件或目录

touch .test:创建一个文件名为.test隐藏空文件。空文件夹也是要占容量的（4k）。

mkdir:创建目录。mkdir -p cn/shandong/jinan:一次性递归创建一堆目录

vim test:创建一个文件名为test的文档。

cat test:查看文件内容。-n：显示行号   -T：不显示制表符  合并：cat new1 new2 new3 > fly.rar

more test:一点点看，回车跳一行或空格跳一页，q退出。less test:more的高级版

tail test:查看尾几行，-数字：设定显示行数；-f 其他进程试用文件时查看，适用于监视日志

head test:头几行，同tail,但无-f功能



mv:move 移动

tree 文件夹名：查看文件夹的树结构

man 命令名：查看帮助，或者 命令名 --help  或者 help 命令名：查看内置命令

cp:复制粘贴一体 ;cp -R 递归复制目录内部

find / -name 'yum.log':从根(/)目录开始找yum.log的文件,也可'*.log'通配，路径可以根据实际情况写如 find /var/ -name 'index.php'

find / -size +10M | xargs ls -lh 找出大于10M的文件并查看结果的详细信息

which ps 或type ps 可找到ps位置

history 查看用过的命令



>文件名：清空文件

grep -n hello yum.log:在yum.log 定位hello字符串 -n,显示行号；-v a:不含有a的 ；  -c:只显示共多少行匹配;  -e :多匹配（或）如：grep -e a -e b -e c file1等价于grep [abc] file1等价于grep [a-c] file1

wc 文件名：统计返回：行数，单词数，字节数，文件名

dd:意思是disk dump，如：dd if=1.txt bs=1 skip=364 of=new.txt

校验md5:md5sum fly.rar         校验sha：shasum fly.rar

更优雅地重启：init 6

sensors:查看温度

修改ip:sudo ipconfig eth0 192.168.18.128

创建软链接：实实在在的文件，只想源文件的链接文件 inode编号不同，创建：ln -s data sl_data

创建硬链接：与源文件是同一个文件，同inode。创建：ln data hl_data

rm -f：强制删除    rmdir：删除目录    rm -ri my_dir:递归删除且询问    rm -rf my_dir:一口气全删

file 文件名：查看文件类型

ps:监测特定时间点进程    top:实时监测    kill pid:尽可能终止进程    killall 进程名（支持通配符）

top中：wa表示cpu的I/O，繁忙的话要么是网口，要么是硬盘；    id表示空闲；   mem总free=free+buffer+cache

mount：挂载，默认输出系统挂载的设备列表。mount /dev/sdb1(设备） /media/disk（挂接点）。umount 路径或设备：卸载，可移动设备必须先卸载再移除。

df -h：查看挂载磁盘使用情况 。 du:查看特定目录使用情况 -c:显示所有已列文件总大小；-h:人性化

tar -zcvf xxx.tar.gz或tgz a.txt b.txt c.txt:打包。

tar -zxvf xxx.tar:解压

gzip *.txt:压缩成gz（可通配符批量转换）；gzcat:查看gz文本内容。gunzip:解压gz文件

/etc/passwd: root:x:0:0:root:/root:/bin/bash意思是 登陆用户名：密码：UID：组ID：备注字段：Home目录位置：默认shell

密码存储在/etc/shadow中

useradd: 添加用户；    userdel -r xx:删除用户;     usermod：修改/etc/passwd配置    passwd xxx:修改自己的密码为xxx

chpasswd < users.txt   (users中的内容为userid:pass)

chsh -s /bin/csh xx  修改默认shell       chfn修改备注     finger xx查看用户信息

/etc/group:组文件

goupeadd 创建新组    usermod -G ga ua:把ua用户添加到组ga中    groupmod 修改组 -g 修改GID -n 修改组名；例如：goupemod -n gb ga 把组名ga改成gb

对于文件，全权限值为666（所有用户rw-）

对于目录，全权限值为777（所有用户rwx）

r：4    w:2   x:1   -:0

chmod 改变权限：方法1：chmod 777 file1   方法2：chmod [ugoa] [[+-=] [rwxXstugo]]

方法2参数说明：

参数1：u用户；g组；o其他；a所有     参数2：在现有基础上增加+ 移除- 设置成=

参数3：X：如果对象为目录或已有执行权限，赋予执行权限；   s:运行时重新设置UID或GID；    t:保留文件或目录；

          u:将权限设置为跟属主一样；     g:将权限设置为跟属组一样；    o:将权限设置为跟其他用户一样

chown:改变所属关系 用法：chown owner[.group] file

例如：chown dan file1 改变属主         chown dan.ga file2 同时改变属主和属组     chown .ga file3 改变属组    chgrp ga file4:改变文件‘默认’属组

free -m:查看内存剩余
```
## ![](../img/github3.png.jpg)通信指令
```
rusers:查看哪些人上机

ku 比 rusers 更好用，并提供 finger, talk, write, mail 等功能

mesg y 接受其他使用者讯息（系统预设值）
mesg n 拒绝其他使用者讯息

talk 线上一对一交谈系统，中文交谈用ctalk

举个栗子：想和hijack聊天，hijack正在使用192.168.1.3这台计算机，就talk hijack@192.168.1.3，前提是hijack在线，而对方可以mesg y接受或mesg n拒绝finger 可查询本地机器或远方机器使用者简要资料，例如：finger hijack@192.168.1.3

rlogin,rsh,telnet 远端登录(login)
```
## ![](../img/github4.png.jpg)系统资讯
```
quota -v 察看自己可用磁盘空间大小（单位∶KB）及档案个数

date 现在的日期、时间

who 查询目前和你使用同一机器的有哪些人及 login 时间地点

w 查询目前上机者详细状况

whoami 察看自己帐号名称

groups ［帐号名］ 查看某人的 group

passwd 更改密码

chsh 更改自己的 login shell

chfn 更改自己的全名（full name，不是帐号名）

cal 印出月历或年历

tty 显示目前所用终端机名称

history 查看自己下过的指令

nslookup 向 Name Server 查询 hostname 及 IP
```
## ![](../img/github5.png.jpg)处理程序（Process）的控制

```
kill 停止处理程序，通常先用 ps 命令查得 Process ID，再杀之 kill -9 立即停止一个
process 　kill -9 -1 杀掉系统内所有属於自己的 process

jobs 列出现在正在执行的工作

fg 将中止的 job 回到前景继续执行

bg 背景执行

at 在指定时间执行命令

batch 依序执行多个命令

crontab 要求系统定期执行特定命令

nice 调整 process 的优先权

nohup 使 process 在 logout 后继续执行

管道（pipe）及输出入重导（redirection）

标准输入（stdin）：平时为键盘，可用 < 转向。例∶mail b82000 < myfile 可将 myfile 档案寄给 b82000

标准输出（stdout）：平时为萤幕，可用 > 转向，用 >> 可将结果附加（append）在档案尾端。例∶finger b81045 > myfile 可将查询结果写在 myfile 档案上。

管道∶管道的符号是 “|” ，用来连接两个命令。 “|” 左边指令的输出作为 “|”右边指令的输入。 例∶ls -l .. | more 可将上一层目录内容以一页一页方式输出； who | grep b.503 | sort| more 可将目前上线的电机系学生名单经过排序后分页输出。
```
## ![](../img/github6.png.jpg)安装软件
```
dpkg;apt-get;apt-cache;aptitude

aptitude:进入aptitude

aptitude show 软件包名:显示软件包详情

aptitude search 软件包：搜索，自带通配符

apitude install 软件包：从软件仓库安装

aptitude safe-upgrade:妥善升级

不保守升级：aptitude full-upgrade;aptitude dist-upgrade

aptitude remove 软件包：保留数据删除软件包

aptitude purge 软件包：全删



dpkg -L 软件包名：显示所有相关文件列表

dpkg --search 文件：显示文件所属那个软件包



apt-get update;

apt-get install xx;

apt-get dist-upgrade



从源码安装软件：

1、解压：tar -zxvf xxx.tar

2、读readme

3、./configure

4、make编译

5、make install 安装到常用位置
```



__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./system)
