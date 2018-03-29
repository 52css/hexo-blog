---
title: lib.js四舍五入
date: 2017-09-09 16:13:14
tags: [js,round]
---
### js

js有个奇怪问题 0.1  + 0.2 = 0.30000000000000004， 所有在计算是需要四舍五入

```javascript
/**
 * [roundDecimal 四舍五入]
 * @param  {[type]} val       [description]
 * @param  {[type]} precision [description]
 * @return {[type]}           [description]
 */
function roundDecimal(val, precision) {
  return Math.round(Math.round(val * Math.pow(10, (precision || 0) + 1)) / 10) / Math.pow(10, (precision || 0));
}

/**
 * [roundTo 另外一个种四舍五入]
 * @param  {[type]} num [description]
 * @param  {[type]} pos [description]
 * @return {[type]}     [description]
 */
function roundTo(num, pos) {
    return +(Math.round(num + 'e+' + pos ) + 'e-' + pos);
}
```
