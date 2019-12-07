```
// 有关Promise的异步执行问题
new Promise(resolve => {
    resolve(1);
    Promise.resolve().then(() => console.log(2));
    console.log(4)
}).then(t => console.log(t));
console.log(3);
```

> 使用逻辑或命名
```
var arg1 = "";
arg1 = arg1 || 10;
alert(txt ? txt : "hehe");
alert(txt || "hehe");

// null没有分配空间，""分配了空间，
// 为null的对象还不是一个实例化的对象，而为""已经实例化。
// undefined则为未定义的
```

```
// 原始值: 存取直接作用于它自身。例如string，number，boolean，null，undefined
var foo =1; 
var bar = foo;
bar = 9;
console.log(foo, bar); // => 1, 9
// 复杂类型: 存取时作用于它自身值的引用。例如object，array，function
var foo = [1, 2];
var bar = foo;
bar[0] =9;
console.log(foo[0], bar[0]); // => 9, 9
```

```
// 用typeof检测数据类型
var fn = function(n){
  console.log(n);
}
var str = 'string';
var arr = [1,2,3];
var obj = {
   a:123,
   b:456
};
var num = 1;
var b = true;
var n = null;       var u = undefined;
//方法一使用typeof方法。
console.log(typeof str);//string
console.log(typeof arr);//object
console.log(typeof obj);//object
console.log(typeof num);//number
console.log(typeof b);//boolean
console.log(typeof n);//null是一个空的对象
console.log(typeof u);//undefined
console.log(typeof fn);//function
// 方法二:instanceof
var o = {
    'name':'lee'
};
var a = ['reg','blue'];
console.log(o instanceof Object);// true
console.log(a instanceof Array);//  true
console.log(o instanceof Array);//  false
```

> JavaScript 获取当前时间戳：
```
// 第一种方法：
var timestamp = Date.parse(new Date());
// 结果：1280977330000
// 第二种方法：
var timestamp = (new Date()).valueOf();
// 结果：1280977330748
// 第三种方法：
var timestamp=new Date().getTime()；
// 结果：1280977330748

// 第一种：获取的时间戳是把毫秒改成000显示，
// 第二种和第三种是获取了当前毫秒的时间戳。
```

> 页面传值的中文乱码可以用escape()编码解决


```
history.back(-1); // 直接返回当前页的上一页，数据全部消息，是个新页面
history.go(-1); // 也是返回当前页的上一页，不过表单里的数据全部还在
history.back(0); // 刷新 
history.back(1); // 前进 
history.back(-1); // 后退
window.location.reload() // 刷新
```

> console
```
console.log()
console.info()
console.warn()
console.error()
```

字符串截取
```
// 提取字符串中介于两个指定下标之间的字符
string.substring(0,1)
// 提取从指定位置开始的指定长度的子字符串
string.substr(0,1)
```

保存小数点后n位数
```
number.toFixed(n);
```

使用length属性清空或截断数组
```
var myArray = [12, 222, 1000, 124, 98, 10]; 
myArray.length = 0;
myArray.length = 4;
```

获取指定范围内的随机数
```
// 直接获取
var x = Math.floor(Math.random() * (max - min + 1)) + min;
// 传入随机数最小值和最大值
random: function(min, max) {
	return parseInt(Math.random() * (max - min) + min);
},
```

数组去重（indexOf版）
```
function unique(array){
	var n = []; 
	for(var i = 0; i < array.length; i++) { 
		if (n.indexOf(array[i]) == -1) n.push(array[i]);
	} 
	return n;
}
var new = [...new Set(array)]; // 装逼版

// 数组根据json数据去重
var newArr = [addr.addrArr[0]];
for(var i = 1; i < addr.addrArr.length; i++){
	var addrItem = addr.addrArr[i];
	var repeat = false;
	for (var j = 0; j < newArr.length; j++) {
		if (addrItem.createTime == newArr[j].createTime) {
       		repeat = true;
       		break;
		}	
	}
	if (!repeat) {
		newArr.push(addrItem);
 	}
}
```

['1','2','3'].map(parseInt)
```
var arr = ['1','2','3'];
console.log(arr.map(parseInt));
console.log(arr.map(function(string, radix, others) {
	console.log(string, radix, others);
}));
// parseInt(string, radix)
// 有两个参数，第一个被转换的数字，第二个向n进制转换
// 在没有参数的时候，第一参数为数组内容，第二参数为索引下标
// 当第二参数大于0时，第一参数若不是数字，即输出NaN
```

