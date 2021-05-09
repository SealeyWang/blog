# React 元素 React 组件 是什么

## 元素和组件

```javascript
// React 元素 （规范 d小写表示元素）
const div = React.createElement('div',....)

// React组件 (规范 D大写表示组件 )
const Div = ()=> React.createElement('div',...)

```

目前来说 一个返回 React 元素的函数就是组件。

在 Vue 里 一个构造选项就可以表示一个组件

## 函数组件 类组件

```javascript
//  函数组件
function Welcome(props) {
  return <h1>hello,{props.name}</h1>;
}

// 类组件

class Welcome extends React.Component {
  render() {
    return <h1>hello , {this.props.name}</h1>;
    // JSX  类似<h1></h1> 这样的标签 会被翻译成React.createElement
  }
}

// 如何使用
<Welcome name="wsl" />;
```

# 如何使用 props 和 state

## state

state 的使用

类组件用 this.state 读 this.setState 写

函数组件用 React.useState(0) 0 是默认值，返回数组，第 1 项读 第 2 项写

### 类组件 setState

setState 不会马上去改 state， 最好使用一个函数

```javascript
// 当前n是0
this.setState({ n: this.state.n + 1 }); // 这行代码是异步的 不会马上改变state
console.log(n); // 打印0

// 高手 都用 函数设置state

this.setState((state) => {
  // 这是旧的state
  const n = state.n + 1;
  console.log(n);
  return { n }; //返回新的state
  // 有助于理解新旧state
});
```

`this.setState(this.state)` 不推荐这样做。因为

数据不可变：一个思维，以前的对象不要去改它，要改的话就产生一个新的对象。

### 函数组件 setState

```jsx
const [n,setN] = React.useState(0)

<div>
      Son:{n}
      <button onClick={() => setN(n + 1)}>+1</button>
      <GrandSon />
  </div>
// setN  也是异步的。永远不会改变n 。 会产生一个新的N
```

类组件 的 setState 会等一会改变 state

函数组件的 setN 永远不会改变 n

例子

```jsx
const Son = () => {
  const [n, setN] = React.useState(0);
  // 等价于
  // const arr = React.useState(0)
  // const n = arr[0]
  // const setN = arr[1]
  return (
    <div>
      Son:{n}
      <button onClick={() => setN(n + 1)}>+1</button>
      <GrandSon />
    </div>
  );
};

class GrandSon extends React.Component {
  constructor() {
    super();
    this.state = {
      n: 0,
    };
  }
  add() {
    this.setState((state) => {
      const newState = {
        n: state.n + 1,
      };
      return newState;
    });
  }
  render() {
    return (
      <div>
        孙子:{this.state.n}
        <button onClick={() => this.add()}>孙子+1</button>
      </div>
    );
  }
}
```

## props

