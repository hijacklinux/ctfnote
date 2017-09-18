---
layout: default
---
# ![](../img/hj.jpg)凯撒密码解密源码（127模式）

```python
lstr="""12345abcde""" //根据实际情况写

for p in range(127):
    str1 = ''
    for i in lstr:
        temp = chr((ord(i)+p)%127)
        if 32<ord(temp)<127 :
            str1 = str1 + temp
            feel = 1
        else:
            feel = 0
            break
    if feel == 1:
        print '****%d****:'%p,str1


```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./crypto)
