Vue 规则：组件不能修改 props 外部数据。否则 Vue 会给你警告。props 应该由外部组件修改。这样逻辑更简单清晰，所以组件需要发送事件，通知外部组件修改 props

Vue 示例实现了 EventBus 通过 this.$emit('funName',data) 可以出发事件，并传参。

Vue 中可以通过$event 获取$emit 的参数。
那么就可以在外部组件中 定义函数修改属性

```html
<my-comp :foo="bar" @update:foo="updateFoo($event)"></my-comp>

new Vue({ data:{ foo:0 } methods: { updateFoo(data){ this.foo = data} } })

<!-- 或者 -->
<my-comp :foo="bar" @update:foo="foo = $event"></my-comp>
```

**.sync 修饰符是一个编译时的语法糖，会被扩展为一个自动更新父组件属性的 v-on 监听器**

```html
<my-comp :foo.sync="bar"></my-comp>

<!-- 就等于 -->
<my-comp :foo="bar" @update:foo="foo = $event"></my-comp>
```

例子

```html
<div id="app">
  <div>{{bar}}</div>
  <my-comp :foo.sync="bar"></my-comp>
  <!-- <my-comp :foo="bar" @update:foo="val =>bar = val"></my-comp> -->
</div>
<script>
  Vue.component("my-comp", {
    template: '<div @click="increment">点我+1</div>',
    data: function () {
      return { copyFoo: this.foo };
    },
    props: ["foo"],
    methods: {
      increment: function () {
        this.$emit("update:foo", ++this.copyFoo);
      },
    },
  });
  new Vue({
    el: "#app",
    data: { bar: 0 },
  });
</script>
```
