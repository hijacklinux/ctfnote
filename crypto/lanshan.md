---
layout: default
---
# ![](../img/hj.jpg)栏栅密码解密源码

```python
#-*- coding:utf-8 -*-
e = raw_input('请输入要解密的字符串\n')
elen = len(e)
field=[]
for i in range(2,elen):
      if(elen%i==0):
        field.append(i)
for f in field:
  b = elen / f
  result = {x:'' for x in range(b)}
  for i in range(elen):
    a = i % b;
    result.update({a:result[a] + e[i]})
  d = ''
  for i in range(b):
    d = d + result[i]
  print '分为\t'+str(f)+'\t'+'栏时，解密结果为： '+d

```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./crypto)
