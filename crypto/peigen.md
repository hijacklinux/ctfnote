---
layout: default
---
# ![](../img/hj.jpg)培根密码解密源码

```python
miwen=raw_input('enter your secretwords:')
flag=input('choose a dict 1/2:')
dict_1={'aaaaa':'a', 'aaaba':'c',  'aaaab':'b', 'aabaa':'e',  'aaabb':'d',  'aabba':'g',  'aabab':'f', 'abaaa':'i',  'aabbb':'h',  'ababa':'k', 'abaab':'j' , 'abbaa':'m', 'ababb':'l',
'abbba':'o',  'abbab':'n',  'baaaa':'q', 'abbbb':'p',  'baaba':'s', 'baaab':'r', 'babaa':'u', 'baabb':'t', 'babba':'w', 'babab':'v', 'bbaaa':'y', 'babbb':'x', 'bbaab':'z'}

dict_2={'aaaaa':'a', 'aaaba':'c',  'aaaab':'b', 'aabaa':'e',  'aaabb':'d',  'aabba':'g',  'aabab':'f', 'abaaa':'i-j' ,  'aabbb':'h',  'abaab':'k' ,  'ababb':'m',  'ababa':'l',
'abbab':'o' , 'abbaa': 'n',  'abbbb':'q',  'abbba':'p', 'baaab':'s' , 'baaaa': 'r',  'baabb':'u-v',  'baaba':'t',  'babaa':'w',  'babba':'y',  'babab':'x',  'babbb':'z'}

if flag==1:
    dict_0=dict_1
if flag==2:
    dict_0=dict_2

def decode(miwen):
    words=[]
    global dict_0
    for i in range(len(miwen)/5):
        words.append(dict_0[miwen[i*5:(i+1)*5].lower()])
    print 'result is :',''.join(words)

decode(miwen)
```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./crypto)
