# 相关英语单词

derived 导出的 衍生的 派生的

render 渲染

super class 父类

property 属性

state 状态

mount 挂载

# 如何创建 Class 组件

ES5 过时

```jsx
import React from "react";

const A = React.createClass({
  render() {
    return <div>hi</div>;
  },
});

export default A;
// es 5 不支持class 才会有这种方式
```

ES6

```jsx

```

# props (properties)

外部数据

props 的作用

- 接受外部数据
  - 只能读不能写
  - 外部数据由父组件传递
- 接受外部函数
  - 在恰当的时机，调用该函数
  - 该函数一般是父组件的函数

```jsx
import React from "react";

class B extends React.Component {
  constructor(props) {
    //初始化props
    super(props);
    // 初始化完成后 this.props是就外部数据对象的地址了
  }

  render() {
    //   读取 props
    return(n <div onClick={this.props.onclick}>hi:{this.props.name}</div>);
  }
}

export default B;
```

不能修改 props， 数据的主人是谁，谁才能改

## 相关钩子

componentWillReceiveProps : 当组件接受新的 props 时，触发此钩子

```jsx

    // 已经过时，被弃用，被重命名 不推荐使用了 更名为 UNSAFE_componentWillReceiveProps
    componentWillReceiveProps(nextProps, nextContext) {
        console.log('props 变化了')
        console.log('当前props')
        console.log(this.props);
        console.log('新的props')
        console.log(nextProps);
    }
//  以前是推荐使用了，旧代码要看得懂

```

# state & setState

```jsx
//  初始化
 constructor(props) {
        super(props)
        this.state = {n: 0, history: []}

    }

//  读
{this.state.n}

//  写  this.setState(???,fn)  fn 是成功后回调  会一级合并
const n = this.state.n+1
const history = this.state.history
history.push(n)
// this.setState(newState,fn)
this.setState( {n,history: history})


//  this.setState(fn)
this.setState((state)=> ({n:state.n+1,history: [...state.history,state.n+1]}))


```

# 生命周期

```javascript
// div create construct
let div = document.createElement('div') s

//  init state
div.textContent = 'hi'

// div mount
document.body.appendChild(div)
// div update
div.textContent = 'hi2'
// div  unmount
div.remove()
```

- **constructor()**
- static getDerivedStateFromProps()
- **shouldComponentUpdate()**
- **render()**
- getSnapshotBeforeUpdate()
- **componentDidMount()**
- **componentDidUpdate()**
- **componentWillUnmount()**
- static getDerivedStateFromError()
- componentDidCatch()

- constructor() 在这里初始化 state
- shouldComponentUpdate() return false 阻止更新
- render() 创建虚拟 DOM
- componentDidMount() 组件已出现在页面
- componentDidUpdate() 组件已更新
- componentWillUnmount() 组件已死

## constructor()

初始化 state， 此时不能调用 setState

```jsx
constructor(props){
  // 初始化 props
  super(props)
  // 给click函数 bind this
  this.onClick = this.onClick.bind(this)
}

// new API   给click函数
onClick = ()=>{}
```

## shouldComponentUpdate

return true 不阻止 UI 更新

return false 阻止 UI 更新

有什么用？
可以手动判断是否要进行组件更新，可以根据应用场景灵活地设置返回值，避免不必要的更新。（自己确定要不要更新组件）

```jsx
class App2 extends React.Component {
  constructor(props) {
    super(props);
    this.state = { n: 0 };
  }

  add = () => {
    this.setState((state) => {
      return { n: state.n + 1 };
    });

    this.setState((state) => {
      return { n: state.n - 1 };
    });
  };
  shouldComponentUpdate(nextProps, nextState, nextContext) {
    // return true 不阻止 UI 更新
    // return false 阻止 UI 更新
    return nextState === this.state;
  }

  render() {
    // 执行了 render  n+1  n-1  n的值没变 但是对象的值变了 会重新执行 render 。 DOM是不会变化，应为DOM diff 发现没有变化。就啥都不变
    console.log("render");
    return (
      <div className="App2">
        App2 <br />
        {this.state.n}
        <button onClick={this.add}>+1</button>
      </div>
    );
  }
}
```

