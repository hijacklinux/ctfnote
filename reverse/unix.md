---
layout: default
---
# ![](../img/hj.jpg)逆向工程-Unix
## Unix
### 常用命令

| 命令            | 描述                                                               |
|:-------------|:------------------|
| b 函数名        | 在指定的函数处设置断点                                             |
| b *mem          | 根据指定的绝对内存地址，设置一个断点                               |
| info b          | 显示有关断点的信息                                                 |
| delete b        | 删除一个断点                                                       |
| run <args>      | 用给定的参数，在 gdb 内部启动调试程序                              |
| info reg        | 显示当前寄存器状态信息                                             |
| stepi 或 si     | 执行一条机器指令                                                   |
| next 或 n       | 执行下一个函数                                                     |
| bt              | 回溯命令，给出当前调用栈信息                                       |
| up/down         | 在调用栈中向上或向下移动                                           |
| print var       | 输出变量 var 的值                                                  |
| print /x $<reg> | 输出指定寄存器的值                                                 |
| x /NT A         | 检查内存，其中 N 是需要显示的数据单元的数目，而 T 是数据单元的类型 |
| x               | 表示十六进制，d 表示十进制，c 表示字符，s 表示字符串，i 表示指令），A 是绝对地址值，或符号名如“main” |
|    quit             |                          退出 gdb                                          |


### 用gdb反汇编
```
要用 gdb 反汇编，可使用下述两个命令：
set disassembly-flavor <intel/att>
disassemble <function name>
第一个命令在 Intel （NASM） 和 AT&T 格式之间进行切换。 默认情况下， gdb 使用 AT&T格式。
第二个命令反汇编给定的函数（可以指定 main()） 。
```
__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
