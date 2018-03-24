---
title: function 函数
tags:
  - JS
  - function
categories: JavaScript
abbrlink: 45833
date: 2018-02-06 16:29:26
thumbnail: /img/javascript/function.jpg
---

<!-- more -->

# Function

> javascript中的函数被称为一等公民（*First-class object*）。函数是一种数据类型、可以当作构造函数、可以当作参数传递、可以赋值成为变量、可以添加属性、可以当作对象、可以当作返回值、可以存储、更加可以去执行。



## 定义

函数的定义分为三种

```js
// 1. 定义式 -- 有名
function fnName(arg1, arg2, ..., args){
  // statement
}

// 2. 表达式 -- 匿名
var fnName = function(arg1, arg2, ..., args){
  // statement
}

// 3. new 关键字实例化创建对象 -- 很少用 
var fnName = new Function(statement);
```

通过 `()` 运算符执行调用函数运行



## 函数理解

刚刚接触函数个人觉得需要认识到几个点

* 函数是一种特殊的数据类型
* 函数是一个独立的作用域
* 函数可以单独执行或者返回具体的结果
* 函数是单纯的个体

分别来看一下，第一条不解释了

独立作用域

```js
var a = 10
function foo(){
  // foo 中的a在函数外部是访问不到的 前提是通过了var let const 定义的变量
  // a相对于script是一个局部的变量 相对于foo是一个全局 理解全局局部的同时希望加上参考范围 不要混淆
  var a = 20;
}
foo();
```



单独执行、返回结果

```js
function sum(a, b){
  console.log(a + b);
}
sum(1, 2); // 3

function add(a, b){
  return a + b;
}
var result = add(1, 2); // 3

/*
  同样都可以返回3 但是前者单纯的去执行 后者将结果返回并且赋值给变量result供其它去使用
  也就是说函数可以有返回值也可以没有返回值 javascript中函数默认返回undefined
*/ 
```



**单纯**

```js
// 所谓的单纯 不会过度的依赖一些变化 导致修改的时候会让复杂度不断提高

// 1
var minAge = 20;
var check = function(age){
  return age > mixAge;
}

// 2
var checkAge = function(age){
  var minAge = 20;
  return age > minAge;
}

/*
  前者会依赖minAge影响最终的结果， 换句话说取决于另外一个程序状态
  后者因为minAge状态不会改变，结果不会受其它程序状态的影响
  有的时候我们可能为了复用一段代码而去封装成一个函数，但是还是多思考一下
  因为当你有一天去修改的时候可能已经对内在的复杂度……懂得吧
  所以尽可能做为一种功能去使用函数
*/
```



## 参数的理解

> 函数中可以传递参数，javascript中传递参数不用去在乎是什么数据类型

```js
/*
  函数定义所写的参数称之为形式参数
  调用时传递的参数称之为实际参数
  1. 形参个数 > 实参个数 少的undefined代替
  2. 实参个数 > 形参个数 多的忽略
  3. 不定参arguments 用于接受传递的所有参数
*/
function fn（a, b）{}
fn(1); // 少写的b被undefined代替 如果有运算就会出问题
fn(1, 2, 3); // 多写的3 会被忽略

function foo(){
  var args = arguments;
  console.log(args);
  console.log(args.length == foo.length);
}
fn(1, 2); // 打印1, 2  打印false 
```

foo.length 返回函数的形式参数的个数也就是定义的时候有多少参数。可以通过这个性质进行判断参数列表是否对应

```js
function foo(a, b){
  if(foo.length == arguments.length) {
    // statement1
  }else{
    // statement2
  }
}
// foo.length 可能为 arguments.callee （返回当前函数 可以想成就近原则）
// arguments.callee 是一个避免函数引用错误的一个很好的做法 但是在'use strict'下callee是禁止使用的
```



在JavaScript中没有函数重载的概念，但是可以通过参数列表 `arguments` 来模拟重载效果，仅仅是类似的效果

```js
function foo(){
  var args = arguments;
  if(args.length === 1){
    return args[0];
  }else if(args.length === 2){
    return args[0] + args[1];
  }
}
```

