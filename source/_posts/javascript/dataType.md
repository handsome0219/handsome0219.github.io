---
title: 数据类型
tags: js
categories: Javascript
abbrlink: 41331
date: 2017-12-28 22:13:22
thumbnail: https://wallpapers.wallhaven.cc/wallpapers/full/wallhaven-369829.jpg
---

<!--more-->

# 数据类型

> javascript中有6中数据类型String Number Boolean Object undefined function ES6新添加了一种原始数据类型Symbol



## typeof

javascript中通过 `typeof` 检测一个数据类型，返回一个字符串

```js
var str = "javascript";
var num = 10;
var nu = null;
console.log(typeof str); 	// string
console.log(typeof num); 	// number
console.log(typeof null); 	// object
console.log(typeof typeof str);  // string （返回的是一个字符串）


// undefined 未定义数据类型
var foo;
console.log(foo); 			// undefined

var undefined = 66;
console.log(undefined); 	// 66 undefined可以被定义成变量名 通常不会这么做容易产生歧义

// null
var bar = null;
console.log(typeof bar); 	// object 返回object 
// undefined是变量未初始化 null是一个空对象 但是undefined == null js在判断 "等值" 操作的时候会返回true 尽管有这样子的关系 但是它们两的使用差别很大
```

`ps` : typeof 并不是一个绝对安全的判断数据类型的方式 后边学习其它数据类型的准确方式。例如精确的判断数组、日期等等。



## var 与 let const

js中声明变量使用 `var` 关键字,同样 `let` `const` 也可以用来声明变量

做一个简单的比较说明一下

```js
var a = 10;
b = 20;
delete a;
delete b;
console.log(a); 	// 10
console.log(b); 	// 报错 not defined
```

声明变量如果不加 `var` 那么这个变量可以被delete删除 var相当于一个标记 毕竟声明在全局的变量相当于window.b = 20 可以通过delete删除对象属性



```js
{
  var a = 10;
  let b = 10;
}
console.log(a); 	// 10
console.log(b);		// 报错 not defined

for(var i=0;i<3;i++){ //... }
console.log(i); 	// 3
  
for(let j=0;j<3;j++){ // ... }
console.log(j); 	// 报错
```

`let` 关键字声明的变量只在所在的代码块生效 看大括号的范围 所以let很适合用在for循环, 在循环体外引用就会报错 最常见的遍历节点获取索引的操作 使用 `let` 的循环可以很轻松的获取到对应的索引



**变量提升**

```js
console.log(a); 	// undefined
var a = 10;

// console.log(b); 	// 报错
let b = 10;
console.log(b);		// 10
```

`var` 声明的变量会发生变量提升 js在编译阶段会把变量进行提升 `let` 声明的变量不会出现 变量一定 "要被声明之后" 才可以被使用也很符合正常的使用 毕竟要先有女朋友才可以一起做羞羞的事情



```js
const PI = 3.1415926;
```

`const` 声明一个只读的常量, 声明之后必须立即赋值, 区别与 `var` `let` 可以声明之后在赋值 const会报错, 同样const不会产生变量提升, 只在所在代码块生效



简单的介绍了一下js中数据类型其中很多地方例如 ( 在后边在补充 )

* 为什么 `typeof` 不能准确的判断数据类型
* 变量提升
* 块级作用域