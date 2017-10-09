---
layout: default
---
# ![](../img/hj.jpg)基础知识
>小贱提示： 这里只是说说方法，至于原理我也懒得说了，毕竟这是笔记嘛，笔记就是要简单方便查阅

## ![](../img/github1.png)Linux管道“|”
将一个进程的标准输出作为另一个进程的标准输入

举例：ls | grep test命令可用于列出当前目录下文件名包含test的文件
## ![](../img/github2.png)命令行参数
C语言的main函数拥有两个参数，

为int类型的argc参数，表示命令行参数的个数

以及char**类型argv参数，argv指向一个字符串数组，该数组存储了具体的命令行参数的内容。

注意程序本身的名字为命令行的第一个参数。

打印命令行参数信息的示例代码：
```
#include <stdio.h>
int main(int argc, char** argv)  //**表示二级指针，可以表示二维数组
{
    int i;
    for (i = 0; i < argc; ++i)
    {
        printf("argv[%d] = %s\n", i, argv[i]);
    }
    return 0;
}
```
Linux的xargs命令可以将输入数据当做命令行参数传给指定的程序。
比如执行命令python -c “print ‘AAA BBB CCC’” | xargs ./test
## ![](../img/github3.png)环境变量参数
缺省情况下, 当一个进程被创建时，除了创建过程中的明确更改外，它继承了其父进程的绝大部分环境变量信息。

扩展的C语言main函数可以传递三个参数，argc，argv，char**类型的envp。

envp指向一个字符串数组，存储了环境变量的内容，envp的最后一个元素指向NULL，作为envp结束的标识符。

打印环境变量参数信息的示例代码：
```
#include <stdio.h>
int main(int argc,char** argv,char** envp)
{
   int i =0;
   while(envp[i])
    {
        printf("envp[%2d] = %s\n", i, envp[i]);
        i +=1;
    }
   return0;
}
```
环境变量的格式为：环境变量名=环境变量值。
在Linux Shell下，通过export可以给Shell添加一个环境变量，此后通过Shell启动的子进程都会拥有这个环境变量。

例如在Shell中执行export testenv=”Hello_World”之后，再执行./test，可以看到新的环境变量已经被子进程继承了。
## ![](../img/github4.png)objdump使用
使用objdump工具可以查看一个目标文件的许多内部信息，objdump有许多可选的参数选项，通过控制这些参数选项可以输出不同的文件信息。

最常用的就是-d，用来获取二进制程序中代码段的反汇编指令列表，从而获取某一个函数的具体地址信息。
## ![](../img/github5.png)函数指针
函数指针只能指向具有特定特征的函数，因而所有被同一指针运用的函数必须具有相同的参数和返回类型。
通常使用typedef来定义一个函数指针类型，如：
```
typedef void(*func)();
```
定义了func这样的函数指针类型，其可以指向返回值类型为void且没有函数参数的函数，
比如void test()这样的函数，可以使用func myfp = test;来定义一个myfp变量，该变量指向test函数，通过执行myfp()可以达到执行test()函数同样的效果。
## ![](../img/github6.png)AT＆T与Inter汇编区别
操作方向相反

指令前缀

内存单元操作数

操作码后缀

跳转后缀（f,b）

注释符号不同

## ![](../img/github7.png)__builtin_return_address函数
__builtin_return_address函数接收一个参数，可以是0,1,2等。

__builtin_return_address(0)返回当前函数的返回地址，如果参数增大1，那么就往上走一层获取主调函数的返回地址。
## ![](../img/github8.png)理解多层跳转
retn指令从栈顶弹出一个数据并赋值给EIP寄存器，程序继续执行时就相当于跳转到这个地址去执行代码了。

如果我们将返回地址覆盖为一条retn指令的地址，那么就又可以执行一条retn指令了，相当于再在栈顶弹出一个数据赋值给EIP寄存器。
## ![](../img/github9.png)strdup函数
strdup可以用于复制一个字符串，我们通常使用字符串时会使用strcpy，

而strdup只接受一个参数，也就是要复制的字符串的地址，

