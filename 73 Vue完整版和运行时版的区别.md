vue 完整版叫 vue.js

vue 运行时版叫 vue.runtime.js

# 区别在哪

vue.js 有编译器(compile), 可以直接从 html or template 里得到视图。把含有 vue 指令的 html、template 字符串变成真实的 dom 节点。

template 用法

```javascript
new Vue({
  template: `<div>{{n}}</div>`,
});
```

vue.runtime.js 没有 compile。所以体积更小 （比完整版小 30%）。但是不能直接 html or template 里得到视图，
需要用 js 去构建视图。如下

```javascript
new Vue({
  render(h) {
    return h("div", [this.n, h("button", { on: { click: this.add } })]);
  },
  methods: {
    add() {
      this.n++;
    },
  },
});
```

直接用 render 太麻烦了吧，所以配合 webpack 用，通过
vue-loader 把 .vue 文件编译成 render 可用的对象

```javascript
<div> {{ n }}</div>;
//  ↓  vue loader
h("div", this.n);
```

# codesandbox.io 创建 Vue 项目

![create Vue project](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bf6326c1523447b7941336bea4054836~tplv-k3u1fbpfcp-watermark.image)

直接开始写代码
![start coding](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/373082ed31944561bed8720dd9d7c490~tplv-k3u1fbpfcp-watermark.image)

file -> export to ZIP 可以到处压缩包 在本地写
