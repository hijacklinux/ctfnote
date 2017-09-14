---
layout: default
---
# ![](../img/hj.jpg)逆向工程-PC逆向-破解-暗桩总结
>小贱提示： 对市面上的恶意暗桩软件的应对方案

## ![](../img/github8.png)暗桩容易出现的字符串
1. physicaldrive0
>小贱提示：右键跟随，然后用00填充
2. 破解
3. 破
4. ntfs
5. format
6. 本地磁盘
7. kill
8. debug
9. ollydbg

## ![](../img/github9.png)干掉退出暗桩
>小贱提示：易语言退出暗桩特点，55开头，有俩call，直接段首retn

## ![](../img/github10.png)干掉自动退出暗桩
>小贱提示：在api常用断点>程序退出下断，堆栈看下面返回到xxxx,跟随。

## ![](../img/github11.png)干掉蓝屏暗桩
>小贱提示：使用蓝屏插件

## ![](../img/github12.png)pchunter防止关机

__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
