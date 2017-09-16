---
layout: default
---
# ![](../img/hj.jpg).NET参数，局部变量，字段寻址指令


## ![](../img/github8.png)arglist
参数列表,
取得参数列表的句柄，并将句柄入栈

## ![](../img/github9.png)ldarg：载入参数

|   指令  | 说明    |
| --- | --- |
| ldarg [unsigned init16]| 载入第[unsigned init16]个参数并入栈|
| ldarg.s| ldarg的短指令|
| ldarg.0| 载入第0个参数并入栈|
| ldarg.1| 载入第1个参数并入栈|
| ldarg.2| 载入第2个参数并入栈|
| ldarg.3| 载入第3个参数并入栈|

## ![](../img/github10.png)ldarga:载入参数的地址

| 指令 | 说明 |
| ---- | ---- |
| ldarga [unsigned init16]   |   载入第[unsigned init16]个参数的地址   并入栈   |
|   ldarga.s [unsigned init8]   |   ldarga的短格式   |


## ![](../img/github11.png)ldloc:载入局部变量

| 指令 | 说明 |
| ---- | ---- |
| ldloc [unsigned init16]| 载入第[unsigned init16]个局部变量并入栈，序号从0开始|
| ldloc.s [unsigned init8]| ldloc的短指令格式|
| ldloc.0| 载入第0个局部变量并入栈|
| ldloc.1| 载入第1个局部变量并入栈|
| ldloc.2| 载入第2个局部变量并入栈|
| ldloc.3| 载入第3个局部变量并入栈|


## ![](../img/github12.png)ldloca:载入局部变量的引用

| 指令 | 说明 |
| ---- | ---- |
|  ldloca [unsigned init16]   |  载入第[unsigned init16]个局部变量的引用（托管指针）并入栈    |
|  ldloca.s [unsigned init8]    |    短指令格式  |


## ![](../img/github13.png)ldfld

| 指令 | 说明 |
| ---- | ---- |
| ldfld [token]| 从栈顶取实例指针，然后按[token]取字段并入栈|
| ldsfld [token]| 载入静态字段并入栈|
| ldflda [token]| 从栈顶取实例指针，然后按[token]取字段的托管指针，并入栈|
| ldsflda [token]| 载入静态字段的托管指针并入栈|



## ![](../img/github14.png)starg:存储参数

| 指令                    | 说明                                        |
| ----------------------- | ------------------------------------------- |
| starg [unsigned init16]| 从栈顶取值并存入到第[unsigned init16]个参数 |
|      starg.s     [unsigned init8]           |        starg的短格式                                     |

## ![](../img/github15.png)stloc:存储局部变量

| 指令                    | 说明                                          |
| ----------------------- | --------------------------------------------- |
| stloc [unsigned init16] | 从栈顶取值并存入第[unsigned init16]个局部变量 |
|       stloc.s    [unsigned init8]              |                             短指令格式                  |


## ![](../img/github16.png)stfld

| 指令 | 说明 |
| ---- | ---- |
|   stfld [token]   |   从栈中取出待存储数据和实例指针，并将该数据存储到[token]对应的字段中   |
|   stsfld [token]   |  存储数据到静态字段中    |

## ![](../img/github17.png)localloc
分配一块局部内存，该指令从栈顶取块的大小，并将分配的内存块的托管指针入栈
## ![](../img/github18.png)unaligned
前缀指令，用于从栈顶取指针的指令之前，表示指针性质

unaligned.[unsigned init8] ：
表明栈顶的指针数据大小为[unsigned init8] 字节
## ![](../img/github19.png)volatile

表明栈顶指针所指的内容可能被别的线程改变





__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
