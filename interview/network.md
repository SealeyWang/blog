# HTTP GET POST 区别

- get 主要用户获取数据 post 主要用于设置数据

- get 把参数包含在 URL 中，post 通过 request body 传递参数。不过都是基于 TCP 协议的。给 get 加 request body 或者 post 带上 url 参数都行的通，不过一般没人那样用

  - RFC 2616(HTTP / 1.1) 各个浏览器对 URL 的长度有限制。 （不同浏览器 URL 长度限制不同）
  - IIS 7 对 URL Query String 有长度限制；`maxQueryString="1024" maxUrl="2048"`默认： url 2048 字节(Byte) queryString 1024 字节
  - GET 请求只能进行 url 编码，POST 请求支持多种编码方式。
    - JS window.encodeURI() window.decodeURI()
    - window.encodeURIComponent() window.decodeURIComponent()
  - 对于参数类型，GET 只接受 ASCII 字符，而 POST 没有限制，可以用加密算法加密数据

- GET 请求会被浏览器主动 cache，而 POST 不会，除非手动设置。

  - get 产生的 URL 可以被添加到书签 post 不行。因为书签仅仅包含 URL，所以表单参数都会丢失
  - get 会将请求参数放在请求的 url 中，回退操作实际上浏览器会从之前的缓存中拿结果；post 每次调用都会创建新的资源。

- get 安全 是指不会改变后端数据状态

# TCP

## TCP 链接

RFC 793 ip 拼接端口号 就是套接字
`socket = ip:port`

tcp 链接
`tcp链接 = {socket1,socket2}= {(ip1:port1),(ip2:port2)}`

# 如何保证 API 调用时数据的安全性？

通信使用 https

请求签名，防止参数被篡改

身份确认机制，每次请求都要验证是否合法

APP 中使用 ssl pinning 防止抓包操作

对所有请求和响应的数据都进行加解密操作 RSA AES
