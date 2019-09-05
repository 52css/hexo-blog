---
title: Javascript 结合ES6 技巧
date: 2019-09-05 09:53:26
tags: [js, lib, es6]
---

# JS 技巧（es6）

## 1. String

### 1.1 格式化千分位金钱

```js
const toThousand = num => num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
const money = toThousand(20190214);
// money => "20,190,214"
```

### 1.2 生成随机ID

```js
const randomStr = len => Math.random().toString(36).substr(3, len);
const id = randomStr(10);
// id => "jg7zpgiqva"
```

### 1.3 生成随机HEX色值

```js
const randomColor = () => "#" + Math.floor(Math.random() * 0xffffff).toString(16).padEnd(6, "0");
const color = randomColor();
// color => "#f03665"
```

### 1.4 生成星级评分

```js
const startScore = rate => "★★★★★☆☆☆☆☆".slice(5 - rate, 10 - rate);
const start = startScore(3);
// start => "★★★☆☆"
```

### 1.5 字符串模板替换

```js
const user = {
  name: 'John',
  surname: 'Doe',
  details: {
    email: 'john@doe.com',
    displayName: 'SuperCoolJohn',
    joined: '2016-05-05',
    image: 'path-to-the-image',
    followers: 45
  }
}

const printUserInfo = (user) => { 
  const text = `The user is ${user.name} ${user.surname}. Email: ${user.details.email}. Display Name: ${user.details.displayName}. ${user.name} has ${user.details.followers} followers.`
  console.log(text);
}

printUserInfo(user);
// 输出 'The user is John Doe. Email: john@doe.com. Display Name: SuperCoolJohn. John has 45 followers.'

```

### 1.6 补零

```js
// es5  
// const addZero1 = (num, len = 2) => (`0${num}`).slice(-len)
const fillZero = (num, len = 2) => num.toString().padStart(len, "0");
const num = fillZero(169, 5);
// num => "00169"
const num2 = fillZero(9);
// num2 = "09"
```

### 1.7 toNumber

```js
// 只对null、""、false、数值字符串有效
const num1 = +null;
const num2 = +"";
const num3 = +false;
const num4 = +"169";
// num1 num2 num3 num4 => 0 0 0 169
```

### 1.8 格式化JSON代码

```js
console.log(JSON.stringify({ alpha: 'A', beta: 'B' }, null, '4'));
// Result:
// '{
//     "alpha": A,
//     "beta": B
// }'

```


## 2. Number

### 2.1 取整

```js
// 代替正数的Math.floor()，代替负数的Math.ceil()
const num1 = ~~ 1.69;
const num2 = 1.69 | 0;
const num3 = 1.69 >> 0;
// num1 num2 num3 => 1 1 1
```

### 2.2 四舍五入

```js
const roundNum = (num, decimal = 2) => Math.round(num * 10 ** decimal) / 10 ** decimal;
const num = roundNum(1.69, 1);
// num => 1.7
```

### 2.3 计算奇数偶数

```js
const num = 100 % 2
const num2 = 100 & 1
```

### 2.4 随机数

```js
const randomNum = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;
const num = randomNum(1, 10);
```

### 2.5 转字符串

```js
const val = 1 + "";
console.log(val); // Result: "1"
console.log(typeof val); // Result: "string"
```

### 2.6 幂次方

```js
console.log(2 ** 3); // Result: 8

Math.pow(2, n);
2 << (n - 1);
2**n;
```

### 2.7 numberEq 相等

```js
function numberEq(a,b){
    return Math.abs(a-b) < Number.EPSILON;
}
var a=0.1+0.2,b=0.3;
console.log("Number.EPSILON:",numEqual(a,b));//true
```

### 2.8 **isNumber**

```js
function isNumber(n){
    return !isNaN(parseFloat(n)) && isFinite(n);
}
```

### 2.9 保留2位小数

```js
const roundNum = (num, decimal = 2) => Math.round(num * 10 ** decimal) / 10 ** decimal;
const toFixed = (num, decimal = 2) => (+roundNum(num, decimal)).toFixed(decimal)
```



## 3. Boolean

### 3.1 转换boolean

```js
var a = '123'
var bA = !!a;
```



## 4. Date

### 4.1 时间戳

```js
const timestamp = +new Date("2019-02-14");
// timestamp => 1550102400000
```



## 5. Array

### 5.1 获取数据最大，最小值

```js
const arrMin = arr => Math.min(...arr)
const arrMax = arr => Math.max(...arr)

const arr = [0, 1, 2]
const min = arrMin(arr) // 0
const max = arrMax(arr) // 2
```

