---
layout: default
---
# ![](../img/hj.jpg)有技术的隐藏方法

## ![](../img/github24.png)插入法
不会改变原始数据，而是为文件增加了额外数据

#### 追加插入法
举例：
jpeg格式是以0xFF 0xD9作为结尾的，可以用winhex在后面继续追加数据，图片不会识别
也可用JPEGX这款隐写软件追加

#### 前置插入法
任何可以插入批注内容的文件都可能被插入数据

例如HTML和JPEG都很容易嵌入数据

以JPEG举例：

jpeg被标识符分成不同的区域，每个标识符以0xFF开头

|   标识符  |  值（hex）   |  大小（字节）   |   详细信息  |
| --- | --- | --- | --- |
| SOI			| FF D8			| 	2				| 	图像起始位置|
| APP0		| 	FF E0			| 		2		| 			App标识符（文件详细信息）|
| SOF0			| FF C0			| 		2		| 			框架起始位置（宽度，高度等）|
| SOS			| FF DA 			| 	2			| 		扫描起始位置|
| EOI			| FF D9		| 		2			| 		图像结束为止/文件结束符（EOF）|


在jpeg首部有很多区域可以用来隐藏信息，用JPHide&Seek可以在FF E0和FF C0之间插入信息
## ![](../img/github25.png)替换法
改变原始数据
#### LSB修改最低有效位
隐藏工具：

S-Tools

ImageHide

Steganos

## ![](../img/github26.png)在PDF中隐藏
工具：wbStego4open

可以把文件隐藏到bmp,txt,htm,pdf中

## ![](../img/github27.png)在可执行文件中隐藏数据

工具:hydan

加密：./hydan tar message.txt>tar.steg

tar是可执行文件，message.txt是要隐藏的密码文件,处理过后tar.steg和tar并没有区别

解密：./hydan-decode tar.steg
## ![](../img/github28.png)在html中隐藏
工具：snow

加密：snow -m 'aaaaa' -p 'zzzzz' hello.htm hello_snow.htm

解密网址：fog.misty.com/perry/ccs/snow/snow/snow.html
## ![](../img/github1.png)多媒体中数据隐藏
#### 数字音频(.wav)中
原理：LSB修改最低有效位

1、xx.wav拖入，右下方显示可隐藏容量，由于8个字节的最低位组成一个字节，因此真实容量=右下角容量/8

2、把要隐藏的数据拖到刚刚那个xx.wav波形图上，输入密码即可隐藏，然后右键另存为

解密：波形图右键，revealing
#### 高级音频(mp3/AAC)中
MP3Stego

嵌入率约0.1%，比如一个6M的MP3，可嵌入6k的数据

以wav和隐藏文件作为输入，mp3作为输出
#### 数字视频
MSU StegoVideo


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./stega)
