# 组件间通信

父子组件：使用 v-on 通过事件通信

爷孙组件：使用两次 v-on 通过爷爷爸爸通信，爸爸儿子通信实现爷孙通信


任意组件：使用 eventBus = new Vue() 来通信，eventBus.$on 和 eventBus.$emit 是主要API

任意组件：使用 Vuex 通信  // 还不会 暂时别说



