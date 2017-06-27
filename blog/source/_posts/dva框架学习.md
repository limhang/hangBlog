---
title: dva框架学习
date: 2017-06-27 14:21:34
tags: dva框架
categories: 前端
---

# 缘由：dva&&redux学习总结

<!--more-->

# 一、遇到的问题
## 1-1、es6语法所引起的问题
### 1-1-1、对象处理相关问题
* 对象的再赋值
eg1:
input:
```js 
let result0 = {x:1,y:2};
let result1 = {x:2};
let result = {...result0,x:3};
console.log(result);
```
output:
```
Object {x: 3, y: 2}
```

eg2:
input:
```js
let result0 = {x:1,y:2,z:3};
let result1 = {x:2,y:3};
let result = {...result0,...result1};
console.log(result);
```

output:
```
Object {x: 2, y: 3, z: 3}
```