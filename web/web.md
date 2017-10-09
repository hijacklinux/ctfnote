---
layout: default
---

# 欢迎来到web安全的世界
### ![](../img/hj.jpg) 以下为小贱所掌握的web安全技能树
>小贱提示：
>
>这些只是基础部分，深入部分就不是笔记这么简单了，需要大量实践总结经验，还需读者日后自行研究

- ![](../img/github1.png)![](../img/yes.png) [ASCII码表和HTML实体符号表](ascii)
- ![](../img/github2.png)![](../img/yes.png) 通用知识
  - ![](../img/yes.png)前端基础
    - ![](../img/yes.png)[url与HTML协议](url)
    - ![](../img/yes.png)[Cookie安全](cookie)
    - ![](../img/yes.png)[本地存储风险+CSS+ActionScript](bendi)
    - ![](../img/yes.png)[伪造ip+用户登录+验证码绕过+浏览器同源策略](tongyuan)
- ![](../img/github3.png)![](../img/yes.png) [信息收集](xinxishouji)
- ![](../img/github4.png)![](../img/yes.png) sql注入
  - ![](../img/yes.png)[漏洞挖掘](sqlwajue)
  - ![](../img/yes.png)[寻找注入点](zhurudian)
  - ![](../img/yes.png)[绕过方法](raoguofangfa)
  - ![](../img/yes.png)[判断类型](panduanleixing)
  - ![](../img/yes.png)[注入格式](zhurugeshi)
  - ![](../img/yes.png)[cheat-sheet](sqlcheatsheet)
  - ![](../img/yes.png)[万能密码](wannengmima)
  - ![](../img/yes.png)[十种MySQL报错注入](baocuozhuru)
  - ![](../img/yes.png)[其他数据库注入](qitashujuku)
  - ![](../img/yes.png)[常用参数](changyongcanshu)
- ![](../img/github5.png)![](../img/yes.png) XSS
  - ![](../img/yes.png) XSS payload
    - ![](../img/yes.png)[获取cookie+浏览器信息+插件](huoqucookie)
    - ![](../img/yes.png)[模拟发送请求](xssrequests)
  - ![](../img/yes.png) 构造思路
    - ![](../img/yes.png) 注意闭合
    - ![](../img/yes.png) 利用\<>标记注射
    - ![](../img/yes.png) [标签属性值注射](biaoqianshuxing)
    - ![](../img/yes.png) [对标签属性值转码](biaoqianzhuanma)
    - ![](../img/yes.png) [关键字拆分嵌套](guanjianzichaifen)
    - ![](../img/yes.png) [产生事件](chanshengshijian)
    - ![](../img/yes.png) 利用eval( )
      - \<script>eval("alert('XSS')");\</script>
    - ![](../img/yes.png) [利用CSS跨站剖析](liyongcss)
    - ![](../img/yes.png) aa.innerHTML="xxxxxxxxxxxx"
      - 这种情况下。xxxxx 只能使用 \<img src=1 onerror=alert(1)> 这种方式来触发 JS
    - ![](../img/yes.png) location.hash
      - \<input type="text" value="" onclick="eval(location.hash.substr(1))"/>
    - ![](../img/yes.png) 使用\<base>标签
      - \<base href="http://www.8pwn.com"/>\<script src="x.js"></script>
    - ![](../img/yes.png) window.name

       ```
       <script>
       window.name = "alert(document.cookie)";
       location.href = "http://www.xss.com/xssed.php";</script>
       ```
    - ![](../img/yes.png) [绕过过滤的方法](xssraoguo)
    - ![](../img/yes.png) 动态调用远程JS
  - ![](../img/yes.png) [XSS模型](xssmoxing)
- ![](../img/github6.png)![](../img/yes.png) [CSRF](csrf)
- ![](../img/github7.png)![](../img/yes.png) [clickjacking](clickjacking)
- ![](../img/github8.png)![](../img/yes.png) 代码审计
  - ![](../img/yes.png) [通用思路](shenjitongyong)
  - ![](../img/yes.png) [PHP核心配置](phphexinpeizhi)
  - ![](../img/yes.png) [文件包含漏洞](wenjianbaohan)
  - ![](../img/yes.png) [文件读取（下载）漏洞](wenjianduqu)
  - ![](../img/yes.png) [文件上传漏洞](wenjianshangchuan)
  - ![](../img/yes.png) [php中可以执行代码的函数](phpfunc)
  - ![](../img/yes.png) [代码执行漏洞](daimazhixing)
  - ![](../img/yes.png) [变量覆盖漏洞](bianliangfugai)
- ![](../img/github9.png)![](../img/yes.png) myql数据库
  - ![](../img/yes.png) [基础知识](sjkjichuzhishi)
  - ![](../img/yes.png) [增删改查](zenshangaicha)
