# html入门笔记1

* html 是谁发明的
* html 起手（最基本的html代码）
* 常用章节标签
* 全局属性
* 常用内容标签


## html 是谁发明的
html 由Tim Berners-Lee（蒂姆·伯纳斯·李）发明 

2004年，英女皇伊丽莎白二世向伯纳斯·李颁发不列颠帝国勋章的爵级司令勋章。

可以叫他李爵士

## html 起手
```html
<!-- 文档类型 -->
<!doctype html>
<!-- 语言 -->
<html lang="en">
<head>
<!-- 文件字符编码 -->
    <meta charset="UTF-8">


<!--
    name="viewport"  能优化移动浏览器的显示。 
    通过content 中设置的一些属性
    width=device-width 表示宽度是设备屏幕的宽度    
    user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0"
    分别是 禁用用户缩放  初始缩放1.0  最大缩放1.0 最小缩放1.0

 -->
    <meta name="viewport" 
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
          <!-- 让ie浏览器 使用最新的ie内核 -->
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <!-- 网页标题 -->
    <title>Document</title>
</head>
<body>

</body>
</html>

```


移动端常见属性
* width：宽度（数值 / device-width）（范围从200 到10,000，默认为980 像素）
* height：高度（数值 / device-height）（范围从223 到10,000）
* initial-scale：初始的缩放比例 （范围从>0 到10）
* minimum-scale：允许用户缩放到的最小比例
* maximum-scale：允许用户缩放到的最大比例
* user-scalable：用户是否可以手动缩 (no,yes)

## 常用章节标签

* 标题 h1~h6
* 章节 section
* 文章 article
* 段落 p
* 头部 header
* 脚部 footer
* 主要内容 main
* 旁支内容 aside
* 划分 div

## 全局属性
* class 
* contenteditable  可以让任何元素可编辑
* hidden  隐藏元素 让元素的display的值为 none 
* id   用于元素的的使用 查找 
* style 可以设置css样式
* tabindex 设置tabindex 可以 按键盘上的tab键  访问元素 
* title  用来显示全部内容 
  
### class
class 的值是一个以空格分隔的元素的类名（classes ）列表，它允许 CSS 和 Javascript 通过类选择器 (class selectors) 或DOM方法( document.getElementsByClassName)来选择和访问特定的元素。

### contenteditable
我们可以将style标签放到body中。然后将style的display属性改成block。

这样就可以在页面中显示style里面的内容了。通过中方式，我们可以在页面上直接修改css。


tabindex 接收number 值
按tab键 访问tabindex的顺序
正的表示顺序访问 
0 是最后一个访问
-1 是永远访问不到 


## 常用内容标签

* ol + li 无序列表
* ul + li 有序列表
* dl + dt + dd 用于描述术语
* pre 保留空格和换行（默认html只保留一个空格）
* hr 水平分隔线
* br 换行
* em 强调（语气） 默认样式是 文字变斜
* strong 加粗 （内容强调）
* code 用于显示代码 
* quote 内联引用
* blockqoute 块级引用

单条术语描述 更多例子参考 [HTML dl 元素](https://developer.mozilla.org/zh-cn/docs/web/html/element/dl)
```html
<dl>
  <dt>Firefox</dt>
  <dd>A free, open source, cross-platform, graphical web browser
      developed by the Mozilla Corporation and hundreds of volunteers.
  </dd>
  <!-- other terms and definitions -->
</dl>
```
