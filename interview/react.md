# React 生命周期

- constructor
- render
- componentDidMount
- componentDidUpdate
- componentWillUnmount

# React 组件通信

父子靠 props 传函数

爷孙可以穿两次 props

任意组件用 Redux（也可以自己写一个 eventBus）

如何用 Redux 通信？

通过 store.dispatch 触发 action , reducers 处理完 action，返回新的 state 。 通过 store.subscribe(render) 当 state 变化的时候，重新渲染，所有组件就感知到了变化。 （可以用 useEffect 监听变化 做一些操作，不知道对不对）

# shouldComponentUpdate 有什么用

没有必要更新 UI 的时 提高渲染新能

如果 shouldComponentUpdate() 返回 false，则不会调用 render()。 以提高渲染性能

# 虚拟 DOM 是什么？

虚拟 DOM 就是用来模拟 DOM 的一个对象，这个对象拥有一些重要属性，并且更新 UI 主要就是通过对比（DIFF）旧的虚拟 DOM 树 和新的虚拟 DOM 树的区别完成的。

## 什么是虚拟 DOM

在 React 中，render 执行的结果得到的并不是真正的 DOM 节点，结果仅仅是轻量级的 JavaScript 对象，我们称之为 virtual DOM。

# Redux 是什么

Redux 是 JavaScript 状态容器，提供可预测化的状态管理。重点是『状态管理』。

## Action

Action/Reducer/Store/单向数据流

Action :是 store 数据的唯一来源 。

一般来说你会通过 `store.dispatch()` 将 action 传到 store。

action actionType 是对数据操作的类型 payload 是具体值

## Reducer

1. Reducer 定义状态的变化如何响应 actions 并发送到 store
2. 核心概念的名字和作用：Action/Reducer/Store/单向数据流
3. 常用 API：store.dispatch(action)/store.getState()

## Store

根据已有的 reducers 创建 store
`const store = createStore(reducers);`

- 维持应用的 state；
- 提供 getState() 方法获取 state；
- 提供 dispatch(action) 方法更新 state；
- 通过 subscribe(listener) 注册监听器;
- 通过 subscribe(listener) 返回的函数注销监听器。

## 单向数据流

react 的 state 都是单向数据流 ， 不直接修改原 state，而是创建新的 state 然后重新 render

# TypeScript

## never 类型是什么？

不应该出现的类型, switch 判断 type 时候 没有匹配上 返回值设为 never 。 好处是 当修改了类型后，编译通不过

```javascript
interface Foo {
  type: "foo";
}

interface Bar {
  type: "bar";
}

type All = Foo | Bar;

function handleValue(val: All) {
  switch (val.type) {
    case "foo":
      // 这里 val 被收窄为 Foo
      break;
    case "bar":
      // val 在这里是 Bar
      break;
    default:
      // val 在这里是 never
      const exhaustiveCheck: never = val;
      break;
  }
}

//  修改类型后,多了一个类型Baz 如果没有处理Switch 编译会报错，never不是 All
type All = Foo | Bar | Baz;
```

## TypeScript 比起 JavaScript 有什么优点？

TypeScript 提供了类型约束，因此更可控、更容易重构、更适合大型项目、更容易维护

# React 合成事件

就是 react 把所有的事件都代理到 document 节点

然后先把所有的事件按节点的 key value 为事件存储起来，然后执行的时候构造一个合成的 event 返回给存储的事件回调

执行下一个宏任务 or 微任务的时候他就呗销毁了，它还用了事件池去存储空的合成事件，提高生成合成事件对象的效率

//////////////////

- react 把所有的事件都代理到 document 节点
- 然后先把所有的事件按节点的 key value 为事件存储起来，
- 执行的时候构造一个合成的 event 返回给存储的事件回调
- 用了事件池去存储空的合成事件，提高生成合成事件对象的效率

## React 合成事件概念

React 合成事件：是 React 模拟原生 DOM 事件所有能力的一个事件对象。-> 浏览器原生事件跨浏览器包装器。 它根据 W3C 规范 来定义合成事件，兼容所有浏览器，拥有与浏览器原生事件相同的接口。

`const button = <button onClick={handleClick}>Leo 按钮</button>`
在 React 中，所有事件都是合成的，不是原生 DOM 事件，但可以通过 e.nativeEvent 属性获取 DOM 事件。

```javascript
const handleClick = (e) => console.log(e.nativeEvent);
const button = <button onClick={handleClick}>Leo 按钮</button>;
```

**为什么会出现这个技术**

1. 进行浏览器兼容，实现更好的跨平台
   1. React 采用的是顶层事件代理机制，能够保证冒泡一致性，可以跨浏览器执行。React 提供的合成事件用来抹平不同浏览器事件对象之间的差异，将不同平台事件模拟合成事件。
2. 避免垃圾回收
   1. 事件对象可能会被频繁创建和回收，因此 React 引入事件池，在事件池中获取或释放事件对象。即 React 事件对象不会被释放掉，而是存放进一个数组中，当事件触发，就从这个数组中弹出，避免频繁地去创建和销毁(垃圾回收)。
