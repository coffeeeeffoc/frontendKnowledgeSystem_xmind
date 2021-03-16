# js

## ecmascript

### es5

### es6

## 类型

## 作用域链

## 事件

### addEventListener

- 语法

	- target.addEventListener(type, listener [, options]);
target.addEventListener(type, listener [, useCapture]);
target.addEventListener(type, listener [, useCapture, wantsUntrusted 
    This API has not been standardized.
    
    
]); // Gecko/Mozilla only

- options:{
  capture?:boolean,
  once?:boolean,
  passive?:boolean,
}

	- capture

		- 是否捕获阶段触发

	- once

		- 是否只触发一次

	- passive

		- 是否事件处理器内不调用preventDefault()

- useCapture:boolean

### 三个阶段

- 捕获阶段

	- 自顶向下

- 目标阶段

	- 目标

- 冒泡阶段

	- 自底向上

## 闭包

## 原型链

