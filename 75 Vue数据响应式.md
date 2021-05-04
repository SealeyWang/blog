# 对 Vue 数据响应式的理解

响应式： 一个物体能对外界的刺激做出翻反应，它就是响应式

在 Vue 中，如果改变 data 里属性的值，在界面中会看到改变，改变数据，界面相应了我，这就是数据响应式。

Vue2 同过 Object.defineProperty 实现数据响应式。对每个属性，添加 getter setter 对数据读写进行监控，当数据发生变化时去更新 UI。 使用代理，避免原数据的修改，而 Vue 不知道（示例 1）。通过修改原数据，key value 变为 getter setter，防止数据被篡改而不知道。

示例 1：简单展示了代理。具体可看下方
**实现代理和原数据数据监控**

```javascript
const vm = proxy({
  data: {
    name: "wsl",
  },
});

function proxy({ data }) {
  // const _data = Object.assign(data);

  // 通过返回obj， 让人无法拿到元数据的引用
  let obj = {};
  Object.defineProperty(obj, "name", {
    get() {
      return data["name"];
    },
    set(name) {
      data["name"] = name;
    },
  });
  return obj;
}
```

# Object.defineProperty

先了解 getter setter 的概念

## getter setter

getter : 访问返回动态计算值的属性，或者你可能需要反映内部变量的状态，而不需要使用显式方法调用(直接用 obj.xxx 而不是 obj.xxx())。

setter: 如果试着改变一个属性的值，那么对应的 setter 将被执行。(直接用 obj.xxx='xxx' 而不是 obj.setXXX("xxx"))

setter 经常和 getter 连用以创建一个伪属性。

```javascript
const obj = {
  log: ["example", "test"],
  get latest() {
    if (this.log.length == 0) return undefined;
    return this.log[this.log.length - 1];
  },
  set latest(name) {
    this.log.push(name);
  },
};
console.log(obj.latest); // "test".
obj.latest = "FA";
console.log(obj.log); // ["example", "test", "FA"]
console.log(obj.latest); //"FA"
```

不能在具有真实值的属性上同时拥有一个 setter 器。

```javascript
const obj1 = {
  name: "wsl",
  get name() {
    return this.name
  }
  set name(n) {
    this.name = n
  }
};
// Uncaught SyntaxError: Unexpected token 'set'
```

通过 Object.defineProperty 自定义 Setters 和 Getters

```javascript
var o = {}; // 创建一个新对象
var bValue = 38;
Object.defineProperty(o, "b", {
  get() {
    // 不能使用 this.b
    return bValue + "===";
  },
  set(newValue) {
    bValue = newValue;
  },
});

console.log(o.b); //38===
```

# 实现代理和原数据数据监控

```javascript
let data = {
  name: "wsl",
  age: "18",
  gender: "男",
};

let proxyData = proxy({ data });

function proxy({ data }) {
  const _data = Object.assign({}, data);

  // 把原对象所有key  value  改成getter  setter 的方式，用于监听原数据的修改
  Object.keys(data).forEach((key) => {
    Object.defineProperty(data, key, {
      get() {
        // TODO something
        console.log("监听原始对象 get");
        return _data[key];
      },
      set(value) {
        // TODO something
        console.log("监听原始对象 set");

        _data[key] = value;
      },
    });
  });

  let obj = {};

  Object.keys(data).forEach((key) => {
    Object.defineProperty(obj, key, {
      get() {
        // TODO something
        console.log("监听代理对象 get");

        return _data[key];
      },
      set(value) {
        // TODO something
        console.log("监听代理对象 set");
        _data[key] = value;
      },
    });
  });
  return obj;
}
console.log("proxyData");
console.log(data.name); // 监听原始对象 get   wsl
console.log(proxyData.age); // 监听代理对象 get 18
data.name = "w"; // 监听原始对象 set
proxyData.age = "19"; // 监听代理对象 set

console.log("proxyData");
console.log(data.name); // 监听原始对象 get   w
console.log(proxyData.age); // 监听代理对象 get 19
```
