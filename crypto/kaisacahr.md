---
layout: default
---
# ![](../img/hj.jpg)凯撒密码解密源码（字母only模式）

```python
#-*- coding:utf-8 -*-

word=raw_input('''请输入你要解密的字符：''')
def crack(n):
    sum=''
    for char in word:
        order1=ord(char)
        if (order1 in range(0,65)) or (order1 in range(91,97)) or (order1 > 122):
            order=order1
        else:
            if order1 in range(65,91):
		order=order1+n
		if order >90:
		    order=64+order-90
	    if order1 in range(97,123):
		order=order1+n
		if order > 122:
	            order=96+order-122

        new_char=chr(order)
        sum=sum+new_char
    print '以%d为位移解密。。'% n
    print '解密结束'
    print '%d位位移解密结果为'% n ,sum
for x in range(1,26):
    crack(x)

```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./crypto)
