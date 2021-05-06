# Vue 的双向绑定如何实现？

Vue 双向绑定， 其实是在问 v-model

v-model 的作用是 绑定一个变量

变量变化的时候 UI 会变化

用户改变 UI 的时候 数据会变化

这就是双向绑定

实际上 v-model 是 v-bind:value 和 v-on:input 的语法糖

```html
<input type="text" v-model="user.username" />
<!-- 等价于 -->
<input
  type="text"
  :value="user.username"
  @input="user.username = $event.target.value"
/>
```

v-model 对应 v-on:input 有两种不同的情况

1. 如果是原生的元素时 $event 是原生的 event

```html
<input type="text" v-model="user.username" />
<!-- 等价于 -->
<input
  type="text "
  :value="user.username"
  @input="user.username = $event.target.value"
/>
```

v-model 默认监听 input 事件，使用.lazy 修饰符 则监听的是 change 事件

input 事件, 键盘、鼠标 任何输入设备的输入

change 事件，只在 input 失去焦点时触发

2. 当使用自定义组件的时候 $event 就是 emit 传递的 data

```html
<!-- 当使用 emit('update:username',data)时，$event 是 data -->

<my-comp
  :username="user.username"
  @update:username="user.username = $event"
></my-comp>

<!-- .sync 修饰符是简写，所以使用 .sync 修饰符时 $event 是 data -->
<my-comp :username.sync="user.username"></my-comp>
<!-- 默认将$event赋值给user.username -->
```

# 自定 input 组件 v-model

自定义 input 组件上想使用 v-model， 必须 this.$emit('input',data) 事件才能实现双向绑定。

```html
<my-input v-model="firstName" />
<!-- 等价于 -->
<my-input :value="firstName" @input="firstName = $event" />
```

完整代码

MyInput.vue

```html
<template>
  <div>
    <input type="text" v-bind:value="value" @input="onInput" />
  </div>
</template>

<script>
  export default {
    name: "MyInput",
    props: {
      value: String,
    },
    mounted() {
      console.log(this.value);
    },
    methods: {
      onInput(e) {
        console.log(e.target.value);
        // this.$emit('myInputEvent',e.target.value) // 不能使用v-model
        this.$emit("input", e.target.value);
      },
    },
  };
</script>

<style scoped></style>
```

App.vue

```html
<template>
  <div id="app">
    {{user }} {{'firstName=' +firstName}}
    <!--    <input type="text" v-model="user.username"/>-->
    <input
      type="text"
      :value="user.username"
      @input="user.username = $event.target.value"
    />
    <button @click="user.username = 'wslbtn'">username = wslbtn</button>

    <!--    <my-input :value="firstName" @input="firstName = $event" />-->
    <my-input v-model="firstName" />
  </div>
</template>

<script>
  import MyInput from "./components/MyInput";

  export default {
    components: {
      MyInput,
    },
    data() {
      return {
        firstName: "w",
        user: {
          username: "wsl",
        },
      };
    },

    name: "App",
  };
</script>

<style lang="scss">
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
```
