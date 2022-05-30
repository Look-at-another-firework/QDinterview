# js的学习

## 什么是防抖和节流?有什么区别?如何实现?

`防抖`：动作绑定事件，动作发生后一定时间后触发事件，在这段时间内，如果该动作又发生，则重新等待一定时间再触发事件。（可以理解为moba的回程）

```js
// 防抖简洁
function debounce(func, time) {
    let timer = null;
	return ()=>{
		clearTimeout(timer);
		timer = setTimeout((o=>{
            func.apply(this,arguments)
        }, time);
	}
}

// 防抖完整
const deb = (fn, delay, immediate) => {
	let timer = null
	return function() {	
		const context = this
		timer && clearTimeout(timer)
		if (immediate) {
			!timer && fn.apply(context, arguments)
		}
		timer = setTimeout(() => {
                       fn.apply(context, arguments)
                }, delay)
	}
}
```

`节流`：动作绑定事件，动作发生后一段时间后触发事件，在这段时间内，如果动作又发生，则无视该动作，直到事件执行完后，才能重新触发。（可以理解为moba的攻击键）

```js
// 节流简洁
function throtte(func, time){
    let activeTime = ;
	return ()=>{
		const current = Date.now();
		if(current - activeTime > time){
            func.apply(this, arguments);
            activeTime = Date.now();
        }
	}
}

// 节流完整
const throttle = (fn, delay = 2000) => {
	let timer = null
	let startTime = new Date()
	return function() {
		const context = this
		let currentTime = new Date()
		clearTimeout(timer)
		if (currentTime - startTime >= delay) {
			fn.apply(context, arguments)
			startTime = currentTime
		} else {
			//让方法在脱离事件后也能执行一次
			timer = setTimeout(() => {
				fn.apply(context, arguments)
			}, delay)
		}
	}
}
```

## 关于const和 let声明的变量不在window上

`const`和`let`会生成块级作用域，可以理解为：

```js
let a = 10;
const b = 20;
相当于:
(function(){
	var a= 10;
    var b= 20;
})()
```

`ES5`没有块级作用域的概念，只有函数作用域，可以近似理解成这样。
所以外层`window`必然无法访问。

## 下列代码输出什么内容，为什么？

```js
var b = 10; 
(function b() {
   b = 20;
   console.log(b);
})();
```

打印的是函数b：

```js
ƒ b() {
      b = 20;
      console.log(b);
    }
```

原因：

内部作用域，会先去查找是有已有变量b的声明，有就赋值20，，发现了具名函数function b(){}，拿此b做赋值;

`IIFE`的函数无法进行赋值(内部机制,类似const定义的常量)

举例：

```js
var b = 10; 
(function b() {
   b = 20;
   console.log(b); // 打印函数b
   console.log(window.b); // 打印10 ，不是20
})();
```

```js
var b = 10; 
(function b() {
   'use strict'
   b = 20;
   console.log(b); // 报错Uncaught TypeError: Assignment to constant variable.
})();
```

```js
var b = 10; 
(function b() {
   window.b = 20;
   console.log(b); // 打印函数b
   console.log(window.b); // 打印20
})();
```

```js
var b = 10; 
(function b() {
   var b = 20;
   console.log(b); // 20
   console.log(window.b); // 10
})();
```

## call和apply的区别是什么，哪个性能更好一些

1.  `call`:第一个参数是为函数内部指定this指向，后续的参数则是函数执行时所需要的参数，一个一个传递。
2. `apply`:第一个参数与call相同，为函数内部this指向，而函数的参数，则以数组的形式传递，作为`apply`第二参数。
3.  `call`的性能更好，不过`lodash`里的源码当参数小于等于3时用`call`，之后用`apply`，个人理解是内部少了一次将`apply`第二个参数解构的操作

## 箭头函数与普通函数(function)的区别是什么?构造函数(function)可以使用new生成实例，那么箭头函数可以吗?为什么?

箭头函数是普通函数的简写，可以更优雅的定义一个函数，和普通函数相比，有以下几点差异:

1. 函数体内的 `this`对象，就是定义时所在的对象，而不是使用时所在的对象。

2. 不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，可以用`rest` 参数代替。

3. 不可以使用`yield`命令，因此箭头函数不能用作`Generator` 函数。

4. 不可以使用`new`命令，因为:

   > 没有自己的this，无法调用call，apply。
   >
   > 没有prototype属性，而new命令在执行时需要将构造函数的prototype赋值给新的对象的\_proto\_

补充一下new的过程大致是这样的：

```js
function newFunc(father, ...rest) {
    var result = {};
	result._proto__ = father. prototype;
	var result2 = father.apply(result,rest);
    if (
	  (typeof result2 === 'object' || typeof result2 === 'function') &&  result2 !== null
	) {
	return result2;
    }
	return result;
}
```

## a.b.c.d和a\['b']\['c']\['d']，哪个性能更高?

a.b.c.d 比a\[ 'b']\[ 'c']\[ 'd']性能高点，后者还要考虑[]中是变量的情况，再者，从两种形式的结构来看，显然编译器解析前者要比后者容易些，自然也就快一点。

## var、let和const 区别的实现原理是什么

简单理解：

1. `var`的话会直接在栈内存里预分配内存空间，然后等到实际语句执行的时候，再存储对应的变量，如果传的是引用类型，那么会在堆内存里开辟一个内存空间存储实际内容，栈内存会存储一个指向堆内存的指针
2. `let`的话，是不会在栈内存里预分配内存空间，而且在栈内存分配变量时，做一个检查，如果已经有相同变量名存在就会报错
3. `const`的话，也不会预分配内存空间，在栈内存分配变量时也会做同样的检查。不过`const`存储的变量是不可修改的，对于基本类型来说你无法修改定义的值，对于引用类型来说你无法修改栈内存里分配的指针，但是你可以修改指针指向的对象里面的属性

