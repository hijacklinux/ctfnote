---
layout: default
---
# ![](../img/hj.jpg)PC逆向-消息循环


## ![](../img/github14.png)WndProc

窗口函数被称为WndProc，一旦有消息产生，就会调用该函数

有4个参数：

| 参数           | 意义                   |
| -------------- | ---------------------- |
| hWnd           | 窗口句柄               |
| message        | 参数定义消息的类型     |
| wParam和lParam | 参数包含消息的附加信息 |

# ![](../img/hj.jpg)
>小贱提示：
>
>如果想要为原程序增加功能，如增加菜单，按钮等功能，就必须在消息循环WndProc里插入相应的代码

## ![](../img/github15.png)定位消息循环(WndProc)处理代码的方法

#### 用OD

【w】里面主窗口的ClsProc就是，里面有很多判断，包括菜单的

#### 利用RegisterClassA(W/ExA/ExW)函数
```
RegisterClassA/RegisterClassW/RegisterClassExA/RegisterClassExW下断点
原理：程序创建窗口前，必须先调用RegisterClass，注册个窗口类
RegisterClass的参数是一个指向WNDCLASS结构的指针，
这个结构的第二个成员lpfnWndProc指向WndProc

RegisterClass-->断下，看参数pWndClass-->数据跟随这个参数结构的地址-->找第二个成员(第5-8个字节),这个双字地址指向的就是WndProc
```

#### 利用SendMessageA(W)函数

```
单击菜单或按钮时，会调用SendMessageA(W)函数发送一个WM_COMMAND给程序，
该消息的wParam参数就是按钮或菜单的ID号，中断后，跟踪程序的处理过程就可发现WndProc处
```

#### 利用菜单或按钮弹出的对话框
```
单击菜单或按钮时会弹出对话框，如果设断拦截，也能找到WndProc
```
# ![](../img/hj.jpg)
>小贱提示：
>
>举例：MessageBoxA下断-->单击按钮，断下-->跟踪找WndProc



#### 通过编程利用GetWindowLongA(W)
```
通过编程获取本进程内窗口的窗口过程，用GetWindowLong实现很简单，直接调用：
lpfnWndProc=(WNDPROC)GetWindowLong(hWnd,GWL_WINDPROC)
（lpfnWndProc指向WndProc）
```

## ![](../img/github16.png)修改WndProc扩充功能

#### 方法1（推荐）
第一步
```
1、用资源工具为pediy.exe增加Open，Save等菜单，
记录各菜单ID，注意ID不能重复
具体：资源工具点击菜单，仿照已有菜单添加代码
2、找到事件代码
假设各ID：Exit：40005（9C45h）;Open：40002(9C42h)；Save：40003(9C43h);Help：40019(9C53h)
单击菜单会发送一个WM_COMMAND消息给程序，WM_COMMAND消息的wParam参数就是菜单的ID值，程序判断消息的地方就是在窗口函数（WndProc）里
WndProc判断菜单ID的代码如下：
004011FA  mov dword ptr [ebp-8],edx        //[ebp-8]是菜单的ID
004011FD  cmp dword ptr [ebp-8],95c3       //判断是不是“关于”菜单的ID
00401204   je     00401208
00401206   jmp  00401222                           //改的时候改这里，跳到空白区域00401250，diy结尾再写上这条
现在就开始动手添加代码：
00401206    jmp    00401250
......
00401250    cmp dword ptr [ebp-08],00009C45      //Exit菜单
00401257    je ????????                                         //Exit事件处理代码
00401259      cmp dword ptr [ebp-08],00009C42    //Open菜单
00401260      je ????????                                       //Open事件处理代码
00401262     cmp dword ptr [ebp-08],00009C43     //Save菜单
00401269      je ????????                                       //Save事件处理代码
0040126F     jmp 00401222                                   //返回系统默认处理过程
```
第二步：
```
1、创建DLL文件
创建的DLL输出两个函数：MenuOpen和MenuSave
2、调用DLL文件（用增加输入函数的方法）

在wndProc中运用增加输入函数的方法修改exe
```
#### 方法2
第一步和上面的第一步相同

第二步：扩充Exit菜单功能
```
可调用PostQuitMessage(0)函数退出，即WM_DESTROY消息事件代码
程序中原有退出代码如下（意味着可以直接调用）：
00401224    push 0                                //ExitCode=0
00401226    call dword ptr [402038]        //PostQuitMessage
这样就可以在上一步的????????改成00401224了，即：
00401250     jmp dword ptr [ebp-08],9C45     //Exit菜单
00401257     je   00401224                             //调用PostQuitMessage
```
## ![](../img/hj.jpg)
>二次开发的常规流程总结：
>
>1、找个空白处
>
>2、做个dll
>
>3、exe中添加个导入函数
>
>4、exe中添加个菜单选项
>
>5、进入WndProc中hook这个函数

__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
