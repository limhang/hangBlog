---
title: 你不知道的js（读书笔记）
date: 2017-04-08 18:59:09
tags: javascript
categories: 前端
---

# 缘由：初期学习JavaScript的时候，一直对其运行源码十分感兴趣，但是网上关于这些的博客或者说资源都写的太随意，有阅读过一个[JavaScript的编译器源码（小型）](https://github.com/sanyueyuxincao/the-super-tiny-compiler)，更对底层点的js更感兴趣，无意间看到此书，隧写点读书笔记。

<!--more-->

# 一、作用域和闭包
## 1-1、基本概念
### 1-1-1、引擎----从头到尾负责整个 JavaScript 程序的编译及执行过程
### 1-1-2、作用域
引擎的另一位好朋友，负责收集并维护由所有声明的标识符(变量)组成的一系列查 询，并实施一套非常严格的规则，确定当前执行的代码对这些标识符的访问权限。
### 1-1-3、编译器----引擎的好朋友之一，负责语法分析及代码生成等脏活累活

编译主要有3个部分构成如下：
***
1、词法分析

这个过程将输入的代码（字符串）转换成词法单元，栗子：var a = 2;

转换成"var" "a" "=" "2" ";"这些词法单元

2、语法分析

这个阶段主要将上一个阶段生成的词法单元，转换成“抽象语法树”，类似于html文档结构中的，父节点，子节点之类的概念。

3、代码生成

这个阶段将抽象语法树转换成机器可以理解的机器执行码
***

**总结：**

上述编译3个步骤，是大部分编译器所执行的主要工作流，JavaScript引擎不会花费太多时间来优化编译过程，因为它不像其他编译器，编译是发生在执行前。JavaScript编译是发生在执行前的几微秒。

简单地说，任何 JavaScript 代码片段在执行前都要进行编译(通常就在执行前)。因此， JavaScript 编译器首先会对 var a = 2; 这段程序进行编译，然后做好执行它的准备，并且 通常马上就会执行它。

## 1-2、块级作用域的重认识
### 1-2-1、let的块级作用域使用

```js
for (var i=0; i<10; i++) {
    console.log( i );
}
```

我们在 for 循环的头部直接定义了变量 i，通常是因为只想在 for 循环内部的上下文中使用 i，而忽略了 i 会被绑定在外部作用域(函数或全局)中的事实。

这就是块作用域的用处。变量的声明应该距离使用的地方越近越好，并最大限度地本地化。作用域泡都是用完就丢掉的，变量应该尽量本地化，不能污染其他作用域。

反面案例：

```js
var a = 2;
function bar (c) {
    a = 3;
    console.log(c+a);
}
bar(1);
console.log(a);
```

let的块级作用域使用，栗子：

```js
for (let i=0; i<10; i++) {
    console.log( i );
}
console.log( i ); // ReferenceError
```

### 1-2-2、const块级作用域使用
除了 let 以外，ES6 还引入了 const，同样可以用来创建块作用域变量，但其值是固定的 (常量)。之后任何试图修改值的操作都会引起错误。

```js
var foo = true;
if (foo) {
    var a = 2;
    const b = 3; // 包含在 if 中的块作用域常量
    a = 3; // 正常!
    b = 4; // 错误! 
}
console.log( a ); // 3
console.log( b ); // ReferenceError!
```

## 1-3、作用域提升
其实我一开始的时候，也只是知其然，不知其所以然，但是看了这本书介绍后，还是有点明白了，这个问题需要从编译器的角度看，还记得第一章的时候说过，js引擎，不会对编译阶段做过多的优化，因为编译的阶段就是执行前的几微秒，所以，编译器，第一步的词法分析就是找到所有所有的变量和函数定义，然后在运行阶段，执行代码，所以变量和函数的声明都会进行提升。

## 1-4、闭包，何为闭包
### 1-4-1、闭包基本认识
之前在学习iOS初期的时候，也一直有这个疑问，在oc中什么叫做闭包呢，在第一遍看oc内存管理的书籍时候，看完觉得晦涩难懂，之后每隔一段时间又重看一遍，基本没次都有新的感悟，对闭包有了诸多的理解，其实js的闭包和oc的闭包的核心思想是一致的，只是语法和实现上有点不同。