[演示](https://codesandbox.io/s/jovial-neumann-w7f5e?file=/src/index.js)

类组件直接读取属性 this.props.xxx

函数组件直接读取参数 props.xxx

```jsx
const Son = (props) => {
  const [n, setN] = React.useState(0);
  // const arr = React.useState(0)
  // const n = arr[0]
  // const setN = arr[1]
  return (
    <div>
      Son: name = {props.name} 。 {n}
      <button onClick={() => setN(n + 1)}>+1</button>
      <GrandSon name="grandson" sonN={n} />
    </div>
  );
};

const Son = (props) => {
  const [n, setN] = React.useState(0);
  // const arr = React.useState(0)
  // const n = arr[0]
  // const setN = arr[1]
  return (
    <div>
      Son: name = {props.name} 。 {n}
      <button onClick={() => setN(n + 1)}>+1</button>
      <GrandSon name="grandson" sonN={n} />
    </div>
  );
};

class GrandSon extends React.Component {
  constructor() {
    super();
    this.state = {
      n: 0,
    };
  }
  add() {
    this.setState((state) => {
      const newState = {
        n: state.n + 1,
      };
      return newState;
    });
  }
  render() {
    return (
      <div>
        孙子: name = {this.props.name}。 sonN = {this.props.sonN} <br />
        my{this.state.n}
        <button onClick={() => this.add()}>孙子+1</button>
      </div>
    );
  }
}
```

# 注意事项

## 类组件注意事项

- this.state.n += 1 无效

  其实 n 已经改变了，只不过 UI 不会自动更新而已，
  调用 setState 才会出发 UI 更新（异步）。
  React 没有像 Vue 监听 Data 一样监听 state

- setState 会异步更新 UI

  setState 之后，state 不会马上改变，立马读 state 会失败。
  更推荐的方式是 setState(函数)，函数接收旧的 state 返回新的 state

- this.setState(this.state) 不推荐

  React 希望我们不要修改旧 state（不可变数据），

  常用代码：setState({n:state.n+1})

- 总结

  这是一种理念（函数式）

## 函数组件注意事项

也要通过 setX(新值) 来更新 UI

没有 this，一律用参数和变量

# Vue VS React 简单对比

相同点:都是对视图的封装，

React 是用类和函数表示一个组件，

Vue 是通过钩子选项钩子一个组件

都提供了 createElement 的 XML 简写

React 提供 jsx 语法

Vue 提供模板写法

React html 放在 js 里写 HTML in JS

Vue 把 js 放在 HTML 里写 JS in HTML

## vue 编程模型

Vue 推荐修改原数据

Model -> view

model 改变 -> 更新 View （修改变动的地方）

## React

React 不能修改原数据，只能创建一个新的替换

Model -> View

newModel -> newView

DOM diff View newView 把变更的地方改掉

# 复杂的 state 怎么怎么处理

## 类组件

class 组件里的 this.setState,如果对期中一部分进行修改，其它的部分会自动沿用上一次的值，不会被 undefined 覆盖 。（仅限第一层 浅拷贝）

```jsx
this.state = {
  n: 0,
  m: 1,
};

// 设置完m还是1
this.setState({ n: this.state + 1 });
```

类组件会自动合并第一层。

## 函数组件

函数组件更简单,尽量用函数组件

```jsx
const [n, setN] = React.useState(0);
const [m, setM] = React.useState(0);

setN(n + 1);
setM(m + 1);
```

不推荐这样写

```jsx
const [state, setState] = React.useState({
  n: 0,
  m: 0,
});

setState({ n: state.n + 1 }); // m没了
setState({ m: state.m + 1 }); // n 没了

// 手动 添加所有值  再覆盖需要的
setState({ ...state, n: state.n + 1 });
setState({ ...state, m: state.m + 1 });
```

函数组件的 setState 不会自动合并

# 事件绑定

onClick onKeyPress

## 类组件事件绑定

各种绑定方法分析

所有函数的 this 都是参数，由调用者决定，所以会变

箭头函数调用不接受 this， 箭头头数 this 是定义时的 this

```jsx
<button onClick={() => this.addN()}>n+1</button>


//  有问题 会让this变成window
<button onClick={this.addN}>n+1</button>

// React 会 button.onClick.call(null,event)

// 可以 麻烦
<button onClick={this.addN.bind(this)}>n+1</button>

// 给箭头函数取个名字。麻烦多次一举

this._addN = ()=> this.addN()
<button onClick={this._addN}>n+1</button>



//  较好的写法
constructor(){
  // 不能写 constructor 外面
  this.addN = ()=> this.setState({n:this.state.n+1})
}

render(){
  return <button  onClick={this.addN}>n+1</button>
}


// 公认 最好的写法
//  直接写constructor 外面  不写this
addN = ()=> this.setState({n:this.state.n+1})

render(){
  return <button  onClick={this.addN}>n+1</button>
}


```

# this

this 原型链 AJAX

this 的值是什么
[this 的值到底是什么？一次说清楚](https://zhuanlan.zhihu.com/p/23804247)

为什么还没搞懂 this
[你怎么还没搞懂 this？](https://zhuanlan.zhihu.com/p/25991271)

如何确定 this

JS 里为什么有 this
[JS 里为什么会有 this](https://zhuanlan.zhihu.com/p/30164164)

为什么要设计 this
