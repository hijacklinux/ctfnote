---
layout: default
---
# ![](../img/hj.jpg)修复IAT
>小贱提示： 这里只是说说方法，至于原理我也懒得说了，毕竟这是笔记嘛，笔记就是要简单方便查阅

## ![](../img/github11.png)脱壳之后如何修复IAT
### 第一步
```
到达OEP之后，od dump记下'修正为'，打开lord pe，选中程序，右键修正镜像大小，
右键完整转存，剩下的就是用imprec修复iat
```
### 第二步
```
记下ope的偏移地址（例如：004670f0就记670f0），填到imprec中，点IAT AutoSearch，
不行的话就修改rva和size，点get imports，会出现一堆不合法的，点show invailid，
然后在高亮处右键，cut thunks，然后fix dump
```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./tools)
