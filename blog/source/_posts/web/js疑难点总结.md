---
title: js疑难点总结
date: 2017-04-09 09:08:05
tags: javascript
categories: 前端
---

# 缘由：javascript的疑难点大概有闭包、作用域、this指向、回调函数和原型链等等，在此做个总结

<!--more-->

# 一、js作用域
## 1-1、概览
* 块级作用域 （es6才支持的）=>{}，在大括号内的就是块级作用域
* 函数作用域  =>就是在函数的实现中的，变量
* 全局作用域  =>在外部声明的变量，如果声明变量的时候，不添加var关键字，则都是全局作用域

本地作用域（函数作用域）的优先级高于全局作用域

## 1-2、常见认知误区
### 1-2-1、在es6之前，块级作用域的一个常见认知错误
```js
	for (var i = 1; i <= 10; i++) {
		console.log (i); // outputs 1, 2, 3, 4, 5, 6, 7, 8, 9, 10;​
	};
	​
	​// The variable i is a global variable and it is accessible in the following function with the last value it was assigned above ​
	​function aNumber () {
	console.log(i);
	}
	​
	​// The variable i in the aNumber function below is the global variable i that was changed in the for loop above. Its last value was 11, set just before the for loop exited:​
	aNumber ();  // 11​
```
​
**解释：**在es6之前没有块级作用域这个概念，所以在块级作用域中声明的变量和赋值的变量，其实就是全局变量。

### 1-2-2、setTimeout中的函数在全局scope中执行
```js
	// The use of the "this" object inside the setTimeout function refers to the Window object, not to myObj​
	​
	​var highValue = 200;
	​var constantVal = 2;
	​var myObj = {
		highValue: 20,
		constantVal: 5,
		calculateIt: function () {
	 setTimeout (function  () {
		console.log(this.constantVal * this.highValue);
	}, 2000);
		}
	}
	​
	​// The "this" object in the setTimeout function used the global highValue and constantVal variables, because the reference to "this" in the setTimeout function refers to the global window object, not to the myObj object as we might expect.​
	​
	myObj.calculateIt(); // 400​
	​// This is an important point to remember.
```

## 1-3、注意事项
### 1-3-1、不要过多的声明全局变量
**wrong way**

```js
	// These two variables are in the global scope and they shouldn't be here​
	​var firstName, lastName;
	​
	​function fullName () {
		console.log ("Full Name: " + firstName + " " + lastName );
	}
```

**right way**

```js
	// Declare the variables inside the function where they are local variables​
	​
	​function fullName () {
		var firstName = "Michael", lastName = "Jackson";
	​
		console.log ("Full Name: " + firstName + " " + lastName );
	}
```

### 1-3-2、使用变量的时候，需要提前声明，虽然有声明提前的功能，但是建议不要这么做
```js
	function showName () {
	console.log ("First Name: " + name);
	​var name = "Ford";
	console.log ("Last Name: " + name);
	}
	​
	showName (); 
	​// First Name: undefined​
	​// Last Name: Ford​
	​
	​// The reason undefined prints first is because the local variable name was hoisted to the top of the function​
	​// Which means it is this local variable that get calls the first time.​
	​// This is how the code is actually processed by the JavaScript engine:​
	​
	​function showName () {
		var name; // name is hoisted (note that is undefined at this point, since the assignment happens below)​
	console.log ("First Name: " + name); // First Name: undefined​
	​
	name = "Ford"; // name is assigned a value​
	​
	​// now name is Ford​
	console.log ("Last Name: " + name); // Last Name: Ford​
	}
```

