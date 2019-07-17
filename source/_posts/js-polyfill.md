---
title: js-polyfill
date: 2019-07-17 11:38:09
tags: [js,polyfill]
---

```js
if (!Array.prototype.indexOf) {
  Array.prototype.indexOf = function(searchElement, fromIndex) {
      var k;
      if (this == null) {
          throw new TypeError('"this" is null or not defined');
      }
      var O = Object(this);
      var len = O.length >>> 0;
      if (len === 0) {
          return -1;
      }
      var n = +fromIndex || 0;
      if (Math.abs(n) === Infinity) {
          n = 0;
      }
      if (n >= len) {
          return -1;
      }
      k = Math.max(n >= 0 ? n : len - Math.abs(n), 0);
      while (k < len) {
          if (k in O && O[k] === searchElement) {
              return k;
          }
          k++;
      }
      return -1;
  };
}

if (!Array.prototype.filter) {
  Array.prototype.filter = function(fun) {
      "use strict";
      if (this === void 0 || this === null)
          throw new TypeError();
      var t = Object(this);
      var len = t.length >>> 0;
      if (typeof fun !== "function")
          throw new TypeError();
      var res = [];
      var thisArg = arguments.length >= 2 ? arguments[1] : void 0;
      for (var i = 0; i < len; i++) {
          if (i in t) {
              var val = t[i];
              if (fun.call(thisArg, val, i, t))
                  res.push(val);
          }
      }
      return res;
  };
}

if ('function' !== typeof Array.prototype.reduce) {
  Array.prototype.reduce = function(callback, opt_initialValue) {
      'use strict';
      if (null === this || 'undefined' === typeof this) {
          throw new TypeError(
              'Array.prototype.reduce called on null or undefined');
      }
      if ('function' !== typeof callback) {
          throw new TypeError(callback + ' is not a function');
      }
      var index, value,
          length = this.length >>> 0,
          isValueSet = false;
      if (1 < arguments.length) {
          value = opt_initialValue;
          isValueSet = true;
      }
      for (index = 0; length > index; ++index) {
          if (this.hasOwnProperty(index)) {
              if (isValueSet) {
                  value = callback(value, this[index], index, this);
              } else {
                  value = this[index];
                  isValueSet = true;
              }
          }
      }
      if (!isValueSet) {
          throw new TypeError('Reduce of empty array with no initial value');
      }
      return value;
  };
}

if (!Array.prototype.forEach) {
  Array.prototype.forEach = function(callback, thisArg) {
      var T, k;
      if (this == null) {
          throw new TypeError(' this is null or not defined');
      }
      var O = Object(this);
      var len = O.length >>> 0;
      if(typeof callback !== "function") {
          throw new TypeError(callback + ' is not a function');
      }
      if (arguments.length > 1) {
          T = thisArg;
      }
      k = 0;
      while (k < len) {
          var kValue;
          if (k in O) {
              kValue = O[k];
              callback.call(T, kValue, k, O);
          }
          k++;
      }
  };
}

if (!String.prototype.trim) {
  String.prototype.trim = function () {
    return this.replace(/^[\s\uFEFF\xA0]+|[\s\uFEFF\xA0]+$/g, '');
  };
}
```