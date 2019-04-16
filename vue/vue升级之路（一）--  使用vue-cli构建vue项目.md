
最近在公司用vue.js做了个移动端OA项目，有点感觉vue2.0的运用感觉有点不足，所以准备用这次项目的累积及之前的一些理解，写一个vue系列的博客，巩固一下自己对vue的使用，也希望对大家有所帮助。

今天从最根本的使用vue-cli构建vue项目开始写，后面相关的，也会更新到这篇文章。

vue-cli 是一个官方发布 vue.js 项目脚手架，使用 vue-cli 可以快速创建 vue 项目，GitHub地址是：++https://github.com/vuejs/vue-cli++

#### 一、安装node.js

首先需要安装node环境，可以直接到中文官网 ++http://nodejs.cn/++ 下载安装包。

有中文官网还是挺方便的，不用墙什么的

安装完成后，可以命令行工具中输入 node -v 和 npm -v，如果能显示出版本号，就说明安装成功。

![查询node及npm版本号.jpg](https://upload-images.jianshu.io/upload_images/1817117-61c61c138f477588.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 二、安装vue-cli

安装好了 node，我们可以用它来安装一个全局 vue-cli：

```
npm install -g vue-cli
```
这里会有一个老生常谈的问题，<font color="#dd0000">就是有时候这种安装方式比较慢，甚至经常失败，这时候就推荐使用国内镜像来安装</font>

```
// 配置镜像路径
npm config set registry https://registry.npm.taobao.org
// 直接使用镜像路径安装
npm install -g XXX --registry=https://registry.npm.taobao.org
```
如果安装失败，可以使用 **npm cache clean** 清理缓存，然后再重新安装。

#### 三、生成项目

首先需要在命令行中进入到项目目录，然后输入：

```
vue init webpack vue-project
```
如果没有webpack可以顺便装一个 **npm install -g webpack**  安装全局的，用起来也方便

其中 webpack 是模板名称，girhub上可以查得到更多的模板，有兴趣的可以自己自行度娘

vue-project 是自定义的项目名称，命令执行之后，会在当前目录生成一个以该名称命名的项目文件夹

![项目生成.jpg](https://upload-images.jianshu.io/upload_images/1817117-b965986133715847.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后启动项目

```
npm run dev
```

如果浏览器打开之后，没有加载出页面，有可能是本地的 8080 端口被占用，需要修改一下配置文件 config>index.js，
建议将端口号改为不常用的端口。


![config.png](https://upload-images.jianshu.io/upload_images/1817117-43d6cd7187b240ac.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 四、打包上线

打包前，我将 build 的路径前缀修改为 ' ./ '（原本为 ' / '），是因为打包之后，外部引入 js 和 css 文件时，如果路径以 ' / ' 开头，在本地是无法找到对应文件的（服务器上没问题）。所以如果需要在本地打开打包后的文件，就得修改文件路径。如上图

自己的项目文件都需要放到 src 文件夹下

项目开发完成之后，可以输入 npm run build 来进行打包工作



打包完成后，会生成 dist 文件夹，如果已经修改了文件路径，可以直接打开本地文件查看

项目上线时，只需要将 dist 文件夹放到服务器就行了。

#### 五、package.json的配置
![package.json文件.jpg](https://upload-images.jianshu.io/upload_images/1817117-79e55193a6f6cd05.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如上图所示，项目运行及打包的命令就在这里设置的，还有各种依赖模块的列表

如果需要自己再引入一些模块，可以使用命名

```
npm install -S xxx
// -S 是--save 的缩写，它可以让你安装的模块记录到package.json文件当中
npm uninstall xxx
npm uninstall -g xxx
// 这是删除模块，也可以删除全局看模块
```
然后就是一个有时候会被误解的-D，上面把uninstall提出来，其实就是证明这个-D真的不是删除

```
npm install -D xxx
// -D就是--save-dev 这样安装的包的名称及版本号就会存在package.json的devDependencies这个里面，而--save会将包的名称及版本号放在dependencies里面。
```

#### 六、proxyTable的代理设置

在不同域之间访问是比较常见，在本地调试访问远程服务器。。。。这就是有域问题。

而且现在一直强调前后端分离，项目在初期进行的时候也可以使用proxyTable代理到后台服务器，方便调试

![proxyTable代理设置.jpg](https://upload-images.jianshu.io/upload_images/1817117-a11f650a270c60da.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用时直接 **'/api/接口名** 就行了

