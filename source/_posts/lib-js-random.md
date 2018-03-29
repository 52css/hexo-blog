---
title: lib.js随机数
date: 2017-09-09 16:11:50
tags: [js,random]
---
```javascript
/**
 * [getNumRandom 获取随机数-数值]
 * @param  {[Number]} maxNum [最大值]
 * @param  {Number} minNum [最小值]
 * @return {[Number]}        [随机数]
 */
function getNumRandom(maxNum, minNum = 1) {
  return Math.floor(Math.random()*maxNum + minNum)
}

/**
 * [getArrRandom 随机一串数组]
 * @param  {[Array]} arr [数组]
 * @return {[Array]}     [打乱数组]
 */
function getArrRandom(arr) {
  return arr.sort(() => {
    return Math.random() - 0.5;
  })
}
```
