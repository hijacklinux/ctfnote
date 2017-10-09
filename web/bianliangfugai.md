---
layout: default
---
# ![](../img/hj.jpg)变量覆盖漏洞
搜寻参数带有变量的extract(),parse_str()函数，然后去回溯变量是否可控。

import_request_variables()函数相当于开了全局变量注册，这时只要找到哪些变量没有初始化且操作之前没有赋值的，然后就大胆地去提交这个变量作为参数吧，

另外只要写在import_request_variables()函数前面的变量，不管是否已经初始化，都可以覆盖

$$符号注册变量会导致变量覆盖，可以搜$$这个关键字去挖掘

## ![](../img/github26.png)extract()函数
extract()函数作用是：将数组中的键值对注册成变量,格式为：
```
extract(array &$var_array [,int $extract_type=EXTR_OVERWRITE [, string $prefix=NULL]])
```
该函数有三种情况会覆盖变量：

情况一：第二个参数为EXTR_OVERWRITE

情况二：只传入第一个参数，其实和情况一原理一样，因为第二个参数默认EXTR_OVERWRITE

情况三：第二个参数为EXTR_IF_EXISTS

举例：
```
<?php
$b=3;
$a=array('b'=>'1');
extract($a);
print_r($b);
?>
```
经过extract()函数对变量$a处理后，变量$b的值被覆盖为了1
## ![](../img/github27.png)parse_str()函数
作用是：解析字符串并注册成变量，会直接覆盖已有变量

格式：parse_str(string $str [,array &$arr])

第一个参数$str：形式为“a=1”,经过函数之后会注册变量$a且赋值为1

第二个参数$arr是一个数组，当第二个参数存在时，注册的变量会放到这个数组里，会覆盖原有数组的键

举例：
```
<?php
$b=1;
parse_str('b=2');
?>
变量$b原有的值1被覆盖为了2
```
## ![](../img/github28.png)import_request_variables()函数
作用：把GET，POST，COOKIE的参数注册成变量，(适用版本php4.1-5.4)

格式：import_request_variables(string $types [, string $prefix])

$types：代表要注册的变量，G代表GET，P代表POST，C代表COOKIE，当$types为GPC时，会注册GET，POST，COOKIE参数为变量

$prefix：要注册的变量前缀

举例：
```
1.php代码为：
<?php
$b=1;
import_request_variables('GP');
print_r($b);
?>
此时访问网址：http://test.com/1.php?b=2，则$b的值1被覆盖成了2
```
## ![](../img/github1.png)$$变量覆盖
经典例子代码：
```
<?php
$a=1;
foreach(array('_COOKIE' , '_POST' , '_GET') as $_request) {
	foreach($$_request as $_key ==> $_value) {
		echo $_key. '<br />';
		$$_key=addslashes($_value);
	}
}
echo $a;
?>
网址提交test.com/1.php?a=2则会输出：
a
2

解释：
$key为a，
$$key为$a，
$a=addslashes($value)
```
## ![](../img/github2.png)register_globals=on时
变量来源可能是各种不同的地方，都可以覆盖
## ![](../img/github3.png)通过$GLOBALS获取的变量
举例：
```
if (ini_get('register_globals'))
foreach($_REQUEST as $k=>$v)
unset($($k));
变量$a没有初始化，在register_globals=on时，控制$a出错
http://www.abc.com/test1.php?a=1$b=2
显示$a未定义
这时
http://www.abc.com/test1.php?GLOBALS[a]=1$b=2则覆盖成功

```
__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
