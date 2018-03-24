---
title: 操作符
tags: JS
categories: JavaScript
abbrlink: 63470
date: 2018-01-04 20:09:44
comments: false
---

![Operators](/img/javascript/Operators.jpg)

<!--more-->



# Operators

> javascript中的操作符 算术运算符 赋值运算符 关系运算符 逻辑运算符 位运算符 逗号运算符

这里我们着重看一下关系运算符 逻辑运算符 位运算符 逗号运算符



## 关系运算符

js的关系操作符需要 **注意** 几个点 

如果比较的是数值, 则执行数值比较

```js
var str = "10";
var num = 11;
console.log(str < num); // true

var str = "100";
var num = 2;
console.log(str > num); // true
```

如果比较的是两个字符, 则比较两个字符对应的编码ASCII值

```js
var str = "100";
var num = "2";
console.log(str > num); // false 比较第一位 如果相等比较第二位

var result = "mike" < "haha";
console.log(result); 	// false
```

如果比较的是布尔值, 则先将其转换为数值

```js
var boolean = true; 	// Number(true) 为 1
var num = 2;
console.log(boolean < num); // true
```

来看几个特殊的

```js
var result = "x" < 10; 			// false
var result = undefined == null; // true
var result = undefined === null;// false
var result = null == 0;			// false
var result = null >= 0; 		// true
var result = NaN == NaN; 		// false
var result = [] == ![]; 		// true
```

"x"字符不能被合理的转化成数字即Number("x") = NaN, 然而NaN和任何操作数比较都返回false, 毕竟NaN == NaN都返回false, 我连我自己都不认识

undefined与null, 两个都表示**无**, 转成数值undefined为NaN, null为0 当进行两个等号判断返回true, 三个等号返回false, 但是在某些特定情况下, 例如定义变量未初始化, 对象扩展属性未赋值, 函数默认返回undefined在这些情况下null是无法替代的, 因为null会被转换成0可能造成运算错误.这个特性记住就好了.

null与0直接判断==操作时返回false, 惊不惊喜意不意外, null的数据类型是object, **如果比较的是对象, 则调用valueOf()方法或者toLocaleString()方法返回的结果去比较**, 但是null很特殊没有这些方法, null做为原型链的尽头,所以null == 0不会做特殊处理 返回false. null >= 0比较时这个问题在ECMA规定中>,=存在运算优先级的问题被分为了两部分,需要注意的是并不是>和=判断的结合,这里只是当 null >= 0判断是会转换成Number(null) >= 0, 记住就好.

在关系判断中会存在很多的数据类型的转化, 抓住本质才好哟. 

```js
console.log(1 + 1 + "1"); 	// 111
console.log(1 + + "1" + 1); // 3
// 相当于
console.log(1 + (+"1") + 1); // 会被隐式转换成number
```





## 逻辑运算符

`&&` and `||`

```js
var num = 20;
if( 10< num < 19){
  console.log(1);
}else{
  console.log(2);
}
// 1  因为 10 < 20 < 19 是依次从左向右执行 true < 19 true被转换成1 < 19 返回true 打印1 这也就是为什么必须要有 && 操作符 应当改写成 num > 10 && num < 19
```

并且操作符其它用法

```js
function fn(){
  console.log(1);
};
(2>1) && fn(); 		// 1

var num = 10 && 5;  // 5
// && 碰到假才停止 只要前后表达式都是真就一直向后运算 那么|| 与之相反 碰到真就停止 也就是常说的惰性赋值

var num = 0 || 1; 	// 1
```

当一个表达式中并且, 或者同时出现, 并且优先级高于或者 适当的添加括号去进行合理的判断



## 位运算符

> 一个字节的大小 8个二进制位, 有符号的整数占4个字节 32个二进制位, 其中31位用于表示整数的值, 最高位为符号位

在学习位运算符首先我们需要清楚计算机存储数字的是怎么样的表现形式. 

位运算符包括 右移(>>)、 左移(<<)、 无符号右移(>>>)、 按位或(|)、 按位与(&)、 按位异或(^)、按位非(~)

* 右移、左移

  ​				![binary](/img/javascript/Binary.png)

  ​	![binary](/img/javascript/Binary01.png) ![binary](/img/javascript/Binary02.png)

```js
console.log(8 << 1); 	// 16   10000 = 2^4*1 + 2^3*0 + 2^2*0 + 2^1*0 + 2^0*0 = 16
console.log(8 >> 2); 	// 2  	00010 = 2^1*1 + 2^0*0
```

简单记左移乘以2, 右移除以2, 无符号右移和右移的区别. 对于正数没有什么区别, 因为每次移动都会在最高位补符号位, 负数是正数的二进制补码直接右移之后会被当成正数的二进制, 会产生很大的误差.

* 按位与、按位或

  ​		![binary](/img/javascript/Binary03.png) ![binary](/img/javascript/Binary04.png)

  ```js
  console.log(10 & 15); // 10
  console.log(10 | 15); // 15
  ```

  原理和逻辑运算符中一致，&运算同1返回1， |运算同0返回0， 在哪里使用？其实当我们想要返回一个数字的有效位就要用到&运算了，可以尝试写一个进制转换的函数。运用一下, 另外在对一个数字取整时可以使用位或0

  ```js
  console.log(Math.floor(6.19));
  console.log(parseInt(6.19));
  console.log(6.19 | 0); // 6  因为二进制是没有小数的
  ```

  ​

* 按位异或

![binary](/img/javascript/Binary05.png)

```js
console.log(10 ^ 7); // 13
```

看图不做解释了， 这里说一下按位异或的用法。当对一个数字连续位异或同一个数字的时候会返回原来的数字也就是说, 可以用来交换两个变量的值，或者toggle一个变量，或者检测两个数正负是否一致

```js
console.log(10 ^ 7 ^ 7); // 10
var a = 10;
var b = 20;
a = a ^ b;
b = a ^ b; // a ^ b ^ b = a
a = a ^ b; // a ^ a ^ b = b

// toggle变量
if (x === a) {
  x = b
} else if (x === b) {
  x = a
}
x = a ^ b ^ x;

// 检测正负
if ((x ^ y) >= 0) {
  // ...
}
```

* 按位非

把二进制的每一位取反就是 `~` ,一个整数的负数逐位取反加一. 这里就不介绍二进制补码的运算原理了可以自行查阅资料.

```js
var num = 8;
console.log(~num+1); // -8

// 取整一个整数
console.log(~~8.5); // 8
```



## 逗号运算符

```js
var a = 10, b = 20, c = 30;
var num = (1,2,3);
var arr = ([1,2],[3,4]);
```

逗号操作符可以用来声明多个变量，用于赋值的时候总是返回表达式最后一项。利用好这个特性可以很方便我们简化代码。