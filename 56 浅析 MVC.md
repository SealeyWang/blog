# MVC (Model View Controller)是最常见的软件架构模式

Model： 用与封装业务逻辑相关的数据，以及对数据处理的方法。一个或多个 View 去监听此 Model。一旦 Model 的数据发生变化，**Model 将事件通知给有关的 View**

View： 是显示在屏幕上，描述 Model 当前状态，当 Model 数据状态变化时 应该更新显 示内容。 用户操作 View 后，将事件通知给 Controller。

Controller : 用于处理用户的行为（用户操作 View 后 View ）、处理业务逻辑、 控制 model 数据的改变

![](https://upload.wikimedia.org/wikipedia/commons/f/f0/ModelViewControllerDiagramZh.png)

实线 方法调用

虚线 事件通知

例如：

1. View ——> Model View 调用 Model 的借口，监听 Model 数据改变，以便更新数据

2. Controller ——> View 调用 View 提供的接口，监听 View 的事件

3. View ---> Controller View 事件改变通知

4. Controller ——> Model Controller 收到 View 的通知后，去调用 Model 的借口修改数据（处理业务）

5. Model ---> View Model 处理完业务逻辑，数据修改完成后。通知 View 需要展示最新数据

通过一个购物例子描述 MCV,用户改变购买数量 n ，总价会变， 收到推送单价（price）涨了 总价会变

```javascript
//通过一个购物例子描述 MCV,用户改变购买数量 n ，总价会变， 收到推送单价（price）涨了 总价会变

// 使用伪代码描述 不保证可运行

const m = {
  data: {
    //购买数量
    n: 1,
    price: 50,
    //总价
    total: function () {
      return m.data.n * m.data.price;
    },
  },
  dataChangeListener: null,
  update(n) {
    // TODO 处理业务逻辑 购买数量不能超过库存量
    m.data.n = n;
    // 5. Model ---> View Model 处理完业务逻辑，数据修改完成后。通知 View 需要展示最新数据
    m.notifyDatasetChange();
  },
  // 价格变动 或者 库存不足
  receiverPush(newPrice, newN) {
    m.data.price = newPrice;
    m.data.n = newN;
    // 4. Model ---> View
    m.notifyDatasetChange();
  },
  notifyDatasetChange() {
    m.dataChangeListener(m.data);
  },
  setOnDataChangeListener(fn) {
    m.dataChangeListener = fn;
  },
};

const v = {
  input: document.querySelector("#input1"),
  price: document.querySelector("#span1"),
  button: document.querySelector("#button1"),

  init() {
    // 1 View  ——>  Model 监听 Model 数据改变，以便更新数据
    m.setOnDataChangeListener(m.render);
  },
  render(data) {
    //  render view with data
    v.input.nodeValue = data.n; // TODO show  缺货提示
    v.price.textContent = data.total;
  },
  // mock dom event
  oncUserChangeInputValue(fn) {
    const newValue = 666; //user input
    // 3 View ---> Controller 事件改变通知
    fn(newValue);
  },
};
const c = {
  init() {
    // bind event   2. Controller ——> View  调用 View 提供的接口，监听 View 的事件
    v.oncUserChangeInputValue(c.onUserTypeContent);
  },
  // handle user type
  onUserTypeContent(val) {
    // update data   4. Controller ——> Model  收到 View 的通知后，去调用 Model 的借口修改数据（处理业务）
    m.update(val);
  },
};
v.init();
c.init();
```

# EventBus

[EventBus for Android](https://juejin.cn/post/6844903832695078920)

EventBus 是一个基于发布/订阅的设计模式的事件总线。

为组件之间提供一个简单的通信方式

![EventBus framework for Android ](https://raw.githubusercontent.com/greenrobot/EventBus/master/EventBus-Publish-Subscribe.png)

用 JS 实现一个简单的 EventBus

```javascript
//简易版EventBus
class EventBus {
  constructor() {
    this._events = {};
  }
  on(eventName, callback) {
    if (!this._events[eventName]) {
      this._events[eventName] = [callback];
    } else {
      this._events[eventName].push(callback);
    }
  }
  off(eventName, callback) {
    if (this._events[eventName]) {
      this._events[eventName].forEach((item, index) => {
        if (item === callback) {
          this._events[eventName].splice(index, 1);
        }
      });
    }
  }
  trigger(eventName, data) {
    if (this._events[eventName]) {
      this._events[eventName].forEach((item) => {
        item.call(this, data);
      });
    }
  }
  // vue 中的 命名
  emit(eventName, data) {
    this.trigger.apply(this, arguments);
  }
}

export default EventBus;
```

把上边的 伪代码 实现下, 原来是用接口回调的方式，去做事件通知。
设置 DOM 事件的回调，还好。但是 View 设置 Model 数据更新的回调
就相对麻烦。主要有

- view 要拿到 model 的引用，去设置监听事件，这样产生耦合
- model 要定义事件回调接口，当事件多起来不够简单

完整代码请看 github 提交记录[mvc 演示 使用接口回调](https://github.com/CaoBaoWang/EventBus/commit/d8e1b657c989b83bb1200977af2a3e2cee5270d2)

```javascript
 init() {
    // 1 View  ——>  Model 监听 Model 数据改变，以便更新数据
    m.setOnDataChangeListener(m.render);
  },
```

这次换成 EventBus 实现事件通信。
一般来说 EventBus 可以作为一个全局的单例对象为不同的模块，组件提供通信服务

两行代码完成 Model ---> View 的通信

github 提交记录[eventbus 实现通信](https://github.com/CaoBaoWang/EventBus/commit/8b34e0415c9a40239b5ee7b79a7afda6f4d6d3a5)

```javascript
 init(){
        // 1 View  ——>  Model 监听 Model 数据改变，以便更新数据
        // m.setOnDataChangeListener(v.render)
        eventBus.on('datasetChange',v.render)

    },

      notifyDatasetChange(){
        eventBus.trigger('datasetChange',m.data)
    },

```

<!-- # JavaScript 下的 MVC -->

# MVP MVC 的一种衍生模式

Model View Presenter

在 MVP 中

1. 各部分之间的通信，都是双向的。
2. View 与 Model 不发生联系，都通过 Presenter 传递。
3. View 非常薄，不部署任何业务逻辑,只处理界面 和 用户事件，称为"被动视图"（Passive View），即没有任何主动性，而 Presenter 非常厚，所有逻辑都部署在那里。

![](https://www.ruanyifeng.com/blogimg/asset/2015/bg2015020109.png)

# 表驱动编程是做什么的

指用查表的方法获取值。

在数值不多的时候我们可以用逻辑语句( if/else 或 case )的方法来获取值，但随着数值的增多逻辑语句就会越来越长，此时表驱动法的优势就显现出来了。

- 更灵活
- 大量不同事件时,代码量简单稳定

```javascript
const events: {

const events = {
  "click #add1": "add",
  "click #minus1": "minus",
  "click #mul2": "mul",
  "click #divide2": "div",
};
for (let key in events) {
  const s = key.split(" ");
  const eventName = s[0];
  const selector = s[1];
  const callback = events[key];
  document.querySelector(selector).addEventListener(eventName, callback);
}
```

# 模块化

上面所说的都是模块化编程的一些方式。模块化编程使得我们的代码结构更加清晰，也比较容易增加新功能，删减不需要的功能。使得代码解耦，维护起来更加简单。

模块化可以是不同类型代码的文件，可以是某块业务，或者是单独的多媒体服务，人脸识别服务

由小直至大 模块化的核心是封装。将功能，应用拆分成模块，封装相关代码、业务处理逻辑，并提供接口访问。更易于将不同模块组合成应用，减小模块间变动带来的影响
