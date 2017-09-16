---
layout: default
---
# ![](../img/hj.jpg).NET结构
>小贱提示：想要了解更多PE结构，请转到操作系统模块查看pe结构，用CFF查看结构更清晰

## ![](../img/github24.png)程序头
主要包括编译后生成可执行文件的一些属性定义，一般三个关键字：

| 关键字           | 意义                         |
| ---------------- | ---------------------------- |
| .assembly        | 声明本程序集名称             |
| .assembly extern | 声明外部（被引用）程序集名称 |
| .module          | 声明主模块名称               |

## ![](../img/github25.png)各类声明

| 关键字     | 意义             |
| ---------- | ---------------- |
| .namespace | 名称空间声明     |
| .class     | 类声明           |
| .method    | 方法声明         |
| .field     | 字段声明         |
| .data      | 数据（常量）声明 |
|     .custom        |        自定义属性声明          |

## ![](../img/github26.png).text节结构
>小贱提示：如果用dnspy查看的话，看到的Section其实是section_table

#### 输入表
#### CLR头
结构叫做Image_cor20_Header，dnspy都已经分析好了

| 名称                          | 说明                                              |
| ----------------------------- | ------------------------------------------------- |
| cb                            | CLR头的大小，以byte为单位                         |
| MajorRuntimeVersion           | 主版本号                                          |
| MinorRuntimeVersion           | 副版本号                                          |
| MetaData                      | 元数据的RVA和Size(最重要)                         |
| Flags                         | 属性字段                                          |
| EntryPointToken/EntryPointRVA | 入口方法的元数据ID                                |
| Resources                     | 托管资源的RVA和Size                               |
| StrongNameSignature           | 强名称数据的RVA和Size                             |
| CodeManagerTable              | CodeManagerTable的RVA和Size                       |
| VTableFixups                  | v-table项的RVA和Size                              |
| ExportAddressTableJumps       | 用于C++的输出跳转地址表的RVA和Size，多数情况下为0 |
| ManagedNativeHeader           | 仅在由ngen生成本地模块中该项不为0,其他情况均为0   |

#### MSIL代码、异常处理表（可选）
#### 强名称的hash数据
#### 元数据
![](../img/hj.jpg)
>小贱提示：
>
>元数据即描述数据的数据
>
>1、元数据本身也是数据
>
>2、它的功能是描述其他数据
>
>元数据描述了一个模块声明或引用的所有项
>
>元数据是以流的形式保存在文件中，保存元数据的流被分为两类:堆（heap）和表（table）

元数据头：
dnspy中Storage Signature和Storage Header

元数据流：
说明了六个堆的性质（ioffset，isize，rcname）

这六个堆分别是：

| 堆  | 说明                                                                   |
| --- | ---------------------------------------------------------------------- |
| #~  | 最重要的堆，几乎所有元数据信息都以表的形式存储于此，因此又叫元数据表流 |
|  #String   |      包含各种元数据名称（如类名，方法名，成员名，参数名），首部有一个0，以0结尾                                                                  |
|  #Blob   |     二进制数据堆，存储程序的非字符串信息，如常量值，方法的签名，强名称等                                                                   |
|  #GUID   |   存储有全局唯一标识                                                                     |
|   #US  |    IL代码中使用的用户字符串，如ldstr调用的字符串                                                                    |
|  #-   |         不常见                                                               |

#### 托管资源数据（可选）
#### 非托管资源数据（可选）
#### 运行时启动信息





__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