我觉得闭包和回调函数一起理解和消化才是最好的，或者说回调函数是一种利用闭包的技术，是回调函数最大程度的发挥了闭包的作用。

下面一起来看一个栗子（js）：

```js
function foo() {
    var a = 2;
    function bar() { 
        console.log( a );
    }
    return bar; 
}
var baz = foo();
baz(); // 2 —— 朋友，这就是闭包的效果。
```

在 foo() 执行后，通常会期待 foo() 的整个内部作用域都被销毁，因为我们知道引擎有垃 圾回收器用来释放不再使用的内存空间。由于看上去 foo() 的内容不会再被使用，所以很 自然地会考虑对其进行回收。
而闭包的“神奇”之处正是可以阻止这件事情的发生。事实上内部作用域依然存在，因此 没有被回收。谁在使用这个内部作用域?原来是 bar() 本身在使用。
拜 bar() 所声明的位置所赐，它拥有涵盖 foo() 内部作用域的闭包，使得该作用域能够一 直存活，以供 bar() 在之后任何时间进行引用。
bar() 依然持有对该作用域的引用，而这个引用就叫作闭包。
因此，在几微秒之后变量 baz 被实际调用(调用内部函数 bar)，不出意料它可以访问定义时的词法作用域，因此它也可以如预期般访问变量 a。

这个函数在定义时的词法作用域以外的地方被调用。闭包使得函数可以继续访问定义时的词法作用域。

### 1-4-2、闭包联合块级作用域的demo
```js
for (var i=1; i<=5; i++) { 
    setTimeout( function timer() {
    console.log( i );
    }, i*1000 );
}
```

正常情况下，我们对这段代码行为的预期是分别输出数字 1~5，每秒一次，每次一个。
但实际上，这段代码在运行时会以每秒一次的频率输出五次 6。
这是为什么?
首先解释 6 是从哪里来的。这个循环的终止条件是 i 不再 <=5。条件首次成立时 i 的值是 6。因此，输出显示的是循环结束时 i 的最终值。
仔细想一下，这好像又是显而易见的，延迟函数的回调会在循环结束时才执行。事实上， 当定时器运行时即使每个迭代中执行的是setTimeout(.., 0)，所有的回调函数依然是在循 环结束后才会被执行，因此会每次输出一个 6 出来。

【代码运行起来很快，延迟函数放在另一队列中，等到循环5次后，才来的急执行队列中的函数】

解决办法：

```js
for (let i=1; i<=5; i++) { 
    setTimeout( function timer() {
        console.log( i );
    }, i*1000 );
}
```

## 1-5、模块
```js
function CoolModule() {
    var something = "cool";
    var another = [1, 2, 3];
    function doSomething() { 
        console.log( something );
    }
    function doAnother(){
        console.log( another.join( " ! " ) );
    }
    return {
        doSomething: doSomething,
        doAnother: doAnother
    }; 
}
var foo = CoolModule(); 
foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```

这个模式在 JavaScript 中被称为模块。最常见的实现模块模式的方法通常被称为模块暴露， 这里展示的是其变体。

ES6 的模块没有“行内”格式，必须被定义在独立的文件中(一个文件一个模块)。浏览 器或引擎有一个默认的“模块加载器”(可以被重载，但这远超出了我们的讨论范围)可 以在导入模块时异步地加载模块文件。

```js
bar.js

function hello(who) {
    return "Let me introduce: " + who;
}export hello; 

foo.js// 仅从 "bar" 模块导入 hello() 

import hello from "bar";
var hungry = "hippo";
function awesome() { 
    console.log(        hello( hungry ).toUpperCase()
    );
}    
export awesome;

baz.js
// 导入完整的 "foo" 和 "bar" 模块

module foo from "foo";
module bar from "bar";
console.log(
    bar.hello( "rhino" )
); // Let me introduce: rhino
foo.awesome(); // LET ME INTRODUCE: HIPPO
```

import 可以将一个模块中的一个或多个 API 导入到当前作用域中，并分别绑定在一个变量 上(在我们的例子里是 hello)。module 会将整个模块的 API 导入并绑定到一个变量上(在 我们的例子里是 foo 和 bar)。export 会将当前模块的一个标识符(变量、函数)导出为公 共 API。这些操作可以在模块定义中根据需要使用任意多次。

# 二、this和对象原型
