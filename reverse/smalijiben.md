---
layout: default
---
# ![](../img/hj.jpg)smali基本语法

## ![](../img/github15.png)指令特点
#### 特点1：
参数方向和汇编一样：mov eax（目标）,ebx（源）

#### 特点2：
根据字节码的大小与类型不同，一些字节码添加了名称后缀以消除岐义。

32位常规类型的字节码无任何后缀。

64位常规类型的字节码添加 -wide后缀。

特殊类型的字节码根据具体类型添加后缀。它们可以是：
-boolean，-byte，-char，-short，-int，-long，-float，-double，-object，-string，-class，-void之一。

#### 特点3：
根据字节码的布局与选项不同，一些字节码添加了字节码后缀以消除岐义。这些后缀通过在字节码主名称后添加斜杠“/”来分隔开。

#### 特点4：
在指令集的描述中，宽度值中每个字母表示宽度为4位。

## ![](../img/github16.png)成员变量
#### 格式
成员变量格式是：
.field public/private [static] [final] varName:<类型>。

对于不同的成员变量有不同的指令：

| 获取指令有：  | 操作指令有：  |
| ------------- | ------------- |
| iget          | iput          |
| sget          | sput          |
| iget-boolean  | iput-boolean  |
| sget-boolean  | sput-boolean  |
| iget-object   | iput-object   |
| sget-object等 | sput-object等 |

>小贱提示：
>
>没有"-object"后缀的表示成员变量的对象是基本数据类型
>
>有"-object"表示操作的成员变量是对象类型
>
>boolean表示对象是布尔类型

