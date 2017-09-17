---
layout: default
---
# ![](../img/hj.jpg)apk的组成
>小贱提示： apk文件其实就是个压缩包，所有代码都在dex文件中

## ![](../img/github1.png)asset文件夹
资源目录

asset目录下的资源文件不需要生成索引，在java代码中需要用AssetManager来访问

使用c++游戏引擎（或使用Lua Unity3D等）的资源均需要放在asset下

## ![](../img/github2.png)lib文件夹
so库存放位置，一般由NDK编译得到，常见于使用游戏引擎或JNI native调用的工程中
## ![](../img/github3.png)META-INF文件夹
存放属性文件如：Manifest.MF
## ![](../img/github4.png)res文件夹
资源目录

res目录下的资源文件在编译时会自动生成索引文件（R.java）,在Java代码中用R.xxx.yyy来引用

使用java开发的android工程使用的资源文件会放到res下

>小贱提示： 注意和asset文件夹的区别哦！

## ![](../img/github5.png)AndroidManifest.xml
Android工程的基础配置属性文件
## ![](../img/github6.png)classes.dex
Java代码编译的到的Dalvik VM能直接执行的文件
## ![](../img/github7.png)resources.arsc
对res目录下的资源的一个索引文件，保存了原工程中strings.xml等文件内容



__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./reverse)
