---
layout: default
---
# ![](../img/hj.jpg)常见的解题思路

## ![](../img/github16.png)copy /b 2.jpg+1.zip output.jpg
binwalk output.jpg可以破解，把后缀改成zip,解压即可
## ![](../img/github17.png)LSB修改最低有效位
使用Stegsolve-Analyse-FrameBrowser
## ![](../img/github18.png)信息隐藏在结束符之后
图片都是以FF D9为结束符的，图片查看器会忽略结束符后面的数据，用binwalk破解
## ![](../img/github19.png)编程辅助
有时候工具不管用，需要自己编程
## ![](../img/github20.png)两张图片
linux下用compare 1.png 2.png diff.png命令来生成一个有差异的图片

还可以用stagesolve-analysis-image combiner对比两个文件，查看Sub或Xor
## ![](../img/github21.png)GIF文档格式
用hex看看gif格式是否符合GIF文档格式

Namo_GIF_gr可以一帧一帧查看图片

## ![](../img/github22.png)png文档格式
pngcheck.exe -v sctf.png来查看IDAT，看看有没有问题

IDAT是png图片中储存图像像数数据的块，
## ![](../img/github23.png)hex 78 9c开头
hex 78 9c开头的是zlib压缩的标志，用zilb解压
__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./stega)
