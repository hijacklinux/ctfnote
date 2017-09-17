---
layout: default
---
# ![](../img/hj.jpg).NET向量操作指令


## ![](../img/github19.png)newarr
newarr [token]：
创建一个向量，向量中的元素由[token]标示，该指令从栈中取元素个数，并将向量的对象引用入栈
## ![](../img/github20.png)ldlen
从栈中取得向量实例的对象引用，并将该向量所含元素个数入栈

## ![](../img/github21.png)ldelema
ldelema [token]:
从栈中依次取得元素序号，向量对象的引用，然后返回该元素的地址（托管指针），元素的类型由[token]标示
## ![](../img/github22.png)readonly
与ldelema连用，此时ldelema返回一个受控不变托管指针，该指针所指内容不可改变，切ldelema时不做类型检查
## ![](../img/github23.png)ldelem
读取向量中的一个元素，它们依次从栈中取得元素序号，向量的对象引用。

| 指令                       | 说明                             |
| -------------------------- | -------------------------------- |
| ldelem.i1                  | 载入int8类型的向量元素           |
| ldelem.i2                  | 载入int16类型的向量元素          |
| ldelem.i4                  | 载入int32类型的向量元素          |
| ldelem.i8                  | 载入int64类型的向量元素          |
| ldelem.i                   | 载入native int类型的向量元素     |
| ldelem.u1                  | 载入unsigned int8类型的向量元素  |
| ldelem.u2                  | 载入unsigned int16类型的向量元素 |
| ldelem.u4                  | 载入unsigned int32类型的向量元素 |
| ldelem.u8                  | 载入unsigned int64类型的向量元素 |
| ldelem.r4                  | 载入float32类型的向量元素        |
| ldelem.r8                  | 载入float64类型的向量元素        |
| ldelem.ref                 | 载入对象引用类型的向量元素       |
| ldelem(ldelem.any) [token] | 读取任意类型的向量元素，包括泛型 |


## ![](../img/github24.png)stelem
对向量元素进行存储操作，它们依次从栈中读取待存的值，元素序号，向量的对象引用，指令结果不影响堆栈。


| 指令                       | 说明                             |
| -------------------------- | -------------------------------- |
| stelem.i    |   将 native int 类型的值存入向量的某个元素|
| stelem.i1   |  将 int8 类型的值存入向量的某个元素|
| stelem.i2   |  将 int16 类型的值存入向量的某个元素|
| stelem.i4   |  将 int32 类型的值存入向量的某个元素|
| stelem.i8   |  将 int64 类型的值存入向量的某个元素|
| stelem.r4   |  将 float32 类型的值存入向量的某个元素|
| stelem.r8   |  将 float64 类型的值存入向量的某个元素|
| stelem.ref  |  将栈中的对象引用存入向量的某个元素。这个过程包含了对象的转换（cast）,所以可能抛出InvalidCast异常|
| stelem.（stelem.any)  [token]| 将[token]所标示的项存入向量的某个元素，和ldelem类似，可用于泛型|

__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
