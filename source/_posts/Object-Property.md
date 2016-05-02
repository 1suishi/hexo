---
title: Object Property
date: 2016-04-27 20:09:27
tags: 
    - js
---
## 枚举对象属性方法
	for...in
	Object.keys

## 对象添加属性
    赋值
    Object.defineProperty() 
## 获取属性描述符
    Object.getOwnPropertyDescriptor
<!-- more -->
## descriptor 可供定义的特性列表
value
writeable
get
set
configurable
enumberable

**descriptor 不能同时具有 value 或 writable 特性，并且具有 get 或 set 特性。
**
***
> writable false 
	重新赋值无效
	严格模式报错
	```
	TypeError: Cannot assign to read only property 'p' of [object Object]
	```
***
>	enumerable
	可以控制属性是否能被枚举 for...in

***
>	configurable  
	控制 writable，configurable, 	enumberable 是否可以改写

***
>	get/set访问器
	```
	var obj = { };
	Object.defineProperty(obj, 'attr', {
	    set: function(val) { this._attr = Math.max(0, val); },
	    get: function() { return this._attr; }
	});
	obj.attr = -1;
	console.log(obj.attr); // 0
	```
***
> 默认的访问器参数
	```
	{ value: 1, writable: true, enumerable: true, configurable: true }
	```