在 Vue.js 的项目中，如果项目结构简单， 父子组件之间的数据传递可以使用  props 或者 $emit 等方式，大家参考一下我之前写的  [vue升级之路（二）--  vue组件间方法调用及数据传输](https://juejin.im/post/5b1b3f646fb9a01e3e5cec45)

但是如果是大一点的项目或者需要在子组件之间频繁传递数据的时，前面使用的方式就不太方便。而Vue 的状态管理工具 [Vuex](https://vuex.vuejs.org/zh-cn/) 的出现就是为了解决这个问题。

### 一、安装并且引入vuex
首先使用 npm 安装 Vuex
```
npm install vuex -S
```
然后在main.js 引入
```
import Vue from 'vue'
import App from './App'
import router from './router'
import Vuex from 'vuex'
import store from './vuex/store'

Vue.use(Vuex)

new Vue({
  el: '#app',
  router,
  store,
  render: h => h(App)
})
```
项目结构：![image.png](https://user-gold-cdn.xitu.io/2018/9/21/165fb822115141dc?w=299&h=320&f=png&s=10115)

### 二、构建核心仓库 store.js
每一个 Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着你的应用中大部分的状态 (state)，所以可以把 store 通俗的理解为一个全局变量的仓库。
但是和单纯的全局变量又有一些区别，主要体现在当 store 中的状态发生改变时，相应的 vue 组件也会得到高效更新。

在 src 目录下创建一个 vuex 目录，将 store.js 放到 vuex 目录下
```
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)

const store = new Vuex.Store({
    // 定义所需的状态
    state: {
        name: 'tree',
    }
})

export default store
```
这是一个最简单的 store.js，里面只存放一个状态 name
虽然在 main.js 中已经引入了 Vue 和 Vuex，但是这里还得再引入一次

### 三、将状态映射到组件
```
<template>
    <div id="user">
        <ul class="ul">
            <li v-for="item in list">{{item.text}}</li>
        </ul>
        <div>欢迎来到Vue.js，<span class="name">{{ name }}</span></div>
    </div>
</template>

<script>
export default {
    name: 'user',
    data() {
        return {
            list: [{
                text: '发现'
            }, {
                text: '视频'
            }, {
                text: '我的'
            }, {
                text: '朋友'
            }, {
                text: '帐号'
            }]
        }
    },
    computed: {
        name () {
            return this.$store.state.name
        }
    }
}
</script>
</script>
```
这是 user.vue 的 html 和 script 部分
主要在 computed 中，将 this.$store.state.name的值返回给 html 中的 name
页面渲染之后，就能获取到 name的值
![image.png](https://user-gold-cdn.xitu.io/2018/9/21/165fb8221196c4f9?w=382&h=227&f=png&s=11530)

### 四、在组件中修改状态
然后在 login.vue 中添加一个输入框，将输入框的值传给 store.js 中的 name
```
<template>
    <div id="login">
        <input type="text" name="" v-model="text" placeholder="输入用户名">
        <button @click="login">login</button>
    </div>
</template>
```
上面将输入框 input 的值绑定为 text，然后在后面的按钮 button 上绑定 click 事件，触发 login 方法
```
methods: {
　login() {
            this.$store.state.name = this.text
            this.$router.push('user')
    },
}
```
在 login 方法中，将输入框的值 text赋给 Vuex 中的状态 name，从而实现子组件之间的数据传递
![](https://user-gold-cdn.xitu.io/2018/9/21/165fb822117fd5e0?w=370&h=203&f=png&s=6665)
![](https://user-gold-cdn.xitu.io/2018/9/21/165fb8221185198d?w=381&h=226&f=png&s=11542)
·
### 五、官方推荐的修改状态的方式
上面的示例是在 login 直接使用赋值的方式修改状态 name，但是 vue 官方推荐使用下面的方法：![image.png](https://user-gold-cdn.xitu.io/2018/9/21/165fb82211a0e0ae?w=670&h=363&f=png&s=6765)

首先在 store.js 中定义一个方法 setName，其中第一个参数 state 就是 $store.state，第二个参数 msg 需要另外传入
然后修改 login.vue 中的 setAuthor 方法
```
methods: {
　login() {
            this.$store.commit('setName', this.text)
            this.$router.push('user')
    },
}
```
这里使用 $store.commit 提交 setNmae，并将 this.text 传给 msg，从而修改 name

这样显式地提交(commit) mutations，可以让我们更好的跟踪每一个状态的变化，所以在大型项目中，更推荐使用第二种方法。



