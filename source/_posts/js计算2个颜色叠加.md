---
title: js计算2个颜色叠加
date: 2016-08-14 16:35:55
tags: [js,color,fill]
---

### [查看demo](/color-overlay/)

### 背景

ps可以对2个图层（变暗、变亮、正片叠底、颜色加深、线性加深、滤色、颜色减淡、线性减淡）等操作，项目中需要取颜色，并且要多套色系配置，之前做法就是ps取最终色，只能通过算法得到

### 技术点攻克

* 颜色转换，例如#fff -> rgb(255, 255, 255)
* 颜色叠层算法

### 颜色转换

```javascript
/**
 * [colorConvert 颜色转换]
 * @param  {[type]} str [description]
 * @return {[type]}     [description]
 */
function colorConvert(str) {
  var json = {
      hex: "",
      rgb: "",
      hexa: "",
      rgba: "",
      op: 1,
      r: '',
      g: '',
      b: ''
    },
    name2hex = {
      black: "#000000",
      silver: "#c0c0c0",
      maroon: "#800000",
      red: "#ff0000",
      navy: "#000080",
      blue: "#0000ff",
      purple: "#800080",
      fuchsia: "#ff00ff",
      green: "#008000",
      lime: "#00ff00",
      olive: "#808000",
      yellow: "#ffff00",
      teal: "#008080",
      aqua: "#00ffff",
      gray: "#808080",
      white: "#ffffff"
    },
    arr;

  if (name2hex[str]) {
    str = name2hex[str]
  }

  str = str.replace(/(\(|\))/g, "");
  arr = str.split(",");

  if (arr.length % 2 == 0) {
    json.op = arr[arr.length - 1];
    str = arr.slice(0, arr.length - 1).join(",");
  }

  if (arr.length > 2) {
    arr = str.replace(/rgba?/g, "").split(",");
    json.r = arr[0];
    json.g = arr[1];
    json.b = arr[2];

    json.rgb = "rgb(" + arr.join(",") + ")";
    arr.push(json.op)
    json.rgba = "rgba(" + arr + ")";
    var hex = arr.map(function(n) {
      return getOpacity(n);
    }).join("").slice(0, 6);
    json.hex = "#" + hex;
    json.hexa = "#" + (getOpacity(json.op) + "") + hex;
  } else {
    arr = str.replace(/#/, "").split("");
    if (arr.length == 5 || arr.length == 8) {
      json.op = roundDecimal(parseInt(arr.slice(0, 2).join(""), 16) / 255, 1);
      arr = arr.slice(2);
    }
    var newArr = [];
    if (arr.length == 3) {
      arr = arr.map(function(n) {
        return [n, n];
      });
    }
    var hex = arr.join("");
    for (var i = 0; i < 6; i += 2) {
      newArr.push(parseInt("0x" + hex.slice(i, i + 2)));
    }

    json.r = newArr[0];
    json.g = newArr[1];
    json.b = newArr[2];

    json.rgb = "rgb(" + newArr + ")";
    newArr.push(json.op);
    json.rgba = "rgba(" + newArr + ")";
    json.hex = "#" + hex;
    json.hexa = "#" + (getOpacity(json.op) + "") + hex;
  }
  return json;

  function getOpacity(num) {
    return padLeft((+parseInt(num * 255, 10)).toString(16), 2);
  }
}

/**
 * [padLeft 左边补充0]
 * @param  {[type]} str [description]
 * @param  {[type]} len [description]
 * @return {[type]}     [description]
 */
function padLeft(str, len) {
  if (str.length >= len) return str;
  return arguments.callee("0" + str, len);
}

/**
 * [roundDecimal 四舍五入]
 * @param  {[type]} val       [description]
 * @param  {[type]} precision [description]
 * @return {[type]}           [description]
 */
function roundDecimal(val, precision) {
  return Math.round(Math.round(val * Math.pow(10, (precision || 0) + 1)) / 10) / Math.pow(10, (precision || 0));
}
```

### 颜色叠加算法

```javascript
/**
 * [getEffect 所有效果]
 * @return {[type]} [description]
 */
function getEffect() {
  return {
    // 1. 变暗
    darker: function(A, B) {
      return Math.min(A, B);
    },
    // 2. 变亮
    lighter: function(A, B) {
      return Math.max(A, B);
    },
    // 3. 正片叠底
    multiply: function(A, B) {
      return A * B / 255;
    },
    // 4. 颜色加深
    colorDeepening: function(A, B) {
      var rtv = A - ((255 - A) * (255 - B)) / B;
      return rtv > 0 ? roundDecimal(rtv) : 0;
    },
    // 5. 线性加深
    linearDarker: function(A, B) {
      return A + B - 255;
    },
    // 6. 滤色
    colorFilter: function(A, B) {
      return Math.min(A + B - (A * B) / 255, 255);
    },
    // 7. 颜色减淡
    colorDodge: function(A, B) {
      return Math.min(A + (A * B) / (255 - B), 255);
    },
    // 8. 线性减淡
    linearDodge: function(A, B) {
      return A + B;
    }
  };
}
```

### 参考文献

* http://bbs.csdn.net/topics/360229760