### 5.2 克隆数组

```js
const _arr = [0, 1, 2];
const arr = [..._arr];
// arr => [0, 1, 2]
```

### 5.3 合并数组

```js
const arr1 = [0, 1, 2];
const arr2 = [3, 4, 5];
const arr = [...arr1, ...arr2];
// arr => [0, 1, 2, 3, 4, 5];
```

### 5.4 去重数组

```js
const arr = [...new Set([0, 1, 1, null, null])];
// arr => [0, 1, null]
```

### 5.5 数组随机

```js
const randomArr = arr => arr.slice().sort(() => Math.random() - .5);

const arr = [0, 1, 2, 3, 4, 5]
const rArr = randomArr(arr)
// arr => [3, 4, 0, 5, 1, 2]

```

### 5.6 清空数组

```js
const arr = [0, 1, 2];
arr.length = 0;
// arr => []

```

### 5.7 截断数组

```js
const arr = [0, 1, 2];
arr.length = 2;
// arr => [0, 1]
```

### 5.8 截取数组

```js
const arr = [0, 1, 2, 3, 4];
arr.slice(1, -1) // 正常正数和负数

// 获取数组中的最后一项
arr.slice(-1)
```

### 5.9 交换赋值

```js
let a = 0;
let b = 1;
[a, b] = [b, a];
// a b => 1 0

// 数组排序(上移，下移，拖拽排序)
var arr = [1, 2, 3, 4, 5];
[arr[0], arr[4]] = [arr[4], arr[0]]
// arr => [5, 2, 3, 4, 1]
```

### 5.10 过滤空值

```js
const arr = [undefined, null, "", 0, false, NaN, 1, 2].filter(Boolean);
// arr => [1, 2]
```

### 5.11 数据Prmose按顺序执行

### 5.12 数组去重

```js
const uniqueArr = arr => [...new Set(arr)];
const cars = [
    'Mazda', 
    'Ford', 
    'Renault', 
    'Opel', 
    'Mazda'
]
const uniqueWithSpreadOperator = uniqueArr(cars);
// uniqueWithSpreadOperator => ["Mazda", "Ford", "Renault", "Opel"]
```



### 5.13 数组首部插入成员

```js
let arr = [1, 2]; // 以下方法任选一种
arr.unshift(0);
arr = [0].concat(arr);
arr = [0, ...arr];
// arr => [0, 1, 2]
```

### 5.14 数组尾部插入成员

```js
let arr = [0, 1]; // 以下方法任选一种
arr.push(2);
arr.concat(2);
arr[arr.length] = 2;
arr = [...arr, 2];
// arr => [0, 1, 2]
```

### 5.15 数组转map

```js
let arr = [{id: 1}, {id: 2}]
const arrObjectMap = (arr, key = 'id') => arr.reduct((rtv, item) => {
    rtv[item[key]] = true;
    return rtv;
}, {})
const map = arrObjectMap(arr, 'id')
// map => {1: true, 2: true}
```

### 5.16 数组结构(通过object来，可以不用定义无用的变量)

```js
const arr = [0, 1, 2];
const { 2: c } = arr;
// c => 2
const [e, f, g] = arr
// g => 2 // 这样定义了无用的变量
```

### 5.17 解构数组成员默认值

```js
const arr = [0, 1, 2];
const [a, b, c = 3, d = 4] = arr;
// a b c d => 0 1 2 4

```

### 5.18 获取随机数组成员

```js
const arr = [0, 1, 2, 3, 4, 5];
const randomOne = arr => arr[Math.floor(Math.random() * arr.length)];

const randomItem = randomOne(arr);
// randomItem => 1
```

### 5.19 创建指定长度数组

```js
const list = len => [...new Array(len).keys()];
const arr = list(3);
// arr => [0, 1, 2]
```

### 5.20 创建指定长度且值相等的数组

```js
const arr = new Array(3).fill(0)
// arr => [0, 0, 0]
```

### 5.21 数组排序

```js
[14.3, 8, 1, 64].sort(function(a, b) {
  return a - b;   //从小到大排
  // return b - a;   //从大到小排
});
//[1, 8, 14.3, 64]

// 对象数组
var arr = [{   //对象数组
    num: 1,
    text: 'num1'
}, {
    num: 5,
    text: 'num2'
}, {
    num: 6,
    text: 'num3'
}, {
    num: 3,
    text: 'num4'
}];   
arr.sort(function(a, b) {
    return a.num - b.num;   //从小到大排
    return b.num - a.num;   //从大到小排
});
```