深度理解：

一、声明过程：

1. `var`:遇到有`var`的作用域，在任何语句执行前都已经完成了声明和初始化，也就是变量提升而且拿到`undefined`的原因由来
2. `function`:声明、初始化、赋值一开始就全部完成，所以函数的变量提升优先级更高
3. `let`:解析器进入一个块级作用域，发现`let`关键字，变量只是先完成声明，并没有到初始化那一步。此时如果在此作用域提前访问，则报错`xx is not defined`，这就是暂时性死区的由来。等到解析到有let那一行的时候，才会进入初始化阶段。如果let的那一行是赋值操作，则初始化和赋值同时进行
4. `const`、`class`都是同`let`—样的道理

二、内存分配：

1. `var`，会直接在栈内存里预分配内存空间，然后等到实际语句执行的时候，再存储对应的变量，如果传的是引用类型，那么会在堆内存里开辟一个内存空间存储实际内容，栈内存会存储一个指向堆内存的指针
2. `let`，是不会在栈内存里预分配内存空间，而且在栈内存分配变量时，做一个检查，如果已经有相同变量名存在就会报错
3. `const`，也不会预分配内存空间，在栈内存分配变量时也会做同样的检查。不过`const`存储的变量是不可修改的，对于基本类型来说你无法修改定义的值，对于引用类型来说你无法修改栈内存里分配的指针，但是你可以修改指针指向的对象里面的属性

三、变量提升：

1. `let` 、`const`和`var`三者其实会存在变量提升
2. `let`只是创建过程提升，初始化过程并没有提升，所以会产生暂时性死区。
3. `var`的创建和初始化过程都提升了，所以在赋值前访问会得到`undefined`
4. `function` 的创建、初始化、赋值都被提升了

## JS异步解决方案的发展历程以及优缺点

1. 回调函数(`callback`)

```js
setTimeout(()=>{
	// callback 函数体
},1000)
```

缺点：回调地狱，不能用`try catch`捕获错误，不能`return`
回调地狱的根本问题在于:

> 1. 缺乏顺序性:回调地狱导致的调试困难，和大脑的思维方式不符
> 2. 嵌套函数存在耦合性，一旦有所改动，就会牵一发而动全身，即（控制反转)
> 3. 嵌套函数过多的多话，很难处理错误

优点：解决了同步的问题(只要有一个任务耗时很长，后面的任务都必须排队等着，会拖延整个程序的执行。)

2. `Promise`

> 1. Promise就是为了解决callback的问题而产生的。
> 2. Promise 实现了链式调用，也就是说每次 then后返回的都是一个全新 Promise，如果我们在then 中return , return的结果会被Promise.resolve()包装

优点：解决了回调地狱的问题

缺点：无法取消`Promise`，错误需要通过回调函数来捕获

3. `Generator`

特点：可以控制函数的执行，可以配合`co 函数库`使用

```js
function *fetch() {
	yield ajax( 'XXX1', ()=>{})
    yield ajax( 'XXX2', ()=>{})
    yield ajax( 'XXX3', ()=>{})
}
let it = fetch()
let result1 = it.next()
let result2 = it.next()
let result3 = it.next()
```

4. `Async` / `await`

`async`、`await`是异步的终极解决方案
优点：代码清晰，不用像`Promise`写一大堆then链，处理了回调地狱的问题
缺点：`await`将异步代码改造成同步代码，如果多个异步操作没有依赖性而使用`await`会导致性能上的降低。

## 使用Array.prototype.at()简化arr.length

Array.prototype.at()接收一个正整数或者负整数作为参数，表示获取指定位置的成员

参数正数就表示顺数第几个，负数表示倒数第几个，这可以很方便的某个数组末尾的元素

不存在-0，-0 = 0

```js
var arr = [1, 2, 3, 4, 5]
// 以前获取最后一位
console.log(arr[arr.length-1]) //5
// 简化后
console.log(arr.at(-1)) // 5

// 第0个
console.log(arr.at(0)) // 1
// 正数第一个
console.log(arr.at(1)) // 2
```

实现步骤：

```js
function at(n) {
  n = Math.trunc(n) || 0; // 去掉小数点
  if (n < 0) n += this.length;
  if (n < 0 || n >= this.length) return undefined;
  return this[n];
}
```

## 事件捕获和事件冒泡以及如何阻止冒泡事件和默认事件

```html
<div id='div1'>
  <div id='div2'>
    <div id='div3'></div>
  </div>
</div>
```

```js
div1.οnclick=function(){
	alert("div1")
}
div2.οnclick=function(){
	alert("div2")
}
div3.οnclick=function(){
	alert("div3")
}
```

当单击中间的div3时，先后弹出div3, div2, div1，此为事件冒泡的过程。

```js
div1.addEventListener('click',function(obj){
	alert("div1")
},true);//如果未false则为事件冒泡，不填的话，默认false

// IE
div1.attachEvent("onclick", doSomething);  // IE浏览器的处理方法
```

阻止冒泡方法:

```js
function stopBubble(event) {
	if(window.event) {
		window.event.cancelBubble = true
	} else {
		event.stopPropagation()
	}
}
```

其他的方法就是判断点击的当前元素是否为自身