- 状态 useState
- 副作用 useEffect
  - useLayoutEffect
- 上下文 useContext
- Redux useReducer
- 记忆 useMemo
  - useCallback
- 引用 useRef
  - useImperativeHandle
- 自定义 Hook
  - useDebugValue

# useState

基本使用

```jsx
const [n, setN] = React.useState(0);

<button onClick={() => setN(n + 1)}>+1</button>;
```

## 如果 state 是一个对象，不能局部更新

useState 不会合并属性
useReducers 也不会

useState 不合并属性[演示](https://codesandbox.io/s/snowy-pine-7gdw2)

```jsx
// 相当于把user设置成一个新对象
setUser({
  name: "Jack",
});
```

setState(obj) 时 如果**地址不变** react 就认为数据没有变化

通过把原来数据赋值到新数据上，解决不能局部更新的问题[演示](https://codesandbox.io/s/restless-fire-bi14m?file=/src/index.js:0-510)

主要是 `...user` 这一行代码

```jsx
setUser({
  // 需要手动合并属性
  ...user,
  name: "Jack",
});
```

完整代码

```jsx
import React, { useState } from "react";
import ReactDOM from "react-dom";

function App() {
  const [user, setUser] = useState({ name: "Frank", age: 18 });
  const onClick = () => {
    setUser({
      // 需要手动合并属性
      ...user,
      name: "Jack",
    });
  };
  return (
    <div className="App">
      <h1>{user.name}</h1>
      <h2>{user.age}</h2>
      <button onClick={onClick}>Click</button>
    </div>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

## useState 接受函数

函数返回初始 state ，只执行一次 避免使用对象时，多次被 JS 引擎解析。因为 react 组件会执行多次

```jsx
const [state, setState] = useState(() => {
  return initialState;
});
```

```jsx
const [user, setUser] = useState({ name: "Frank", age: 18 });
// 变更为
const [user, setUser] = useState(() => {
  return { name: "Frank", age: 18 };
});
```

## setState 接受函数

为什么要给 setState 一个函数

```jsx
const [n, setN] = useState(0);
// 当前组件的n 做了两次+1 操作 如果当前n值为1
setN(n + 1); // 此处等于n的值 1 + 1  创建一个新值
setN(n + 1); // 此处等于n的值 1 + 1  创建一个新值。
//  相同的操作做了两次。n只会 +1

// 使用函数 可以避免这样的情况  所以优先使用函数
setN((i) => i + 1); //把最新的n 传给i  然后+1
setN((i) => i + 1);
// n 会+2
```

# useEffect (副作用)

为函数组件增加了执行副作用的能力

## 副作用的解释

```js
function f1() {} // 没有作用的函数

//  有副作用的函数，就是依赖了不知道从哪来的东西的函数， 有可能产生意外的结果
function f2() {
  console.log(1); // 依赖了 console， 依赖的东西不是函数传进来的   如果有一天console被改了 有可能出现意想不到的结果
}

//没有副作用的 函数（也叫纯函数）。 不依赖外部属性和方法的函数
function f3(a, b) {
  return a + b;
}
```

useEffect(副作用)对环境的改变就是副作用。 比如
`document.title = xxx` 。

其实叫 afterRender 更好理解。每次 render 之后就会调用

```js
import React from "react";
import "./styles.css";
import { useEffect } from "react";

// 先打印2 再打印1
export default function App() {
  useEffect(() => {
    console.log(1);
  });

  return (
    <div className="App">
      {console.log(2)}
      <h1>Hello CodeSandbox</h1>
      <h2>Start editing to see some magic happen!</h2>
    </div>
  );
}
```

使用 useEffect 可以模拟 类组件的生命周期

- componentDidMount, []作为第二个参数
- componentDidUpdate [n], 指定 n 变化了触发函数
- componentWillUnmount 通过 return 函数。组件 unmount 之前会执行 return 的函数
- 存在多个 useEffect 会按出现顺序依次执行

## 模拟 componentDidMount

```jsx
// 模拟 componentDidMount
useEffect(() => {
  console.log("App3 mount");
}, []); //[] 里变量变化执行 => 不会执行
```

## 模拟 componentDidUpdate

```jsx
const [n, setN] = useState(0);
const [m, setM] = useState(0);
// 模拟 componentDidUpdate
useEffect(() => {
  console.log("m 或 n 变了");
  //   m n 初始化也算 变
}, [m, n]);
```

自定义 Hook 完全模拟 componentDidUpdate （state props 初始化不算变化）

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

## 模拟 componentWillUnmount

```jsx
// 模拟 componentWillUnmount
useEffect(() => {
  console.log("Child渲染了");
  return () => {
    console.log("Child组件要死了");
  };
}); //任何一个state变化都执行
```

```jsx
useEffect(() => {
  console.log("第一次渲染执行");
}, []); //[] 里变量变化执行 => 不会执行
```

## useLayoutEffect

useEffect 在浏览器渲染完成后执行

useLayoutEffect 在浏览器渲染前执行

useLayoutEffect 总是比 useEffect 先执行

useLayoutEffect 里的人物最好影响了 layout

最好优先使用 useEffect(优先渲染，避免用户卡顿)

```jsx
useLayoutEffect(() => {
  // 改成 useEffect 试试
  console.log("......");
});
```

# useContext 上下文

全局变量是全局的上下文

上下文是局部的全局变量

useContext 和 useRef 类似 。

每次重新 render 不会创建新数据

useContext 可以跨组件共享数据

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

# useReducer

用来践行 Flux/Redux 的思想

- 创建初始值 initialState
- 创建所有操作 reducer(state,action)
- 传给 useReducer，得到 读(state)写(dispatch)API
- 调用 dispatch({type:'add'})\
- useReducer 相当于 useState 的复杂版

```jsx
const initial = {
  n: 0,
};
const reducer = (state, action) => {
  // action是dispatch(obj)中的obj
  if (action.type === "add") {
    return { n: state.n + action.number };
  } else if (action.type === "multi") {
    return { n: state.n * 2 };
  } else {
    throw new Error("unknown type");
  }
};
const [state, dispatch] = useReducer(reducer, initial);
const onClick = () => {
  dispatch({ type: "add", number: 1 });
};
```

## useReducer 代替 Redux

Redux 用来做是全局状态管理的，类似 Vuex

- 将数据集中在一个 store 对象
- 将所有操作集中在 reducer
- 创建一个 Context
- 创建对数据的读写 API
- 将读写 API 放到 Context
- 用 Context.Provider 将 Context 提供给所有组件
- 组件用 useContext 获取读写 API

```jsx
// 1.将数据集中在一个 store 对象
const store = {
  user: null,
  book: null,
  movies: null,
};
// 将所有操作集中在 reducer
function reducer(state, action) {
  switch(action.type){
    case "setUser"
    return {...state,user:action.user}
    case "setBooks"
    return {...state,user:action.books}
    case "setMovies"
    return {...state,user:action.movies}
  }
}
//  创建一个 Context
const Context = React.createContext(null);
function App() {
  // 创建读写API
  const [state, dispatch] = useReducer(reducer, store);

  const api = { state, dispatch };
  return (
    // 用 Context.Provider 将 Context 提供给所有组件
    <Context.Provider value={api}>
      <User />
      <hr />
      <Books />
      <Movies />
    </Context.Provider>
  );
}

function User() {
  // 组件用 useContext 获取读写 API
  const { state, dispatch } = useContext(Context);
  useEffect(() => {
    ajax("/user").then(user => {
      dispatch({ type: "setUser", user: user });
    });
  }, []);
  return (
    <div>
      <h1>个人信息</h1>
      <div>name: {state.user ? state.user.name : ""}</div>
    </div>
  );
}

function Books(){
  // ........
 const { state, dispatch } = useContext(Context);
  useEffect(() => {
    ajax("/books").then(books => {
      dispatch({ type: "setBooks", books: books });
    });
  }, []);

  // ........
}
function Movies(){
  // ........
 const { state, dispatch } = useContext(Context);
  useEffect(() => {
    ajax("/movies").then(movies => {
      dispatch({ type: "setMovies", movies: movies });
    });
  }, []);
  // ........
}



// 假 ajax
// 两秒钟后，根据 path 返回一个对象，必定成功不会失败
function ajax(path) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (path === "/user") {
        resolve({
          id: 1,
          name: "Frank"
        });
      } else if (path === "/books") {
        resolve([
          {
            id: 1,
            name: "JavaScript 高级程序设计"
          },
          {
            id: 2,
            name: "JavaScript 精粹"
          }
        ]);
      } else if (path === "/movies") {
        resolve([
          {
            id: 1,
            name: "爱在黎明破晓前"
          },
          {
            id: 2,
            name: "恋恋笔记本"
          }
        ]);
      }
    }, 2000);
  });
}


