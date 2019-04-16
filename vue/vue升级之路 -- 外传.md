- filters
- computed

### 一、 iOS 或者Android调用vue.js 里面的方法

在iOS或者Android里面是调用不了这个getFn方法的。所以我们要把getFn这个方法挂到window上面去，然后直接可以用方法名调用了。

![vue方法.jpg](https://upload-images.jianshu.io/upload_images/1817117-acda96395ab2b75a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

后面有再做了测试，似乎在方法传参传不了对象
其实应该是说传了对象那边会接收不到


### 二、vue实例化
vue $mount()手动挂载
```
当Vue实例没有el属性时，则该实例尚没有挂载到某个dom中；
假如需要延迟挂载，可以在之后手动调用vm.$mount()方法来挂载。例如：

new Vue({
//el: '#app',
router,
render: h => h(App)
// render: x => x(App)
// 这里的render: x => x(App)是es6的写法
// 转换过来就是：  暂且可理解为是渲染App组件
// render:(function(x){
//  return x(App);
// });
}).$mount("#app");
```
或者
```
new Vue({
el: '#app',
router,
render: h => h(App)
// render: x => x(App)
// 这里的render: x => x(App)是es6的写法
// 转换过来就是：  暂且可理解为是渲染App组件
// render:(function(x){
//  return x(App);
// });
});
```