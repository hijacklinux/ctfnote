---
layout: default
---
# ![](../img/hj.jpg)逆向工程-PC逆向-消息循环


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




__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