千分符间隔（正则版）
```
function toThousands(num) {
    return (num || 0).toString().replace(/(\d)(?=(?:\d{3})+$)/g, '$1,');
}
toThousands(123456789)
// > "123,456,789"
function format(number) {
    var regx = /\d{1,3}(?=(\d{3})+$)/g;
    return (number + '').replace(regx, '$&,')  // $&表示与regx相匹配的字符串
}
```

时间戳格式化
```
function formatDate(timeStamp) {    
    var date = new Date(timeStamp);
    var y = date.getFullYear();    
    var m = date.getMonth() + 1;    
    m = m < 10 ? ('0' + m) : m;    
    var d = date.getDate();    
    d = d < 10 ? ('0' + d) : d;    
    var h = date.getHours();  
    h = h < 10 ? ('0' + h) : h;  
    var minute = date.getMinutes();  
    var second = date.getSeconds();  
    minute = minute < 10 ? ('0' + minute) : minute;    
    second = second < 10 ? ('0' + second) : second;   
    return y + '-' + m + '-' + d+' '+h+':'+minute+':'+second;    
}
```

> 添加script Tag，动态添加JS
```
<script>
    window.load_page = function (path) {
        var staticPath = "..." //动态设置的static path
        var $script = document.createElement("script");
        $script.src = static + path + '.js'; //这里想怎么改就怎么改
        document.body.appendChild($script); //动态生成的js放到body最底部
    }
</script>
```

```
// 监听页面关闭事件
window.onbeforeunload = function (e) {
    e = e || window.event;
    // 兼容IE8和Firefox 4之前的版本
    if (e) {
        e.returnValue = '关闭提示';
    }
    // Chrome, Safari, Firefox 4+, Opera 12+ , IE 9+
     return '关闭提示';
};
window.onunload = function(event) {
    alert('onunload');
}
window.onunloadcancel = function() {
    alert(onunloadcancel);
}
```

```
retrun true;    // 返回正确的处理结果。
return false;   // 分会错误的处理结果，终止处理。
return;         // 把控制权返回给页面。
```
- return;返回null，起到中断方法执行的效果，只要不return false事件处理函数将会继续执行，表单将提交
- return false;，事件处理函数会取消事件，不再继续向下执行。比如表单将终止提交。




> js对中文进行url编码
 - 3个函数：`escape, encodeURI, encodeURIComponent`
    - encodeURI 用于对整个URL进行编码
    - encodeURIComponent 用于对URL的组成部分进行个别编码
 - 相应3个解码函数：`unescape,decodeURI,decodeURIComponent `
 
1. escape不编码字符有69个：*，+，-，.，/，@，_，0-9，a-z，A-Z
2. encodeURI不编码字符有82个：!，#，$，&，'，(，)，*，+，,，-，.，/，:，;，=，?，@，_，~，0-9，a-z，A-Z
3. encodeURIComponent不编码字符有71个：!， '，(，)，*，-，.，_，~，0-9，a-z，A-Z

> HTML5手机界面 输入法把footer挤压的问题
```
// 修改定位
var oHeight = $(document).height(); //浏览器当前的高度
$(window).resize(function(){             
    if($(document).height() < oHeight){     
        $("#footer").css("position","static");
    }else{
        $("#footer").css("position","absolute");
    }
});
```

> 返回顶部
```
$(window).scroll(function() {
    var a = $(window).scrollTop();
    if(a > 100) {
        $('.go-top').fadeIn();
    }else {
        $('.go-top').fadeOut();
    }
});
$(".go-top").click(function(){
    $("html,body").animate({scrollTop:"0px"},'600');
});
```