[外文原稿](http://javascriptissexy.com/javascript-variable-scope-and-hoisting-explained/)

# 二、闭包
## 2-1、闭包的基本概念
闭包就是内部函数获取外部函数的变量（来自scope chain），闭包有3中scope chain，一种是自身的作用域，还有一种是外部函数的作用域，最后是全局的作用域。

闭包（inner function）不仅可以获取外部函数（outer function）的变量还可以获取外部函数的参数。

最简单的闭包如下：

```js
	function showName (firstName, lastName) {
	​var nameIntro = "Your name is ";
	    // this inner function has access to the outer function's variables, including the parameter​
	​function makeFullName () {
	​    return nameIntro + firstName + " " + lastName;
	}
	​return makeFullName ();
	}
	showName ("Michael", "Jackson"); // Your name is Michael Jackson
```
## 2-2、闭包的规则和副作用
### 2-2-1、闭包可以获取外部函数的变量，即使外部函数调用完成（就是执行完成）后。
```js
	function celebrityName (firstName) {
	    var nameIntro = "This celebrity is ";
	    // this inner function has access to the outer function's variables, including the parameter​
	   function lastName (theLastName) {
	        return nameIntro + firstName + " " + theLastName;
	    }
	    return lastName;
	}
	​
	​var mjName = celebrityName ("Michael"); // At this juncture, the celebrityName outer function has returned.​
	​
	​// The closure (lastName) is called here after the outer function has returned above​
	​// Yet, the closure still has access to the outer function's variables and parameter​
	mjName ("Jackson"); // This celebrity is Michael Jackson
```

### 2-2-2、闭包获取的外部函数的变量值，会实时改变
Closures store references to the outer function’s variables; they do not store the actual value. 
Closures get more interesting when the value of the outer function’s variable changes before the closure is called. And this powerful feature can be harnessed in creative ways, such as this private variables example first demonstrated by Douglas Crockford:

```js
	function celebrityID () {
	    var celebrityID = 999;
	    // We are returning an object with some inner functions​
	    // All the inner functions have access to the outer function's variables​
	    return {
	        getID: function ()  {
	            // This inner function will return the UPDATED celebrityID variable​
	            // It will return the current value of celebrityID, even after the changeTheID function changes it​
	          return celebrityID;
	        },
	        setID: function (theNewID)  {
	            // This inner function will change the outer function's variable anytime​
	            celebrityID = theNewID;
	        }
	    }
	​
	}
	​
	​var mjID = celebrityID (); // At this juncture, the celebrityID outer function has returned.​
	mjID.getID(); // 999​
	mjID.setID(567); // Changes the outer function's variable​
	mjID.getID(); // 567: It returns the updated celebrityId variable
```
### 2-2-3、最常见的循环导致闭包出的bug
需要结合2来看，因为闭包获取的外部函数的变量会实时改变

这个例子需要仔细看，其实就是结合了例子1和例子2，最后形成的一个套路
```js
	​function celebrityIDCreator (theCelebrities) {
	    var i;
	    var uniqueID = 100;
	    for (i = 0; i < theCelebrities.length; i++) {
	      theCelebrities[i]["id"] = function ()  {
	        return uniqueID + i;
	      }
	    }
	    return theCelebrities;
	}
	var actionCelebs = [{name:"Stallone", id:0}, {name:"Cruise", id:0}, {name:"Willis", id:0}];
	​var createIdForActionCelebs = celebrityIDCreator (actionCelebs);
	​var stalloneID = createIdForActionCelebs[0];
	​console.log(stalloneID.id()); // 103
```

**解决办法：**核心就是使用立即执行函数，这样返回的就不是一个函数了，而是执行完成的结果
```js
	function celebrityIDCreator (theCelebrities) {
	    var i;
	    var uniqueID = 100;
	    for (i = 0; i < theCelebrities.length; i++) {
	        theCelebrities[i]["id"] = function (j)  { // the j parametric variable is the i passed in on invocation of this IIFE​
	            return function () {
	                return uniqueID + j; // each iteration of the for loop passes the current value of i into this IIFE and it saves the correct value to the array​
	            } () // BY adding () at the end of this function, we are executing it immediately and returning just the value of uniqueID + j, instead of returning a function.​
	        } (i); // immediately invoke the function passing the i variable as a parameter​
	    }
	​
	    return theCelebrities;
	}
	​
	​var actionCelebs = [{name:"Stallone", id:0}, {name:"Cruise", id:0}, {name:"Willis", id:0}];
	​
	​var createIdForActionCelebs = celebrityIDCreator (actionCelebs);
	​
	​var stalloneID = createIdForActionCelebs [0];
	console.log(stalloneID.id); // 100​
	​var cruiseID = createIdForActionCelebs [1];
	​console.log(cruiseID.id); // 101
```

# 三、回调函数
## 3-1、基本概念
在javascript语法中，函数即使对象，就像String,Array,Number等其他对象一样。所以:

function可以被赋值给变量，可以作为函数的参数进行传递，可以在函数内创建新的function，可以被函数作为返回值，这些情况其实就是回调函数。

因为函数是first-class对象，我们可以将其作为参数传递给其他函数，然后在该函数内部执行传递进去的函数，也可以将其作为返回值，然后在执行该返回值函数。

## 3-2、常见的回调函数
```js
	var friends = ["Mike", "Stacy", "Andy", "Rick"];
	​
	friends.forEach(function (eachName, index){
	console.log(index + 1 + ". " + eachName); // 1. Mike, 2. Stacy, 3. Andy, 4. Rick​
	});
```

## 3-3、实现回调函数的基本准则
### 3-3-1、Use Named OR Anonymous Functions as Callbacks
```js
	// global variable​
	​var allUserData = [];
	​
	​// generic logStuff function that prints to console​
	​function logStuff (userData) {
	    if ( typeof userData === "string")
	    {
	        console.log(userData);
	    }
	    else if ( typeof userData === "object")
	    {
	        for (var item in userData) {
	            console.log(item + ": " + userData[item]);
	        }
	​
	    }
	​
	}
	​
	​// A function that takes two parameters, the last one a callback function​
	​function getInput (options, callback) {
	    allUserData.push (options);
	    callback (options);
	​
	}
	​
	​// When we call the getInput function, we pass logStuff as a parameter.​
	​// So logStuff will be the function that will called back (or executed) inside the getInput function​
	getInput ({name:"Rich", speciality:"JavaScript"}, logStuff);
	​//  name: Rich​
	​// speciality: JavaScript
```

### 3-3-2、Make Sure Callback is a Function Before Executing It
```js
	function getInput(options, callback) {
	    allUserData.push(options);
	​
	    // Make sure the callback is a function​
	    if (typeof callback === "function") {
	    // Call it, since we have confirmed it is callable​
	        callback(options);
	    }
	}
```

### 3-3-3、Problem When Using Methods With The this Object as Callbacks
```js
	// Define an object with some properties and a method​
	​// We will later pass the method as a callback function to another function​
	​var clientData = {
	    id: 094545,
	    fullName: "Not Set",
	    // setUserName is a method on the clientData object​
	    setUserName: function (firstName, lastName)  {
	        // this refers to the fullName property in this object​
	      this.fullName = firstName + " " + lastName;
	    }
	}
	​
	​function getUserInput(firstName, lastName, callback)  {
	    // Do other stuff to validate firstName/lastName here​
	​
	    // Now save the names​
	    callback (firstName, lastName);
	}

	getUserInput ("Barack", "Obama", clientData.setUserName);
	​
	console.log (clientData.fullName);// Not Set​
	​
	​// The fullName property was initialized on the window object​
	console.log (window.fullName); // Barack Obama
```

**Use the Call or Apply Function To Preserve this**

```js
	//Note that we have added an extra parameter for the callback object, called "callbackObj"​
	​function getUserInput(firstName, lastName, callback, callbackObj)  {
	    // Do other stuff to validate name here​
	​
	    // The use of the Apply function below will set the this object to be callbackObj​
	    callback.apply (callbackObj, [firstName, lastName]);
	}
		
	// We pass the clientData.setUserName method and the clientData object as parameters. The clientData object will be used by the Apply function to set the this object​
	getUserInput ("Barack", "Obama", clientData.setUserName, clientData);
	// the fullName property on the clientData was correctly set​
	console.log (clientData.fullName); // Barack Obama
```

[译文参考](http://javascriptissexy.com/understand-javascript-callback-functions-and-use-them/)


# 四、特殊函数答疑
## 4-1、bind函数应用分析
### 4-1-1、bind函数功能
bind()方法会创建一个新函数，当这个新函数被调用时，它的this值是传递给bind()的第一个参数, 它的参数是bind()的其他参数和其原本的参数. 

react应用：
```js
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(prevState => ({
      isToggleOn: !prevState.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}
```

### 4-1-2、bind函数demo
bind() 最简单的用法是创建一个函数，使这个函数不论怎么调用都有同样的 this 值。JavaScript新手经常犯的一个错误是将一个方法从对象中拿出来，然后再调用，希望方法中的 this 是原来的对象。（比如在回调中传入这个方法。）如果不做特殊处理的话，一般会丢失原来的对象。从原来的函数和原来的对象创建一个绑定函数，则能很漂亮地解决这个问题：

```js
this.x = 9; 
var module = {
  x: 81,
  getX: function() { return this.x; }
};

module.getX(); // 返回 81

var retrieveX = module.getX;
retrieveX(); // 返回 9, 在这种情况下，"this"指向全局作用域

// 创建一个新函数，将"this"绑定到module对象
// 新手可能会被全局的x变量和module里的属性x所迷惑
var boundGetX = retrieveX.bind(module);
boundGetX(); // 返回 81
```

## 4-2、call,apply函数应用分析
### 4-2-1、call与apply基本使用
在 javascript 中，call 和 apply 都是为了改变某个函数运行时的上下文（context）而存在的，换句话说，就是为了改变函数体内部 this 的指向。JavaScript 的一大特点是，函数存在「定义时上下文」和「运行时上下文」以及「上下文是可以改变的」这样的概念。

**eg:**

```
function fruits() {}
 
fruits.prototype = {
    color: "red",
    say: function() {
        console.log("My color is " + this.color);
    }
}
 
var apple = new fruits;
apple.say();    //My color is red
```

但是如果我们有一个对象banana= {color : "yellow"} ,我们不想对它重新定义 say 方法，那么我们可以通过 call 或 apply 用 apple 的 say 方法：

```
banana = {
    color: "yellow"
}
apple.say.call(banana);     //My color is yellow
apple.say.apply(banana);    //My color is yellow
```

### 4-2-2、call和apply使用区别
JavaScript 中，某个函数的参数数量是不固定的，因此要说适用条件的话，当你的参数是明确知道数量时用 call 。
而不确定的时候用 apply，然后把参数 push 进数组传递进去。当参数数量不确定时，函数内部也可以通过 arguments 这个伪数组来遍历所有的参数。

```
var  numbers = [5, 458 , 120 , -215 ]; 
var maxInNumbers = Math.max.apply(Math, numbers),   //458
    maxInNumbers = Math.max.call(Math,5, 458 , 120 , -215); //458
```

## 4-3、参考资料
[妙用Javascript中apply、call、bind](https://www.cnblogs.com/coco1s/p/4833199.html)

# 五、js原型，原型链，继承，构造函数归纳总结
写在前面：
这些概念，对于理解js这一门语言非常重要，要清楚的理解上述概念，需要记住以下几点原则：

* 在js世界中，一切皆对象，函数也是对象，对象都有__proto__
* 函数（fn）都有一个prototype属性，该属性（prototype）有一个属性contructor指向该函数（fn）
* 继承在设计中一定要明白一个原则，公用性【多个对象公用方法或者属性，节约资源】，独立性【各个对象有自己私有的属性和方法，修改它只会影响到自己】

记住以上三点，就可以开始学习下面的内容了

## 5-1、原型、原型链和构造函数

```
 // var a = new A('hehe') =>
 var a = new Object();
 a.__proto__ = A.prototype; (proto)
 A.call(a, 'hehe');
```

下面用一个例子来理解一下

```
    function Boss(name,money){
        this.name = name ;
        this.money = money ;
    };
    Boss.prototype = {
        constructor:Boss ,
        business:function(){
            console.log(this.name+"生意做的不错") ;
        }
    } ;

    function RichSecondGeneration(name,money,graceful){
        Boss.call(this,name,money) ;
        this.graceful = graceful ;
    } ;
    RichSecondGeneration.prototype = new Boss() ;
    
    RichSecondGeneration.prototype.pickUpGirl = function(){
        if(this.graceful){
            console.log(this.name + "把妹很厉害") ;
        }else{
            console.log(this.name + "把妹方式很下流") ;
        }
    } ;

    var wangsicong = new RichSecondGeneration("王思聪",100000000000,true) ;
        console.log(wangsicong.name,wangsicong.money,wangsicong.graceful) ;
        wangsicong.business() ; 
        wangsicong.pickUpGirl() ;
    
    var lizongrui = new RichSecondGeneration("李宗瑞",1000000000,false) ;
        console.log(lizongrui.name,lizongrui.money,lizongrui.graceful) ;
        lizongrui.business() ; 
        lizongrui.pickUpGirl() ;  
```
上面代码是最常见的继承实现，常用的继承方式是 原型链继承加借用构造函数构造函数继承。

* 构造函数可以使得实例对象的属性互不相干，原型链继承可以使得实例对象共享已定义好的方法 ；
* 属性继承原理：(new + call);
* 方法继承原理：原型链

但是大家有没有想过一个问题，为什么需要new + call这个语句呢，如果不加上【Boss.call(this,name,money);】会怎样？


## 5-2、继承
![js原型继承](http://ok2nitkry.bkt.clouddn.com/js%E5%8E%9F%E5%9E%8B%E7%BB%A7%E6%89%BF.png)

## 参考资料
[js继承设计思想](http://www.ruanyifeng.com/blog/2011/06/designing_ideas_of_inheritance_mechanism_in_javascript.html)

[理解JavaScript的原型链和继承](https://blog.oyanglul.us/javascript/understand-prototype.html)