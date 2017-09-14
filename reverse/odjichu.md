---
layout: default
---
# ![](../img/hj.jpg)逆向工程-PC逆向-很基础的东西
>小贱提示： 如果没忘的话，这篇文章没必要看
## ![](../img/github21.png)OD 快捷键

| 快捷键   | 功能                                                                      |
| ------ | ------------------------------------------------------------------------- |
| f3     | 载入程序                                                                  |
| f8     | 跑，不进call                                                              |
| ctr+f8 | 自动跑                                                                    |
| f2     | 设断点                                                                    |
| ctr+f2 | 重新载入                                                                  |
| f9     | 运行                                                                      |
| f7     | 跑，进入call，进去之后f8跑，进入之后若要返回被调用之前，用ctr+f9走ret回去 |
| alt+b  | 管理断点，也可换成别的字母，工具栏的字母                                  |
| ctr+g  | 搜索地址或随表达式                                                        |
| ctr+f9 | 执行到返回                                                                |
| ctr+n  | 输入getwindowtexta或者右键查找参考，之后右键在所有下断点                  |
| ctr+a  | 分析代码                                                                  |
|      shift+x  |                复制二进制指令：（复制od指令）                                                           |

## ![](../img/github22.png)以下为杂乱的非常基础的知识
```
bpx GetDlgItemTextA     在所有call这个api处下断点
xchg eax，ecx 交换二者的值
大端高地址低位     小端高地址高位(常见）
常见的小端举例：
fishc为0066h,0069h,0073h,0068h,0063h
存储为(高位高地址，word型，unicode就是双字节型，即word）
66h     00h  |  69h     00h  |   73h    00h  |
1000  1001    1002  1003     1004  1005  //这行是地址

eax:00401000
mov ebx,dword ptr [eax]
方括号表示取内存上地址为401000的值，类型为dword四字节

byte:字节
word:字，双字节
dword:双字，四字节

call的方式:
call 404000h方式
call eax
call dword ptr [eax]
call dword ptr [eax+5]
call dword ptr [<&API>]
所有函数返回值都是保存在eax文件中

过程入口点：55  push ebp
一不小心进入call了想回头看看哪个call触发的按'-'类似回退（实际已执行，只是看看），'+'类似前进

Delphi破解也比较特殊：
首先国际惯例搜字符串注册成功
Delphi的call特别多，尽量f8过
双击之后看上面有没有retn，Delphi特色就是retn上面有个push 地址xxxx，retn就返回到这个地址xxxx，也就是说用push xxxx和retn配合达到jmp xxxx的效果

除lea外，中括号整体表示某地址存的值
mov eax dword ptr [400125]表示400125这个地址存的只以双字型给eax


od：api函数搜索或字符串搜索
下断点，进入函数记得看栈，ctr+f9

从指定地址执行：右键此处为新eip
可模拟nop

辨别vb是不是p-code：
1、入口点下方是不是有大片字节码
2、有没有MethCallEngine
pushad是把eax，ecx，edx。。。edi依次压入栈中
popad相反 ，取出依次放入edi。。。edx，ecx，eax中

KillTimer用于实现switch  case
找到表达式后f2下个断点，
任何函数的返回值都是存在eax里面的
保存，选中修改的东西，右键复制到可执行文件，再备份，保存数据到文件，生成个新的exe

模态对话框：即必须完成对话框，否则无法切换
DialogBoxParam函数
非模态对话框：可随意切换
CreateDialogParam函数

test eax, eax 如果eax的值为0，则Z标志位置为1
xor ebx,ebx 表示ebx清零
强制跳修改为jmp,不跳修改为nop
cmp 相等z为1，不等z为0
程序领空回到用户领空用alt+f9
想跳出死循环，f12暂停，在循环的下一条指令f2,然后f9即可

mov dest src扩展:
    movs/movsb/movsw/movsd edi esi
按串/byte字节/字word/双字dword  将esi复制到edi中

eip存放下一条指令的地址

查语言工具：die64
```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
