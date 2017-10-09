---
layout: default
---
# ![](../img/hj.jpg)gdb的使用

## ![](../img/github17.png)编译c的时候用gcc -ggdb
如编译meet.c

gcc -o meet -ggdb meet.c

或

gcc –ggdb –mpreferred-stack-boundary=2 –o meet meet.c
## ![](../img/github18.png)gdb载入程序
方法1：
```
gdb meet
```
方法2：
```
gdb
file meet
```
## ![](../img/github19.png)下断点 b
```
b <行号>       			举例：b 8
b <函数名称>				举例：b main
b *<函数名称>			举例：b *main
b *<内存地址>			举例：b *0x804835c

d [编号]
删除断点
d 全删
```
## ![](../img/github21.png)继续执行 c
Continue的简写，继续执行被调试程序，直至下一个断点或程序结束。
## ![](../img/github20.png)run \<args>或r
用给定的参数，在 gdb 内部启动调试程序
## ![](../img/github22.png)s,n/si,ni
s 相当于其它调试器中的“Step Into (单步跟踪进入)”；
n 相当于其它调试器中的“Step Over (单步步过)”。

si/ni所针对的是汇编指令，而s/n针对的是源代码
## ![](../img/github23.png)info或i
info b  显示有关断点的信息

info reg  显示当前寄存器状态信息
## ![](../img/github24.png)bt 回溯
回溯命令，给出当前调用栈信息
## ![](../img/github25.png)up/down
在调用栈中向上或向下移动
## ![](../img/github26.png)print或p
print var  输出变量 var 的值

print /x $<reg>  输出指定寄存器的值
## ![](../img/github27.png)x /NT A
检查内存，其中

N 	是需要显示的数据单元的数目

T 	是数据单元的类型（候选值，x 表示十六进制，d 表示十进制，c 表示字符，s 表示字符串，i 表示指令）

A 	是绝对地址值，或符号名如“main”
## ![](../img/github28.png)list/l
列出源码

## ![](../img/github1.png)quit或q
退出 gdb
## ![](../img/github2.png)用gdb反汇编
```
set disassembly-flavor <intel/att>		设置 Intel （NASM）还是 AT&T 格式，默认情况下， gdb 使用 AT&T格式
disassemble <function name>			反汇编给定的函数（可以指定 main()）

举例（反汇编meet程序的greeting函数）：
gdb meet
(gdb) disassemble greeting
```

__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./pwn)
