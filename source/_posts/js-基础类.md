---
title: js-基础类
date: 2017-02-15 14:04:49
tags: [js,base]
---

```javascript
(function (global, factory) {
	if (typeof define === 'function' && (define.amd || define.cmd)) {
		define(factory);
	} else {
		global.BaseClass = factory();
	}
}(this, function () {
	//超级父类
	//为超级父类添加extend方法
	BaseClass.extend = _extend;
	BaseClass.mixin = _mixin;

	return BaseClass;

	/**
	 * [BaseClass 空函数]
	 */
	function BaseClass() {}

	/**
	 * [_mixin 混合函数]
	 * @param  {[type]} r [description]
	 * @param  {[type]} s [description]
	 * @return {[type]}   [description]
	 */
	function _mixin(r, s) {
		for (var p in s) {
			if (s.hasOwnProperty(p)) {
				r[p] = s[p]
			}
		}
	}

	/**
	 * [_extend 扩充方法]
	 * @return {[type]} [description]
	 */
	function _extend() {

		//开关 用来使生成原型时,不调用真正的构成流程init
		this.initPrototype = true
		var prototype = new this()
		this.initPrototype = false

		var items = Array.prototype.slice.call(arguments) || []
		var item

		//支持混入多个属性，并且支持{}也支持 Function
		while (item = items.shift()) {
			_mixin(prototype, item.prototype || item)
		}


		// 这边是返回的类，其实就是我们返回的子类
		function SubBaseClass() {
			if (!SubBaseClass.initPrototype && this.init)
				this.init.apply(this, arguments)//调用init真正的构造函数
		}

		// 赋值原型链，完成继承
		SubBaseClass.prototype = prototype

		// 改变constructor引用
		SubBaseClass.prototype.constructor = SubBaseClass

		// 为子类也添加extend方法
		SubBaseClass.extend = _extend

		return SubBaseClass
	}
});
```
