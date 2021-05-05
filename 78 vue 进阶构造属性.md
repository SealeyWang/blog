# directives 指令

作用是减少 DOM 操作的重复代码

全局用 Vue.directive('x',{...})

局部用 options.directives

[directive demo](https://codesandbox.io/s/gallant-heyrovsky-j3x7p?file=/src/main.js)

# mixins 混入

做用是减少 option 里的重复

全局 Vue.mixin({...})

局部 `options.mixins: [mixing1,mixin2]`

[mixins demo](https://codesandbox.io/s/happy-zhukovsky-7xn63)

# extends 继承/扩展

全局用 Vue.extends({...})

局部用 options.extends:obj

作用和 mixins 差不多 。 不过便于扩展单文件组件

```javascript
var CompA = { ... } // 在没有调用 `Vue.extend` 时候继承 CompA var CompB = {
extends: CompA, ... }
```

# Provide Inject

provide 和 inject 主要在开发高阶插件/组件库时使用。并不推荐用于普通应用程序代码中。

祖先提供东西，后代注入

作用范围大 隔 N 代共享信息（数据、方法)

```javascript
// 父级组件提供 'foo'
var Provider = {
  provide: {
    foo: "bar",
  },
  // ...
};

// 子组件注入 'foo'
var Child = {
  inject: ["foo"],
  created() {
    console.log(this.foo); // => "bar"
  },
  // ...
};
```

利用 ES2015 Symbols、函数 provide 和对象 inject：

```javascript
const s = Symbol();

const Provider = {
  provide() {
    return {
      [s]: "foo",
    };
  },
};

const Child = {
  inject: { s },
  // ...
};
```
