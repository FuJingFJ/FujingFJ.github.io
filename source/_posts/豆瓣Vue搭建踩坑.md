---
title: 豆瓣Vue搭建踩坑
date: 2017-08-31 21:00:11
tags:
---
### 1. computed

- 计算后属性会被添加到vue实例中，可以直接用`vue.属性名`调用
  ```
computed: {
    attr: function(){
        return xxx
    }
}
  ```

### 2. vuex 中的getters

- 可以当成store的计算属性

### 3. vuex 的初步用法

- 安装vuex
```
  npm install vuex --save --dev
```

- 在src目录下创建store文件夹，并创建index.js文件，文件内引入vuex
```
    import Vue from 'vue'
    import Vuex from 'vuex'
    Vue.use(Vuex)
```

1. 创建store

  ```
const store = new Vuex.Store({
    state: {}, //数据对象
    getters: {},//state派生出的数据，store的计算属性，类似于computed
    mutations: {},//mutations触发会改变store的状态/state的数据
    actions: {}//提交mutations的函数，触发mutations
})
  ```

2. 在main.js的Vue实例中绑定store

  ```
import store from './stote'  
new Vue({  
    el: '#app',  
    router,  
    store,//绑定到根实例中  
    template: ' <App/>',  
    components: {App},  
})
  ```

3. 分割store模块，再统一导出

      - 可以在store目录下创建modules目录，分割模块，再在store目录下的index内导出，需要写在store的modules属性内
    
  ```
//index中
import example from './modules/example' 
export default new Vuex.store({
    modules: { 
        example
    }
    })
//example中,把模块当成一个对象导出到index里，再右index中的module接收管理 
const state = {
    a: xx,
    b:xx,
}
export default {
    state
}
  ```

### router-link :to

- 通过to属性指定目标地址，默认渲染为带正确连接的`<a>`标签
- to 后可带的值:
```
<!-- 字符串 -->
<router-link to="home">Home</router-link>
<!-- 渲染结果 -->
<a href="home">Home</a>
<!-- 使用 v-bind 的 JS 表达式 -->
<router-link v-bind:to="'home'">Home</router-link>
<!-- 不写 v-bind 也可以，就像绑定别的属性一样 -->
<router-link :to="'home'">Home</router-link>
<!-- 同上 -->
<router-link :to="{ path: 'home' }">Home</router-link>
<!-- 命名的路由 -->
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
<!-- 带查询参数，下面的结果为 /register?plan=private -->
<router-link :to="{ path: 'register', query: { plan: 'private' }}">Register</router-link>

```

### vue组件

- 引用单文件组件的时候，用标签标示
- 子组件使用父组件的数据，需要设置子组件的props选项

  ```
Vue.component('child', {
// 声明 props
props: ['message'],
// 就像 data 一样，prop 可以用在模板内
// 同样也可以在vm实例中像“this.message”这样使用
template: '<span>{{ message }}</span>'
})
  ```
- slot插口
  - 子组件中有`<slot></slot>`插口，会保留父组件在子组件中写入的内容
  - 具名slot
    - slot下有个name属性可以在父组件中配置对应的slot内容
    ```
//父组件  
<app>
    <h1 slot="header"></h1>
    <compotent></compotent>
</app>  
//子组件
<template>
    <slot name ="header"></slot>
    <div> content</div>
</template>
  ```
- 组件中引用本地图片，还是按照目录的顺序引用，就算子组件还会被别的组件引用。路径按照引入图片的组件的目录为准