组件化的项目开发中，组件应当划分为三个层次：**组件、模块、页面**
微信小程序已经为开发者封装好了[基础组件](https://developers.weixin.qq.com/miniprogram/dev/component/)，页面文件（pages）也有了详细的规定
而模块就需要自行开发，并且要和页面文件区分开，这就涉及到自定义组件

**开发者可以将页面内的功能模块抽象成自定义组件，以便在不同的页面中重复使用；也可以将复杂的页面拆分成多个低耦合的模块，有助于代码维护。**自定义组件在使用时与基础组件非常相似。

#####  一、基本用法
在根目录下创建一个 component 目录，用于存放自定义组件
组件也是由 json、wxml、wxss、js 四个文件组成

其中 wxml 部分没有什么特殊的地方，和页面的写法一致
**wxss 也是只对组件生效，而且 app.wxss 中的样式也不会对自定义组件生效**

在组件wxss中不应使用ID选择器、属性选择器和标签名选择器
```
#a { } /* 在组件中不能使用 */
[a] { } /* 在组件中不能使用 */
button { } /* 在组件中不能使用 */
.a > .b { } /* 除非 .a 是 view 组件节点，否则不一定会生效 */
```

最关键的地方在于，需要在 json 中添加配置项：**将 component 字段设为 true**，这样才能注册这个自定义组件
```
{
  "component": true, 
  "usingComponents": {}
}
```

usingComponents字段组件里面可以引入其他组件，在页面引用组件同样也需要在json设置这个字段
```
{
  "navigationBarTitleText": "登录",
  "usingComponents": {
    "toast": "../component/Toast/toast"
  }
}
```
然后就能在登录页面的 wxml 中直接使用该组件

#### 二、组件间的参数传递 
- **组件的属性列表**
小程序的页面 pages 需要使用 [Page()](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/page.html) 来注册，而组件则需要 [Component()](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/component.html) 构造器

如果组件需要接受一个外部数据，比如一个列表组件的数据源 data，可以这么配置：

![](https://upload-images.jianshu.io/upload_images/1817117-1de4333f63e3184a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里的** properties** 类似于 vue 中的 props，表示该对象下的属性将从外部传入
properties 可设置 type、value、observer 三个属性

其中 type 用于指定字段类型（Number，String，Array，Object，Function）
value 表示字段的默认值，**observer 用于定义该字段的监听函数**

在页面上调用组件的时候，直接给 data 赋值就好：`<toast text="登录成功"></toast>`

***PS：开发中应当避免使用 data 为前缀的属性名（如 data-text），这会被识别为 dataset 中的参数***

- **组件事件触发回调**
```
// toast.wxml
<view class="toast" style="{{ pos }}: 60rpx">
  <view class='text' catchtap='_tapEvent'>{{text}}</view>
</view>
// toast.js
methods: {
    // 私有方法：以下划线为前缀命名
    _tapEvent() {
      //触发tap回调(子组件调用父组件的方法)
      this.triggerEvent("tapEvent")
    }
}

// 然后在login调用组件的时候用tapEvent绑定_tapEvent方法
// login.wxml
<toast text="登录成功" pos="top" bind:tapEvent="_tapEvent"></toast>
// login.js
methods: {
    //tab事件
    _tapEvent() {
      console.log('你点击了一下');
    }
}
```
整个的流程是
我们在点击toast组件的时候，执行了catchtap='_tapEvent'方法，这个方法用了this.triggerEvent("tapEvent")，这种是类似广播或者是说自定义事件，触发了在login调用toast组件里面绑定的事件bind:tapEvent并执行了login.js里面定义的方法_tapEvent

- **组件插槽**（具名插槽）
小程序的组件也可以使用 <slot> 来扩展内容
然后**父组件**引入这个组件的时候，可以在组件中插入节点，节点内容会渲染到 <slot> 节点的位置

组件默认只能有一个 <slot>，**如果需要添加多个插槽，首先需要在组件 js 中声明**,这就是具名插槽。比较建议使用这个，因为很容易辨识你加入的节点会渲染指到那个<slot>里面

```
<view class='wx_dialog_container' hidden="{{!isShow}}">
    <view class='wx-mask'></view>
    <view class='wx-dialog'>
        <slot name="title"></slot>
        <view class='wx-dialog-content'>{{ content }}</view>
        <view class='wx-dialog-footer'>
          <view class='wx-dialog-btn' catchtap='_cancelEvent'>{{ cancelText }}</view>
          <view class='wx-dialog-btn' catchtap='_confirmEvent'>{{ confirmText }}</view>
        </view>
    </view>
</view>

<dialog id="dialog" title='提示' content="{{quickData}}" bind:cancelEvent="_cancelEvent"
   bind:confirmEvent="_confirmEvent">
    <view class='wx-dialog-title' slot="title">温馨提示</view>
  </dialog>
```
这样使用插槽之后title就可以随意在父组件进行修改，甚至设置其他元素，输入框、按钮、icon都是可以的

#### 三、事件传参
在view中 ，我们用bindtap绑定了showDia方法，并且自定义data-name属性（使用过JQ的人会知道这种方法跟JQ的自定义属性取值是一样的）
```
<view class="float-item" wx:for="{{quick}}" wx:key="*this" data-name="{{item}}" bindtap='showDia'>
        <view class="icon"></view>
        {{ item }}
</view>
// 然后通过e.currentTarget.dataset可以拿到自定义的值
showDia(e) {
    console.log(e.currentTarget.dataset.name)
 },
```
这时候大家就可以看到上面说的，避免使用data作为前缀的属性名的原因了，
，data作为前缀的属性名会被dataset识别，混搅到自定义属性取值的使用

这里还有注意两点：
1、data-名称 不能有大写字母，如果需要，可以通过 - （中划线）来连接单词，编译的时候小程序会将第二个单词首字母自动大写。data-* 属性中不可以存放对象。
2、注意打印结果中target和currentTarget的区别。
     - target 触发事件的源组件。 
     - currentTarget 事件绑定的当前组件。

#### 四、URL 传参
如果需要页面跳转，那么就可以使用 **navigateTo()** 方法或者使用**navigator标签的url属性** 跳转页面，这样可以在 url 后面接 query 参数
```
wx.navigateTo({
   url: '../login/login?source=index'
})
// 或者
<navigator url="{{item.url + '?name=' + item.name}}"></navigator>
```

然后在 **Page 页面**的生命周期函数 **onLoad** 中可以接收到这些参数
```
onLoad: function (options) {
    console.log(options)
 }
```
![](https://upload-images.jianshu.io/upload_images/1817117-8508938aaa560ae3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这种方式**只能通过 navigateTo方法或者navigator 标签发送，onLoad 接收**
所以只能用于跳转到非 tabbar 的**页面**

#### 五、简易动画
因为做组件的时候需要一下动画效果，所以顺便说下小程序封装好的动画函数wx.createAnimation()的使用
它创建一个动画实例animation，调用实例的方法来描述动画，最后通过动画实例的`export`方法导出动画数据传递给组件的`animation`属性。
**注意: `export` 方法每次调用后会清掉之前的动画操作**

```
// 首先我们需要在data里面定义一个参数来存放动画
data: {
    animationData: {}
 },
// 然后把参数用animation属性绑定到组件上，在哪使用就绑定在哪
<view class="toast" animation="{{animationData}}"></view>
``` 

```
methods: {
    // 公有方法
    show: function() {
      // 创建一个动画实例，可配置参数（持续时间，运动曲线，延时，设置transform-origin）
      var animation = wx.createAnimation({
        duration: 500,
        timingFunction: 'ease-out',
        // delay: 1000,
        // transformOrigin: 50% 50%
      })
      // animation有rotate旋转、scale缩放、translate偏移、skew倾斜等方法
      animation.translateY(100).rotate(360).scale(2, 2).step()
      this.setData({
        // 清掉前面动画操作
        animationData: animation.export()
      })
    }
}
```
然后可以在父组件调用toast里面的方法
```
// 调用前需要用获取toast，才可以使用toast组件里面的方法
onReady: function () {
      this.toast = this.selectComponent("#toast");
},
// 然后定义showToast
showToast() {
      this.toast.show()
},
```

今天写的涉及的一些api
自定义组件：[https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/)
事件传参：[https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html)
url传参：[https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html](https://developers.weixin.qq.com/miniprogram/dev/component/navigator.html)
[https://developers.weixin.qq.com/miniprogram/dev/api/ui-navigate.html#wxnavigatetoobject](https://developers.weixin.qq.com/miniprogram/dev/api/ui-navigate.html#wxnavigatetoobject)
动画函数：
[https://developers.weixin.qq.com/miniprogram/dev/api/api-animation.html#wxcreateanimationobject](https://developers.weixin.qq.com/miniprogram/dev/api/api-animation.html#wxcreateanimationobject)