# 同源策略

浏览器故意设计的一个功能限制 。

浏览器规定 JS 运行源在 A 里 就只能获取源 A 的数据 ， 不能获取源 B 的数据
即 **不允许跨域**

## 同源

源

window.origin 或 location.origin 可以得到当前源

源 = 协议 + 域名 + 端口号

同源：如果两个 url 的 协议 + 域名 + 端口号 完全一致 这两个 url 就是同源的

https://qq.com 和 https://baidu.com不同源

https://baidu.com 和 https://www.baidu.com 不同源

# 为什么要设计同源策略

为了保护用户隐私

问题的根源

- 不同域名的 JS 发的请求到后台几乎没有区别（referer 有区别）
- 后台开发者没有检查就完全没区别
- 所以没有同源策略，任何页面都能偷数据 甚至余额

# 关于同源策略的一些疑问

a.qq.com qq.com 也算跨域？

历史上出现过 不同公司共用域名（开发说的时候也会这样搞）

不同端口也算跨域？
不同公司共用服务器（开发时也会这样搞）

两个网站 IP 是一样的，也算跨域？
原因同上 IP 可以共用

**为啥跨域可以使用 css js 图片？**
同源策略限制的是数据访问，引用 css js 图片的时候，并不能在 JS 里拿到它们的内容

# CORS（Cross-Origin Resource Sharing 跨源资源共享）

[CORS MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/CORS)
突破浏览器限制的一个方法

Access-Control-Allow-Origin

```javascript
// web 服务器response header设置     Access-Control-Allow-Origin
//设置单个   值要完全一样才行
response.setHeader("Access-Control-Allow-Origin", "http://baidu.com:80");
//设置所有 源都能访问
response.setHeader("Access-Control-Allow-Origin", "*");
```

# JSONP

IE 时代的妥协  
IE 6 7 8 9 都不支持 CORS

**JSONP 是什么 （优缺点）?**
当前浏览器不支持 cors，必须使用另外一种方式跨域

当前网站创建一个 script 标签去请求另一个网站的 JS 文件，JS 里会执行回调，回调里夹带想要的数据.

**回调的名字是什么？**
回调的名字是可以随机生成的一个随机数，把这个随机数以 callback 为参数传给后台，后台会把这个函数返回，并执行

优点

1. 兼容 IE
2. 跨域

缺点

1. 由于它是 script 标签，所以没有 AJAX 的状态码精确 只能知道成功和失败 ，通过监听 srcitp.onload srcipt.onerror 函数
2. 由于是 script 标签 ，所以只能发 get 请求，不支持 post 请求

# 其他跨域

postMessage （其实两个 tab 标签通信用的）

websocket 太重了

nginx 反向代理 iframe 后端的东西

nginx 反向代理 iframe
