# 一、定义
**GIS** - 地理信息系统，是一种特定的十分重要的空间信息系统。它是在计算机硬、软件系统支持下，对整个或部分地球表层（包括大气层）空间中的有关地理分布数据进行采集、储存、管理、运算、分析、显示和描述的技术系统。

**webGIS** - 网络地理信息系统，是指工作在Web网上的GIS,是传统的GIS在网络上的延伸和发展，具有传统GIS的特点，可以实现空间数据的检索、查询、制图输出、编辑等GIS基本功能，同时也是Internet 上地理信息发布、共享和交流协作的基础。

**知识结构** - 进行WebGIS开发之前，你一定要有这样的清晰的模型，WebGIS是如何工作的，地图是如何展示出来的。你需要了解以下名词和它们之间的关系：

地图服务器

地图服务（OGC、WMS、WFS、TMS、WMTS）

GeoJson

地图切片

后台

# 二、开发基础
Html5、CSS、JavaScript——Web开发共同基础
ES6——JavaScript的新一代标准，新版本的WebGIS平台多依赖于ES6
基于一定的后台开发的经验或知识，毕竟要从html页面去后台查询空间数据信息

# 三、WebGIS开发平台
- 商业平台

ArcGIS、超图、MapGIS等商业平台，其中以ArcGIS JS开发应用最广，性能也稳定，学习资料和代码也比较多。

1) ArcGIS

目前商业解决方案中认为是最好的一个，是国外的解决方案，技术领先；非常专业，一般的应用可能只会使用其中一小部分功能。

ArcGIS Desktop基础操作——会数据简单处理，坐标系转换
ArcGIS Server——可以发布各类地图服务
ArcGIS API for JavaScript可以调用各类地图服务
可以使用ArcGIS Server发布空间处理服务（更高级要求）

2) 超图GIS

国内的解决方案，整合了一些国内流行的框架，如ECharts等，借助了云、大数据等概念，有较完善的中文文档。


- 开源平台

开源WebGIS平台很多，如OpenLayers、Leaflet等，其中OpenLayers的应用最为广泛，功能也最强大，而且一直在更新，使用者众多。比较有意思的是，在之前打开MapGIS的Web开发代码，居然发现了OpenLayers的JS文件。

应用服务：GeoServer——类似于ArcGIS Server，开源地图服务器，一般与开源地图平台搭配

数据库：PostgreSQL与PostGIS——开源空间数据库，存储数据，简单分析

客户端：OpenLayers

开源桌面GIS： uDig、QGIS、OpenJUMP——用来配置GeoServer中地图样式

- 百度、高德等地图平台

这些平台虽然不是为GIS而生，但是其定期更新的地图，省去了开发时搭建地图服务器的麻烦。很多非GIS开发人员也能轻易开发。对于个人或者非商业的应用是免费的，实际开发过程中，也有很多不侧重与GIS开发的公司在进行百度等地图平台的开发。

- 基于WebGL或其他图形语言的底层开发

前面三个说的都是二次开发，有一次强调自己品牌和地位的公司会基于WebGL或其他技术进行更底层的图形发开，对开发人员的GIS知识和算法准备有很高要求。开发周期长，前期投入较多，进行此类开发的人员较少。

# 三、WebGIS框架
![clipboard.png](https://upload-images.jianshu.io/upload_images/1817117-f744c375ff5b7b2d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在初步学习和分析了上述开源方案、ArcGIS和超图商业方案，并学习了解一些GIS基本知识之后，对于WebGIS开发方法，有了以下一些认识：

1）这些解决方案的架构基本上有3个层面的核心：数据存储、地图服务和操作API；

2）数据存储：地图有2种存储方式，一是文件存储，二是空间数据库；不同的解决方案支持的文件存储类型有所不同；空间数据库常用的有Oracle、MySql；一般各种解决方案均会提供客户端应用程序来管理基本的地图数据，对数据进行编辑、创建、删除等；

3）地图服务：地图服务提供对地图数据的访问、操作接口，通常搭建在中间件上即可，支持Tomcat、IIS等常用中间件；无论是客户端应用程序还是要开发的Web应用，均必须通过该服务提供的接口来访问和操作地图数据；

4）操作API：针对不同的开发方案，各种解决方案均提供不同种类的API，包括JavaScript、Flash、Java、.NET等，在开发中可选择使用；开源的JavaScript库OpenLayers是非常流行好用的脚本库，超图GIS也选择集成该库作为浏览器端开发的技术方案。

5）通过这样的三层架构，应用建立在这个架构之上，实现应用->接口->服务->存储的GIS开发路线。


