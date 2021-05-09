# 如何引用 React

需要引入 React ReactDOM。

```javascript



yarn add react react-dom
import React from 'react'
import ReactDOM from 'react-dom'

```

cdn 和 webpack 引入都不推荐，
最好使用 create-react-app

```
yarn global add create-react-app

create-react-app  react-demo1
```

## cjs 和 umd 区别

cjs 全程 CommonJS， 是 Node.js 支持的模块规范

umd 是统一模块定义，兼容各种模块规范（含浏览器）

理论上优先使用 umd，同时支持 Node.js 和浏览器

最新的模块规范是使用 import 和 export 关键字

# 函数与延迟执行 —— 延迟

```javascript
let n = 0;
const App1 = React.createElement("div", null, n);
// App1 是一个React元素

App2 = () => React.createElement("div", null, n);
// App2 是一个React 函数组件
```

App2 是延迟执行的代码，会在被调用的时候执行

注意 这是代码执行的时机，不是说同步和异步

# JSX 的用法

X 表示扩展，jsx 是 js 扩展板。

babel-loader 集成 了 jsx-loader

webpack 内置了 babel-loader，所以能直接用

JSX 语法 ： 插入 JS 的变量(包括对象) 或 语法 需要用{} 包裹起来 。 没了

```javascript
const App = () =>
  React.createElement("div", { className: "red" }, [
    n,
    React.createElement(
      "button",
      {
        onClick: () => {
          n++;
          console.log(n);
          ReactDOM.render(App(), root);
        },
      },
      "+1"
    ),
  ]);

const AppJSX = () => (
  <div className={"red"}>
    {n}
    <button
      onClick={() => {
        n++;
        ReactDOM.render(AppJSX(), root);
      }}
    >
      +1
    </button>
  </div>
);

ReactDOM.render(<AppJSX />, root);
```

注意 className 不要写错 。 插入对象{{name:'wsl'}} 。 return 要加()

# 条件判断与循环

```

```
