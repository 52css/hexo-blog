---
title: js-最短路径算法
date: 2018-11-21 11:49:38
tags: [js,aStar,minDistance]
---
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <title>最短路径算法</title>
</head>

<body>
  <script type="text/javascript">

    let navMap = {
      '0,486,256': {
          '1,486,256': 1000
      },
      '1,486,256': {
          '0,486,256': 1000,
          '2,486,256': 1000
      },
      '2,486,256': {
          '1,486,256': 1000
      }
    };
    let minDistance1 = getMinDistance(navMap, '0,486,256', '2,486,256');
    let minDistance2 = getMinDistance(navMap, '0,486,256', '2,486,258');

    console.log(minDistance1); // ["0,486,256", "1,486,256", "2,486,256", distance: 2000]
    console.log(minDistance2); // []

    /**
     * [getMinDistance 获取最短路径]
     * @param  {[Object]} navMap    [通过svg转换的navMap，点与点的关系，val是路径]
     * @param  {[String]} from      [来源点]
     * @param  {[String|Array]} to  [结束点，可能是多个结束点，例如找最近的扶梯]
     * @return {[Array]}            [返回最近路径，[]代表无路径，有值代表寻找到]
     */
    function getMinDistance(navMap, from, to) {
      var minDistance = Infinity,
        rtvPath = [];

      getMinDistanceRel([{
        from: from,
        parent: [],
        distance: 0
      }], to);

      // console.log(rtv);

      return rtvPath;

      /**
       * [getMinTo 判断到达点是否是终点]
       * @param  {[String]} key [到达点]
       * @param  {[String|Array]} to  [终点]
       * @return {[Boolean]}     [description]
       */
      function getMinTo(key, to) {
        if (typeof to === 'string') {
          return key === to ? to : false;
        }
        for (let i = 0, len = to.length; i < len; i++) {
          if (to[i] === key) {
            return to[i];
          }
        }
        return false;
      }

      /**
       * [getMinDistanceRel 最短路径核心算法]
       * @param  {[type]} froms [多个来源]
       * @param  {[type]} to    [是否走到结束]
       * @return {[type]}       [description]
       */
      function getMinDistanceRel(froms, to) {
        let newFroms = [];

        // console.log(froms);

        for (var i = 0, len = froms.length; i < len; i++) {
          var from = froms[i].from,
            parent = froms[i].parent,
            hasKey,
            newParent = [].concat(parent),
            distance,
            minTo;

          // 组建一个parent
          newParent.push(from);

          for (var key in navMap[from]) {
            // 获取路径长度
            distance = froms[i].distance + (+navMap[from][key]);

            // 之前的路径是否走过key，走过代表走返回路径，直接break
            hasKey = parent.some(function(item) {
              return item === key;
            });

            // 如果本次路径大于最大路径直接退出
            if (distance > minDistance) {
              break;
            }

            minTo = getMinTo(key, to);
            // 是否到了终点
            if (minTo) {
              // console.log('find times', distance);
              // 如果当前的路径小于上次的minDistance（初始化无穷大）
              if (distance < minDistance) {
                // 重新赋值
                minDistance = distance;

                newParent.push(minTo);

                rtvPath = newParent;
                rtvPath.distance = distance;
              }

              break;
            } else if (!hasKey) {
              // 没有走返回路
              let newFromsIndex = -1;

              for (var newFromsI = 0, newFromsLen = newFroms.length; newFromsI < newFromsLen; newFromsI++) {
                if (newFroms[newFromsI].from === key) {
                  newFromsIndex = newFromsI;
                  break;
                }
              }

              // 之前也到过这个节点
              if (newFromsIndex >= 0) {
                // 如果当前路径小于之前到这个节点最短路径
                if (distance < newFroms[newFromsIndex].distance) {
                  // 更新最短距离信息
                  newFroms[newFromsIndex] = {
                    from: key,
                    parent: newParent,
                    distance: distance
                  }
                }
              } else {
                // 没有到过这里，直接下次循环添加一个froms
                newFroms.push({
                  from: key,
                  parent: newParent,
                  distance: distance
                });
              }
            }
          }
        }

        // 如果有form，就再次循环，直到没有newFroms，并且找到最短路局
        if (newFroms.length) {
          // console.log(newFroms);
          getMinDistanceRel(newFroms, to);
        }
      }
    }
  </script>
</body>

</html>


```