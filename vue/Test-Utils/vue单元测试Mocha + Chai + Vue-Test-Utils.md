
### 一、定义
**Mocha**：定义JavaScript测试模块的测试框架。作用是：配合chai断言库，运行测试脚本进行单元测试。
一个测试脚本包含一个或多个describe(测试模块), 每个describe(测试模块)包括一个或多个it(测试用例)。同时，describe测试模块是一个函数, 具有两个参数，其中第一个参数是测试模块的名称(一般情况下写测试组件的名称), 第二个参数是一个实际执行的函数；it测试用例也是一个函数, 第一个参数是测试用例的名称(通常写你的断言结果,), 第二个参数是一个实际执行的函数。

**Chai**：断言库，所谓断言, 就是判断源码的实际执行结果与预期结果是否一致, 如果不一致, 就会抛出错误。它具有两种风格分别是TDD和BDD。
TDD风格:：先编写单元测试用例代码，在开发功能代码。它的断言方式有：assert。
BDD风格：先编写开发功能代码，在编写单元测试。它的断言方式有：expect和should。
其中，expect使用构造函数来创建断言对象实例，而should是要新增方法来实现断言（所以should不支持IE）；expect直接指向chai.expect，而should则是chai.should()。个人建议使用expect，兼容性高。

**Vue-test-utils**：是Vue.js官方的单元测试实用工具库, 提供很多便捷的接口, 比如挂载组件, 设置Props, 发送emit事件等操作。

### 二、安装

![image.png](https://upload-images.jianshu.io/upload_images/1817117-e5ab9b694590822e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

创建vue项目的时候将Uint Testing选中，生成的项目就是自动集成，也可以自己安装相关依赖然后使用

### 三、API
1、Mocha：
describe块称为"测试套件"(test suite), 表示一组相关的测试. 它是一个函数, 第一个参数是测试套件的名称(通常写测试组件的名称, 这里即为Counter.js), 第二个参数是一个实际执行的函数.
it块称为"测试用例"(test case), 表示一个单独的测试, 是测试的最小单位. 它也是一个函数, 第一个参数是测试用例的名称(通常描述你的断言结果, 这里即为"点击按钮后, count的值应该为1"), 第二个参数是一个实际执行的函数.

Mocha在describe块中提供了四个钩子: before(), after(), beforeEach(), afterEach(). 它们会在以下时间执行

![image.png](https://upload-images.jianshu.io/upload_images/1817117-8642fc71f4b6f983.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

常用的方法：
 - Slow（Number）：设置时间。当运行时间超过Number,会被标红。
 - Timeout（Number）：设置测试用例的最大超时时间。如果执行时间超过了最大超时时间，测试结果将为错误。
- Retries（Number）：设置测试失败后, 测试重试的次数

2、Chai：
常用的属性：
- .not：对之后的断言取反
- .a/an：判断被测试的值的类型
- .Above（value）:判断断言目标的值是否大于（超过）value
- .least（value）:判断断言目标的值不小于（大于或等于）value
- .below（value）:判断断言目标的值是否小于value
- .most（value）：判断断言目标的值不大于（小于或等于）value
- .Within(start, finish)：断言目标的值在某个区间内
- .equal(value)：判断断言目标的值是否完全等于 value

3、Vue-test-utils：
常用的属性：
- vm：访问挂载好的vue组件中的方法和属性。
常用的方法：
- mount()：挂载当前组件实例；
- shallowMount()：挂载当前组件实例和可以挂载子组件；
- find()：返回匹配的选择器，可以使用任何有效的选择器。
- Trigger()：监听触发事件
- setData()：设置data的属性并强制更新它的值
- Text()：返回实例对象中的文本内容
- Html()：返回实例对象中DOM 节点的 HTML 字符串

### 四、实例
chai的一些语法使用

![image.png](https://upload-images.jianshu.io/upload_images/1817117-ad89b2358659c3d8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

mocha Demo

![image.png](https://upload-images.jianshu.io/upload_images/1817117-d3ba73191414e3c3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

第一个用例获取组件里面的文字，判断文字里面是否包含msg
第二个用例使用不同数字计算1000次，输出计算时间，可测试性能

结果：

![image.png](https://upload-images.jianshu.io/upload_images/1817117-16c2791da0b3982c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

加入把第二个测试用例的对比数字改成1000，结果：

![image.png](https://upload-images.jianshu.io/upload_images/1817117-fc8c26229c0e37ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

就会出现断言抛错，可以看到对错结果的对比及所在文件位置
