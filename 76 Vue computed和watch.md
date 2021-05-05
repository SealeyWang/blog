# computed 和 watch 有什么区别

computed 是计算属性

它会根据所依赖的数据，动态显示最新的计算结果，计算的结果会被缓存起来 。 如果数据发生改变时，就会重新调用 getter 来计算最新结果

watch 用于监听 data 的数据。

当 data 数据发生变时，执行相应函数

immediate ： true 参数的作用是当数据初始化时就执行，默认值 false

deep:true 的作用监听对象所有内部属性,当任意内部属性变化时，执行回调函数。
默认值 false，基本数据值改变、对象地址改变，执行函数。相当于 ===

computed 用法

```javascript
var vm = new Vue({
  data: { a: 1 },
  computed: {
    // 仅读取
    aDouble: function () {
      return this.a * 2;
    },
    // 读取和设置
    aPlus: {
      get: function () {
        return this.a + 1;
      },
      set: function (v) {
        this.a = v - 1;
      },
    },
  },
});
vm.aPlus; // => 2
vm.aPlus = 3;
vm.a; // => 2
vm.aDouble; // => 4
```

watch 的用法

```javascript
var vm = new Vue({
  data: {
    a: 1,
    b: 2,
    c: 3,
    d: 4,
    e: {
      f: {
        g: 5,
      },
    },
  },
  watch: {
    a: function (val, oldVal) {
      console.log("new: %s, old: %s", val, oldVal);
    },
    // 方法名
    b: "someMethod",
    // 该回调会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深
    c: {
      handler: function (val, oldVal) {
        /* ... */
      },
      deep: true,
    },
    // 该回调将会在侦听开始之后被立即调用
    d: {
      handler: "someMethod",
      immediate: true,
    },
    // 你可以传入回调数组，它们会被逐一调用
    e: [
      "handle1",
      function handle2(val, oldVal) {
        /* ... */
      },
      {
        handler: function handle3(val, oldVal) {
          /* ... */
        },
        /* ... */
      },
    ],
    // watch vm.e.f's value: {g: 5}
    "e.f": function (val, oldVal) {
      /* ... */
    },
  },
});
vm.a = 2; // => new: 2, old: 1
```
