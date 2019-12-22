闭包好处：避免命名冲突（全局变量污染）
```javascript
(function(a, b) {
    console.log(a+b);  //30
})(10, 20);
```


自执行匿名函数
```javascript
(function() { return 1 }) ();
(function(x) { return x * x }) (3);
```

封装私有变量
```javascript
function counter(init) {
    var x = init || 0;
    return {
        inc: function () {
            x += 1;
            return x;
        }
    }
}
var c = counter(10);
c.inc(); // 11
c.inc(); // 12
c.inc(); // 13
```

把多参数的函数变成单参数的函数
```javascript
function make_pow(n) {
    return function (x) {
        return Math.pow(x, n);
    }
}
// 创建两个新函数:
var pow2 = make_pow(2); // 2次方函数
var pow3 = make_pow(3); // 3次方函数

console.log(pow2(5)); // 5的2次方 -- 25
console.log(pow3(7)); // 7的3次方 -- 343
```

> 闭包
```javascript
var fn = (function() {
	var text = 'text';
	return {
		getText: function() {
            return text;
        },
        newText: function(newText) {
            text = newText;
        }
	}
}())

fn.getText();
fn.text;
fn.newText('new text');
fn.getText();
```

> JavaScript利用闭包实现模块化
```javascript
var foo = (function CoolModule() {
    var something = "cool";
    var another = [1, 2, 3];

    function doSomething() {
        alert( something );
    }
    function doAnother() {
        alert( another.join( " ! " ) );
    }
    return {
        doSomething: doSomething,
        doAnother: doAnother
    };
})();
foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```

> 闭包 - 事件委派
```javascript
//利用事件委派可以写出更加优雅的  
    (function(){    
        var resources = document.getElementById('resources');    
        resources.addEventListener('click',handler,false);    
          
        function handler(e){    
            var x = e.target; // get the link tha    
            if(x.nodeName.toLowerCase() === 'a'){    
                alert(x);    
                e.preventDefault();    
            }    
        };    
    })();  
```

```javascript
// 惰性载入单例
var LazySingle = (function(){
    // 单例实例引用
    var _instance = null;
    // 单例
    function Single() {
        /* 这里定义私有属性和方法 */
        return {
            publicMethod: function(){},
            publicProperty: '1.0'
        }
    }
    // 获取单例对象接口
    return function(){
        // 如果未创建单例，将创建单例
        if(!_instance) {
            _instance = Single();
        }
        // 返回单例
        return _instance;
    }
})();
```

```javascript
// 实现1： 最简单的对象字面量
var singleton = {
        attr : 1,
        method : function(){ return this.attr; }
    }
var t1 = singleton ;
var t2 = singleton ;
那么很显然的， t1 === t2 。
十分简单，并且非常使用，不足之处在于没有什么封装性，所有的属性方法都是暴露的。对于一些需要使用私有变量的情况就显得心有余而力不足了。当然在对于 this 的问题上也是有一定弊端的。

//  实现2：构造函数内部判断
function Construct(){
    // 确保只有单例
    if( Construct.unique !== undefined ){
        return Construct.unique; 
    }
    // 其他代码
    this.name = "NYF";
    this.age="24";
    Construct.unique = this;
}
var t1 = new Construct() ;
var t2 = new Construct() ;
其实和最初的JS实现有点类似，不过是将对是否已经存在该类的实例的判断放入构造函数内部。
那么也有的， t1 === t2 。
也是非常简单，无非就是提出一个属性来做判断，但是该方式也没有安全性，一旦我在外部修改了Construct的unique属性，那么单例模式也就被破坏了。

// 实现3 : 闭包方式 
var single = (function(){
    var unique;
    function Construct(){
        // ... 生成单例的构造函数的代码
    }
    unique = new Constuct();
    return unique;
})();
只要 每次讲 var t1 = single; var t2 = single;即可。 与对象字面量方式类似。不过相对而言更安全一点，当然也不是绝对安全。
如果希望会用调用 single() 方式来使用，那么也只需要将内部的 return 改为
    
return function(){
    return unique;
} 
以上方式也可以使用 new 的方式来进行（形式主义的赶脚）。当然这边只是给了闭包的一种例子而已，也可以在 Construct 中判断单例是否存在 等等。 各种方式在各个不同情况做好选着即可。
```