> 判断滚动条是否到底部
```
$(window).scroll(function() {　　
	var scrollTop = $(this).scrollTop();　　
	var docHeight = $(document).height();　　
	var windowHeight = $(this).height();
	var scrollHeight=document.body.scrollHeight;
	console.log("scrollTop:"+scrollTop);
	console.log("scrollbottom:"+(docHeight-scrollTop-windowHeight));
	if(scrollTop + windowHeight == docHeight) {　　　　
		alert("已经到最底部了！");　　
	}
})

window.onscroll = function() {
    var scrollT = document.documentElement.scrollTop || document.body.scrollTop; //滚动条的垂直偏移
    var scrollH = document.documentElement.scrollHeight || document.body.scrollHeight; //元素的整体高度
    var clientH = document.documentElement.clientHeight || document.body.clientHeight; //元素的可见高度
    if (scrollT == scrollH - clientH) {
        console.log("到底部了");
    } else if (scrollT < scrollH - clientH) {
        console.log("到顶了")
    }
}
//文档高度
function getDocumentTop() {
    var scrollTop = 0, bodyScrollTop = 0, documentScrollTop = 0;
    if (document.body) {
        bodyScrollTop = document.body.scrollTop;
    }
    if (document.documentElement) {
        documentScrollTop = document.documentElement.scrollTop;
    }
    scrollTop = (bodyScrollTop - documentScrollTop > 0) ? bodyScrollTop : documentScrollTop;    
    return scrollTop;
}
//可视窗口高度
function getWindowHeight() {
    var windowHeight = 0;    if (document.compatMode == "CSS1Compat") {
        windowHeight = document.documentElement.clientHeight;
    } else {
        windowHeight = document.body.clientHeight;
    }
    return windowHeight;
}
//滚动条滚动高度
function getScrollHeight() {
    var scrollHeight = 0, bodyScrollHeight = 0, documentScrollHeight = 0;
    if (document.body) {
        bodyScrollHeight = document.body.scrollHeight;
    }
    if (document.documentElement) {
        documentScrollHeight = document.documentElement.scrollHeight;
    }
    scrollHeight = (bodyScrollHeight - documentScrollHeight > 0) ? bodyScrollHeight : documentScrollHeight;    
    return scrollHeight;
}
// 监听srcoll事件
window.onscroll = function () {
    if(getScrollHeight() == getWindowHeight() + getDocumentTop()){
        alert("已经到最底部了！")
    }
}
```

> 判断页面滚动方向
```
$(function() {
    function scroll(fn) {
        var beforeScrollTop = document.body.scrollTop,
            fn = fn || function() {};
        window.addEventListener("scroll", function() {
            var afterScrollTop = document.body.scrollTop,
                delta = afterScrollTop - beforeScrollTop;
            if(delta === 0) return false;
            fn(delta > 0 ? "down" : "up");
            beforeScrollTop = afterScrollTop;
        }, false);
    }
    scroll(function(direction) {
        if(direction == "down") {
            console.log("向下滚");
        } else {
            console.log("向上滚");
        }
    })
})
```

> 手指滑动方向
```

<script type="text/javascript">
    var windowHeight = $(window).height();
    $("body").css("height", windowHeight);
    var startX, startY, moveEndX, moveEndY, X, Y;
    $("body").on("touchstart", function(e) {
        e.preventDefault();
        startX = e.originalEvent.changedTouches[0].pageX, startY = e.originalEvent.changedTouches[0].pageY;
    });
    $("body").on("touchmove", function(e) {
        e.preventDefault();
        moveEndX = e.originalEvent.changedTouches[0].pageX, moveEndY = e.originalEvent.changedTouches[0].pageY, X = moveEndX - startX, Y = moveEndY - startY;
        if (Math.abs(X) > Math.abs(Y) && X > 0) {
            alert("left to right");
        } else if (Math.abs(X) > Math.abs(Y) && X < 0) {
            alert("right to left");
        } else if (Math.abs(Y) > Math.abs(X) && Y > 0) {
            alert("top to bottom");
        } else if (Math.abs(Y) > Math.abs(X) && Y < 0) {
            alert("bottom to top");
        } else {
            alert("just touch");
        }
    });
</script>

```

> 判断是否空对象
```
// for...in...遍历属性，为真则为“非空数组”；否则为“空数组”
function  isEmptyObject (e) {
    for (var t in e) {
        console.log(t)
        return !1
    }
    return !0
}

// hasOwnProperty：是用来判断一个对象是否有你给出名称的属性或对象。不过需要注意的是，此方法无法检查该对象的原型链中是否具有该属性，该属性必须是对象本身的一个成员。
// isPrototypeOf:是用来判断要检查其原型链的对象是否存在于指定对象实例中，是则返回true，否则返回false。
// data.hasOwnProperty('name')

// 通过JSON自带的.stringify方法来判断
if(JSON.stringify(obj)=='{}') {
    conosle.log('空对象')
}

// ES6新增的方法Object.keys()
if(Object.keys(obj).length==0) {
    conosle.log('空对象')
}
```

