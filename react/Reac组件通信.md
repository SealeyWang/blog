# React 父子组件如何通信？

父组件通过传递函数 给子组件的 props

子组件在适当的时候调用

## 爷 孙 组件如何通信

爷 (函数)-> 父 (函数)-> 孙

- 爷组件传递函数到父组件
- 父组件传递函数到孙组件
- 孙组件调用函数

# 任意组件通信 （Redux)

## eventHub （发布订阅模式） 实现通信

发布订阅模式 也叫 eventHub 模式。

- eventHub 订阅一个事件（通过事件名），构造回调函数
- eventHub 发布一个事件。所有订阅此事件 都会执行回调函数

实现一个[EventBus](https://github.com/CaoBaoWang/EventBus)

[MVC 使用 EventBus 通信](https://juejin.cn/post/6956507948759646222)

## 使用 Redux 代替 eventHub

store:仓库 约定 所有数据集合 命名为 store

`eventHub.trigger(actionType,payload)`

action:动作 trigger 时 actionType + payload 叫 action。

payload:荷载 即传递的数据

Redux 写法
`store.dispatch({type:'actionType',payload:100})`

reducer: 对数据的变动称为 reducer
`store.money.amount -= 100` // reducer
eventHub 一般放在 callback 函数里 去修改数据

`eventHub.on(eventName,callback)`//on 订阅 subscribe

`store.subscribe(render) ` state 改变时从新 render

`<App store={store.getState} />` store 传入根组件 App

`<Component1 money={this.props.store.money} />` 使用 store

```jsx
let createStore = Redux.createStore;

let reducers = (state, action) => {
  state = state || {
    money: { amount: 100000 },
  };
  switch (action.type) {
    case "actionTypeName":
      return {
        money: {
          amount: state.money.amount - action.payload,
        },
      };
    default:
      return state;
  }
};

const store = createStore(reducers);
store.subscribe(render); // 监听store 变化 重新渲染

const render = () => {
  ReactDOM.render(
    //   传入
    <App store={store.getState} />,
    document.querySelector("#app")
  );
};

render();

class App extends React.Component {
  constructor() {
    super();
  }
  render() {
    return (
      <div>
        <Component1 money={this.props.store.money} />
        <Component2 money={this.props.store.money} />
      </div>
    );
  }
}
```
