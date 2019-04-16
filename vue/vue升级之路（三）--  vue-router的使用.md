使用 Vue 构建的项目，一个页面是由多个组件构成的，而且经常是做成单页面应用，所以在跳转页面的时候，传统的 href 已经跟不上时代的步伐了，于是 vue-router 应运而生

在使用 vue-router 的时候，需要看看自己是否装了这个依赖，没有的话可以使用  `npm install vue-router -S` ，不过现在构建vue项目时有可以选择是否安装 vue-router，大家注意一下就行了

### 一、路由的配置

自动安装的vue-router，会在src 文件夹下有个一个 router -> index.js 文件
在 index.js 中创建 routers 对象，引入所需的组件并配置路径

![路由配置.jpg](https://upload-images.jianshu.io/upload_images/1817117-97522e9caf36b6a9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


在创建的 routers 对象中， path 配置了路由的路径，component 配置了映射的组件

然后在main.js里面引入router文件

![引入路由文件.jpg](https://upload-images.jianshu.io/upload_images/1817117-ddc0cb15e436b514.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在创建的 router 对象中，如果不配置 mode，就会使用默认的 hash 模式，该模式下会将路径格式化为 #! 开头。

添加 mode: 'history' 之后将使用 HTML5 history 模式，该模式下没有 # 前缀，而且可以使用 pushState 和 replaceState 来管理记录。

关于 HTML5 history 模式的更多内容，可以自行度娘

### 二、嵌套路由

在构建的vue实例中，为了加深项目层级，App.vue 只是作为一个存放组件的容器

![app.png](https://upload-images.jianshu.io/upload_images/1817117-0ea36fa15229045c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

其中 <router-view> 是用来渲染通过路由映射过来的组件，当路径更改时，<router-view> 中的内容也会发生更改

上面已经配置了两个路由，当打开 http://localhost:8080 或者 http://localhost:8080/index的时候，就会在 <router-view> 中渲染 index.vue 组件
index.vue 是真正的父组件，其它的子组件都会渲染到 index.vue 中的 <router-view>
![父组件.jpg](https://upload-images.jianshu.io/upload_images/1817117-ad2aa13486c00fb5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

想要在一级路由中实现嵌套二级路由，就要修改 router -> index.js
![嵌套路由.jpg](https://upload-images.jianshu.io/upload_images/1817117-b827da1448b18e09.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在配置的路由后面，添加 children，并在 children 中添加二级路由，就能实现路由嵌套。
配置 path 的时候，以 " / " 开头的嵌套路径会被当作根路径，所以子路由的 path 需不需要添加 " / " 就要看个人需求了

### 三、使用 <router-link> 映射路由

如果只需要跳转页面，不需要添加验证方法的情况，可以使用 <router-link> 来实现导航的功能：
```
 <router-link class="item"  to="/index/login" >{{ text }}</router-link>
 <router-link class="item"  :to="{path:url, query:data}" >{{ text }}</router-link>
```
在编译之后，<router-link> 会被渲染为 <a> 标签， to 会被渲染为 href，当 <router-link> 被点击的时候，url 会发生相应的改变

如果使用 v-bind 指令，还可以在 to 后面接变量，配合 v-for 指令可以渲染路由菜单

如果需要传入不同参数 ，可以在路由中添加动态参数，给 to 传入一个对象
```
{
  path: item.url,
  query: { id: '007' }
}
```

然后还可以使用 $route.query.id 来获取到对应的参数

### 四、编程式导航
然而在实际项目下，有很多链接在执行跳转之前，还会执行方法对数据进行处理，这时可以使用 this.$router.push(location) 来修改 url 完成跳转
```
// 绑定goLogin
<button class="login" @click="goLogin"></button>
// 定义goLogin
methods: {
        goLogin() {
            this.routes.push('/login')
        }
    },
```
push 后面可以是对象，也可以是字符串：
```
// 字符串
this.$router.push('/index')

// 对象
this.$router.push({ path: '/index' })

// 命名的路由
this.$router.push({ name: 'login', params: { userId: '123' }})
```



