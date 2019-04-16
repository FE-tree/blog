在本篇文章所讲的用户登录，包括以下三点：

- open-data获取用户某个信息
- wx.getUserInfo获取用户基本信息
- 调用微信wx.login接口，实现服务器端获取openid登录

用户头像和昵称对于我们开发小程序几乎算是刚需，那么我们应该怎么样正确高效的获取&利用它们呢？

#### 第一种：open-data获取用户某个信息

其实可以将open-data看作图片或字符串，想要控制样式在外层加上view标签以及相应的class即可。

![image.png](https://upload-images.jianshu.io/upload_images/1817117-26391d432d003c44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

相比之前微信公众号获取用户基本信息的方式，这个方案还是比较走心的，如果一些小程序只是对用户的头像、昵称等基本信息中的某个有需求的话就可以直接拿到了，不需要和以前一样大费周章的调用授权去拿回来一堆用不上的东西。

还有就是这里有个小坑，open-data标签是个行元素，如果要控制这个标签，例如说头像的大小时，就需要修改一下display属性了

还有就是获取手机号的方法了，这个算是敏感数据吧，所以被小程序单独弄出来了，
并且获取微信用户绑定的手机号，需先调用**wx.login**接口。

因为需要用户主动触发才能发起获取手机号接口，所以该功能不由 API 来调用，需用 `<button>` 组件的点击来触发。

```
<button open-type="getPhoneNumber" bindgetphonenumber="getPhoneNumber"> </button> 

Page({ 
    getPhoneNumber: function(e) { 
		console.log(e.detail.errMsg) 
		console.log(e.detail.iv) 
		console.log(e.detail.encryptedData) 
	} 
}) 
```
微信小程序文档提到
**目前该接口针对非个人开发者，且完成了认证的小程序开放（不包含海外主体）。需谨慎使用，若用户举报较多或被发现在不必要场景下使用，微信有权永久回收该小程序的该接口权限。**
所以使用时还是要注意一下的

#### 第二种：wx.getUserInfo获取用户基本信息

这个接口更新过可以直接使用了  
```
wx.getUserInfo({
      success: function (res) {
        console.table(res)
      },
      fail: function (res) { },
      complete: function (res) { },
    })
```
![image.png](https://upload-images.jianshu.io/upload_images/1817117-b1d39d67271d0740.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

因为微信基础库更新，取消通过api调用getUserInfo的能力，并且不会出现授权弹框了，如果需要，可以使用button组件的开放能力[open-type]去调用[getUserInfo的文档](https://link.juejin.im/?target=https%3A%2F%2Fdevelopers.weixin.qq.com%2Fminiprogram%2Fdev%2Fapi%2Fopen.html%23wxgetuserinfoobject)方法。

这个需要注意的是
**请确保wx.login早于getUserInfo，不仅是代码执行层面的早，最好是login回调成功之后才去getUserInfo，不然可能会出现后端解密失败的情况，导致登录失败。**

#### 第三种：调用微信wx.login接口，实现服务器端获取openid登录
```
wx.login({
  success: res => {
    console.log(res)
    // res = {errMsg: "login:ok", code: "×××××××××××××××××"}
    // 发送 res.code 到后台换取 openId, sessionKey, unionId
    wx.request({
      url: '/wxapi/openid',
      data: {
        code: data.code
      },
      success: function (res) {
        console.log('拉取openid成功', res)
        wx.showModal({
          title: '获取信息',
          content: JSON.stringify(res.data),
          showCancel: false,
          confirmText: '确定'
        })
        self.globalData.openid = res.data.openid
      },
      fail: function (res) {
        console.log('拉取用户openid失败，将无法正常使用开放接口等服务', res)
      }
    }
})
```
就可以获取到openid了，作为微信开发的都知道，微信的很多接口都需要以openid去作为参数请求接口的。
![image.png](https://upload-images.jianshu.io/upload_images/1817117-de815e65ac9eef2c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

还有就是关于接口的问题
弄个后台 请求一下微信的接口
```
'https://api.weixin.qq.com/sns/jscode2session' + 
        '?appid=' + config.appid + 
        '&secret=' + config.secret + 
        '&js_code=' + code + 
        '&grant_type=authorization_code'
```
传进去一个code，然后在加上自己的appid及密钥就可以了

![image.png](https://upload-images.jianshu.io/upload_images/1817117-0dc043af9626161e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
