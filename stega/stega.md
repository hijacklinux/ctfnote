---
layout: default
---

# 欢迎来到隐写术的世界
### ![](../img/hj.jpg) 以下为小贱所掌握的隐写术技能树
>小贱提示：
>
>这些只是基础部分，深入部分就不是笔记这么简单了，需要大量实践总结经验，还需读者日后自行研究
>
>一般得到图片先用binwalk跑，然后stagesolve，再用winhex看看有没有敏感字符
>
>有些文件会修改二进制头部，对应后缀文档格式来修正

- ![](../img/github1.png)![](../img/yes.png) [常见的解题思路](jietisilu)
  - copy /b 2.jpg+1.zip output.jpg
  - LSB修改最低有效位
  - 信息隐藏在结束符之后
  - 信息隐藏在属性中
  - 编程辅助
  - 两张图片
  - GIF文档格式
  - png文档格式
  - hex 78 9c开头
- ![](../img/github2.png)![](../img/yes.png) [各种隐藏方法](jishuyincang)
  - 追加插入法
  - 前置插入法
  - LSB修改最低有效位
  - 在PDF中隐藏
  - 在可执行文件中隐藏数据
  - 在html中隐藏
  - 数字音频(.wav)中
  - 高级音频(mp3/AAC)中
  - 数字视频
  - Word中数据隐藏
    - 设置隐藏文本
    - 使用白色字体
    - 信息放在属性里
  - 文件元数据EXIF
  - 移动设备数据隐藏
  - 文件压缩工具数据隐藏即：copy /b 2.jpg+1.zip output.jpg
