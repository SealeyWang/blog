# webpack 文件打包优化

默认使用 create-react-app 生成项目后， 开发了一段时间，尝试 build 一下, main.js 5m+ (gzip 后 1.5m)

使用 optimization.runtimeChunk = 'single'.
runtimeChunk 包含映射关系单独从 js 里提出来，避免每次修改都变动，充分利用浏览器缓存

使用了 optimization.splitChunks.cacheGroup 选项。把一些 node_nodules 里 打包后的的东西与源代码分离

主要文件大小 after gzip (为了方便我就不加文件 hash 后缀了)

- main.js 34.28 kB
- vendor1 529.37 kB (antd 等)
- vendor2 366.7 kB (echarts 等)
- vendor 623.4kB (其他 node_modules)

首先能避免代码变动重新下载 vendors 依赖。

其次想要并发下载文件。但是在限制网速测试中，变现并不好

slow 3G

- 优化前 35.15s
- 优化后 35.66s

fast 3G

- 优化前 9.96s
- 优化后 10.02s

300KB/s 200ms 延迟

- 优化前 5.77s
- 优化后 5.81s

1M/s 100ms 延迟

- 优化前 1.98s
- 优化后 1.97s

可先在限制网速的情况下，加载速度没有提升，且因设置了延迟会更慢一些。（像通过 chrome 限制网速测试，路由器限制用户网速 ,办公室固定 ip 限速等情况，加载速度没变）

理论上来说， 分多个 vendor 相当于 大文件 多线程分片下载，

1.  可以在名下多开几个链路，争抢路由器调度资源。
2.  旧 TCP 协议滑动窗口大小限制、高抖动网络中 TCP 滑窗会很快停止增长，使得链路传输速率上限极低。 ，多开链路相当于变相增大滑动窗口， 绕过 TCP 流控协议本身的缺陷。

参考 [多线程下载一个大文件的速度更快的真正原因是什么？](https://www.zhihu.com/question/376805151)
