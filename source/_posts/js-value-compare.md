---
title: js-value-compare
date: 2017-09-21 09:00:09
tags: [js,value,compare]
---

### 从 null >= 0 说起

我们先从一组 用例说起. 看代码:

```javascript
null > 0 // false
null == 0 // false
null >= 0 //true
```

解释上面问题，需要拆分下，关系运算符（ > < >= <=） 和 相等运算符 (==)

### 关系运算符（ > < >= <=）

1. 类型一致，直接比较
2. 把值转向getValue再比较

```javascript
// null
null > 0
  => Number(null) > Number(0)
  => 0 > 0 // false

null >= 0
  => Number(null) >= Number(0)
  => 0 >= 0 // true

// undefined
undefined > 0
  => Number(undefined) > Number(0)
  => NaN > 0 // false

undefined <= 0
  => Number(undefined) <= Number(0)
  => NaN <= 0 // false

// string
"abc" > 0
  => Number("abc") > Number(0)
  => NaN > 0 // false

"123" > 0
  => Number("123") > Number(0)
  => 123 > 0 // true

"-123" > 0
  => Number("-123") > Number(0)
  => -123 > 0 // false

// Number
123 > 0
  => true

-123 > 0
  => false

// object
let obj = {valueOf: function() {return 2}, toString: function() { return -2 } };

obj > 0
  => Number(obj) > Number(0)
  => obj.valueOf() > 0
  => 2 > 0 // true

let obj2 = {};

obj2 > 0
  => Number(obj2) > Number(0)
  => obj.valueOf() > 0
  => NaN > 0 // false

// array
let arr = [1, 2];

arr > 0
  => Number(arr) > Number(0)
  => NaN > 0 // false

let arr2 = [];

arr2 > 0
  => Number(arr2) > Number(0)
  => 0 > 0 // false

// bool
true > 0
  => Number(true) > Number(0)
  => 1 > 0 // true

false > 0
  => Number(false) > Number(0)
  => 0 > 0 // false
```

### 相等运算符 (==)

> The Abstract Equality Comparison Algorithm
>
> The comparison x == y, where x and y are values, produces true or false. Such a comparison is
> performed as follows:
> 1. If Type(x) is different from Type(y), go to step 14.
> 2. If Type(x) is Undefined, return true.
> 3. If Type(x) is Null, return true.
> 4. If Type(x) is not Number, go to step 11.
> 5. If x is NaN, return false.
> 6. If y is NaN, return false.
> 7. If x is the same number value as y, return true.
> 8. If x is +0 and y is −0, return true.
> 9. If x is −0 and y is +0, return true.
> 10. Return false.
> 11.If Type(x) is String, then return true if x and y are exactly the same sequence of > characters (same length and same characters in corresponding positions). Otherwise, return false.
> 12. If Type(x) is Boolean, return true if x and y are both true or both false. Otherwise, > return false.
> 13.Return true if x and y refer to the same object or if they refer to objects joined to each > other (see
> 13.1.2). Otherwise, return false.
> 14. If x is null and y is undefined, return true.
> 15. If x is undefined and y is null, return true.
> 16.If Type(x) is Number and Type(y) is String,return the result of the comparison x == ToNumber> (y).
> 17.If Type(x) is String and Type(y) is Number,return the result of the comparison ToNumber(x) > == y.
> 18. If Type(x) is Boolean, return the result of the comparison ToNumber(x) == y.
> 19. If Type(y) is Boolean, return the result of the comparison x == ToNumber(y).
> 20.If Type(x) is either String or Number and Type(y) is Object,return the result of the > comparison x == ToPrimitive(y).
> 21.If Type(x) is Object and Type(y) is either String or Number,return the result of the > comparison ToPrimitive(x) == y.
> 22. Return false.

理解下对应的
1. 有NaN 返回 false
2. 类型一致，直接比较
2. 如果是 null == undefined 返回true，否则里面包含 null或undefined 都返回 false
3. 其他 比较都转换成 Number(x) == Number(y) 比较

```javascript
0 == undefined
  => // false

1 == true
  => Number(0) == Number(true) // true

let obj = {valueOf: function(){return 2}};
2 == obj
  => Number(2) == Number(obj)
  => 2 == obj.valueOf()
  => 2 == 2 // true

NaN == NaN
  => false

1 == undefined
  => false

null == {toString: function(){return 2}}
  => false

0 == null
  => false

null == 1
  => false

[] == 0
  => Number([]) == 0
  => Number([].valueOf()) == 0
  => Number([].toString()) == 0
  => Number('') == 0
  => 0 == 0 // true

let obj2 = { toString:function(){ return 1 } , valueOf:function(){ return [] }}
obj2 == 1
  => Number(obj2) == 1
  => Number(obj.valueOf()) == 1 // 不是原始类型（Number、String、Boolean）
  => Number(obj.toString()) == 1
  => 1 == 1 // true

[123,456] == [123,456]
  => Number([123,456]) == Number([123,456])
  => Number([123,456].valueOf()) == Number([123,456].valueOf())
  => Number([123,456].toString()) == Number([123,456].toString())
  => Number('123,456') == Number('123,456')
  => NaN == NaN // false

```

1. 关系运算符 和 相等运算符 并不是一个类别的.
2. 关系运算符,在设计上,总是需要运算元尝试转为一个number . 而相等运算符在设计上,则没有这方面的考虑.
3. 最重要的一点, 不要把 拿 a > b ,  a == b 的结果 想当然的去和 a >= b 建立联系. 正确的符合最初设计思想的关系是  a > b 与 a >= b是一组 . a == b 和其他相等运算符才是一组. 比如  a === b , a != b, a !== b .


### 还是用三个等号来判断，少了隐式转换

### 参考文献

* [JavaScript 对象转换之 toString 和 valueOf](!http://frontenddev.org/link/conversion-of-tostring-and-the-valueof-javascript-object.html)
