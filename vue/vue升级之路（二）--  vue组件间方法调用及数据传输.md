
> #### 注册vue组件的几种方式

1. 全局注册(这种方式注册组件必须在vue实例化之前声明)
```
Vue.component('my-component',{})
```
2. 局部注册

```
var Child = {
  template: '<div>A custom component!</div>'
}
new Vue({
  // ...
  components: {
    // <my-component> 将只在父模板可用
    'my-component': Child
  }
})
```
3. 扩展实例

```
// 定义一个混合对象
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}
// 定义一个使用混合对象的组件
var Component = Vue.extend({
  mixins: [myMixin]
})
var component = new Component() // -> "hello from mixin!"
```

> ### 组件之间的数据传递

首先src的目录下，App.vue 是父组件，components 文件夹下都是子组件。

 Vue 的组件作用域都是孤立的，不允许在子组件的模板内直接引用父组件的数据。必须使用特定的方法才能实现组件之间的数据传递。


#### 1. 父组件向子组件传递数据

父组件可以使用 **props** 向子组件传递数据。

父组件：

![父组件动态参数.jpg](https://upload-images.jianshu.io/upload_images/1817117-f7a143ecf273caf0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在调用组件时，父组件的数据可以存在data()里面，然后使用 v-bind 将值绑定在子组件上，方便动态传输

子组件：

![子组件接收参数.jpg](https://upload-images.jianshu.io/upload_images/1817117-27f0b75556e992d8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从父组件获取 data 的值，就需要使用 props: { data: Object }, 另外在 props 中添加了元素之后，就不需要在 data 中再添加变量了

#### 2. 子组件向父组件传递数据

子组件的数据主要通过事件传递给父组件

子组件：

![子组件1.jpg](https://upload-images.jianshu.io/upload_images/1817117-09277688ecb73b2f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![子组件2.jpg](https://upload-images.jianshu.io/upload_images/1817117-4294a2a9d2632010.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

HTML 里面，当 click 事件触发的时候，将 submit 传递给 父组件

我们需要声明一个了方法 submit，用 click 事件来调用 submit 把数据传过去

父组件：

![父组件1.jpg](https://upload-images.jianshu.io/upload_images/1817117-5523c1e36b2b0a68.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![父组件2.jpg](https://upload-images.jianshu.io/upload_images/1817117-9c04fbf30382ffc9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在父组件中，声明了一个方法 changeData，用 submit 事件调用 changeData 方法，获取到从子组件传递过来的 数据

submit 方法中的数据就是从子组件传递过来的数据

#### 3. 子组件向子组件传递数据

Vue没有直接子对子传参的方法，建议将需要传递数据的子组件，都合并为一个组件。如果一定需要子对子传参，可以先从传到父组件，再传到子组件。

如果有比较的数据需要传输，可以使用Vue状态管理工具 Vuex，可以很方便实现组件之间的参数传递


> ### 组件之间的方法调用

#### 1. 父组件调用子组件的方法

这个相对简单一点，首先在子组件引用的时候，给一个ref参数CheckSubmit

```
<check-submit ref="CheckSubmit" :data="{source: 'Check', name: $route.query.name}"></check-submit>
```

然后可以先这个 **this.$ref.CheckSubmit.open** 直接调用CheckSubmit组件里面的open方法

#### 2. 子组件调用父组件的方法

- 上面说的子组件向父组件传递数据说的，使用$emit向父组件触发一个事件，父组件监听这个事件

- 直接用this.$parent.xxxx这样直接调用父组件的方法
