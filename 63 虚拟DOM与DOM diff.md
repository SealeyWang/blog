# 虚拟 DOM 是什么

一个能代表 DOM 树的对象，通常含有标签名，标签上的属性、事件监听和子元素们，以及其他属性

# 虚拟 DOM 有什么优点

1. 能减少不必要的 DOM 操作
2. 能跨平台宣渲染

**减少 DOM 操作的次数**

虚拟 DOM 可以把多次操作合并成一次操作，
比如添加 1000 个节点，一个个操作就会慢。
而把 1000 个节点一次性放到 DOM 上就会快

**减少 DOM 操作的范围**

虚拟 DOM 借助 DOM diff 把多余的操作省略
添加 1000 个节点 其实只有 20 个节点是新增的。
Vue React 借助虚拟 DOM 对比 只把 20 个添加到 DOM 上

## 虚拟 DOM 的优点 跨平台

虚拟 DOM 本质上是一个对象，可以变成 html 元素 或者 Android 里 View 。或者 IOS 、小程序应用

# 虚拟 DOM 有什么缺点

需要额外的创建函数 如 React createElement 或 Vue h
但是，可以通过 JSX 或 .vue 文件简化写法。但是又需要 babel / vue-loader 去编译，这样就严重依赖打包工具，增加额外的构建流程

# 真实的 DOM 和虚拟 DOM 谁更慢

规模较小的时候虚拟 DOM 是快的，规模较大的时候，还是原生 DOM 快

JS 原生操作在 十万个节点 的时候可以在 2 秒左右, react 插入 1 十万个元素要 30 秒左右（可以优化）。vue 不用优化就接近原生的速度（插入十万个 2 秒左右）

react 1000 个 的时候比原生快

# DOM diff 是什么

虚拟 DOM 的对比算法

DOM diff 就是一个 函数 patch
patches = patch (oldVNode, newVNode)

patches 是需要运行的 DOM 操作

patches 可能是这样

```javascript
[
    {type:'INSERT',vNode:....}
    {type:'TEXT',vNode:....}
    {type:'PROPS',propsPatch:[....]}
]
```

## DOM diff 的大概逻辑

### tree diff

- 将新旧两棵树逐层对比，找出需要更新的节点，并记录。
- 如果节点是组件就进入 Component diff
- 如果节点是标签就进入 Element diff

### Component diff

- 如果节点是组件，就先看组件类型
- 类型不同直接替换
- 类型相同只更新属性
- 然后深入组件做 tree diff （递归）

### Element diff

- 如果节点是原生标签，看标签名
- 标签名不同直接替换，相同只更新属性
- 然后进入标签后代做 Tree diff （递归）

# DOM diff 的优点

与虚拟 DOM 结合 对比出新、旧虚拟 DOM 的不同之处，以及分析出需要改变的操作，可以减少不必要 DOM 的操作，在一定规模性，有利于性能的提升

# DOM diff 的问题

同级别对比存在 bug。在这是 vue 中加 key 的原因。
[Vue2.0 v-for 中 :key 到底有什么用？](https://www.zhihu.com/question/61064119/answer/766607894)

[1,2,3] [1,3]
默认用 index 对比，计算机会这样理解

- 1 没变
- 2 变成 3
- 3 删掉

所以不能用 index 做 key

如果加 key 用 key 对比

- 1 没变
- 2 没有了 删掉
- 3 没变

[React 虚拟 Dom 和 diff 算法](https://juejin.cn/post/6844903529161850893)

[React 源码剖析系列 － 不可思议的 react diff](https://zhuanlan.zhihu.com/p/20346379)

# 为什么虚拟 DOM 比 DOM 快?

**减少 DOM 操作的次数**

虚拟 DOM 可以把多次操作合并成一次操作，
比如添加 1000 个节点，一个个操作就会慢。
而把 1000 个节点一次性放到 DOM 上就会快

**减少 DOM 操作的范围**

虚拟 DOM 借助 DOM diff 把多余的操作省略
添加 1000 个节点 其实只有 20 个节点是新增的。
Vue React 借助虚拟 DOM 对比 只把 20 个添加到 DOM 上

所以虚拟 DOM 比 DOM 快