```

# useMemo

## React.memo

React.memo 使一个组件只有在 props 变化的时候才执行一遍，重新渲染

```jsx
//是用 React.memo封装Child, 当props.data变化时 才重新执行
const Child2 = React.memo(Child);
```

React 有多余的 render。 当外部组件 state。n 重新改变，触发重新渲染。

Child 组件没有任何变化(state.m)，却重新执行了

```jsx
function App() {
  const [n, setN] = React.useState(0);
  const [m, setM] = React.useState(0);
  const onClick = () => {
    setN(n + 1);
  };

  return (
    <div className="App">
      <div>
        <button onClick={onClick}>update n {n}</button>
      </div>
      <Child data={m} />
      {/* <Child2 data={m}/> */}
    </div>
  );
}

function Child(props) {
  console.log("child 执行了");
  console.log("假设这里有大量代码");
  return <div>child: {props.data}</div>;
}

//是用 React.memo封装Child, 当props.data变化时 才重新执行
const Child2 = React.memo(Child);
```

React.memo 对比的是内存地址，如果是 对象或函数变量依然会重新执行

```jsx
const [n, setN] = React.useState(0);
const [m, setM] = React.useState(0);
// 当state变化重新执行时，onClickChild地址改变
const onClickChild = () => {};

const Child2 = React.memo(Child);

