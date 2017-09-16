---
layout: default
---
# ![](../img/hj.jpg)IDA PRO中的小tips
>小贱提示： 就是一些快捷键啦，修改代码的插件啦，支持中文啦

## ![](../img/github4.png)修改代码保存插件keypatch.py
```
下载安装keystone
https://github.com/keystone-engine/keystone/releases/download/0.9.1/keystone-0.9.1-python-win32.msi
把keypatch放进plugins目录下
edit->plugins
选1或2或3项修改
选第4项保存
```
## ![](../img/github5.png)IDA Pro 快捷键

| 快捷键     | 作用                                                 |
| ---------- | ---------------------------------------------------- |
| 空格键     | 反汇编窗口切换文本跟图形，右键Group node可折叠       |
| ESC        | 退到上一个操作地址                                   |
| G          | 搜索地址或者符号                                     |
| N          | 重命名                                               |
| 分号键     | 注释（伪c/c++代码的情况下，注释是/）                 |
| ALT+M      | 添加标签                                             |
| CTRL+M     | 列出所有标签                                         |
| ctrl+s     | 看见系统所有的模块                                   |
| C          | 把机器码变成汇编                                     |
| P          | 在函数开始处使用P，从当前地址处解析成函数            |
| D          | data解析成数据                                       |
| A          | ASCII解析成ASCII                                     |
| U          | 取消把函数汇编变成机器码                             |
| X          | 交叉引用                                             |
| F5         | C伪代码                                              |
| ALT+T      | 搜索文本，ctrl+t查找下一个                           |
| ALT+B      | 搜索16进制 搜索opcode 如ELF文件头，ctrl+b:查找下一个 |
| CTRL+ALT+B | 打开断点列表                                         |
| F7         | 单步步入                                             |
| F8         | 单步不过                                             |
| CTRL+F7    | 运行到函数返回地址                                   |
| F4         | 运行到光标处                                         |

## ![](../img/github6.png)IDA Pro 支持中文

```
cfg目录中ida.cfg
把// (cp866 version) 到// (full version)之间的行用 '//'注释掉
把// (full version)  // the following characters are allowed ..之间的行前面的 '//'去掉,保存
```

__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
