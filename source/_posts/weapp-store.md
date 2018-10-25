---
title: weapp-store
date: 2018-10-25 17:28:38
tags: [微信小程序,状态机]
---

### 微信小程序状态机管理

需要回到数据双绑原理，拦截get方法，订阅操作方法，再set的发布相应的订阅,现在更新的数据

|参数| 意义| 类型 | 备注 |
| ---- | ---------- | -------------------- | -------------------- |
| data | 绑定数据 | Object | 支持get方法 |
| computed | 动态计算 | Object | 内部计算方法 |
| actions | 所有操作方式 | Obejet | 支持同步和async |

```javascript
let target = null

/**
 * 监控运行哪些参数
 * @param {*} 监控函数 
 */
function watcher(myFun) {
  target = myFun
  target()
  target = null
}

/**
 * 依赖类
 */
class Dep {
  constructor () {
    this.subscribers = []
  }
  depend () {
    if (target && !this.subscribers.includes(target)) {
      this.subscribers.push(target)
    }
  }
  notify () {
    this.subscribers.forEach(sub => sub())
  }
}


/**
 * 最小化状态机管理
 */
class MiniStore {
  constructor (opts) {
    let bindGetList = [];
    this.actionMap = opts.actoins;


    if (opts && opts.data) {
      Object.keys(opts.data).forEach(key => {
        let fn =  Object.getOwnPropertyDescriptor(opts.data, key).get;
        // get 方法使用
        if (typeof fn === 'function') {
          bindGetList.push(() => {
            opts.data[key] = fn.call(this.state);
          });
        }
      })
    }

    // 重新赋值数据
    this.state = this.setData(opts.data);

    // 绑定get
    if (bindGetList && bindGetList.length) {
      bindGetList.forEach(fn => {
        this.setWatch(fn);
      });
    }

    // 有计算属性
    if (opts && opts.computed) {
      Object.keys(opts.computed).forEach(key => {
        this.setWatch(opts.computed[key]);
      })
    }
  }

  /**
   * 设置监控
   * @param {} fn 监控函数 
   */
  setWatch(fn) {
    watcher(() => {
      fn.call(this.state);
    });
  }

  /**
   * 设置数据，通过Object.defineProperty 属性拦截
   * @param {*} data 
   */
  setData(data) {
    Object.keys(data).forEach(key => {
      let internalValue = data[key]

      const dep = new Dep()

      if (isObject(internalValue)) {
        internalValue = this.setData(internalValue)
      }

      Object.defineProperty(data, key, {
        get() {
          // console.log('get', data, key);
          dep.depend()
          return internalValue
        },
        set(newVal) {
          // console.log('set', data, key, newVal);
          internalValue = newVal
          dep.notify()
        }
      })
    })

    return data;
  }

  /**
   * 调用方法
   * @param {*} name 方法名称
   * @param  {...any} arg 传递参数
   */
  dispatch(name, ...arg) {
    let { actionMap, state } = this;

    if (actionMap && actionMap[name]) {
      actionMap[name].apply(this, [state, ...arg])
    }
  }
}

/**
 * 是否是对象
 * @param {*} obj 
 */
function isObject(obj) {
  return Object.prototype.toString.call(obj) === '[object Object]'
}

/**
 * 异步等待-测试使用
 * @param {*} timer 
 */
function wait(timer) {
  return new Promise(resolve => {
    setTimeout(resolve, timer)
  });
}

/**
 * 给微信小程序页面绑定watch
 * @param {*} wxPage 
 * @param {*} store 
 * @param {*} obj 
 */
function mapState(wxPage, store, obj) {
  store.setWatch(() => {
    Object.keys(obj).forEach(key => {
      let fn = obj[key];
      store.setWatch(() => {
        if (wxPage.setData) { // wxApp
          wxPage.setData({
            [key]: fn(store.state)
          });
        }
      });
    });
  });
}

module.exports = {
  MiniStore,
  mapState
};
```

### 初始化store

```javascript
// initial state
import {
  MiniStore,
  mapState
} from '../utils/miniStore';

var store = new MiniStore({
  data: {
    price: 5,
    quantity: 2,
    arr: [],
    get unfinishedCount() {
      return this.arr.filter(item => !item.finished).length;
    }
  },
  computed: {
    total() {
      this.total = this.price * this.quantity
    }
  },
  actoins: {
    setPrice(state, price) {
      state.price = price;
    },
    async addPrice(state, data = 1) {
      await wait(3000);
      state.price += data;
      console.log('state.total', state.total);
    },
    addItem(state, data = 1) {
      // state.arr.push(data);
      state.arr = [...state.arr, data];
      console.log('state.unfinishedCount', state.unfinishedCount);
    }
  }
});

module.exports = store;
```

### 小程序页面绑定

```javascript
//index.js
//获取应用实例
const app = getApp()
import { mapState } from '../../utils/miniStore';
import store from '../../store/index';

Page({
  data: {
  },
  onLoad: function () {
    // 绑定数据
    mapState(this, store, {
      inputValue: state => state.price
    });
  }
})

```

### 小程序调用方法

```javascript
import store from '../../store/index';
store.dispatch('setPrice', this.data.inputValue + 2);
```