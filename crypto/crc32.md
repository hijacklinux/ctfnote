---
layout: default
---
# ![](../img/hj.jpg)CRC32解密源码

```python
import binascii

real = 0x4D1FAE0B //根据实际情况设置

for y in range(1000, 3000):
    for m in range(1, 10):
        for d in range(1, 10):
            al = str(y) + '0' +  str(m) + '0' + str(d)
            if real == binascii.crc32(al):
                print(al)

for y in range(1000, 3000):
    for m in range(10, 13):
        for d in range(10, 32):
            al = str(y) +  str(m) + str(d)
            if real == binascii.crc32(al):
                print(al)


```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./crypto)
