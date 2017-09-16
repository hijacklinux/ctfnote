---
layout: default
---
# ![](../img/hj.jpg)PC逆向-java
>java反编译类似.NET,都是通过中间语言

## ![](../img/github12.png)工具
IDEA或者Eclipse，读者们自行网上下载吧
## ![](../img/github13.png)混淆
### 混淆分类
  - 典型：去除所有调试信息，如：变量表，行编号，机器生成名称的重命名包，类和方法
  - 先进混淆：更进一步，重构逻辑和插入不执行的伪代码来改变控制流程

#### 去除调试信息
```
去除调试信息后，sendMessage参数就是s1，s2而不再是host和message
```
#### 名称处理
```
包、类、方法名称变成没有意义的名称
如ChatServer.sendMessage混淆后为d.a
不同方法签名，由于多态的原因，可以起同一个方法名字
```
#### 将字符串经过编码
```
如转换成\0279这种十六进制
```
#### 改变控制流
#### 插入讹用的代码
```
允许程序将不正确的字节码引入到类文件中，所引入的代码不会影响源代码的执行，但反编译是就会引发
```
### 混淆产品
  - KLASSSMASTER
  - PROGUARD
  - RETROGUARD
  - DASH-O
  - JSHRINK
### 反混淆
>这方面还没深入研究，以后会补上




__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
