---
layout: default
---
# ![](../img/hj.jpg)逆向工程-PC逆向-针对SE壳的应对方法
>小贱提示： 如果是次数或者时间等这种限制，跟踪注册表，然后删除对应注册表即可

## SE壳如何patch
### 准备数据
```
jmp_addr (源码按jmp机器码为5，根据情况修改机器码长度）
se_offset=sn_addr-sbv_addr
se_call_addr
字节集_还原（正版hex数据）
填入se补丁机器码源码中。编译生成一个hook.dll,loader.e编译个注入器.exe。

bp RegCreateKeyExA之前可以先ctrl g，ZwCreateFile，下断，反复运行，
直到堆栈advapi32.dll出现（让它模拟完advapi32），取消断点，ctrl g，RegCreateKeyExA,
从段首复制几行二进制，去m里找到模拟api的位置，反汇编跟随去这个位置，retn处下断，或者直接bp也行。
```
### 找正版数据
```
se壳查看版本：载入od后跟随两次jmp，ctrl a，注释栏就有版本了
然后需要找到机器码位置，将本机的机器码替换成正版机器码
```
### 找sn_addr和jmp_addr和sbv_addr
```
查找patch位置:ctrl b,RegQueryValueExA，ctrl l，
有个ascii 0（有的od不分析，那也直接跟随下面的跳转），从这开始往下找的第一个跳转，跟随，
再继续当前选中行往下找第一个跳转，跟随，一直这样，直到jmp跳后，
当前选中行和retn之间没有jmp为止(或者f8单步跟也行），
按-号键，返回最后那个跳转，下断并记下jmp_addr，运行，断下，看堆栈，有个字符串，
数据跟随并记下sbv_addr，这个字符串是帮助我们确定机器码位置的.f8单步，出retn，
下面第一个跳转下断，两次f9断下，发现数据窗口数据改变了，里面就有机器码（租实开头）记下sn_addr。
```
### 找se_call_addr
```
虽然找到位置了，但是时机不对，已经判断完了，需要记录机器码地址，重载，
数据窗口定位机器码地址，f9，在第一个断点断下，
在数据窗口中机器码尾部下硬件写入断点（没断下来就下内存断点），出retn，两次f9，
此时就可以替换正版机器码了，此时就可以hook了，往下找个jmp，ctrl x记录下地址作为se_call_addr
```

>![](../img/hj.jpg)小贱提示：
>
>se双击软件ctrl v的机器码需要base64hex软件转换，得到hex数据才能用。关于得到正版机器码：在正版机中删除key，就会有ctrl v数据是正版数据。
>
>se有的时候跟随跳转死循环，那么可能中间有哪个是je下面有个jnz，试试不跟je，跟jnz。

__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
