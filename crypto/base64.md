---
layout: default
---
# ![](../img/hj.jpg)BASE64爆破大小写源码

```python
import base64,re
from itertools import combinations
b=raw_input('enter your chars:')
s=list(b)

for i in range(len(s)):
    for j in list(combinations([x for x in range(len(s))], i)):
        a=list(s)
        for k in j:
           a[k]= a[k].lower()

        r=repr(base64.b64decode(''.join(a)))
        if '\\x' not in r:
            print r[1:-1]
```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./crypto)
