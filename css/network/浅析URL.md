#  浅析URL

* IP的作用 ping命令的作用
* 域名是什么,分别是哪几类域名
* DNS作用是什么？nslookup命令如何使用
* URL包含哪几部分，作用是什么


## IP的作用 ping命令的作用
IP (Internet Protocal)主要解决了
1. 如何定位一台设备
2. 如何封装数据报文，以跟其他设备交流

ping的作用
1. 检测网络网路连接情况、网络延迟、是否丢包等
2. 根据域名得到服务器IP

只要在互联网中，就至少有一个IP


## 域名是什么,分别是哪几类域名

域名就是IP对应的别称
例如想知道baidu.com对应的ip 就  ping baidu.com 

一个域名可以对应不同IP 防止一台机器扛不住，这个交负载均衡

一个IP对应不同域名，当一个公司有多个域名时，或多个公司共用
一台主机（共享主机）

域名的分类

* 一级域名(顶级域名): 包括通用顶级域，例如.com、.net和.org；以及国家和地区顶级域，例如.us、.cn和.tk。
* baidu.com 是二级域名（俗称一级域名）
* www.baidu.com 是三级域名 
* 他们是父子关系，可以是同一家公司baidu.com 也可以不是 

域名前要不要加www?

对于大访问量或多子域名的网站来说，不建议使用裸域。小流量或子域名少的网站的话就看个人爱好了


不管你决定使用还是不使用裸域，最好不要在同时保留 www 前缀和裸域的 URL，这样既不方便用户的浏览器区分访问历史，也会对你做访问统计带来不少麻烦。最佳的方式是采用 301 跳转，并且跳转的时候保留 URL 里域名后的全部内容。

比如，如果你决定使用裸域 http://example.com，那么请务必将   

http://www.example.com/foo/bar?spam=egg 

301跳转到

http://example.com/foo/bar?spam=egg 

反之同理

## DNS作用是什么？nslookup命令如何使用
Domain Name System (DNS)，域名系统。 当在浏览器输入baidu.com时。浏览器会向域名服务器询问 baidu.com 对应什么IP。
然后浏览器拿到队友的IP对80/443端口发送请求，请求的内容是查看
baidu.com的首页

http 默认80 https默认443


nslookup（from name server lookup）命令 。查询DNS的记录，查看域名解析是否正常，在网络故障的时候用来诊断网络问题。

```
nslookup www.douban.com [dns-server]
```
如果没有指定 dns-server，使用系统默认的 DNS 服务器。

## URL包含哪几部分，作用是什么
URL是统一资源定位符，Uniform Resource Locator (URL)。

URL = 协议+域名或IP+端口号+路径+查询字符串+锚点 组成

例如一个网址 https://www.baidu.com/s?wd=hello&rsv_spt=1#5
其中 

* https:// 是协议
* www.baidu.com 是域名
* /s 是路径
* ?wd=hello&rsv_spt=1 是查询参数
* #5是锚点
* 浏览器会默认省略 http的80端口  https的443端口



## curl命令

curl 是常用的命令行工具。用来请求 Web 服务器。它的名字就是客户端（client）的 URL 工具的意思。

```
curl https://www.example.com
```
过程是先根据url，请求DNS得到对应的IP，然后进行TCP连接，连接成功再发起HTTP请求。相应结束后关闭TCP连接，实现真正的结束。

[参考博客](https://juejin.cn/post/6844903989096497160)


