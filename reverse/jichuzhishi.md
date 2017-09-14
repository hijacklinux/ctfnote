---
layout: default
---

# ![](../img/hj.jpg)逆向工程-基础知识
## ![](../img/github14.png)调用约定
### cdecl(C调用约定)
```
X86体系许多C编译器使用的默认调用约定
函数的参数从右向左入栈
由调用方负责平栈
如：
push y
push x
call 00401000
add esp,8

在windef.h中：
#define WINAPIV     __cdecl
```
### _stdcall(标准调用约定)
```
小贱提示：其实并不“标准”，这个只是微软为自己的调用约定所起的名称（臭不要脸的）
微软对所有DLL文件输出的参数数量固定的函数使用stdcall约定
函数的参数从右向左入栈
由被调用函数负责平栈
如：
push y
push x
call 00401000

00401000:
......
ret 8

在windef.h中：
 #define CALLBACK    __stdcall
 #define WINAPI      __stdcall
 ```
### x86 fastcall约定
```
是stdcall约定的一个变体
将前两个参数分别放到ECX和EDX中，其余参数从右向左入栈
被调用函数负责平栈
举例：
void fastcall demo(int w,int x,int y,int z)
demo(1,2,3,4):

push 4
push 3
mov edx,2
mov ecx,1
call demo

demo中：
......
ret 8
```
### thiscall(C++调用约定)
```
C++类中的非静态成员函数需要使用this指针，该指针指向用于调用函数的对象
用于调用函数的对象的地址必须由调用方提供，在调用非静态成员函数时，this作为参数提供
不同的编译器this指针的技巧不同
Microsoft VC++：
将this指针放到ecx中，要求被调用的非静态成员函数内部平栈
GNU g++:
在调用函数之前，this被放到栈顶，且结束后由调用方平栈
```
### register-Delphi默认模式
```
参数传递模式：前三个数据.eax,edx,ecx，超过三个参数部分.放在堆栈传递
其他和stdcall一样，函数自己恢复堆栈
```
### 唯一从左向右push参数的约定：Pascal
```
小贱tips：此约定正好与cdecl(C调用约定)完全相反
参数由左向右push
由函数内部平栈
```
### 函数调用步骤
  1. #### 参数入栈
      ```
      从右向左依次push参数
      push 参数3
      push 参数2
      push 参数1
      ```
  2. #### 返回地址入栈
      将当前调用指令的下一条指令压栈，供函数返回时继续执行
  3. #### 代码区跳转
      跳转到被调用函数的入口处
  4. #### 栈帧调整
      ```
      push		ebp					//	保存旧栈帧的底部
      mov		ebp,esp 			//设置新栈帧的底部	(栈帧切换）
      sub		esp,xxx				//设置新栈帧顶部（抬高栈顶，为新栈帧开辟空间）
      ```
### 函数返回步骤
  1. ##### EAX保存返回值
      通常将函数的返回地址保存在EAX中
  2. #### 通常将函数的返回地址保存在EAX中
      ```
      举例：
      add esp,xxx 	//降低栈顶，回收当前栈帧
      pop ebp		//将上一个栈帧底部位置恢复到ebp
      ```
  3. #### retn跳出
      ```
      retn有两个功能：
      弹出当前栈顶元素，即弹出栈帧中的返回地址
      跳到这个返回地址
      ```



## ![](../img/github15.png)函数内部的“序言”与“尾声”
### 序言
```
序言：
push ebp
mov ebp,esp
sub esp,8
```
### 尾声
```
mov esp,ebp
pop ebp
ret
或：
leave
ret
```



## ![](../img/github16.png)寻址模式
### 寄存器寻址
```
mov ebx, edx
add al, ch
```
### 立即数寻址
```
mov eax, 1234h
mov dx, 301
```
### 直接寻址
```
mov bh, 100
mov[4321h],bh
```
### 寄存器间接寻址
```
mov [di],ecx
```
### 基址相对寻址
```
mov edx,20[ebx]
或
mov edx,[ebx+20]
```
### 索引相对寻址
```
类似于基址相对寻址，用来保存偏移量的是 edi 和 esi
mov ecx,20[esi]
或
mov ecx,[esi+20]
```
### 基址索引相对寻址
```
有效地址， 是通过将基址寄存器和索引寄存器加起来计算得到
mov ax, [bx][si]+ 1
```
## ![](../img/github17.png)汇编语言源文件结构
### .model
```
.model 指令用来标明.data 和.text 节的大小。
```
### .stack
```
.stack 指令标记了栈段的开始，用来标明栈的大小（按字节计算） 。
```
### .data
```
.data 指令标记了数据段的开始，用于定义变量，包括初始化和未初始化的变量。
```
### .text
```
.text 指令标记代码段，用来保存程序的命令。
```
__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
