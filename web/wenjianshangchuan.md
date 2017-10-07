---
layout: default
---
# ![](../img/hj.jpg)文件上传漏洞
搜索关键函数:move_uploaded_file() 接着看调用这个函数的代码是否存在为限制上传格式或者可以绕过。
## ![](../img/github22.png)客户端校验（如javascript）
上传1.jpg,用burp抓包，改成1.asp
## ![](../img/github23.png)服务端校验
上传1.php,用burp抓包，将content-type字段改为image/gif或image/jpeg
## ![](../img/github24.png)黑名单扩展名过滤：限制不够全面
IIS默认支持解析.asp,.cdx, .asa,.cer等(这些都解析成asp)，再试试大小写如/test.aSp。

    IIS6.0 默认的可执行文件除了asp还包含这三种 :
    ```
    /test.asa
    /test.cer
    /test.cdx
    ```
    对于php：可以上传“1.php "注意php后面有个空格
## ![](../img/github25.png)文件头 content-type验证绕过
1、getimagesize()函数：

验证文件头只要为GIF89a，就会返回真（意思是上传不是图片的文件也会认为是图片）。

举例：

在木马内容基础上再加了一些文件信息，如muma.gif的内容为GIF89a<?php phpinfo(); ?>

2、 限制$_FILES["file"]["type"]的值 就是人为限制content-type为可控变量。

## ![](../img/github26.png)形式：www.xxx.com/xx.asp/xx.jpg
原理: 服务器默认会把.asp，.asp目录下的文件都解析成asp文件。
## ![](../img/github27.png)形式：www.xxx.com/xx.asp;.jpg
原理：服务器默认不解析;号后面的内容，因此xx.asp;.jpg便被解析成asp文件了。
## ![](../img/github28.png)Apache 解析缺陷绕过上传漏洞
修改后缀，文件名为122.php.7zz（7zz为不能识别的后缀名）

或者修改后缀，文件名为1.phpX(X代表某个字符)
## ![](../img/github1.png)AddHandler 和AddType
1、如果在 Apache 的 conf 里有这样一行配置 AddHandler php5-script .php 这时只要文件名里包含.php 即使文件名是 test2.php.jpg 也会以 php 来执行。

2、如果在 Apache 的 conf 里有这样一行配置 AddType application/x-httpd-php .jpg 即使扩展名是 jpg，一样能以 php 方式执行。

## ![](../img/github2.png)www.xx.com/phpinfo.jpg/1.php
当访问www.xx.com/phpinfo.jpg/1.php这个URL时,会将phpinfo.jpg作为PHP文件来解析（1.php是不存在的文件）

例如（代码在1.jpg中）访问如下网址：
```
www.xxxx.com/UploadFiles/image/1.jpg/1.php
www.xxxx.com/UploadFiles/image/1.jpg%00.php
www.xxxx.com/UploadFiles/image/1.jpg/%20\0.php
```
## ![](../img/github3.png)test.jpg/.php
上传一个名字为test.jpg，然后访问test.jpg/.php,在这个目录下就会生成一句话木马shell.php
## ![](../img/github4.png)上传不符合windows文件命名规则的文件名
```
test.asp.
test.asp(空格)
test.php:1.jpg
test.php:: $DATA
```
会被windows系统自动去掉不符合规则符号后面的内容。
## ![](../img/github5.png)%00截断绕过上传（php5.3及之前可用）
```
1.php .jpg   用burp把空格二进制20改为00
搞配合：
test.php(0x00).jpg
test.php%00.jpg
路径/upload/1.php(0x00)，文件名1.jpg，结合/upload/1.php(0x00)/1.jpg
```
## ![](../img/github5.png)文件扩展名绕过
php除了可以解析php后缀 还可以解析php2.php3，php4 后缀
文件内容检测绕过：
抓包，在正常图片末尾添加一句话木马
## ![](../img/github5.png)htaccess解析漏洞
上传的jpg文件都会以php格式解析
## ![](../img/github5.png)编辑器解析漏洞

fck编辑器版本识别及信息收集

版本地址（2.2.4/2.2.6）

_samples/default.html

_whatsnew.html

Fck2.2.4 上传地址：

editor/filemanager/browser/default/comectors/test.html

editor/filemanager/upload/test.html

V2.2.6

editor/filemanager/connectors/test.html

............................................../uploadtest.html

fck编辑器解析漏洞

/1.asp/下创建文件夹2.asp （2.asp会被转成2_asp）
__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
