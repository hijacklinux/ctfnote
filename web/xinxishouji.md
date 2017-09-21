---
layout: default
---
# ![](../img/hj.jpg)信息收集

## ![](../img/github17.png)全面扫描
#### ./discover.sh
全面被动扫描
#### 先用nikto扫
nikto -host http://..../

加-标签 特殊扫描节省时间，一些敏感网页
#### vega扫描漏洞
#### openvas
## ![](../img/github18.png)踩点网站
myiptest.com

netcraft.com 查os

archive.org

webmaster-toolkit.com

dnsqueries.com 查一堆

aruljohn.com mac查系统

archive.org
## ![](../img/github19.png)获得网站真实ip
#### [cloudflare](http://cloudflare-watch.org.statstool.com)
#### nslookup
nslookup demo.baidu.com

查询测试dns
#### dig 之后ping各dns
## ![](../img/github20.png)收集二级域名
#### Google Hack
#### fierce -dns
域名查询，该域有几台主机
#### theharvester
#### nmap dns-brute
## ![](../img/github21.png)扫路径
#### uniscan
uniscan -u url -bqweds类似wwwscan

结果报告在usr/share/uniscan中
#### DirBuster
url: http://www.baidu.com

字典在/pentest/web/dirbuster/中
#### burp的spider
## ![](../img/github22.png)DNS
## ![](../img/github23.png)whois
whois baidu.com

查询域名注册信息
## ![](../img/github24.png)nmap
## ![](../img/github25.png)cms扫描
python3 wig.py 网址/子页
## ![](../img/github26.png)robots.txt审计有用信息
parsero -u url
## ![](../img/github28.png)扫描包含漏洞
```
fimap -h
fimap -H -u http://... -d 3 -w /root.haha.txt
收集包含网址列表

然后fimap -m -l /root/haha.txt找漏洞

然后另开fimap -x
漏洞利用，不行就在后面加上--force-run
跟着步骤走，可选不回弹，如果回弹，
用netcat -v -l -v -p 4444监听
```


__原创文章，转载请注明转载自[http://www.8pwn.com](http://www.8pwn.com)__

[返回上一层](./web)
