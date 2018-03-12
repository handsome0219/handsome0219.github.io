---
title: scope
tags: js
categories: Javascript
abbrlink: 31182
date: 2018-02-07 14:40:35
thumbnail: /img/javascript/scope.jpg
---

<!-- more -->

> Javascript的执行环境（ *execution context* 执行上下文）依赖于浏览器，每一个变量，函数都会有自己的执行环境，在web浏览器中执行环境 *window* 对象。即全局创建的变量函数都做为 *window* 对象的属性和方法。需要强调的是 *function* 也是一个独立的执行环境。程序执行依次进入 *call stack* 进栈执行，执行到函数进入函数内部执行上下文，也就是进入执行环境会创建变量对象的作用域链（ *scope chain* ）按这种机制有序的访问所有的变量与函数。所以学习 *Javascript* 执行机制是非常必要的。



# scope

> 我们习惯性的把script的大环境称之为全局，单独的函数，块级作用域称之为局部。所谓相对绝对都是依靠一个参照点。



## hosit

首先来了解一下 *js* 在浏览器中执行所经过的两个阶段 **词法分析** 与 **执行阶段** 词法分析也可以想成其它语言的编译阶段。

```js
var num = 10;
num += 1;
console.log(num);
// 当浏览器的js解析引擎工作会把上述代码变成 比较简单
var num;
num = 10;
num = num + 1;
console.log(num);
```

再来看一个

```js
console.log(num); // undefined
var num = 10;
console.log(num); // 10

// js解析过程
var num;
console.log(num);
num = 10;
console.log(num);
```

这里有一个概念 *Hoist*（ 提升），词法分析阶段所有定义过程譬如定义变量，定义函数都会被提升到当前`context` 的**顶端**，在执行阶段在依次执行譬如赋值、运算、函数调用。但是函数是一个独立的作用域进入函数的 `context` 依然是进行这两个阶段。

```js
var num = 10;
console.log(foo); 	// foo
console.log(fn); 	// undefined
var fn = function(){ alert(1) };
function foo(){}；

//  js解析过程
// 词法分析
var num;
var fn;
function foo(){}
// 执行
num = 10;
console.log(foo);
console.log(fn);
fn = function(){ alert(1) };
```

需要注意的是函数的定义方式与表达式的写法在运行的过程中是不一样的。定义变量初始化赋值是两个过程。否则在调用的过程中可能会出现错误。

### let

`let` 不存在 *hosit* 现象更好的规避了可能出现访问失败的问题，更好按照一种逻辑去执行。在这里提一下



## scope chain

> 作用域链。执行环境的不同，所以当程序访问不同的变量与函数需要一定的规则去辨别

```js
// eg1
var num = 10;
function fn(){
   console.log(num);
   num = num + 10;
}
fn();
```

![scope](/img/javascript/scope1.png)

```js
// eg2
var num = 10;
function fn(){
   console.log(num);
   var num = 20;
   console.log(num);
}
fn()

//  js解析过程
// 词法分析
var num；
function fn(){}
// 执行
num = 10;
fn();
// fn context 词法分析
fn(){
  var num;
}
// 执行
```

![scope](/img/javascript/scope2.png)

上边两个情况做一个简单的对比 `eg1` fn内部可以访问到外部的 `num` 变量顺着作用域链，`eg2` fn内部同样可以访问到外部的 `num` 但是结果不同，试想一下如果外部与自己具有相同变量名称时，就会出现一个访问顺序的问题，当fn在搜索 `num` 这个变量时当前自己的 *scope* 不存在就会沿着 *chain* 去继续往上一级 *scope* 寻找，如果自己有就不会在顺着 *chain* 查找。

可能有人开始有这样的想法。那如果我想外部访问到函数内部的变量呢？

```js
var num = 10
function fn(){
   var num = 20;
   return num;
}
console.log(num);     // 10
console.log(fn(num)); // 20 可以通过return返回 这个并没有延长作用域链 return num同样也会去沿着作用域链去去查找 fn中的num生命周期会随着函数的结束而消失 js中垃圾回收
```



## block scope

js中之前没有明确的块级作用域

```js
for(var i=0;i<4;i++){}
console.log(i);     // 4

if(true){
  var num = 10;
  let block1 = 20;
}
console.log(num);   // 10
console.log(block1); // not defined

{
  let block2 = 30;
}
console.log(block2); // not defined
```

`let` `const` 会把被大括号限制范围，也就是说外界无法通过大括号访问内部的用`let` `const`声明的变量。

**特殊的**

```js
fn(); //  TypeError: a is not a function
if(true)
  function fn(){ console.log(1) }
else
  function fn(){ console.log(2) }
```

> *if* 会将函数定义转变为表达式去处理

```js
var fn;
fn();
if()...else...
```