// onClickChild地址改变 React.memo就认为Child2 变了。然后重新执行。
<Child2 data={m} onClick={onClickChild} />;
```

如何解决这一问题，需要用到 React.useMemo

## useMemo

类似 Vue2 的 computed，只有相关属性改变了，才重新计算返回值

使用 useMemo 解决函数缓存问题,可以实现函数重用。

React.useMemo(fn,arr) fn 的返回值（value）。就是要缓存 value。 只有 arr 中依赖的属性变了，才会计算新的值.

如果 arr 依赖不变，就重用之前的，value

```jsx
const Child2 = React.memo(Child);
<Child2 data={m} onClick={onClickChild} />;

const onClickChild = React.useMemo(() => {
  const cacheFn = () => {
    console.log(m);
  };
  // cacheFn就是要缓存的函数
  // 也可以是对象
  return cacheFn;
}, [m]); // 只有当m变化时，才重新去生成新的函数
```

## useCallback

**useCallback 出现的原因**
由于 useMemo(fn,arr) 的第一个参数必须是 fn，如果再返回一个 fn 会很难用（写着奇怪，函数再返回函数）
`userMemo(()=> (x)=>console.log(x))`。

所以有了 useCallback(fn,arr) fn 就是要返回的函数。其他等价于 useMemo

```jsx
const onCLickChild = React.useCallback(() => {
  console.log(m);
}, [m]);
```

# useRef

作用是生成一个值，这个值在组件重新 render 的时候保持不变。

可以用来引用 DOM 对象,和普通对象

```jsx
const count = useRef(0);
count.current; // 通过用对象包装数值。对象引用地址是不变，修改变的只是对象的property
```

Vue3

```javascript
const count = ref(0);
count.value;
//与react不同  count.value变化时 ，Vue3会自动render
```

## forwardRef

props 不包含 ref（大部分时候不需要） ，所以函数组件需要 forwardRef

```jsx
const Button =(props)=>{
  return <button ></button>
}

