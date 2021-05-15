Vue 规则：组件不能修改 props 外部数据。否则 Vue 会给你警告。props 应该由外部组件修改。这样逻辑更简单清晰，所以组件需要发送事件，通知外部组件修改 props

Vue 示例实现了 EventBus 通过 this.$emit('funName',data) 可以出发事件，并传参。

Vue 中可以通过$event 获取$emit 的参数。
那么就可以在外部组件中 定义函数修改属性

在有些情况下 需要对 prop 进行双向绑定，但会带来维护上的问题，而子组件可以变更父组件，在父组件和子组件都没有明显的变更来源(不知道是啥时候改的)。

所以 vue 推荐以 `update:foo` (EventBus 事件通知)的模式

```html
<!-- 子组件需要 -->

this.$emit('update:foo',data)

<!-- 父组件需要监听事件通知 -->

<my-comp :foo="bar" @update:foo="updateFoo($event)"></my-comp>

new Vue({ data:{ foo:0 } methods: { updateFoo(data){ this.foo = data} } })

<!-- 或者 -->
<my-comp :foo="bar" @update:foo="foo = $event"></my-comp>
```

为了方便，所以为这种模式提供一个缩写，即 .sync 修饰符
**.sync 修饰符是一个编译时的语法糖，会被扩展为一个自动更新父组件属性的 v-on 监听器**

```html
<my-comp :foo.sync="bar"></my-comp>

<!-- 就等于 -->
<my-comp :foo="bar" @update:foo="foo = $event"></my-comp>
```

**.sync 不能的和 v-bind 的表达式一起使用**

v-bind:title.sync=”doc.title + ‘!’” 无效

只能绑定属性(property)名 类似 v-model

用一个对象同时设置多个 prop 的时候，也可以将这个 .sync 修饰符和 v-bind 配合使用：

```html
<text-document v-bind.sync="obj"></text-document>
```

这样会把 obj 对象中的每一个 property (如 name) 都作为一个独立的 prop 传进去，然后各自添加用于更新的 v-on 监听器。

将 v-bind.sync 不能用在一个字面量的对象上，例如 v-bind.sync=”{ title: doc.title }”

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
