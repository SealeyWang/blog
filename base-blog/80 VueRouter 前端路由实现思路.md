# 路由是什么

## 路由

一个东西分发了请求，它就是路由。
这个东西就叫路由器。

路由形式
![路由形式](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e9b24bfa660241558ca26deb7367c1ee~tplv-k3u1fbpfcp-watermark.image)

## 分发

满足下图一对多的情况的，叫分发。（多对多也 ok）

![分发](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d4d53f16589d41e982924a3b46b015c0~tplv-k3u1fbpfcp-watermark.image)

## 路由表

一个存储 到各个目的 地最佳路径的表

## 前端路由相关

```javascript
window.location.hash; // 获取网页的hash值

// hash就是#后边的值

// hash变化监听
window.addEventListener(
  "hashchange",
  function () {
    console.log("The hash has changed!");
  },
  false
);

// 或
function locationHashChanged() {
  if (location.hash === "#cool-feature") {
    console.log("You're visiting a cool feature!");
  }
}

window.onhashchange = locationHashChanged;
```

## 默认路由

路由值为空，应该去哪，前端一般是 index.html

## 404 路由(保底路由)

找不到当前路由值 对应的页面。 去显示一个找不到的页面。

## 嵌套路由

路由里面还有路由。

![嵌套路由](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ecc157f0ed704e01a002d839fa95239d~tplv-k3u1fbpfcp-watermark.image)

1/1 分成两部分 第一层 1 第二层 1 。先从第一层找，找到后从一次 层的子节点中找。

类似树结构的广度优先搜素

# hash 模式 history 模式 memory 模式 路由

## hash 模式

hash:任何情况下都可以用 hash 做前端路由。

缺点是 seo 不友好。原因是服务器收不到 hash，浏览器不会把#之后的内容发给服务器。会把不同 hash 内容，都当成一个内容

![hash路由的缺点](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/28a9b4b252bc435382789561925ce2b8~tplv-k3u1fbpfcp-watermark.image)

## history 模式

只有一种情况下可以使用 history 模式

后端将所有前端路由都渲染同一页面，不能是 404 页面。

比如：后端需要实现，用户输入任何内容都到首页。只是内容不一样，根据/xxx 是什么来显示不同内容。

history 模式的缺点:IE8 以下不支持，没别的了。

默认情况下 用 a 标签 直接访问/后面的路径 要刷新页面，体验很差。

最新的 window.history API 可以解决这个问题

url hash 是# history 是 /

```javascript
// 获取 / 后面的内容
window.location.pathname;
```

## memory 模式

把 /x1 or #x1 之类的路径存到用户看不见的地方， 如：localStorage。看不见就是 memory 模式。

memory 模式适合 非浏览器路由，比如
Android/IOS 应用，没有路径。只能用 memory 模式做路由。如 react native、weex

memory 的缺点是没有 url。不可分享，相当于单机版路由

# VueRouter 的一些 API
