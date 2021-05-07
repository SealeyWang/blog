# css 知识总结

- 浏览器渲染原理
- css 动画
  - transform
  - animation

## 浏览器渲染原理

google 参考文章

[渲染树构建、布局及绘制](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-tree-construction)

[渲染性能](https://developers.google.com/web/fundamentals/performance/rendering/)

[使用 transform 来实现动画](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count)

### 浏览器渲染过程

- 根据 html 构建 HTML 树（DOM）
- 根据 css 构建 css 树（CSSOM）
- 将两棵树合并成一颗渲染树（render tree）
- Layout 布局（文档流、盒模型、计算大小和位置）
- Paint 绘制（把边框颜色、文字颜色、阴影画出来）
- Compose 合成（根据层叠关系展示画面）

![render-tree-construction](img/render-tree-construction.png)

### 浏览器渲染的三种方式

如何查看浏览器重新渲染了

可以在 chrome 浏览器中打开开发者工具 --> 按 ESC --> 弹出的窗口，左边有三个小点，点击 --> 选择 Rendering -> 勾选 Paint flashing -> 接下来样式改变的时候，如果哪个地方变绿，就说明哪里被重新渲染了，没有变绿就没有被重新渲染

![render-mode](img/render-mode.png)

1. 如果修改了元素的"layout"属性，如改变宽、高，或删除一个 dom 元素，影响其他元素的位置，浏览器会将检查其所有其他元素，然后 relayout。任何受影响的部分都需要重新绘制，而且最终绘制的元素需进行合成。
2. 如果修改“paint only”属性（例如背景图片、文字颜色或阴影等），不会影响页面布局的属性，浏览器会跳过布局，但仍将执行绘制、合成
3. 如果更改一个既不布局（layout）也不绘制（paint）的属性，则浏览器将只执行合成(composite)。这个最后的版本开销最小，最适合于应用生命周期中的高压力点，例如动画或滚动。

修改不同的 css 属性会触发不同的渲染流程
[csstriggers](https://csstriggers.com/) 网站记录了所有 css 属性触发以上哪种版本的。

### [css 动画优化](https://developers.google.com/web/fundamentals/performance/rendering/stick-to-compositor-only-properties-and-manage-layer-count) 参考谷歌文章

JS 优化

requestAnimationFrame 代替 setTimeout 或 setInterval

css 优化

使用 will-change 或 translate

## css 动画

### transform

transform 改变，使…变形

transition 过度

translate 平移

scale 缩放

rotate 旋转

skew 倾斜

```css
/* Keyword values */
transform: none;

/* Function values */
transform: matrix(1, 2, 3, 4, 5, 6);
transform: translate(12px, 50%);
transform: translateX(2em);
transform: translateY(3in);
transform: scale(2, 0.5);
transform: scaleX(2);
transform: scaleY(0.5);
transform: rotate(0.5turn);
transform: skew(30deg, 20deg);
transform: skewX(30deg);
transform: skewY(1.07rad);
transform: matrix3d(1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16);
transform: translate3d(12px, 50%, 3em);
transform: translateZ(2px);
transform: scale3d(2.5, 1.2, 0.3);
transform: scaleZ(0.3);
transform: rotate3d(1, 2, 3, 10deg);
transform: rotateX(10deg);
transform: rotateY(10deg);
transform: rotateZ(10deg);
transform: perspective(17px);

/* Multiple function values */
transform: translateX(10px) rotate(10deg) translateY(5px);

/* Global values */
transform: inherit;
transform: initial;
transform: unset;
```

### animation

使用

```css
/* @keyframes duration | timing-function | delay |
   iteration-count | direction | fill-mode | play-state | name */
animation: 3s ease-in 1s 2 reverse both paused slidein;

/* @keyframes duration | timing-function | delay | name */
animation: 3s linear 1s slidein;

/* @keyframes duration | name */
animation: 3s slidein;

/* 定义开始和结束关键帧 */
@keyframes slidein {
  from {
    transform: scaleX(0);
  }
  to {
    transform: scaleX(1);
  }
}

/* 定义不同执行进度时的关键帧 */
@keyframes xxx {
  0% {
    transform: none;
  }
  66.66% {
    transform: translateX(200px);
  }
  100% {
    transform: translateX(200px) translateY(100px);
  }
}
```

animation 属性是 animation-name，animation-duration, animation-timing-function，animation-delay，animation-iteration-count，animation-direction，animation-fill-mode 和 animation-play-state 属性的一个简写属性形式。

- animation-name:通过@keyframes 定义的动画的名称
- animation-duration：动画持续时间（transition 也可以配合 transform 定义动画时间）
- animation-timing-function:动画过度方式（如淡入 淡出），作用于一个关键帧周期而非整个动画周期
  常用属性

```css
/* Keyword values */
animation-timing-function: ease;
animation-timing-function: ease-in;
animation-timing-function: ease-out;
animation-timing-function: ease-in-out;
animation-timing-function: linear;
animation-timing-function: step-start;
animation-timing-function: step-end;
```

- animation-delay：延迟
- animation-direction:执行次数
- animation-direction: 方向（类似 flex-direction）
  - normal （正常）| reverse （反转）| alternate（来回交替） | alternate-reverse（反转后来回交替）
- animation-fill-mode: 动画在执行之前和之后如何处理 css 样式（关键词状态，动画执行前 & 动画执行后状态，执行前位置、样式，执行后位置、样式）
  - 补充 css 样式包含位置、样式
  - none （动画执行前样式，样式、位置不变。动画执行后，恢复执行前样式和位置）
  - forwards（动画执行前->样式位置不变，执行后->保持动画执行后的样式和位置）
  - backwards（动画执行前->是执行后的样式，执行前的位置。 动画执行后->恢复执行前的样式、位置）
  - both 动画执行前->执行后的样式、执行前的位置。动画执行后->执行后的样式、位置。
- animation-play-state： 定义一个动画是否运行或者暂停