### 使用 React.PureComponent 自动判断组件更新

React 内置了 React.PureComponent 类

通过继承，实现对 state 、props 每个新旧值都对比。

所有 key 的值都一样 不会执行 render

如果有 key 的值不同 就执行 render

```jsx
class App2 extends React.PureComponent {
  // .............
}
```

这样就不用手动判断了。[具体变化代码](https://github.com/CaoBaoWang/react-demo1/commit/3f5bd59b57e03ba78bc7048b9309e7b37404d1fd)

## render

- 展示视图`return(<div></div>)`

- 只能有一个根元素

- 如果有两个根元素，就要用<React.Fragment> 包起来

- <React.Fragment /> 可以缩写成 <></>

## componentDidMount()

- 在元素插入页面后执行代码，这些代码依赖 DOM （如获取 div 的宽度）
- 此处可以发起 **加载数据的**AJAX 请求， 官方推荐写这里
- 这个方法是比较适合添加订阅的地方。如果添加了订阅，请不要忘记在 componentWillUnmount() 里取消订阅
- 首次渲染会执行此钩子

[操作 DOM 完整代码](https://github.com/CaoBaoWang/react-demo1/commit/40d4b3cb9a90ada4976f643cd7e552273fdb8e79)

```jsx
   componentDidMount() {
        console.log('mount') // 首次渲染执行
        // 操作DOM
        const divxxx = document.getElementById('divxxx')
        const width = divxxx.getBoundingClientRect().width
        this.setState({width})
    }


     render() {
        // 执行了 render  n+1  n-1  n的值没变 但是对象的值变了 会重新执行 render 。 DOM是不会变化，应为DOM diff 发现没有变化。就啥都不变
        console.log('render')
        return (
            <div className="App2">App2 <br/>
                <div id="divxxx">{this.state.n}</div>
                <button onClick={this.add}>+1</button>
                div.width= {this.state.width}
            </div>
        )
    }
```

更好的做法 使用 React ref[完整代码](https://github.com/CaoBaoWang/react-demo1/commit/c4f0a630b7e74a831e6c07228d079ee7474a6c6a)

```jsx
 ref= undefined
    constructor(props) {
        super(props)
        this.state =  {n:0,width:null}
        //初始化 ref
        this.ref = React.createRef()
    }

       componentDidMount() {
        console.log('mount') // 首次渲染执行
        // 使用ref 获取DOM
        const divxxx = this.ref.current
        const width = divxxx.getBoundingClientRect().width
        this.setState({width})
    }


    render() {
        // 执行了 render  n+1  n-1  n的值没变 但是对象的值变了 会重新执行 render 。 DOM是不会变化，应为DOM diff 发现没有变化。就啥都不变
        console.log('render')
        return (
            <div className="App2">App2 <br/>
                {/*给DOM绑定ref*/}
                <div ref={this.ref}>{this.state.n}</div>
                <button onClick={this.add}>+1</button>
                div.width= {this.state.width}
            </div>
        )
    }
```

## componentDidUpdate

`componentDidUpdate(prevProps, prevState, snapshot)`

```jsx
componentDidUpdate(prevProps) {
  // Typical usage (don't forget to compare props):
  //避免引发无线循环
  if (this.props.userID !== prevProps.userID) {
    this.fetchData(this.props.userID);
    this.setState(...)
  }
}
```

- 在视图更新后执行代码
- 此处也可以发起 AJAX 请求，用于更新数据[文档](https://reactjs.org/docs/react-component.html#componentdidupdate)
- 首次渲染不会执行函数
- 在 componentDidUpdate 中使用 setState 可能会引发无限循环，记得做条件判断（if）
- shouldComponentUpdate 返回 false。则不触发此钩子

## componentWillUnMount

- 组件将要**被移除页面然后被销毁**时执行
- unmount 过的组件不会再次 mount

主要用来取消一些操作。如 取消 AJAX 请求、 取消 Timer、 取消监听 window.scroll。会让内存越用越多
