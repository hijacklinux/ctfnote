---
layout: default
---
# ![](../img/hj.jpg)PHP核心配置
>小贱提示： 配置文件的文件名通常都带有“ config”这样的关键字，只要搜索带有这个关键字的文件名即可

## ![](../img/github14.png)SET NAMES
set names'gbk'相当于干了三件事,容易存在宽字节漏洞：
```
SET
character_set_connection='gbk',
character_set_results='gbk',
character_set_client='gbk'
```
## ![](../img/github12.png)PHP_INI_*常量的定义

|  定义   |  说明   |
| --- | --- |
| PHP_INI_USER|该配置选项可在用户的PHP脚本或Windows注册表中设置|
| PHP_INI_PERDIR|该配置选项可在php.ini..htaccess或httpd.conf中设置|
| PHP_INI_SYSTEM|该配置选项可在php.ini或httpd.conf中设置|
| PHP_INI_ALL|该配置选项可在任何地方设置|
| php.ini only|该配置选项仅可在php.ini中配置|

## ![](../img/github13.png)register_globals(全局变量注册开关)
该选项在设置为 on 的情况下，会直接把用户 GET、POST 等方式提交上来的参数
注册成全局变量并初始化值为参数对应的值，使得提交参数可以直接在脚本中使用。
register_globals 在 PHP 版本小于等于 4.2.3 时设置为 PHP_INI_ALL，从 PHP 5.3.0 起
被废弃，不推荐使用，在 PHP 5.4.0 中移除了该选项。
当 register_globals 设置为 on 且 PHP 版本低于 5.4.0 时，如下代码输出结果为 true。
```
<?php
if($user=='admin') {
echo 'true';
//do something
}
//提交的网址：xxx/1.php?user=admin
```
## ![](../img/github15.png)allow_url_include（是否允许包含远程文件）
这个配置指令对 PHP 安全的影响不可小觑。

在该配置为 on 的情况下，它可以直接包含远程文件，
当存在 include ($var) 且 $var 可控的情况下，可以直接控制 $var 变量来执行 PHP 代码。
allow_url_include 在 PHP 5.2.0 后默认设置为 off，配置范围是 PHP_INI_ALL。
与之类似的配置有 allow_url_fopen，配置是否允许打开远程文件，不过该参数对安全的影响没有 allow_url_include 大，故这里不详细介绍。
配置 allow_url_include 为 on，可以直接包含远程文件。测试代码如下：
```
<?php
include $_GET['a'];
//http://xxx/1.php?a=http://xxx/2.txt
```
## ![](../img/github16.png)magic_quotes_gpc（魔术引号自动过滤）
当该选项设置为 on 时，会自动在 GET、POST、COOKIE 变量中的单引号（'） 、双引号（"） 、反斜杠（\\）及空字符（NULL）的前面加上反斜杠（\\） ，
但在 PHP 5 中 magic_quotes_gpc 并不会过滤 $_SERVER 变量，导致很多类似 client-ip、referer 一类的漏洞能够利用。5.4后无此配置
在 PHP 版本小于 4.2.3 时，配置范围是 PHP_INI_ALL；在 PHP 版本大于 4.2.3时，是 PHP_INI_PERDIR。
测试代码如下：
```
<?php
echo $_GET['hj'];
//http://xxx/1.php?hj=1'会返回1\'
```
## ![](../img/github17.png)magic_quotes_runtime（魔术引号自动过滤）
也是自动在单引号（'） 、双引号（"） 、反斜杠（\\）及空字符（NULL）的前面加上反斜杠（\\） 。
它跟 magic_quotes_gpc 的区别是，处理的对象不一样，magic_quotes_runtime 只对从数据库或者文件中获取的数据进行过滤
PHP 5.4之后也被取消，配置范围是 PHP_INI_ALL
测试代码如下：
```
#文件1.txt
1'2"3\4
#文件1.php
<?php
ini_set("magic_quotes_runtime", "1");
echo file_get_contents("1.txt");
//返回1\'2\"3\\4
```
## ![](../img/github18.png)safe_mode（安全模式）
当 safe_mode=on 时，联动可以配置的指令有:
```
safe_mode_include_dir、
safe_mode_exec_dir、
safe_mode_allowed_env_vars、
safe_mode_protected_env_vars。
```
safe_mode 指令的配置范围为PHP_INI_SYSTEM，PHP 5.4 之后被取消。

这个配置会出现下面2种限制：

1）所有文件操作函数（例如 unlink()、file() 和 include()）等都会受到限制

2）通过函数 popen()、system() 以及 exec() 等函数执行命令或程序会提示错误

下面是启用 safe_mode 指令时受影响的函数、变量及配置指令的完整列表：
```
apache_request_headers()、
ackticks()、
hdir()、hgrp()、
chmode()、
chown()、
copy()、
dbase_open()、
dbmopen()、
dl()、
exec()、
filepro()、
filepro_retrieve()、
ilepro_rowcount()、
fopen()、header()、
highlight_file()、
ifx_*、ingres_*、link()、
mail()、
max_execution_time()、
mkdir()、
move_uploaded_file()、
mysql_*、
parse_ini_
file()、
passthru()、
pg_lo_import()、
popen()、
posix_mkfifo()、
putenv()、
rename()、
zmdir()、
set_time_limit()、
shell_exec()、
show_source()、
symlink()、
system()、
touch()。
```
## ![](../img/github19.png)open_basedir  PHP 可访问目录
用来限制 PHP 只能访问哪些目录

该指令的配置范围在 PHP 版本小于 5.2.3 时是 PHP_INI_SYSTEM，在 PHP 版本大于等于 5.2.3 是 PHP_INI_ALL
## ![](../img/github20.png)disable_functions（禁用函数）
本指令配置范围为 php.ini only
## ![](../img/github21.png)display_errors 和 error_reporting 错误显示
这两个指令的配置范围都是 PHP_INI_ALL

__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
