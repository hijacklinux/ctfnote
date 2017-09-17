---
layout: default
---
# ![](../img/hj.jpg)smali概括知识


## ![](../img/github8.png)smali包信息
smali前三行：
```
.class public Lcom/aaaaa;     //.class <访问权限> [修饰关键字] <类名>
.super Lcom/bbbbb;            //.super 父类名
.source "ccccc.java"              //.source 源文件名
```
解释：
```
这是一个由ccccc.java编译得到的smali文件
他是com.aaaaa这个包下的一个类
继承自com.bbbbb这个类

程序中所有的类都会在相应的目录下生成独立的smali文件
```
## ![](../img/github9.png)v命名法和p命名法

假设有M个寄存器，N个参数

| v命名 | p命名 | 寄存器含义 |
| ----- | ----- | ---------- |
| v0    | v0    |   第一个局部变量         |
| v1    | v1    |    第二个局部变量        |
| v2    | v2    |     第三个局部变量       |
| ...   | ...   |     以此类推       |
| vM-N  |   p0    |   第一个参数         |
| ...   |   ...    |     以此类推       |
| vM-1      |   pN-1    |    第N个参数        |

由此可见，p命名法更容易让人区分局部变量与参数
## ![](../img/github10.png)native,非静态方法与静态方法

静态方法：不需要创建实例就能调用，修饰：static，对于静态方法p0是起始参数

非静态方法：需要创建实例才能调用，p0代表this,p1是起始参数

native意思是告诉虚拟机，这个方法是原生的，在so里面，so的名字举例：如果是jni,那么so的名字就是libjni.so
格式：public static native 类型 方法名(参数类型)

## ![](../img/github11.png)Dalvik数据类型

| 简写 | 全写 | 说明 |
| ----- | ----- | ---------- |
| V  | void       |        只能用于返回值类型，表示没有返回值，只能修饰函数|
| Z  | boolean    |      变量值只能是 真(true) 或 假(false)|
| B  | byte      |          字节型    与字符型区别是：字节型无符号，字符型有符号|
| S  | short      |         短整型|
| C  | char      |          字符型    长度是1个字节|
| I  | int      |              整数型|
| J  | long（64位） |   长整型  对于64位数据，用两个寄存器来存放|
| F  | float      |           浮点型 |
| D  | double(64位) |  双精度浮点型|
| L  | java类类型   |      package.name.ObjectName Lpackage/name/ObjectName; 出现频率非常高|
| [   | 数组类型 | [I表示一个整型一维数组，等于java中的int[]。注意多位数数组的维数最大为255个。|

>小贱提示：
>
>[[I表示int[][] ，同理，[[[I表示int[][][]
>
>有多少维就数多少个[


## ![](../img/github12.png)类、方法、字段表示方法

对象(或类)的表示以L作为开头，格式为L+包名（即类的完整路径）
```
如：LpackageName/objectName;
String对象就是：Ljava/lang/String;
```
方法（类+“;->”+方法名（参数签名）返回类型）：
```
Lpackage/name/ObjectName;->MethodName(参数签名)返回类型
```
字段：
```
Lpackage/name/ObjectName;->FieldName:类型
```
![](../img/hj.jpg)
>小贱提示：
>
>参数签名就是数据类型的集合，举个栗子：有三个参数，分别是整数型，整数型，布尔型，那么参数签名就是：IIZ。;->表示访问


举例：
```
new-instance v2, Ljava/io/OutputStreamWriter;
```
```
invoke-static {}, Lcom/debug;->getTime()Ljava/lang/String;
解释：
Lcom/debug：类
getTime()：方法名，没有参数，所以括号是空的
Ljava/lang/String：返回值类型，是一个字符串
```
```
Landroid/util/Log;->d(Ljava/lang/String;Ljava/lang/String;)I
解释：
Landroid/util/Log：类
d（）：方法名
Ljava/lang/String;Ljava/lang/String;：参数签名，两个都是字符串类型
I：返回值类型，int整数型
```
## ![](../img/github13.png)smali中的声明
一般来说在Smali文件中声明是这个样子的：
```
# annotations  这是注解的意思
.annotation system Ldalvik/annotation/MemberClasses;
value = {
Lcom/aaa$qqq;,
Lcom/aaa$www;
}
.end annotation
```
>小贱提示：
>
>这个声明是内部类的声明：aaa这个类它有两个成员内部类——qqq和www（$表示子类）

## ![](../img/github14.png)Dlavik语法约定
```
1、如果参数采用vX方式表示，如v0，v1表明它是个寄存器
2、如果参数采用#+X方式表示，表明是个常量数字
3、如果采用+X方式，表明是个相对指令的地址偏移
4、如果参数采用kind@X方式表示，表明它是个常量池索引值
5、synthetic this$X表示它是被编译器合成的，虚构的，作者并没有声明过该字段
其中：
kind：表示常量值类型，如string，type，field，meth
举例：op vAA，string@BBBB
指令用到一个vAA寄存器，且附加了一个字符串常量池索引string@BBBB，其实这条指令就代表const-string
```




__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