> 设置cookie
```
document.cookie = name + "=" + value + ";expires=" + date + ";path=/";
```


> 数组合并的两种方法
```
// 第一种
var a = [4, 6], 
    b = [8, 5], 
    c = a.concat(b);
// c: [4, 6, 8, 5]

// 第二种
var a = [4,5,6],
    b = [7,8,9];  
Array.prototype.push.apply(a, b);  
// a: [4, 5, 6, 7, 8, 9]  
```

> 将数组arr2插入到数组arr1的index位置： 
```
var arr1 = ['a', 'b', 'c'];
var arr2 = ['1', '2', '3'];
var index = 1;
arr2.unshift(index, 0);
Array.prototype.splice.apply(arr1, arr2);
console.log(arr1);
// 显示结果： ["a", "1", "2", "3", "b", "c"]
```

> 十进制转十六进制
```
function 10to16(n) {
    n = n.toString(16)
    if (n.length === 1) {
        n = '0' + n.toUpperCase()
    }
    return n
}
```

```
new Date().toLocaleString().replace(/[:\-\s\u4e00-\u9fa5]/g, '')
new Date().toLocaleString().replace(/[:\/\s\u4e00-\u9fa5]/g, '')
```

```
//计算两个日期之间的天数
function getDateDiff(date1, date2) {
    var arr1 = date1.split('-');
    var arr2 = date2.split('-');
    var d1 = new Date(arr1[0], arr1[1], arr1[2]);
    var d2 = new Date(arr2[0], arr2[1], arr2[2]);
    return (d2.getTime() - d1.getTime()) / (1000 * 3600 * 24);
}
//日期的下一天
function getNextDay(d) {
    d = new Date(d);
    d = +d + 1000 * 60 * 60 * 24;
    d = new Date(d);
    return d.getFullYear() + "-" + (d.getMonth() + 1) + "-" + d.getDate();
}
```

> ios微信浏览器返回不刷新问题
```
window.onpageshow = function (e) {
    if (e.persisted) {
        window.location.reload(true)
    } 
}
```

> canvas将图片转Base64
```
var img = "imgurl"; //imgurl 就是你的图片路径  
function getBase64Image(img) {
    var canvas = document.createElement("canvas");
    canvas.width = img.width;
    canvas.height = img.height; var ctx = canvas.getContext("2d");
    ctx.drawImage(img, 0, 0, img.width, img.height); 
    var ext = img.src.substring(img.src.lastIndexOf(".") + 1).toLowerCase(); 
    var dataURL = canvas.toDataURL("image/" + ext);
    return dataURL;
}
var image = new Image();
image.src = img;
image.onload = function() {
    var base64 = getBase64Image(image);
    console.log(base64);
}
```

> 通过Ajax方式上传文件(input file)，使用FormData进行Ajax请求

```
<div>
    <input type="file" name="FileUpload" id="FileUpload">
    <a class="layui-btn layui-btn-mini" id="btn_uploadimg">上传图片</a>
</div>

<script type="text/jscript">
$(function() {
    $("#btn_uploadimg").click(function () {
        var fileObj =  document.getElementById("FileUpload").files[0]; // js 获取文件对象
        if (typeof (fileObj) == "undefined" ||  fileObj.size <= 0) {
            alert("请选择图片");
            return;
        }
        var formFile = new FormData();
        formFile.append("action", "UploadVMKImagePath");  
        formFile.append("file", fileObj);  //加入文件对象
    
        //第一种  XMLHttpRequest 对象
        //var xhr = new XMLHttpRequest();
        //xhr.open("post",   "/Admin/Ajax/VMKHandler.ashx", true);
        //xhr.onload = function () {
        //    alert("上传完成!");
        //};
        //xhr.send(formFile);

        //第二种 ajax 提交

        var data = formFile;
        $.ajax({
            url: "/Admin/Ajax/VMKHandler.ashx",
            data: data,
            type: "Post",
            dataType: "json",
            cache: false,//上传文件无需缓存
            processData: false,//用于对data参数进行序列化处理 这里必须false
            contentType: false, //必须
            success: function (result) {
                alert("上传完成!");
            },
        })
        // 跨域时使用jsonp
        // dataType : "jsonp", // 返回的数据类型，设置为JSONP方式
        // jsonp : 'callback', //指定一个查询参数名称来覆盖默认的 jsonp 回调参数名 callback
        // jsonpCallback: 'handleResponse', //设置回调函数名
    })
})
   </script>
```

