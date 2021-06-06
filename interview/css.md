# 两种盒模型 ？

css 盒模型 从外到内 依次是 margin border padding content

盒模型分为

`box-sizing: border-box;`

`box-sizing: content-box;`

- border-box （边框盒模型）
  - 边框是盒子的边界 元素的 width 是内容的宽度
  - width = contentWidth + padding + border
- content-box（内容盒模型）
  - 内容就是盒子的边界 元素的 width 是 内容的宽度
  - width = contentWidth

平常用 border-box, 因为美工给的标注，基本不会有内容的宽高 ，用 border-box 更方便。

# 如何垂直居中

[七种方式实现垂直居中](https://www.yuque.com/docs/share/708bd899-0c46-47ea-a94c-d7a189c0f7dc?#)

1. table 自带垂直居中
2. 100% 高度的 after before 加上 inline block
3. div 装成 table (个人感觉不好)
4. margin-top -50%
5. translate -50%
6. absolute margin auto
7. flex

关键点

1.  table 布局
2.  100% 高度的 after before 加上 inline block
3.  div 以 table 方式布局
    1.  display : table
    2.  display: table-row
    3.  display: table-cell
4.  margin-top -50% 在绝对布局下使用
    1.  position: absolute;
    2.  top: 50%; left: 50%;
    3.  height: 100px; margin-top: -50px;
5.  `transform:translate(-50% , -50%)` 和 4 差不多
6.  absolute margin auto
7.  主轴和交叉轴对齐方式为: 居中对齐
    1. justify-content: center; /_主轴居中_/
    2. align-items: center; /_交叉轴居中_/

# BFC 是什么

块格化上下文 (Block Formatting Context，BFC)。
可以理解为独立的布局容器，里面的元素不会影响外面的元素

## BFC 触发条件

1. 浮动元素（元素的 float 不是 none）
2. 绝对定位元素（元素的 position 为 absolute 或 fixed）
3. 行内块元素（元素的 display 为 inline-block）
4. overflow 值不为 visible 的块元素
5. 弹性元素（display 为 flex 或 inline-flex 元素的直接子元素）

# CSS 选择器优先级

1. 越具体优先级越高
2. 同样优先级写在后面的覆盖写在前面的
3. !important 优先级最高，但是要少用

# 清楚浮动

```html
<style>
  .clearfix:after {
    content: "";
    display: block; /*或者 table*/
    clear: both;
  }
  .clearfix {
    zoom: 1; /* IE 兼容*/
  }
</style>
```