### 5.22 扁平化多维数组

```js
function flattenArray(arr) {
  const flattened = [].concat(...arr);
  return flattened.some(item => Array.isArray(item)) ? 
    flattenArray(flattened) : flattened;
}

const arr = [11, [22, 33], [44, [55, 66, [77, [88]], 99]]];
const flatArr = flattenArray(arr); 
//=> [11, 22, 33, 44, 55, 66, 77, 88, 99]

```

### 5.23 使用 Array.includes 来处理多重条件

以前的判断方式：

```js
// 条件语句
function test(fruit) {
  if (fruit == 'apple' || fruit == 'strawberry') {
    console.log('red');
  }
}
```

现在的方式：

```js
function test(fruit) {
  // 把条件提取到数组中
  const redFruits = ['apple', 'strawberry', 'cherry', 'cranberries'];

  if (redFruits.includes(fruit)) {
    console.log('red');
  }
}
```


## 6. Object

### 6.1 判断是否是Object

```js
const isObject = obj => Object.prototype.toString.call(obj).slice(-7, -1) === 'Object'

const isObj = isObject({}) // true
```

### 6.2 判断Object是否是空

```js
const isEmptyObject = obj => isObject(obj) && Object.keys(obj).length === 0;

const isEmptyObj = isEmptyObject({}) // true
```

### 6.3 克隆对象(不支持自己指向自己)

```js
const _obj = { a: 0, b: 1, c: 2 }; // 以下方法任选一种
const obj = { ..._obj };
const obj = JSON.parse(JSON.stringify(_obj));
// obj => { a: 0, b: 1, c: 2 }
```

### 6.4 合并对象

```js
const obj1 = { a: 0, b: 1, c: 2 };
const obj2 = { c: 3, d: 4, e: 5 };
const obj = { ...obj1, ...obj2 };
// obj => { a: 0, b: 1, c: 3, d: 4, e: 5 }
```

### 6.5 对象字面量

```js
const env = "prod";
const link = {
    dev: "Development Address",
    test: "Testing Address",
    prod: "Production Address"
}[env];
// link => "Production Address"
```

### 6.6 对象变量属性

```js
const flag = false;
const obj = {
    a: 0,
    b: 1,
    [flag ? "c" : "d"]: 2
};
// obj => { a: 0, b: 1, d: 2 }
```

### 6.7 创建纯空对象

```js
const obj = Object.create(null);
// obj => {}
```

### 6.8 删除对象无用属性

```js
const obj = { a: 0, b: 1, c: 2 }; // 只想拿b和c
const { a, ...rest } = obj;
// rest => { b: 1, c: 2 }
```

### 6.9 解构对象属性嵌套

```js
const obj = { a: 0, b: 1, c: { d: 2, e: 3 } };
const { c: { d, e } } = obj;
// d e => 2 3
```

### 6.10 解构对象属性别名

```js
const obj = { a: 0, b: 1, c: 2 };
const { a, b: d, c: e } = obj;
// a d e => 0 1 2
```

### 6.11 解构对象属性默认值

```js
const obj = { a: 0, b: 1, c: 2 };
const { a, b = 2, d = 3 } = obj;
// a b d => 0 1 3
```

我们常用的写res.data 结构使用

```js
const { data: { listObj: data = [] } } = await promise()
```



## 7. Function

### 7.1 隐式返回值

```js
// 只能用于单语句返回值箭头函数，如果返回值是对象必须使用()包住
const func = function(name) {
    return "I Love " + name;
};
// 换成
const func = name => "I Love " + name;

```

### 7.2 检测非空参数

```js
function required() {
    throw new Error("param is required");
}
function func(name = required()) {
    console.log("I Love " + name);
}
func(); // "param is required"
func("You"); // "I Love You"

```

### 7.3 优雅处理Async/Await参数

```js
async function errorCaptured(asyncFunc) {
    try {
        const res = await asyncFunc()
        return [null, res]
    } catch (ex) {
        return [ex, null]
    }
}

function asyncFunc() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            Math.random() > 0.5 ? resolve('success') : reject('error')
        }, 1000)
    })
}


async function func() {
    let [err, res] = await errorCaptured(asyncFunc)
    console.log('res', res)
    console.log('err', err)
}

func()

```

### 7.4 优雅处理多个函数返回值

