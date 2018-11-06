---
title: JS如何优雅取值
date: 2018-11-06 10:04:00
tags: [js,proxy,chain]
---

## js取值一直是一个坑

例如
```js
var res = {
  result: [
    {
      id: ''
    }
  ]
}
// 取第一项id需要

if (res && res.result && res.result[0] && res.result[0].id) { // ...

}

// 一大段就是为了取一个简单的值，有什么更优雅的方式取值吗？
// 第一解决可以一直“点下去”，需要返回对象（object、array、function）
// 第二解决取值通过执行进行，这样返回的对象只能是function
// es6的Proxy可以对get、apply拦截
function pointer(obj, path = []) {
  return new Proxy(() => {}, {
    get (target, property) {
      // 拦截 path， apply里面使用
      return pointer(obj, path.concat(property))
    },
    apply (target, self, args) {
      let val = obj;
      let parent;
      for(let i = 0; i < path.length; i++) {
        // 如果没值不用再次取值
        if(val === null || val === undefined) break;
        parent = val;
        val = val[path[i]]
      }
      if(val === null || val === undefined) {
        // 如果有默认值，附上默认值
        val = args[0]
      }
      return val;
    }
  })
}


let c = {a: {b: [1, ,2 ,3]}}

console.log(pointer(c).a());   // {b: [1,2,3]}

console.log(pointer(c).a.b()); // [1,2,3]
console.log(pointer(c).a.a.a); // ƒ anonymous()
console.log(pointer(c).a.a.a()); // undefined

console.log(pointer(c).a.b.d('default value'));  // default value
```