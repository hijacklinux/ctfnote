---
layout: default
---
# ![](../img/hj.jpg)smali指令


## ![](../img/github19.png)nop
空操作指令的助记符为nop。它的值为00，通常nop指令被用来作对齐代码之用，无实际操作
## ![](../img/github20.png)move
宽度值中每个字母表示宽度为4位
```
 “move vA, vB”：将vB寄存器的值赋给vA寄存器，源寄存器与目的寄存器都为4位。

 “move/from16 vAA, vBBBB”：将vBBBB寄存器的值赋给vAA寄存器，源寄存器为16位，目的寄存器为8位。

 “move/16 vAAAA, vBBBB”：将vBBBB寄存器的值赋给vAAAA寄存器，源寄存器与目的寄存器都为16位。

 “move-wide vA, vB”：为4位的寄存器对赋值。源寄存器与目的寄存器都为4位。

 “move-wide/from16 vAA, vBBBB”与“move-wide/16 vAAAA, vBBBB”实现与“move-wide”相同。

 “move-object vA, vB”：为对象赋值。源寄存器与目的寄存器都为4位。

 “move-object/from16 vAA, vBBBB”：为对象赋值。源寄存器为16位，目的寄存器为8位。

 “move-object/16 vAA, vBBBB”：为对象赋值。源寄存器与目的寄存器都为16位。

 “move-result vAA”：将上一个invoke类型指令操作的单字非对象结果赋给vAA寄存器。

 “move-result-wide vAA”：将上一个invoke类型指令操作的双字非对象结果赋给vAA寄存器。

 “move-result-object vAA"：将上一个invoke类型指令操作的对象结果赋给vAA寄存器。

 “move-exception vAA”：保存一个运行时发生的异常到vAA寄存器，这条指令必须是异常发生时的异常处理器的一条指令。否则的话，指令无效。
 ```
## ![](../img/github21.png)return
```
"return-void"：表示函数从一个void方法返回。无返回值

 “return vAA”：表示函数返回一个32位非对象类型的值，返回值寄存器为8位的寄存器vAA。

 “return-wide vAA”：表示函数返回一个64位非对象类型的值，返回值为8位的寄存器对vAA。

 “return-object vAA”：表示函数返回一个对象类型的值。返回值为8位的寄存器vAA。

```

## ![](../img/github22.png)const
数据定义指令用来定义程序中用到的常量，字符串，类等数据。它的基础字节码为const。可以理解为赋值
```
    “const/4 vA, #+B”：将数值符号扩展为32位后赋给寄存器vA。

    “const/16 vAA, #+BBBB”：将数据符号扩展为32位后赋给寄存器vAA。

    “const vAA, #+BBBBBBBB”：将数值赋给寄存器vAA。

    “const/high16 vAA, #+BBBB0000“：将数值右边零扩展为32位后赋给寄存器vAA。

    “const-wide/16 vAA, #+BBBB”：将数值符号扩展为64位后赋给寄存器对vAA。

    “const-wide/32 vAA, #+BBBBBBBB”：将数值符号扩展为64位后赋给寄存器对vAA。

    “const-wide vAA, #+BBBBBBBBBBBBBBBB”：将数值赋给寄存器对vAA。

    “const-wide/high16 vAA, #+BBBB000000000000”：将数值右边零扩展为64位后赋给寄存器对vAA。

    “const-string vAA, string@BBBB”：通过字符串索引构造一个字符串并赋给寄存器vAA。

    “const-string/jumbo vAA, string@BBBBBBBB”：通过字符串索引（较大）构造一个字符串并赋给寄存器vAA。

    “const-class vAA, type@BBBB”：通过类型索引获取一个类引用并赋给寄存器vAA。

    “const-class/jumbo vAAAA, type@BBBBBBBB”：通过给定的类型索引获取一个类引用并赋给寄存器vAAAA。这条指令占用两个字节，值为0xooff（Android4.0中新增的指令）。
```
## ![](../img/github23.png)跳转指令
```
if-eq   等于则跳转 ==                equal to
if-ne   不等于则跳转 !=                not equal to
if-lt     小于则跳转 <                  less than 举例：if-lt v0,v3,:cond_0 如果v0小于v3则跳转到cond_0,注意谁小于谁
if-ge   大于或等于则跳转 >=          greater than or equal to
if-gt    大于则跳转  >                greater than
if-le     小于或等于则跳转 <=          less than or equa to
if-eqz   等于0                                 equal to zero
if-nez   不等于0                              not equal to zero
goto    无条件跳转
switch   分支跳转
```
## ![](../img/github24.png)跳转依据-比较指令
比较指令用于对两个寄存器的值（浮点型或长整型）进行比较。
它的格式为“cmpkind vAA, vBB, vCC”，
其中vBB寄存器与vCC寄存器是需要比较的两个寄存器或寄存器对，比较的结果放到vAA寄存器。Dalvik指令集中共有5条比较指令：
```
“cmpl-float”：比较两个单精度浮点数。如果vBB寄存器大于vCC寄存器，结果为-1，相等则结果为0，小于的话结果为1

“cmpg-float”：比较两个单精度浮点数。如果vBB寄存器大于vCC寄存器，则结果为1，相等则结果为0，小于的话结果为-1

“cmpl-double”：比较两个双精度浮点数。如果vBB寄存器对大于vCC寄存器对，则结果为-1，相等则结果为0，小于则结果为1

“cmpg-double”：比较两个双精度浮点数。如果vBB寄存器对大于vCC寄存器对，则结果为1，相等则结果为0，小于的话，则结果为-1

“cmp-long”：比较两个长整型数。如果vBB寄存器大于vCC寄存器，则结果为1，相等则结果为0，小则结果为-1

cmp XXXX
JXXXX
```
>小贱提示：
>
>破解思路：1、删掉比较指令;2、删掉跳转或修改跳转标号



## ![](../img/github25.png)实例操作指令

与实例相关的操作包括实例的类型转换，检查及新建等：

####    “check-cast vAA, type@BBBB”：


将vAA寄存器中的对象引用转换成指定的类型，如果失败会抛出ClassCastException异常。如果类型B指定的是基本类型，对于非基本类型的A来说，运行时始终会失败。

####      “check-cast/jumbo vAAAA, type@BBBBBBBB”：

指令功能与“check-cast vAA, type@BBBB”相同，只是寄存器值与指令的索引取值范围更大（Android4.0中新增的指令）。


####       “instance-of vA, vB, type@CCCC”：

判断vB寄存器中的对象引用是否可以转换成指定的类型，如果可以vA寄存器赋值为1，否则vA寄存器赋值为0。

####       “instance-of/jumbo vAAAA, vBBBB, type@CCCCCCCC”：

指令功能与“instance-of vA, vB, type@CCCC”相同，只是寄存器值与指令的索引取值范围更大（Android4.0中新增的指令）。

####       “new-instance vAA, type@BBBB”：

构造一个指定类型对象的新实例，并将对象引用赋值给vAA寄存器，类型符type指定的类型不能是数组类。

####       “new-instance/jumbo vAAAA, type@BBBBBBBB”：

指令功能与“new-instance vAA, type@BBBB”相同，只是寄存器值与指令的索引取值范围更大（Android4.0中新增的指令）。


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