strdup()会先用maolloc()配置与参数字符串相同大小的的空间，然后将参数字符串的内容复制到该内存地址，然后把该地址返回。

strdup返回的地址最后可以利用free()来释放。
## ![](../img/github10.png)grep命令
对匹配特定正则表达式的文本进行搜索，并只输出匹配的行或文本

*是正则表达式里面的通配符，如果要查找，需要使用反斜杠进行转移，即\\*。
举例：

如果我们要查找call %eax，在shell中执行objdump -d pwn| grep “call *%eax”即可。

## ![](../img/github11.png)checksec脚本
在编写漏洞利用代码的时候，需要特别注意目标进程是否开启了DEP（Linux下对应NX）、ASLR（Linux下对应PIE）等安全防护机制
例如

存在DEP（NX）的话就不能直接执行栈上的数据，
存在ASLR的话各个系统调用的地址就是随机化的。

使用checksec.sh脚本可以方便的查看可执行程序是否启用了这些安全机制。

例如：在shell中执行./checksec.h —file test
checksec脚本的下载地址为:
http://www.trapkit.de/tools/checksec.html

## ![](../img/github12.png)栈帧
在高级语言中，当函数被调用时，系统栈会为这个函数开辟一个栈帧，并把它压入到栈里面。新开辟的栈帧中的空间被它所属的函数所独占，当函数返回的时候，系统栈会清理该函数所对应的栈帧以回收栈上的内存空间。

每个函数都拥有自己独占的栈帧空间，有两个特殊的寄存器用于标识栈帧的相关参数：

    ESP寄存器，永远指向栈帧的顶端；
    EBP寄存器，永远指向栈帧的底部；

在调用一个函数的时候，函数所需要的参数首先会依次被压入的栈上，其次压入对应的返回地址，最后跳转到被调用的函数执行代码并开辟新的栈帧。新的栈帧的底部保存有EBP寄存器的值，基于EBP寄存器可以获取到被调用函数所需要的参数信息。
## ![](../img/github13.png)NX选项
NX即No-eXecute（不可执行）的意思，NX选项会将进程特殊区域的内存标记为不可执行，当CPU跳转到这些区域执行代码的时候便会产生异常，以阻止缓冲区溢出时直接在栈上执行恶意代码。

gcc编译器默认开启了NX选项，如果需要关闭NX选项，可以给gcc编译器添加-z execstack参数。在Windows下，类似的概念为DEP（Data Execution Prevention，数据执行保护），在最新版的Visual Studio中默认开启了DEP编译选项。
## ![](../img/github14.png)GOT和PLT
GOT（Global Offset Table，全局偏移表）是Linux ELF文件中用于定位全局变量和函数的一个表。

PLT（Procedure Linkage Table，过程链接表）是Linux ELF文件中用于延迟绑定的表，即函数第一次被调用的时候才进行绑定。
## ![](../img/github15.png)信息泄露的实现
在进行缓冲区溢出攻击的时候，如果我们将EIP跳转到write函数执行，并且在栈上安排和write相关的参数，就可以泄漏指定内存地址上的内容。
比如我们可以将某一个函数的GOT条目的地址传给write函数，就可以泄漏这个函数在进程空间中的真实地址。
如果泄漏一个系统调用的内存地址，结合libc.so.6文件，我们就可以推算出其他系统调用（比如system）的地址。

## ![](../img/github16.png)libc.so.6文件的作用
利用目标程序的漏洞来泄漏某一个函数的地址，可以计算出system函数的地址

当然，被泄露地址的函数必须也定义在libc.so.6中（libc.so.6中通常也存在有/bin/bash或者/bin/sh这个字符串）。

计算system函数地址的基本原理是：

在libc.so.6中，各个函数的相对地址是固定的，比如函数A相对于libc.so.6的起始地址为addr_A，函数B相对于libc.so.6的起始地址为addr_B，

那么，如果我们能够泄漏进程内存空间中函数A的地址address_A，那么函数B在进程空间中的地址就可以计算出来了，为address_A - addr_A + addr_B。

__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./pwn)
