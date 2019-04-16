
#### 一、创建项目

首先要有一个小程序开发者账号，可以通过官网教程去了解一下
[https://mp.weixin.qq.com/debug/wxadoc/dev/index.html](https://mp.weixin.qq.com/debug/wxadoc/dev/index.html)
然后可以在 “开发设置” 中获取到**AppID**
，然后打开微信web开发者工具创建项目

下面是微信web开发者工具的下载地址：
[https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)

打开后可以看到创建项目的界面，需要建空目录供项目存放，还有就是需要上面所说的AppID
![image.png](https://upload-images.jianshu.io/upload_images/1817117-d37432809242c0f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以选择模板创建普通及插件快速启动模板，还有用nodejs和php创建有后台模板，不过需要绑定腾讯云。


 **一个 AppId 可以开发多个小程序，不过发布的时候只能发布一个**

生成的项目结构如下：
![image.png](https://upload-images.jianshu.io/upload_images/1817117-c961c4d873f48e49.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

其中 **project.config.json** 是整个项目的配置文件，里面包含了微信（小程序基础库）版本、appid 等信息

具体配置项可以参考 [https://developers.weixin.qq.com/miniprogram/dev/devtools/projectconfig.html](https://developers.weixin.qq.com/miniprogram/dev/devtools/projectconfig.html)

utils 文件夹用于存放一些公用脚本，pages 文件夹存放页面文件，app.js、app.json、app.wxss 构成程序主体

#### 二、app全局配置
项目根目录下的app.json、app.js、app.wxss都是做为全局配置的存在

app.json里面有配置页面路径列表的pages、配置全局默认窗口的window、及配置底部菜单的tabBar
![image.png](https://upload-images.jianshu.io/upload_images/1817117-aa85cd44f9889682.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

app.js 里面的App() 函数可以用来注册一个小程序，它可以监听app在运行时的事件，例如程序的初始化、页面的进入及跳转、触发脚本出错时的信息等等，还可以定义一些全局数据globalData，方便其余页面调用

![image.png](https://upload-images.jianshu.io/upload_images/1817117-9d553dfa5cfa2fec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

app.wxss 就是全局的css了，里面设置的样式将会运用在所有的页面里面

想了解更多可以看看下面的文档，里面还有对以上配置的详解
[https://developers.weixin.qq.com/miniprogram/dev/framework/config.html](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html)


#### 三、页面结构

从创建的项目中我们可以看得出来，微信小程序中采用 WXML + WXSS + JS + JSON 的方式渲染页面，在pages里面每个页面都是一个单独的文件夹存放着这四种文件

**1\. WXML**  网页模板，用于编译小程序的 html 部分*

微信小程序也采用了MVVM的形式，所以 WXML 实际写法更类似于 Vue

比如常见的用 {{ }} 绑定数据

```
// 基础绑定
<view> {{ message }} </view>

// 组件属性
<view id="item-{{id}}"> </view>

// 控制属性
<view wx:if="{{condition}}"> </view>
```

WXML 还可以实现列表渲染、自定义模板，详情可以查看文档中关于[WXML](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/)的介绍

需要留意的是，小程序底层将 html 元素都封装成了组件

**所以不能在 WXML 文件中直接使用 div、p、span 等标签，而是使用[组件](https://developers.weixin.qq.com/miniprogram/dev/component/)**

**2\. WXSS** 样式语言，基于CSS扩展*

可以直接写CSS样式，但是只支持部分选择器，**比如 * 和 > 选择器都是不支持的**

另外只有 app.wxss 中的样式是全局样式，对所有组件生效。**pages 目录下的 wxss 仅对当前组件生效**

WXSS 中还新增了尺寸单位 **rpx**，并规定屏幕宽度固定为750rpx

所以在宽度为 750px 的设计稿中，1rpx = 750/750 = 1px，

如果是宽度为 640px 的设计稿，1rpx = 750/640 = 1.17px

**3\. js** 还需要解释么*

微信小程序中没有再对 js 进行扩展，只是在触发[事件](https://developers.weixin.qq.com/miniprogram/dev/framework/view/wxml/event.html)的时候，分为 bind 和 catch 两种形式

```
<view id="outer" bind:tap="handleTap1">
  outer view
  <view id="middle" catch:tap="handleTap2">
    middle view
    <view id="inner" bind:tap="handleTap3">
      inner view
    </view>
  </view>
</view>
```

二者的区别在于，**bind 不会阻止冒泡事件向上冒泡，catch 可以阻止冒泡事件向上冒泡**

另外，bind 和 catch 后面的冒号可以省略

**4\. json** 页面配置文件*

app.json 是小程序的全局配置文件，包括了小程序的所有页面路径、界面表现等

详细配置信息可以查看这里：[https://developers.weixin.qq.com/miniprogram/dev/framework/config.html](https://developers.weixin.qq.com/miniprogram/dev/framework/config.html)

pages 目录下的 json 文件和 app.json 类似，但是 page.json 只对当前组件生效

 #### 四、页面配置

假如有个文件夹index
那么里面的index.json 就是用于设置小程序的状态栏、导航条、标题、窗口背景色

![image.png](https://upload-images.jianshu.io/upload_images/1817117-9716739ca4457327.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要注意的是，这个json里面不能加注释，否则会文件解析错误

另外就是index.js文件
js里面的 Page(Object) 函数用来注册一个页面。接受一个 Object 类型参数，其指定页面的初始数据、生命周期函数、事件处理函数等。
![image.png](https://upload-images.jianshu.io/upload_images/1817117-03e1ee879f4e3e48.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




