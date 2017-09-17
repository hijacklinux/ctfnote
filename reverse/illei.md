---
layout: default
---
# ![](../img/hj.jpg).NET类与值类型操作指令


## ![](../img/github1.png)ldnull
读取空的对象引用并入栈
## ![](../img/github2.png)ldobj
ldobj [token]：从堆栈获取值类型实例的托管指针（通过ldarga,ldloca,ldflda或ldsflda取得该指针），然后读取[token]所标示的值类型的实例，并入栈。

## ![](../img/github3.png)ldstr
ldstr [token]:读入一个字符串，建立[mscorlib]System.String实例，并入栈
>小贱提示：
>
>在IL中，[token]可以用字符串表示，也可以用字节队列表示
>
>如：ldstr "hello"+"world"
>
>ldstr bytearray(A1 00 A2 00 A3 00 A4 00 A5 00 A6 00)

## ![](../img/github4.png)stobj
stobj [token]:依次从堆栈中获取值类型的值，以及值类型实例的指针，然后将该值存储在指针所指的实例中

## ![](../img/github5.png)cpobj
cpobj [token]:
从栈中获取源和目标实例的指针，并将值类型的值从源复制到目标，源和目标都必须符合[token]所标示的值类型

## ![](../img/github6.png)newobj
newobj [token]:分配内存，创建某个类（不是值类型）的新实例，调用[token]所标示 的实例构造方法，并将对象引用入栈。若该构造方法需要参数，则从堆栈中取得这些参数。
>小贱提示：
>
>该指令可用于类和队列，例如：
>
>newobj isntance void [mscorlib]System.Object::.ctor( )
>
>newobj isntance void int32[0...,0...]::.ctor(int32,int32)


## ![](../img/github7.png)initobj
initobj [token]：从堆栈中取得一个值类型实例的托管指针，并对该实例进行初始化，默认将值类型的所有字段清零

## ![](../img/github8.png)castclass
castclass [token]:从栈中取初始类实例的对象引用，将其转换为[token]所标示的类，然后将类实例的对象引用入栈



## ![](../img/github9.png)isinst
isinst [token]:
从栈中取得对象引用，检查其是否是[token]所标示的类的实例，并将结果入栈。
```
如果检测成功，结果为对象引用
如果检测失败，结果为null指针
下列三种情况，可以得到成功结果：
1）如果栈中的对象就是(或继承自)<token>所标示的对象
2）如果<token>标示接口，且栈中对象实现了该接口
3）如果<token>标示一个值类型，而栈中对象引用是经过装箱（boxed）的该值类型的实例
```
## ![](../img/github10.png)box
box [token]：装箱指令，该指令从栈中获取值类型的实例，将该值类型装箱为类，并创建新的对象实例，将对象引用入栈

## ![](../img/github11.png)cpobj


## ![](../img/github12.png)unbox
unbox [token]:装箱的逆操作，从栈中取得对象引用，入栈的结果是值类型实例的托管指针
```
unbox.any [token]:
执行顺序与unbox指令相同，
当用于装箱后的值类型时，其功能等同于unbox
当用于对象引用时，其功能等同于castclass [token]
```
## ![](../img/github13.png)mkrefany
mkrefany [token]:从栈顶取得一个指针，将其转换为TypedReference，并入栈


## ![](../img/github14.png)refanytype
refanytype [token]:从栈中取TypedReference，从中得到类型信息，并将类型的句柄入栈
## ![](../img/github15.png)refanyval
refanyval [token]：从栈中取TypedReference，从中得到实例指针（或native int）,并将该指针入栈
## ![](../img/github16.png)ldtoken
ldtoken [token]:将[token]所标示的项目转换为内部句柄，并入栈。
```
这种句柄可供[mscorlib]System.Reflection的方法使用，转换后的句柄将是下列三种之一：
System.RuntimeMethodHandle
System.RuntimeTypeHandle
System.RuntimeFieldHandle
例如：
ldtoken [mscorlib]System.String
ldtoken method instance void [mscorlib]System.Object::.ctor( )
ldtoken field int32 Foo.Bar::ff
```

## ![](../img/github17.png)sizeof
sizeof [token]:读取值类型所占的字节大小并入栈。
>小贱提示：
>
>用于引用类型时，仅返回指针大小（4或8）

## ![](../img/github18.png)throw
从栈中取引用对象，并将之作为异常抛出

rethrow 重新抛出捕捉到的异常







__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