```js
function func() {
    return Promise.all([
        fetch("/user"),
        fetch("/comment")
    ]);
}
const [user, comment] = await func(); // 需在async包围下使用
```

### 7.5 获取函数参数

```js
function useCall(...args) {
    console.log(args)
}
useCall('Bob Dylan', 'Bob Marley', 'Steve Vai')
```

### 7.6 利用chunk函数，执行只执行一次函数

```js
function once(fn, context) {
  var result;
  return function() {
    if(fn) {
      result = fn.apply(context || this, arguments);
      fn = null;
    }
    return result;
  }
}
//使用
var canOnlyFireOnce = once(function() {
  console.log('只触发一次！');
})
canOnlyFireOnce(); // "只触发一次"
canOnlyFireOnce(); // undefined
```

### 7.7 防抖

### 7.8 节流

## 8. DOM

### 8.1 过滤XSS

```js
function filterXss(content) {
    let elem = document.createElement("div");
    elem.innerText = content;
    const result = elem.innerHTML;
    elem = null;
    return result;
}
```



## 9. BOM

### 9.1 操作URL查询参数

```js
const searchParams = (url = location.search) => new URLSearchParams(url.replace(/\?/ig, "")); 
// location.search = "?name=young&sex=male"
const params = searchParams('?name=young&sex=male');
params.has("name"); // true
params.get("sex"); // "male"

```

### 9.2 存取LocalStorage

```js
const love = JSON.parse(localStorage.getItem("love"));
localStorage.setItem("love", JSON.stringify("I Love You"));
```

### 9.3 设置微信title

```js
function setTitle(title) {
  document.title = title;
  if (/ip(hone|od|ad)/i.test(navigator.userAgent)) {
    var i = document.createElement('iframe');
    i.src = '/favicon.ico';
    i.style.display = 'none';
    i.onload = function() {
      setTimeout(function(){
        i.remove();
      }, 9)
    }
    document.body.appendChild(i);
  }
}
setTitle("要修改的标题");
```

### 9.4 获取一个链接的绝对地址

```js
var getAbsoluteUrl = (function() {
  var a;
  return function(url) {
    if(!a) a = document.createElement('a');
    a.href = url;
    return a.href;
  };
})();
//使用
getAbsoluteUrl("/something"); //https://rockjins.github.io/something
```



## 10. 其他

### 10.1 颜色混合

```js
const colourBlend = (c1, c2, ratio) => {
    ratio = Math.max(Math.min(Number(ratio), 1), 0)
    let r1 = parseInt(c1.substring(1, 3), 16)
    let g1 = parseInt(c1.substring(3, 5), 16)
    let b1 = parseInt(c1.substring(5, 7), 16)
    let r2 = parseInt(c2.substring(1, 3), 16)
    let g2 = parseInt(c2.substring(3, 5), 16)
    let b2 = parseInt(c2.substring(5, 7), 16)
    let r = Math.round(r1 * (1 - ratio) + r2 * ratio)
    let g = Math.round(g1 * (1 - ratio) + g2 * ratio)
    let b = Math.round(b1 * (1 - ratio) + b2 * ratio)
    r = ('0' + (r || 0).toString(16)).slice(-2)
    g = ('0' + (g || 0).toString(16)).slice(-2)
    b = ('0' + (b || 0).toString(16)).slice(-2)
    return '#' + r + g + b
}
colourBlend('#ff0000', '#3333ff', 0.5) // "#991a80"

```

### 10.2 判断是否为质数

```js
const mathIsPrime = n => {
  if (n === 2 || n === 3) {
    return true
  }
  if (isNaN(n) || n <= 1 || n % 1 != 0 || n % 2 == 0 || n % 3 == 0) {
    return false;
  }
  for (let x = 6; x <= Math.sqrt(n) + 1; x += 6) {
    if (n % (x - 1) == 0 || n % (x + 1) == 0) {
      return false
    }
  }
  return true
}
mathIsPrime(0) // true

```

### 10.3 时间格式化

```js
const dateFormatter = (formatter, date) => {
	date = (date ? new Date(date) : new Date)
	const Y = date.getFullYear() + '',
          M = date.getMonth() + 1,
          D = date.getDate(),
          H = date.getHours(),
          m = date.getMinutes(),
          s = date.getSeconds()
    return formatter.replace(/YYYY|yyyy/g, Y)
        			.replace(/YY|yy/g, Y.substr(2, 2))
        			.replace(/MM/g, (M < 10 ? '0' : '') + M)
        			.replace(/DD/g, (D < 10 ? '0' : '') + D)
        			.replace(/HH|hh/g, (H < 10 ? '0' : '') + H)
        			.replace(/mm/g, (m < 10 ? '0' : '') + m)
        			.replace(/ss/g, (s < 10 ? '0' : '') + s)
}

dateFormatter('YYYY-MM-DD HH:mm', '1995/02/15 13:55') // 1995-02-15 13:55
```

