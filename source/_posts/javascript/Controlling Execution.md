---
title: 控制语句
tags: js
categories: Javascript
abbrlink: 15838
date: 2018-02-05 15:56:01
thumbnail: /img/javascript/Control.jpg
---

<!-- more -->

# Controlling Execution

> 所有生物都会去控制自己的行为。程序也必须在执行的过程中控制它的过程。程序在执行的时候要么在顺序执行要么跳转。因此选择合适的控制语句来帮助程序做出正确的判断。控制语句包含了 **判断** 、**循环遍历** 一系列简单的命令。



## 流程控制

### if 

 在逻辑运算中已经使用到了 *if* 语句

```js
/*
  if( boolean ) statement1 else statement2
  大括号可以省略不写 看情况
  boolean的含义不仅仅是简单的true or false 
  false 0 NaN undefined null 空字符"" 在判断中都代表假
*/
if(10 > 5)
  alert("max:" + 10);
else
  alert("min:" + 5)

if(condition1)
  // statement1
else if(condition2)
  // statement2
else
  // statement3
```



### switch

*switch* 与 *if* 有点相似

```js
/*
	注意点：
		1. 每一条case加break，除非是要合并结果可以不加
		2. value不会发生隐式转换
*/
switch(表达式){
  case value1:
   	statement1
  break;
  case value2:
   	statement2
  break;
  case value3:
   	statement3
  break; 
}
```



## 循环

> 所谓循环就是多次重复执行某些类似的操作。这个操作一般不是完全一样的操作，而是类似的操作。

### for

```js
for（var i = 0; i < 3; i++）{
  // statement
};

var i = 0;
for（; i < 3; i++）{
  // statement
};

var i;
for(i = 0; i < 3; i++){
  // statement
}

var i = 0;
for(; i < 3; i+=1){
  // statement
}

var i = 0;
for(; i < 3; ){
  i+=1
  // statement
}
// 和上一种有区别

for(;;){
  // statement 死循环
}

// 在循环体外 访问console.log(i) 是可以找到的
```

循环的写法有很多种，同样可以省略大括号。大家对循环的认识可能只是简单的遍历某一个内容，其实循环的使用是很广泛的是一类问题处理的总结。需要细细的去体会



### while

> while是循环的一种变体

```js
/*
  while(expression){
    // statement
  }
  expression 条件为假终止循环体 条件为假的情况在if中已经提到
  类似的场景下 满足while的这个特性都可以使用while循环
*/
var i = 0;
while(i<3){
  // statement
  i++; // i+=1;
}
```



### do while

> 后测试语句，优先执行statement语句后进行判断

```js
do{
  // statement
}while(expression);
// 至少会执行一次statement 如果有这种情况使用do...while的情况会比较多
```



### for in

> 用于枚举对象属性、数组每一项

```js
// 或者是数组 虽然不推荐用来迭代数组但是有效果 这个后面再看
for(var k in obj){
  // statement  k 属性
  console.log(k +'---'+ obj[k]);
}

// for in 可以结合if语句使用 让一个语句更加清晰 只是这种写法并没有被ECMA视为规范
// 但是在平时的书写中还是可以让程序很清晰 便于阅读
var arr = [1,2,3,4];
for(var k in arr) if(k > 1){
  console.log(arr[k]); // 2, 3, 4
}
```



### for of

> 可迭代对象（泛指）IE对此迭代不兼容

```js
for (variable of iterable) {
  //statements
}

var iterable = [1,2,3];
for(var value of iterable){
  console.log(value); // 1, 2, 3
}
// 同样 for of也可以结合if
for(var value of iterable) if (value > 1) console.log(value);
```



## break & continue

break 用于终止当前循环，并且跳出整个循环体

continue 用于结束当前语句，继续执行下一次循环迭代。

都是可选语句。



## last

虽然循环看起来只是重复执行一些类似的操作而已，但它其实是计算机程序解决问题的一种基本思维方式，计算机程序可以发挥出强大的能力，重复的工作都可以使用这种思想去解决，或者同一类相似的任务。更加应该明白的是for循环的执行过程以及与控制语句`break` `contiune` 的结合。