const buttonRef = useRef(null)

//会报错 函数组件不能接受ref
<button ref={buttonsRef}>按钮</button>

// 使用forwardRef拿到 useRef

const Button =(props,ref)=>{
  return <button ref={ref} ></button>
}
// 函数组件想用Ref就必须用forwardRef
const Button2= React.forwardRef(Button)


```

## useImperativeHandle

Imperative 重要的

可以理解为 setRef

用于封装**引用**

```jsx
const Button2 = React.forwardRef((props, ref) => {
  const realButton = createRef(null);
  const setRef = useImperativeHandle;
  setRef(ref, () => {
    return {
      x: () => {
        realButton.current.remove();
      },
      realButton: realButton,
    };
  });
  return <button ref={realButton} {...props} />;
});
```

# 自定义 Hook

封装数据操作

可以在自定义 hook 里使用 Context

useState 不能在 if 里 但可以在函数里运行，只要函数在函数组件里运行即可

[简单例子](https://codesandbox.io/s/wizardly-tesla-sy077)

[复杂例子](https://codesandbox.io/s/recursing-darkness-zuo4b)

```jsx
const useList = () => {
  const [list, setList] = useState(null);
  useEffect(() => {
    ajxx("/list").then((list) => {
      setList(list);
    });
  }, []);

  return {
    list,
    setList,
  };
};

function ajax() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve([
        { id: 1, name: "Frank" },
        { id: 2, name: "Jack" },
        { id: 3, name: "Alice" },
        { id: 4, name: "Bob" },
      ]);
    }, 2000);
  });
}

const { list, setList } = useList();
```

# Stale Closure

过时闭包

```javascript
function createIncrement(incBy) {
  let value = 0;

  function increment() {
    value += incBy;
    console.log(value);
  }

  const message = `Current value is ${value}`;
  function log() {
    console.log(message);
  }

  return [increment, log];
}

const [increment, log] = createIncrement(1);
increment(); // logs 1
increment(); // logs 2
increment(); // logs 3
// Does not work!
log(); // logs "Current value is 0"
```

解决 stale closure

```javascript
function createIncrement(incBy) {
  let value = 0;

  function increment() {
    value += incBy;
    console.log(value);
  }

  function log() {
    //  调用log 时去取最新的message的值
    const message = `Current value is ${value}`;
    console.log(message);
  }

  return [increment, log];
}

const [increment, log] = createIncrement(1);
increment(); // logs 1
increment(); // logs 2
increment(); // logs 3
// Works!
log(); // logs "Current value is 3"
```

React 中过时的闭包

```jsx
function WatchCount() {
  const [count, setCount] = useState(0);

  useEffect(function () {
    // 只在函数组件第一次调用时执行
    setInterval(function log() {
      // count值永远不变
      console.log(`Count is: ${count}`);
    }, 2000);
  }, []);

  return (
    <div>
      {count}
      <button onClick={() => setCount(count + 1)}>Increase</button>
    </div>
  );
}
```

解决办法

```jsx
function WatchCount() {
  const [count, setCount] = useState(0);

  useEffect(
    function () {
      // 创建新的Interval
      const id = setInterval(function log() {
        console.log(`Count is: ${count}`);
      }, 2000);
      return function () {
        // 清空上次渲染组件的 Interval
        clearInterval(id);
      };
    },
    [count] // 监听count变化
  );

  return (
    <div>
      {count}
      <button onClick={() => setCount(count + 1)}>Increase</button>
    </div>
  );
}
```