### 10.4 获取当月多少天

```js
    function getCountDays() {
            var curDate = new Date();
            /* 获取当前月份 */
            var curMonth = curDate.getMonth();
        /*  生成实际的月份: 由于curMonth会比实际月份小1, 故需加1 */
        curDate.setMonth(curMonth + 1);
        /* 将日期设置为0, 这里为什么要这样设置, 我不知道原因, 这是从网上学来的 */
        curDate.setDate(0);
        /* 返回当月的天数 */
        console.log( curDate.getDate());
        return curDate.getDate();
        
    }
    //例如,  获取当前月份(现在是6月)的总天数: 
    getCountDays()       // 返回30

```

### 10.5 代码复用

虽然将 `foo.bar` 写成 `foo ['bar']` 是一种常见的做法，但是这种做法构成了编写可重用代码的基础。许多框架使用了这种方法，比如element的[表单验证](https://link.juejin.im/?target=http%3A%2F%2Felement-cn.eleme.io%2F%23%2Fzh-CN%2Fcomponent%2Fform%23biao-dan-yan-zheng)。

请考虑下面这个验证函数的简化示例：

```js
function validate(values) {
  if(!values.first)
    return false;
  if(!values.last)
    return false;
  return true;
}
console.log(validate({first:'Bruce',last:'Wayne'})); // true

```

上面的函数完美的完成验证工作。但是当有很多表单，则需要应用验证，此时会有不同的字段和规则。如果可以构建一个在运行时配置的通用验证函数，会是一个好选择。

```js
// object validation rules
const schema = {
  first: {
    required:true
  },
  last: {
    required:true
  }
}
 
// universal validation function
const validate = (schema, values) => {
  for(field in schema) {
    if(schema[field].required) {
      if(!values[field]) {
        return false;
      }
    }
  }
  return true;
}
console.log(validate(schema, {first:'Bruce'})); // false
console.log(validate(schema, {first:'Bruce',last:'Wayne'})); // true

```

### 10.6 使用 reduce 检测括号是否对齐封闭

```js
//Returns 0 if balanced.
const isParensBalanced = (str) => {
  return str.split('').reduce((counter, char) => {
    if(counter < 0) { //matched ")" before "("
      return counter;
    } else if(char === '(') {
      return ++counter;
    } else if(char === ')') {
      return --counter;
    }  else { //matched some other char
      return counter;
    }
    
  }, 0); //<-- starting value of the counter
}

isParensBalanced('(())') // 0 <-- balanced
isParensBalanced('(asdfds)') //0 <-- balanced
isParensBalanced('(()') // 1 <-- not balanced
isParensBalanced(')(') // -1 <-- not balanced

```

### 10.7  使用 reduce 计算数组中的重复项

```js
var cars = ['BMW','Benz', 'Benz', 'Tesla', 'BMW', 'Toyota'];
var carsObj = cars.reduce(function (obj, name) { 
   obj[name] = obj[name] ? ++obj[name] : 1;
  return obj;
}, {});

carsObj; // => { BMW: 2, Benz: 2, Tesla: 1, Toyota: 1 }

```

### 10.8 使用IIFE解决循环问题

**unexpected**

```js
var funcs = [];
for (var i = 0; i < 10; i++) {
    funcs.push(function() { console.log(i); });
}
funcs.forEach(function(func) {
    func();     // 输出数值 "10" 十次
});
```

**用let解决这个问题**

```js
var funcs = [];
for (let i = 0; i < 10; i++) {
    funcs.push(function() { console.log(i); });
}
funcs.forEach(function(func) {
    func();     // 从 0 到 9 依次输出
});
```

### 10.9 RGB色值生成16进制色值

```js
const rgb2Hex = rgb => {
    let rgbList = rgb.toString().match(/\d+/g)
    let hex = '#'
    for (let i = 0, len = rgbList.length; i < len; ++i) {
      hex += ('0' + Number(rgbList[i]).toString(16)).slice(-2)
    }
    return hex
};
rgb2Hex('100, 50, 0') // '#643200'

```

### 10.10 利用Map，或object快速查找, 查表法

