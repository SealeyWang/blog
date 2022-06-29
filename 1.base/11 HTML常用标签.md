# HTML常用标签

* a 标签的用法
* img 标签的用法
* table 标签的用法
* 其他感想


##  a 标签的用法
HTML a 元素（或称锚元素）可以创建通向其他网页、文件、同一页面内的位置、电子邮件地址或任何其他 URL 的超链接。

href包含超链接指向的 URL 或 URL 片段。

URL: 不限于基于 Web（HTTP）的文档，也可以使用浏览器支持的任何协议。例如，在大多数浏览器中正常工作的file:、ftp:和mailto：

URL片段: 是哈希标记（#）前面的名称，哈希标记指定当前文档中的内部目标位置（HTML 元素的 ID）。

可以使用 href="#top" 或者 href="#" 链接返回到页面顶部。这种行为是 HTML5 的特性。


href 的值可以是

* 网址

https://baidu.com

http://baidu.com

//baidu.com

```html
<!--自动选择使用http  or https 协议-->
<a href="//google.com">超链接</a>
```

* 路径
  * 绝对路径  http://localhost：8080/ERP/index.jsp
  * 相对路径  '/' 开头   “/ERP/index.jsp”
  * 当前目录  './'
  * 根目录    '/'
  * 上级陌路  '../'

* 伪协议

javascript：代码；
```html
<a href="javascript:alert(1);">javascript伪协议</a>

<!-- javascript伪协议 用来实现特殊需求 点击a标签 什么都不做-->
<a href="javascript:;">空的伪协议</a>
```

mailto：邮箱
```html
<a href="mailto:1125550225@qq.com">发邮件给某</a>
```
tel：手机号

```html
<a href="tel:188888888">打电话给我</a>
```

a target 取值

* 内置
  * _blank 在新窗口打开
  * _top 在顶层 iframe 打开
  * _parent 在父级iframe 打开
  * _self 在当前窗口打开
* 程序员自己命名
  
  
  window的 name 
```html
<!--target="xxx"  如果没有窗口就 创建一个窗口 把它叫做xxx  如果有xxx窗口 就在当前xxx窗口打开链接-->
<a href="//google.com" target="xxx">window name google</a>
```
iframe 的 name
```html
<!--在 名叫 yyy 的iframe中打开 链接-->
<a href="//baidu.com" target="yyy">window name baidu</a>
<iframe style="width: 100%; height: 400px" frameborder="0" name="yyy"></iframe>
```

##  [img](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img) 标签的用法
 发送get请求，展示一张图片

可以通过只设置宽度防止图片变形

max-width:100%  用于做移动端响应式

 属性
 * alt 属性包含一条对图像的文本描述，这不是强制性的，但它可以
   * 屏幕阅读器会将这些描述读给需要使用阅读器的使用者听，让他们知道图像的含义。
   * 如果由于某种原因无法加载图像，普通浏览器也会在页面上显示alt 属性中的备用文本：例如，网络错误、内容被屏蔽或链接过期时。
   * 提供文字描述给搜索引擎使用，搜索引擎会将图片的文字描述和搜索条件进行匹配，主要用于seo。
 * height 高
 * width 宽    
 * src  图片资源路径

onload 图片加载成功
onerror 图片加载失败

## table 的用法

```html
<table>
    <thead>
    <tr>
        <th>英语</th>
        <th>翻译</th>
    </tr>
    </thead>
    <tbody>
    <tr>
        <td>hyper</td>
        <td>超级</td>
    </tr>
    <tr>
        <td>target</td>
        <td>目标</td>
    </tr>

    <tr>
        <td>reference</td>
        <td>引用</td>
    </tr>

    <tr>
        <td>frame</td>
        <td>边框，框架</td>
    </tr>
    </tbody>
    <tfoot>
        <tr>
            <td>空</td>
            <td>空</td>
        </tr>
    </tfoot>
</table>

```

table-layout : 

auto  默认值   自动计算竖排和横排的宽高 （根据内容预测宽高）   
fixed （尽量平均）

border-collapse   
用来决定表格的边框是分开的还是合并的

collapse 合并

```css
border-collapse: collapse;
```

separate 分隔

```css
border-collapse: separate;
```

border-spacing 
指定相邻单元格边框之间的距离（只适用于 边框分离模式 ）

```css 
border-spacing: 10px;
````

## 其他感想
尽快系统的过一遍 年后找工作