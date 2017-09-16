---
layout: default
---
# ![](../img/hj.jpg)获得函数的调用
>小贱提示： 经常遇到API不在输入表中
>
>解决办法有两种：
>
>1、修改输入表结构，增加相应API函数
>
>2、显示链接方式调用DLL相关函数

## ![](../img/github10.png)增加输入函数(隐含链接),推荐
```
用LordPE
1、PE编辑器->目录，点击输入表的"..."
2、在任意一个DLL上右键，添加导入表，输入DLL文件和函数名（如USER32.dll和MessageBoxA)，
点+，点OK，LordPE会新增一个区块存放新增的IID数据
3、之后回到【输入表】窗口，选中刚刚的DLL文件，勾选总是查看FirstThunk。
则ThunkRVA的值就是该函数在IAT中的RVA地址，调用的时候代码就写call [基址+ThunkRVA],
//此处的基质为当前文件的基址，如exe文件基址通常是400000，如果是代码修改在dll中，则需要重定位
4、之后随便找一个空白地方填上想要的字符串参数
push 0
push title字符串地址
push text字符串地址
push 0
call dword ptr [400000+ThunkRVA]   //od中的话也可以直接call MessageBoxA
```
# ![](../img/hj.jpg)
>小贱提示：
>
>上述方法也可以把自己写的动态链接库添加到输入表中哦


## ![](../img/github11.png)显式链接调用函数
>小贱提示：
>
>显式链接是通过LoadLibraryA(W)或LoadLibraryExA(W)将DLL文件映射到调用进程的地址空间中，函数返回的HINSTANCE值用于标识文件映像映射到的虚拟内存地址。一旦DLL被加载，线程可以调用GetProcAddress函数获取输入函数的地址。
>
>LoadLibrary、GetProcAddress本身也是API，如果原输入表没有，就要在输入表里增加这些API函数。下面举个例子，调用MessageBoxA


```
push   4020B0                     //FileName="USER32.dll"
call     dword ptr [402008]    //LoadLibraryA,结果返回到eax中
push   4020C0                     //ProcNameOrOrdinal="MessageBoxA"
push   eax                           //hModule
call     dword ptr [402004]    //GetProcAddress
push   0
push   4020E0                      //"PEDIY"
push   4020D0                      //"Hello"
push   0
call     eax                            //eax是USER32.MessageBoxA地址，直接调用
```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