> 判断一个变量是对象还是数组？
```
function isObjArr(value){
    if (Object.prototype.toString.call(value) === "[object Array]") {
        console.log('value是数组');
    } else if (Object.prototype.toString.call(value)==='[object Object]'){//这个方法兼容性好一点
        console.log('value是对象');
    } else {
        console.log('value不是数组也不是对象')
    }
}
// 不能使用typeof来判断对象和数组，因为这两种类型都会返回"object"。
```

> 进入全屏
```
// Launch full screen
function launchFullscreen(element) {
  if(element.requestFullscreen) {
    element.requestFullscreen();
  } else if(element.mozRequestFullScreen) {
    element.mozRequestFullScreen();
  } else if(element.webkitRequestFullscreen) {
    element.webkitRequestFullscreen();
  } else if(element.msRequestFullscreen) {
    element.msRequestFullscreen();
  }
}
```

> 获取URL参数
```
function GetURLlist(name) {
    var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)");
    var r = window.location.search.substr(1).match(reg);
    if(r != null) return unescape(r[2]);
    return null;
}

function getURLParam(name) {
    return decodeURIComponent((new RegExp('[?|&]' + name + '=' + '([^&;]+?)(&|#|;|$)', "ig").exec(location.search) || [, ""])[1].replace(/\+/g, '%20')) || null;
}
```

> 全部替换 replaceAll
```
//把bigStr中的所有str1替换为str2
var replaceAll = function(bigStr, str1, str2) {  
    var reg = new RegExp(str1, 'gm');
    return bigStr.replace(reg, str2);
}
```

> IOS和安卓判断
```
var u = navigator.userAgent, 
    app = navigator.appVersion;
var isAndroid = u.indexOf('Android') > -1 || u.indexOf('Linux') > -1; //android终端或者uc浏览器
var isiOS = !!u.match(/\(i[^;]+;( U;)? CPU.+Mac OS X/); //ios终端
if(isAndroid){
    return 'android';
}else if(isiOS){
    return 'ios';
}
else{
    return false;
}
```

> 判断微信
```
function isWeiXin(){
    var ua = window.navigator.userAgent.toLowerCase();
    if(ua.match(/MicroMessenger/i) == 'micromessenger'){
        return true;
    }else{
        return false;
    }
}
```
```
var u = navigator.userAgent;
var isAndroid = u.indexOf('Android') > -1 || u.indexOf('Linux') > -1|| u.indexOf('MI') > -1|| u.indexOf('XiaoMi') > -1; //android终端或者uc，小米等奇葩浏览器
if(!isAndroid) {} 
if(isAndroid)  {}
```

> base64 to blob
```
function base64UrlToBlob(urlData) {
    var bytes = window.atob(urlData); //去掉url的头，并转换为byte
    //处理异常,将ascii码小于0的转换为大于0
    var ab = new ArrayBuffer(bytes.length);
    var ia = new Uint8Array(ab);
    for (var i = 0; i < bytes.length; i++) {
        ia[i] = bytes.charCodeAt(i);
    }
    return new Blob([ab], {
        type: 'image/jpg'
    });
}
```

> base64 to File
```
function base64UrlToFile(img) {
    var blob = new Blob([img], {
        type: 'image/png'
    });
    var file = new File([blob], 'imageFileName.png');
}
// 可以转换，但在测试时上传后，取出来出现图片格式错误
```

> 参数非空检测
这个方法特别适用于封装函数时使用，也许我们知道可以在函数参数中直接指定一个默认值，像下面这样
```
这个方法特别适用于封装函数时使用，也许我们知道可以在函数参数中直接指定一个默认值，像下面这样
```
然而，我们也可以直接赋值一个函数，如果没有传参，我们就直接抛出错误提醒，如果一个组件里面有很多方法，我们就可以直接复用，而不用每个方法里面都去做非空判断处理。
```
const isRequired = () => { throw new Error('param is required'); };

const hello = (name = isRequired()) => { console.log(`hello ${name}`) };
// 抛出一个错误，因为参数没有传
hello()；
// 没有问题
hello('hello')

```