# 如何理解 HTML 语义化 ？

总结： html 语义化就是正确的使用 html 标签

段落写用 p 、按钮用 button 、标题用 h1~h6，用 header、main footer 作为头部 主要内容 和脚部的根标签。

# meta viewport 是做什么用的，怎么写？

viewport:视口、当前课件的计算机图形区域。在 web 中，通常是浏览器窗口（不包括浏览器 UI）, 指正在浏览的网页

在大多数移动设备中，浏览器是全屏的，viewport 是整个屏幕的大小。

meta viewport 是用来定义视口（viewport）的

```html
<meta
  name="viewport"
  content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no,viewport-fit=cover"
/>
```

width 定义视口大小

initial-scale 定义初始缩放

maximum-scale minimum-scale 定义最大最小缩放

user-scalable=no 移动端 禁止双指或双击放大

viewport-fit=cover 适配 iphone X 刘海

# 用过那些 html5 标签？

header main footer aside （侧边栏） section (章节)
canvas(画布)

video source 标签

```html
<video controls width="250">
  <source src="/media/cc0-videos/flower.webm" type="video/webm" />

  <source src="/media/cc0-videos/flower.mp4" type="video/mp4" />

  Sorry, your browser doesn't support embedded videos.
</video>
```

# h5 是什么

一种叫法 指 移动端页面
