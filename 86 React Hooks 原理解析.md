React v16.8.0 推出 Hooks API。总结他的功能就是：让 FunctionalComponent 具有 ClassComponent 的功能。

# useState 原理

## 总结在前面

\_state 真实名称 memorizedState, 本应该用链表实现，为了方便用了数组和 index

- 每个函数组件对应一个 React 节点（FiberNode）
- 每个节点保存 state 和 index
- useState 会读取 state[index]
- index 由 useState 出现的顺序觉得
- setState 会修改 state，并触发更新

[React Hooks 原理](https://github.com/brickspert/blog/issues/26)

## useState 作用

传入初始值，创建一个数组

index 0 是当前数据的引用

index 1 是设置当前数据的函数，每次调用一次都会创建新的 state 然后重新渲染

```jsx
function App() {
  const [n, setN] = React.useState(0);
  return (
    <div>
      {n}
      <button onClick={() => setN(n + 1)}>+1</button>
    </div>
  );
}

ReactDOM.render(<App />, rootElement);
```

点击 button 会发生什么?

调用 setN -> 再次 render<App /> -> **调用 App()** -> 得到虚拟 div -> DOM Diff -> 更新真的 div

![每次调用都会运行useState](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/08dfeb1fb7fb4f96b408f91693e35b67~tplv-k3u1fbpfcp-watermark.image)

每次都会调用 App() 运行 useState(0)

setN 一定会修改数据 x 将 n+1 存入 x

setN 一定会触发<App /> 重新渲染 re-render

useState 会从新 x 读取 n 的最新值

每个组件有自己的数据 x，一般称为 state

## 实现最简单的 useState

github commit[实现最简单的 React.useState](https://github.com/CaoBaoWang/react-hooks-demo1/blob/d022a60443a5c29e8e4d5f72770895fe428a81e9/src/index.js)代码

```jsx
import React from "react";
import ReactDom from "react-dom";

const root = document.getElementById("root");

let _state;

const myUseState = (initialValue) => {
  _state = _state === undefined ? initialValue : _state;
  const setState = (newValue) => {
    _state = newValue;
    render();
  };
  return [_state, setState];
};

const render = () => {
  ReactDom.render(<App />, root);
};

const App = () => {
  const [n, setN] = myUseState(0);

  const addN = () => {
    setN(n + 1);
  };

  return (
    <div>
      {n}
      <button onClick={addN}>+1</button>
    </div>
  );
};

render();
```

这个实现有问题，只能使用一个 State

## 一个组件用了多个 useState 怎么办

数据都在\_state 里 都是一样的值，用不了多个。

解决一下吧

**\_state 改成对象。**`_state= {n:0,m:0}`
但是不知道 key 呀，要么把 key 传过去。很麻烦的

**\_state 改成数组。** 可以。不过 React Hooks 是用链表
`_state = [0,0]`

缺点 useState 调用顺序不能变。每次渲染必须保证 useState 调用顺序一致

所 React 不允许出现如下代码

```jsx
let m, setM;
if (n % 2 === 1) {
  [m, setM] = React.useState(0);
}
```

React 把\_state & index 放在对应的虚拟节点对象上。让每个对象都有自己的\_state & index

组件更新流程
![组件更新流程](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/493938f0479f494fa9526529cd149726~tplv-k3u1fbpfcp-watermark.image)

github commit[解决 myUseState 存储多个值](https://github.com/CaoBaoWang/react-hooks-demo1/commit/885dc107fdaffb93e3b2256d1d0d80dd8d9d5da7)

```jsx
import React from "react";
import ReactDom from "react-dom";

const root = document.getElementById("root");

let _state = [];
let index = 0;

const myUseState = (initialValue) => {
  const currentIndex = index;
  _state[currentIndex] =
    _state[currentIndex] === undefined ? initialValue : _state[currentIndex];

  const setState = (newValue) => {
    _state[currentIndex] = newValue;
    render();
  };
  index++;
  return [_state[currentIndex], setState];
};

const render = () => {
  index = 0;

  ReactDom.render(<App />, root);
};

const App = () => {
  const [n, setN] = myUseState(0);
  const [m, setM] = myUseState(0);

  console.log(`_state=${_state}`);

  const addN = () => setN(n + 1);
  const addM = () => setM(m + 1);

  return (
    <div>
      {n} <br />
      {m}
      <button onClick={addN}>n+1</button>
      <button onClick={addM}>m+1</button>
    </div>
  );
};

render();
```

# useRef 的作用

useState 改变数据都会生成新的 state，保留原 state。
异步操作只有原数据的引用，变更后，新数据拿不到

[useState 示例](https://codesandbox.io/s/fancy-fast-qh4yd)

useRef 改变数据时，不会创建新数据，会修改原数据。异步操作直接用原数据的引用，拿到最新值
[useRef 示例](https://codesandbox.io/s/flamboyant-elion-jvtl2?file=/src/index.js:486-504)

iseRef 仅在当前组件中

```jsx
const nRef = React.useRef(0);
<p>{nRef.current} 这里并不能实时更新</p>;

// 非要强制更新 （最好不要）


const nRef = React.useRef(0);
const update = React.useState()[1];


<p>{nRef.current} 能实时更新</p>

// 点击后改Ref同时改state触发刷新
<button onClick={() => ((nRef.current += 1), update(nRef.current))}>+1</button>;
```

[强制更新示例](https://codesandbox.io/s/vibrant-sunset-f1lld?file=/src/index.js)

# useContext 的作用

useContext 和 useRef 类似 。

每次重新 render 不会创建新数据

useContext 可以跨组件共享数据

[完整代码](https://codesandbox.io/s/wizardly-grothendieck-ek0i5?file=/src/index.js)

```jsx
const themeContext = React.createContext(null);

function App() {
  const [theme, setTheme] = React.useState("red");

  return (
    <themeContext.Provider value={{ theme, setTheme }}>
      <div className={`App ${theme}`}>
        <p>{theme}</p>
        <div>
          <ChildA />
        </div>
        <div>
          <ChildB />
        </div>
      </div>
    </themeContext.Provider>
  );
}

function ChildA() {
  const { setTheme } = React.useContext(themeContext);
  return (
    <div>
      <button onClick={() => setTheme("red")}>red</button>
    </div>
  );
}

function ChildB() {
  const { setTheme } = React.useContext(themeContext);
  return (
    <div>
      <button onClick={() => setTheme("blue")}>blue</button>
    </div>
  );
}
```

Vue3 ref 变化刷新页面
