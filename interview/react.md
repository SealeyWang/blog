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

# Webpack

## Webpack 有哪些常见 loader 和 plugin，你用过哪些？

**常见的 loader**

- file-loader 把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件
- url-loader 和 file-loader 类似，但是能在文件很小的情况下以 base64 的方式把文件内容注入到代码中去
- source-map-load 不熟别说 加载额外的 Source Map 文件，以方便断点调试
- image-loader 加载并且压缩图片文件
- babel-loader 把 ES6 转换成 ES5
- css-loader 加载 CSS，支持模块化、压缩、文件导入等特性
- style-loader 把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS。
- eslint-loader 通过 ESLint 检查 JavaScript 代码

**常见的 plugin**

- HtmlWebpackPlugin 用来生成 html 文件
- MiniCssExtractPlugin 用来把多个 css 合并成一个 css
- babel-plugin-import 按需加载

## Webpack loader 和 plugin 的区别是什么？

loader 是加载器 ,用于加载文件。让 webpack 拥有加载和解析非 JavaScript 文件的能力

因为 webpack 把所有文件视为模块，原生 webpack 只能解析 JS 文件，别的文件想打包就需要用到 loader

plugin 是插件。

是用来加强 webpack 功能的，比如说 HtmlWebpackPlugin 插件可以生成 html
文件，MiniCssExtractPlugin 用来把 css 合并成一个 css 文件的
套路

### 用法不同

**loader 在 module.rules 中配置，作为模块的解析规则**而存在。

类型为数组，每一项都是一个 Object，里面描述了对于什么类型的文件（test），使用什么加载(loader)和使用的参数（options）

**plugin 在 plugins 中单独配置**

类型为数组，每一项是一个 plugin 的实例，参数都通过构造函数传入。

## webpack 如何按需加载代码？

babel-plugin-import 按需加载

安装`npm i babel-plugin-import -D`

在.babelrc || babel.config.js 文件 配置

```javascript
{
  "presets": [["es2015", { "modules": false }]],
  "plugins": [

    [
        'import',
        {
        libraryName: 'vant',
        libraryDirectory: 'es',
        style: true
        },
        'vant'
    ],
    [
      "component",
      {
        "libraryName": "element-ui",
        "styleLibraryName": "theme-chalk"
      }
    ]

  ]
}
```

## webpack 如何提高构建速度？

- 多入口情况下，使用 CommonsChunkPlugin 来提取公共代码
- 通过 externals 配置来提取常用库
- 利用 DllPlugin 和 DllReferencePlugin 预编译资源模块 通过 DllPlugin 来对那些我们引用但是绝对不会修改的 npm 包来进行预编译，再通过 DllReferencePlugin 将预编译的模块加载进来。
- 使用 Happypack 实现多线程加速编译
- 使用 webpack-uglify-parallel 来提升 uglifyPlugin 的压缩速度。 原理上 webpack-uglify-parallel 采用了多核并行压缩来提升压缩速度
- 使用 Tree-shaking 和 Scope Hoisting 来剔除多余代码

## webpack 怎么配置单页应用？怎么配置多页应用？

单页应用可以理解为 webpack 的标准模式，直接在 entry 中指定单页应用的入口即可.

多页应用的话，可以使用 webpack 的 AutoWebPlugin 来完成简单自动化的构建，但是前提是项目的目录结构必须遵守他预设的规范。 多页应用中要注意的是：

- 每个页面都有公共的代码，可以将这些代码抽离出来，避免重复的加载。比如，每个页面都引用了同一套 css 样式表
- 随着业务的不断扩展，页面可能会不断的追加，所以一定要让入口的配置足够灵活，避免每次添加新页面还需要修改构建配置

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
