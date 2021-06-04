# 如何创建

```jsx
const Hello = (props) => {
  return <div>props.message</div>;
};

const Hello = (props) => <div>props.message</div>;

const Hello = (props) => <div>props.message</div>;

function Hello(props) {
  return <div>{props.message}</div>;
}
```

# 没有 state 怎么办

React v16.8.0 推出 Hooks API

其中有一个 API 叫 useState 可以解决没有 state 的问题

```jsx
const [n, setN] = React.useState(0);
```

# 没有生命周期怎么办 Hooks API

React v16.8.0 推出 Hooks API

中有一个 API 叫 useEffect 可以解决生命周期的问题

Effect ： 函数式编程的专有名词

使用 React.useEffect 模拟
componentDidMount
componentDidUpdate
componentWillUnMount

```jsx
import React, { useState, useEffect } from "react";

const App3 = () => {
  const [n, setN] = useState(0);
  const [m, setM] = useState(0);

  // 模拟 componentDidMount
  useEffect(() => {
    console.log("App3 mount");
  }, []);

  // 模拟 componentDidUpdate
  useEffect(() => {
    console.log("m 或 n 变了");
    //   m n 初始化也算 变
  }, [m, n]);

  const addN = () => {
    setN(n + 1);
  };
  const addM = () => {
    setM(m + 1);
  };

  let [childVisible, setChildVisible] = React.useState(true);
  const hideChild = () => setChildVisible(false);
  const visibleChild = () => setChildVisible(true);

  return (
    <div className="App">
      App3 <br />
      n= {n} <br />m = {m}
      <br />
      <button onClick={addN}>n+1</button>
      <button onClick={addM}>m+1</button>
      <br />
      {childVisible ? <Child /> : null}
      <button onClick={visibleChild}>visible</button>
      <button onClick={hideChild}>hide</button>
    </div>
  );
};

const Child = () => {
  // 模拟 componentWillUnmount
  useEffect(() => {
    console.log("Child第一次渲染");
    return () => {
      console.log("Child组件要死了");
    };
  });
  return <div>Child</div>;
};

export default App3;
```

其它生命周期怎么模拟

**constructor**
函数组件执行的时候，就相当于 constructor

**shouldComponentUpdate**
后面的 React.memo 和 useMemo 可以解决

render

含住组件的返回值就是 render 的值

## 自定义 Hook 完全模拟 componentDidUpdate （state props 初始化不算变化）

React 规范 use 开头的函数就是自定义 Hook

实现功能

```jsx
// 自定义Hook模拟 componentDidUpdate  n初始化不算变化 start
const useX = (n) => {
  const [nUpdateCount, setNUpdateCount] = useState(0);
  useEffect(() => {
    setNUpdateCount((nUpdateCountX) => nUpdateCountX + 1);
  }, [n]);

  return {
    nUpdateCount,
  };
};
const { nUpdateCount } = useX(n);
useEffect(() => {
  if (nUpdateCount > 1) {
    console.log("n 变化了");
  }
}, [nUpdateCount]);

// 模拟 componentDidUpdate  n初始化不算变化end
```

优化代码,[完整代码](https://github.com/CaoBaoWang/react-demo1/commit/bbb204d983a8759b285333985abbd105cc3f6a88)

```jsx
import { useEffect, useState } from "react";

const useUpdate = (fn, dep) => {
  const [count, setCount] = useState(0);
  useEffect(() => {
    setCount((countX) => countX + 1);
  }, [dep]);

  useEffect(() => {
    if (count > 1) {
      fn();
    }
  }, [count, fn]);
};

export default useUpdate;

import useUpdate from "./useUpdate";
useUpdate(() => {
  console.log("n 变化了");
}, n);
```
