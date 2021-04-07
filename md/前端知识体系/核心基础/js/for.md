[toc]

## for-in
### 使用方式
  - 对数组或者对象的属性进行循环操作
  - 以任意顺序遍历一个对象的可枚举属性
### 关键点
  可枚举属性
### 原理
  可枚举属性，属性的属性描述符descriptor的enumerable属性的值为true
### 缺点
  - 键名为字符串，即使是遍历数组时也是字符串
  - 可以遍历手动添加的键、原型链上的键
  - 某些情况下，for...in循环会以任意顺序遍历键名

  总之，for...in循环主要是为遍历对象而设计的，不适用于遍历数组

## for-of
  - ES6引入
### 关键点
  可迭代对象
### 使用方式
  - 可迭代对象（包括 Array，Map，Set，String，TypedArray，某些类似数组的对象[arguments 对象、DOM NodeList 对象]，Generator 对象等等）
  - 一个数据结构只要部署了Symbol.iterator属性，就被视为具有 iterator 接口
### 原理
  - 一个数据结构只要部署了Symbol.iterator属性，就被视为具有 iterator 接口
### 优点
  - 有着同for...in一样的简洁语法，但是没有for...in那些缺点
  - 它可以与break、continue和return配合使用
  - 提供了遍历所有数据结构的统一操作接口