#### sget-object
```
sget-object v0,Lcom/aaa;->ID:Ljava/lang/String;

sget-object：用来获取变量值并保存到寄存器中
本例解释：获取ID这个String类型的成员变量并放到v0寄存器中
注意（重点）：前面需要该变量所属类的类型，后面加一个冒号和该成员变量的类型
```
#### iget-object
```
iget-object v0,p0,Lcom/aaa;->view;Locm/aaa/view;

可以看到iget-object比sget-object多了一个参数，就是该变量所在类的实例，在这里就是p0即“this"
获取array的话用aget-object
```
#### put用法与上面get相同
```
其实与iget的区别就是赋值方向相反
const/4 v3,0x0
sput-object v3,Lcom/aaa;->timer:Lcom/aaa/timer;

相当于this.timer=null
注意：之所以是null，是因为这里是赋值object

例二：
.local v0,args:Landroid/os/Message;
const/4 v1,0x12
iput v1,v0,Landroid/os/Message;->what:I
解释：相当于args.what = 18;(args是Message的实例)
```
## ![](../img/github17.png)函数调用
#### direct method和virtual method
```
direct method：就是private函数
virtual method：public和protected函数
```
#### invoke-static
```
调用static函数
举例1：
invoke-static {},Lcom/aaa;->CheckSignature()Z
这里的大括号{}其实是调用该方法的实例+参数列表，由于这个方法既不需要参数，也是static的所以{}为空

举例2：
const-string v0,"NDKLIB"
invoke-static{v0},Ljava/lange/System;->loadLibrary(Ljava/lang/String;)V
这个是调用static void System.loadLibrary(String)来加载NDK编译的so库用的方法，这里v0就是参数“NDKLIB”
```
#### invoke-super
```
调用父类方法
一般用于onCreate、onDestroy等方法
```
#### invoke-direct
```
调用private函数
invoke-direct {p0},Landroid/app/TabActivity;-><init>()V
这里init()就是定义在TabActivity中的private函数
```
#### invoke-virtual
```
调用protected或public函数
sget-object v0,Lcom/dddd;->bbb:Lcom/ccc;
invoke-virtual {v0,v1},Lcom/ccc;->Messages(Ljava/lang/Object;)V

v0是bbb:Lcom/ccc
v1是传递给Messages方法的Ljava/lang/Object参数
```
#### invoke-xxx/range
```
当方法的参数大于等于5个时，加上/range表示范围
invoke-direct/range {v0 .. v5},Lcmb/pb/ui/PBContainerActivity;->h(ILjava/lang/CharSequence;Ljava/lang/String;Landroid/content/Intent;I)Z
需要传递v0到v5共6个参数，这时候大括号内参数采用省略形式，且需要连续
```
#### 函数返回的结果的操作
```
非void类型函数调用后需用move-result（返回基本数据类型）或move-result-object(返回对象)指令
举例：
const-string v0,"Eric"
invoke-static {v0},Lcmb/pbi;->t(Ljava/lang/String)Ljava/lang/String;
move-result-object v2
v2保存的就是调用t方法返回的String字符串
```
## ![](../img/github18.png)举例分析
#### if函数分析
```
.method private ifRegistered()Z
.locals 2                                       //在这个函数中本地寄存器的个数为2个
.prologue                                    //标识方法开始
const/4 v0,0x1
.local v0,temFlage：Z
if-eqz v0：cond_0  //如果v0等于0则跳到cond_0；如果不等于0，继续执行至return
const/4 v1,0x1
：goto_0                                       //这就是个标签而已
return v1                                       //返回v1的值
：cond_0                                       //这也是个标签
const/4 v1,0x0
goto ：goto_0        //跳到goto_0执行，即返回v1的值，这里直接改成return v1也行
.end method
```
#### for函数分析
```
const/4 v0,0x0
.local v0,i:I                                                                                  //本地寄存器int i=v0
:goto_0
if-lt v0,v3,:cond_0
return-void
:cond_0
iget-object v1,p0,Lcom/aaa/MainActivity;->listStrings:Ljava/util/List;   //引用对象
const-string v2,"Eric"
invoke-interface {v1,v2},Ljava/util/List;->add(Ljava/lang/Object;)Z       //List是接口，执行接口方法add
add-int/lit8 v0,v0,0x1                                                                      //v0=v0+1
goto：goto_0
```
#### packed-switch连续
```
packed-switch的case是连续的0，1，2，3......
.method private packedSwitch(I)Ljava/lang/String;       //传入整数型参数p1，根据p1返回字符串
    .locals 1
    .param p1, "i"    # I

    .prologue
    .line 21
    const/4 v0, 0x0

    .line 22
    .local v0, "str":Ljava/lang/String;								//v0=null
    packed-switch p1, :pswitch_data_0							//switch(p1)

    .line 36
    const-string v0, "she is a person"							//default "she is a person",对于default分支会放在packed-switch后面

    .line 39
    :goto_0
    return-object v0														//返回字符串v0

    .line 24
    :pswitch_0																//case 0
    const-string v0, "she is a baby"

    .line 25
    goto :goto_0

    .line 27
    :pswitch_1																//case 1
    const-string v0, "she is a girl"

    .line 28
    goto :goto_0

    .line 30
    :pswitch_2																//case 2
    const-string v0, "she is a woman"

    .line 31
    goto :goto_0

    .line 33
    :pswitch_3																//case 3
    const-string v0, "she is an obasan"

    .line 34
    goto :goto_0

    .line 22
    nop

    :pswitch_data_0
    .packed-switch 0x0													//上面只是标签，决定顺序的是这段代码块
        :pswitch_0															//当case 0时，调到pswitch_0
        :pswitch_1
        :pswitch_2
        :pswitch_3
    .end packed-switch
.end method
```
#### sparse-switch无规律
```
sparse-switch的case是无规律的
.method private sparseSwitch(I)Ljava/lang/String;
    .locals 1
    .param p1, "age"    # I                          //参数为int p1,形参为age

    .prologue
    .line 43
    const/4 v0, 0x0										//v0=null,因为下文说v0是字符串

    .line 44
    .local v0, "str":Ljava/lang/String;
    sparse-switch p1, :sswitch_data_0			//根据p1的值，与sswitch_data_0跳转表跳转

    .line 58
    const-string v0, "he is a person" 				//紧跟sparse-switch的，且没有判断的，为default

    .line 61
    :goto_0													//公共出口，返回v0字符串
    return-object v0

    .line 46
    :sswitch_0												//这段代码为各种跳转标签
    const-string v0, "he is a baby"

    .line 47
    goto :goto_0

    .line 49
    :sswitch_1
    const-string v0, "he is a student"

    .line 50
    goto :goto_0

    .line 52
    :sswitch_2
    const-string v0, "he is a father"

    .line 53
    goto :goto_0

    .line 55
    :sswitch_3
    const-string v0, "he is a grandpa"

    .line 56
    goto :goto_0

    .line 44
    nop

    :sswitch_data_0										//跳转表
    .sparse-switch
        0x5 -> :sswitch_0							 	//case p1=5,跳转到:sswitch_0标签，下同理
        0xf -> :sswitch_1
        0x23 -> :sswitch_2
        0x41 -> :sswitch_3
    .end sparse-switch
.end method
```
__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
