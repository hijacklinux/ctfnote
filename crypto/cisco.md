---
layout: default
---
# ![](../img/hj.jpg)CISCO密码解密

```python
import cisco_decrypt
cisco_pass=raw_input('enter your pwd:')
crack=cisco_decrypt.CiscoPassword()
password=crack.decrypt(cisco_pass)
print password

```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./crypto)
