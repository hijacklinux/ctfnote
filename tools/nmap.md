---
layout: default
---
# ![](../img/hj.jpg)nmap
>小贱提示： 把常用的说说就好了，如果想看详细的网上一堆

常用：

-sP 抓活的

-O sS Pn sV 通常用这个

常用的五个脚本：

格式：nmap -Pn --script=脚本名 10.10.10.130

脚本：

dns-brute

sitemap-generator

mysql-info

vulscan

http-xssed

(vulscan太慢，只针对exploitdb命令为 nmap -sV --script-args vulscandb-exploit 10.10.10.130)


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./tools)
