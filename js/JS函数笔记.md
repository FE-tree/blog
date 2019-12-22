
### 镇楼函数
``````javascript
window.onload = function() {
	setTimeout(function() {
		console.log(1);
		setTimeout(function() {console.log(2)}, 1000)
	}, 1000)
}

new Promise(resolve => {
    resolve(1);
    Promise.resolve().then(() => console.log(2));
    console.log(4)
}).then(t => console.log(t));
console.log(3);



async function async1 () {
    console.log('async1 start');
    await async2();
    console.log('async1 end');
}

async function async2 () {
    console.log('async2');
}

console.log('script start');

setTimeout(function () {
    console.log('setTimeout');
}, 0);

async1();

new Promise(function (resolve) {
    console.log('promise1');
    resolve();
}).then(function () {
    console.log('promise2');
});

console.log('script end');
```

#### 字符串数组互相转化
```javascript
// 数组转字符串
// 需要将数组元素用某个字符连接成字符串，示例代码如下：
var a, b;
a = newArray(0,1,2,3,4);
b = a.join("-");
// 二、字符串转数组
// 实现方法为将字符串按某个字符切割成若干个字符串，并以数组形式返回，示例代码如下：
var s = "abc,abcd,aaa";
console.log(s.split(','));
```

#### map
map()方法定义在JavaScript的Array中，我们调用Array的map()方法，传入我们自己的函数，就得到了一个新的Array作为结果
```javascript
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var results = arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81]
console.log(results);

// map()函数方法对数据循环处理
var squares = [1,2,3,4].map(function (val) { 
    return val * val; 
});
// squares will be equal to [1, 4, 9, 16]
```

#### filter
Array的filter()也接收一个函数。和map()不同的是，filter()把传入的函数依次作用于每个元素，然后根据返回值是true还是false决定保留还是丢弃该元素
```javascript
var arr = [1, 2, 4, 5, 6, 9, 10, 15];
var r = arr.filter(function (x) {
    return x % 2 !== 0;
});
r; // [1, 5, 9, 15]
```

#### sort
根据ASCII码进行排序,并且会把元素都转换为String再进行排序
```javascript
// 正序
var arr = [10, 20, 1, 2];
arr.sort(function (x, y) {
    if (x < y) {
        return -1;
    }
    if (x > y) {
        return 1;
    }
    return 0;
});
console.log(arr); // [1, 2, 10, 20]

// 倒序
var arr = [10, 20, 1, 2];
arr.sort(function (x, y) {
    if (x < y) {
        return 1;
    }
    if (x > y) {
        return -1;
    }
    return 0;
}); // [20, 10, 2, 1]

// sort()方法会直接对Array进行修改，它返回的结果仍是当前Array：
var a1 = ['B', 'A', 'C'];
var a2 = a1.sort();
a1; // ['A', 'B', 'C']
a2; // ['A', 'B', 'C']
a1 === a2; // true, a1和a2是同一对象
```

#### 差集
```javascript
const minst = function minst(a, b) {
    // 把a数组转化成object
    const hash = {};
    const data = {
        diff: [],
        same: [],
    };
    for (let i = 0, max = a.length; i < max; i++) {
        hash[a[i]] = true;
    }
    // console.log(hash);
    // 通过hash检测b数组中的元素
    for (let i = 0, max = b.length; i < max; i++) {
        if (typeof hash[b[i]] !== 'undefined') {
            // console.log(`相同元素${b[i]}`);
            data.same.push(b[i]);
        } else {
            // console.log(`不同元素${b[i]}`);
            data.diff.push(b[i]);
        }
    }
    return data;
};
```


#### localStorage的封装
```javascript
//在get时，如果是JSON格式，那么将其转换为JSON，而不是字符串。以下是基础代码：
var Store = {
    get: function(key) {
        var value = localStorage.getItem(key);
        if (value) {
            try {
                var value_json = JSON.parse(value);
                if (typeof value_json === 'object') {
                    return value_json;
                } else if (typeof value_json === 'number') {
                    return value_json;
                }
            } catch(e) {
                return value;
            }
        } else {
            return false;
        }
    },
    set: function(key, value) {
        localStorage.setItem(key, value);
    },
    remove: function(key) {
        localStorage.removeItem(key);
    },
    clear: function() {
        localStorage.clear();
    }
}
```
```javascript
在此基础之上，可以扩展很多方法，比如批量保存或删除一些数据：
// 批量保存，data是一个字典
Store.setList = function(data) {
    for (var i in data) {
        localStorage.setItem(i, data[i]);
    }
}

// 批量删除，list是一个数组
Store.removeList = function(list) {
    for (var i = 0, len = list.length; i < len; i++) {
        localStorage.removeItem(list[i]);
    }
}
```

#### 深度拷贝对象
```javascript
function cloneObj(obj) {
    var o = obj.constructor == Object ? new obj.constructor() : new obj.constructor(obj.valueOf());
    for(var key in obj){
        if(o[key] != obj[key] ){
            if(typeof(obj[key]) == 'object' ){
                o[key] = mods.cloneObj(obj[key]);
            }else{
                o[key] = obj[key];
            }
        }
    }
    return o;
}
```

#### 合并对象
```javascript
// 通过...延展操作符可以很容易的合并对象

const person = { a:1 };
const tools = { b:2 };
const attributes = { c:3 };

const summary = {...person, ...tools, ...attributes};

>> {a:1,b:2,c:3}

```

##### 过滤空值
```javascript
let res = [1,2,0,undefined,null,false,''].filter(Boolean);
>> 1,2

```

##### Array.fill
```javascript
Array(10).fill('naive').map((v, i) =>{ return {number: i}});
```

##### Array.from
```javascript
Array.from(new Array(10), (v,i) => {
    return {number: i}
});
```

##### 递归
```javascript
(function wallace (i) { 
    return (i < 0) ? [] : wallace(i - 1).concat({number: i}); 
})(10);
```

##### 尾递归
```javascript
(function mistake (i, acc) {
    return (i < 10) ? mistake(i + 1, acc.concat({number: i})) : acc; 
})(0, []);
```

##### Generator
```javascript
function* angry(i) {
  yield {number: i};
  if (i < 10) { yield* angry(i + 1); }
};
Array.from(angry(0));
```

##### apply方式
```javascript
Array.apply(null, { length: 10 }).map((v, i) => i);
```

##### JS生成随机字符串
```javascript
var random_str = function() {
    var len = 32;
    var chars = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890';
    var max = chars.length;
    var str = '';
    for (var i = 0; i < len; i++) {
　　　　str += chars.charAt(Math.floor(Math.random() * max));
    }

    return str;
};
//这样生成一个32位的随机字符串，相同的概率低到不可能。
```

##### 生成随机数
```javascript
function randombetween(min, max){
    return min + (Math.random() * (max-min +1));
}
```










#### 封装
```javascript
function Person (name,age,sex){
    this.name = name;
    this.age = age;
    this.sex = sex;
}
Pserson.prototype = {
    constructor: Person,
    sayHello: function() {
        console.log('hello');
    }
}
```