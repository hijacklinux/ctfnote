---
layout: default
---
# ![](../img/hj.jpg)找断点的几种方法
>小贱提示： 这篇文章我打算先把篇幅短的找断点方法放在这里
>
>篇幅长的方法我会另外起一个页面做针对性说明，文章结尾会有相应链接

## ![](../img/github1.png)Ctrl+N找函数name
```
或者右键：查找>所有模块间的调用
或者右键：查找参考
```
## ![](../img/github2.png)F12暂停大法
```
f12暂停，看上面的k，alt+k来查看当前调用的函数
或者使用f12 回溯法：
弹框，f12，alt f9，返回程序点确定，返回od，ctrl f9，f8，ctrl f9 ，f8 到达;
（当然具体几次Ctrl F9,F8取决与你断的函数深入到多少层）
```
## ![](../img/github3.png)内存断点或硬件断点
```
内存断点假死的话：先硬件执行，再内存断点，鼠标放到程序，断下 f8
```
## ![](../img/github4.png)借助其他反汇编工具找断点
```
例如借助dede就可以很容易找到delphi的断点
```
## ![](../img/github6.png)按钮事件

| 针对语言 | 找按钮事件的方法                              |
| -------- | --------------------------------------------- |
| vb       | OD中vb按钮事件的断点脚本                      |
| delphi   | OD中delphi按钮事件的断点脚本                  |
| 易语言   | FF 55 FC 5F 5E 或用 e-debug或去 FF 25易语言体 |
| VC++     | sub eax,0a                                    |
| MFC      | sub eax,0a                                    |

## ![](../img/github5.png)万能断点
```
F3A58BC883E103F3A4E8或断点插件
用的时候再下
啥都断，用的时候再用
```
![](../img/hj.jpg)
> 小贱提示：下面的方法由于篇幅较长，另做其他页面表述，点击链接即可

## ![](../img/github7.png)[常用API法](apibp)
## ![](../img/github8.png)[字符串法](stringbp)
## ![](../img/github9.png)[消息及条件断点](messagebp)
## ![](../img/github10.png)[特征码法](tezhengmabp)
__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
