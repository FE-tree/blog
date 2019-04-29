### 一、安装一个本地服务器
我自己选择的phpstudy，一来是以前用过一下，二来是这样的集成包用起来方便

### 二、部署ArcGIS API for JavaScript的开发环境
 1、获取ArcGIS API for JavaScript的开发工具包
进入 https://developers.arcgis.com/javascript/ 点击Get the API
![image.png](https://upload-images.jianshu.io/upload_images/1817117-1b243a1e9d4cee07.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/1817117-b75177173b30e97c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/1817117-aebe9bac74791f49.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

把API及ADK（就是相应的Documentation）下载下来
需要注意一下的是：**下载API需要注册arcgis的帐号，需要翻墙才能注册**

 2、设置ArcGIS API for JavaScript开发工具包
- 解压其中下载的arcgis_js_v324_api压缩包，进入解压的文件夹，直至下面的层次目录
path：arcgis_js_v410_api\arcgis_js_api\library

![image.png](https://upload-images.jianshu.io/upload_images/1817117-6dc029ee3f318eb4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

选取4.10这个文件夹，将其复制phpstudy文件夹中的www文件夹的根目录中，即D:\phpStudy\PHPTutorial\WWW\ ，这里我将4.10文件夹改名为arcgis4.10，增加辨识度。

- 修改文件信息

    修改文件一(init.js)
    位置：D:\phpStudy\PHPTutorial\WWW\arcgis4.10\init.js
    将https://[HOSTNAME_AND_PATH_TO_JSAPI]dojo替换成http://localhost/arcgis4.10/dojo

    修改文件二(dojo.js)
    位置：D:\phpStudy\PHPTutorial\WWW\arcgis4.10\dojo\dojo.js
    将https://[HOSTNAME_AND_PATH_TO_JSAPI]dojo替换成http://localhost/arcgis4.10/dojo

 3、测试环境
经过以上的步骤，ArcGIS API for JavaScript本地开发环境已经配置完成，接下来对其进行测试。

新建一个test.html文件，将下面的测试代码复制进去
```
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="initial-scale=1, maximum-scale=1, user-scalable=no">
    <title>ArcGIS DevLabs: Create a JavaScript starter app</title>
    <!-- <link rel="stylesheet" href="https://js.arcgis.com/4.10/esri/css/main.css"> -->
    <!-- <script src="https://js.arcgis.com/4.10/init.js"></script> -->
    <link rel="stylesheet" href="http://localhost/arcgis4.10/esri/css/main.css">
    <script src="http://localhost/arcgis4.10/init.js"></script>
    <style>
    html,
    body,
    #viewDiv {
        padding: 0;
        margin: 0;
        height: 100%;
        width: 100%;
    }
    </style>
    <script>
    require([
        "esri/Map",
        "esri/views/MapView",
        "dojo/domReady!"
    ], function(Map, MapView) {

        var map = new Map({
            basemap: "topo-vector"
        });

        var view = new MapView({
            container: "viewDiv",
            map: map,
            center: [-118.71511, 34.09042],
            zoom: 11
        });
    });
    </script>
</head>

<body>
    <div id="viewDiv"></div>
</body>

</html>
```

把里面的css和js修改成自己搭建的本地服务器地址
<link rel="stylesheet" href="http://localhost/arcgis4.10/esri/css/main.css">
<script src="http://localhost/arcgis4.10/init.js"></script>


使用浏览器打开test.html文件
如果浏览器加载网页如下，说明环境已经配置成功！

![image.png](https://upload-images.jianshu.io/upload_images/1817117-d6b2853f0515ab99.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

另外，可以到以下网址下载arcgis的一些简单的demo
https://developers.arcgis.com/javascript/latest/sample-code/index.html
![image.png](https://upload-images.jianshu.io/upload_images/1817117-48ce1798173e8777.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/1817117-cf2de1113dfe733f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/1817117-719f72f7297b3015.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 残留问题
1、test.html文件打开是成功了，但是font字体文件加载失败

原因： 可能是http头部 MIME类型（如图）
![image.png](https://upload-images.jianshu.io/upload_images/1817117-9afc4648a23d0857.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
