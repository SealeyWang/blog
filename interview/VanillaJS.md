# ES6 语法知道哪些？ 怎么用

[es6 新特性列表](https://fangyinghang.com/es-6-tutorials/)

- let
- const
- import export

  ```javascript
  import defaultExport from "module-name";
  import * as name from "module-name";
  import { export } from "module-name";
  import { export as alias } from "module-name";
  import { export1 , export2 } from "module-name";
  import { foo , bar } from "module-name/path/to/specific/un-exported/file";
  import { export1 , export2 as alias2 , [...] } from "module-name";
  import defaultExport, { export [ , [...] ] } from "module-name";
  import defaultExport, * as name from "module-name";
  import "module-name";

  ```

- 箭头函数
  - `const sum = (a, b) => a + b`
  - `nums.forEach( v => { console.log(v) })`
- 展开操作符

  ```javascript
    function sum(x, y, z) {
    return x + y + z;
    }
    const numbers = [1, 2, 3];
    console.log(sum(...numbers);
  ```

- 默认参数
  ```javascript
  function multiply(a, b = 1) {
    return a * b;
  }
  ```
- 解构赋值

  ```javascript
  let a, b, rest;
  const [a, b] = [10, 20];

  [b, a] = [a, b]; // 数组匹配
  let { a, b, c } = objABC; // 对象匹配
  function g({ name: n, val: v }) {} // 参数匹配
  ```

- 模板字符串

  ```javascript
  `string text`;
  `string text line 1 string text line 2`;
  `string text ${expression} string text`;
  tag`string text ${expression} string text`;
  ```

- class

  ```javascript
  class Cat {
    constructor(name) {
      // 构造函数
      this.name = name;
    }

    speak() {
      console.log(this.name + " makes a noise.");
    }
  }
  // extends 继承
  class Lion extends Cat {
    constructor(name) {
      // 重写父类构造函数
      super(name);
      console.log("other code");
    }

    speak() {
      // 重写父类函数
      super.speak(); // 调用父类函数
      console.log(this.name + " roars.");
    }
  }
  ```

- Promise

# Promise Promise.all Promise.race 分别怎么用

## Promise 的用法

```javascript
function fn() {
  return new Promise((resolve, reject) => {
    成功时调用;
    resolve(数据);
    失败时调用;
    reject(错误);
  });
}

fn().then(success, fail).then(success2, fail2);
```

## Promise.all 的用法

**promise1 和 promise2 都成功才会调用 success1**

```javascript
Promise.all([promise1, promise2]).then(success1, fail1);
```

## Promise.race 的用法

```javascript
Promise.race([promise1, promise2]).then(success1, fail1);
```

1. promise1 和 promise2 只要有一个成功就会调用 success1；
2. promise1 和 promise2 只要有一个失败就会调用 fail1
3. 总之，谁第一个成功或失败，就认为是 race 的成功或失败。

# 手写节流 和 防抖 函数

## 节流

节流：减少一段时间的触发频率.

一段时间内，执行一次之后，就不执行第二次

注意，有些地方认为节流函数不是立刻执行的，而是在冷却时间末尾执行的（相当于施法有吟唱时间），那样说也是对的。

```javascript
function throttle(fn, delay) {
  let canUse = true;
  return function (...args) {
    if (canUse) {
      fn.apply(this, ...args);
      canUse = false;
      setTimeout(() => (canUse = true), delay);
    }
  };
}

const throttled = throttle(() => console.log(1), 200);
throttled();
throttled();
```

## 防抖

防抖： 通过 setTimeout 的方式，在一定的时间间隔内，将多次触发变成一次触发；

在第一次触发事件时，不立即执行函数，而是给出一个期限值比如 200ms

- 如果在 200ms 内没有再次触发滚动事件，那么就执行函数
- 如果在 200ms 内再次触发滚动事件，那么当前的计时取消，重新开始计时

效果： 如果短时间内大量触发同一事件，只会执行一次函数

```javascript
function debounce(fn, delay) {
  let timerId = null;
  return function (...args) {
    const context = this;
    if (timerId) {
      window.clearTimeout(timerId);
    }
    timerId = setTimeout(() => {
      fn.apply(context, args);
      timerId = null;
    }, delay);
  };
}

const debounced = debounce(() => console.log(1), 100);
debounced();
debounced();
```

# 手写 AJAX

```javascript
let request = new XMLHttpRequest();
request.open("GET", "/a/b?name=wsl", true);
request.onreadystatechange = function () {
  if (request.readyState === XMLHttpRequest.DONE && request.status === 200) {
    console.log(request.responseText);
  }
};
request.send();
```

# 判断 this 是什么

```
fn()
this => window/global
obj.fn()
this => obj
fn.call(xx)
this => xx
fn.apply(xx)
this => xx
fn.bind(xx)
this => xx
new Fn()
this => 新的对象
fn = ()=> {}
this => 外面的 this

```
