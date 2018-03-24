---
title: Object
tags: JS
categories: JavaScript
thumbnail: /img/javascript/Object.jpg
abbrlink: 59346
date: 2018-02-11 15:22:55
comments: false
---

> 多多少少听到过一句话，javascript中一切皆对象。其中有哪些东西是我们需要了解的呢

<!-- more -->

# Object

> ECMAScript中，引用类型是一种结构好比一个类，将数据和功能结合到一起。描述一类对象的属性和方法。

## 定义

js中属于 *Object* 数据类型的很多数组、日期、正则、甚至是函数。创建对象的方式

```js
// new 关键字
var obj = new Object();

// 字面量对象
var obj = {};
```

对象可以有自己的方法和属性，例如可以给obj扩展 *property* 和方法 *method* 

```js
var person = new Object();
person.name = "xq";
person.say = function(){ console.log(this.name) };

// key value形式书写 key可以加引号或者不加
var person = {name: "xq", say: function(){ console.log(this.name) }};
```

访问对象的属性和方法可以采用方括号的语法 `[]` 

```js
console.log(person['name']);
var method = 'say';
person[method]();
// 为什么会有这样的写法呢？
var person = {"e-mail": "youname@mail.com", "first name": "xx"};
person.e-mail;       // error
person.first name    // error
person["e-mail"]     // bingo
person["first name"] // bingo
```

上边访问方式可以看到也可以用一个变量，但是能去点访问就不要写变量，毕竟多一个变量多一份担忧。



## in & hasOwnProperty

> 去检测一个对象中的属性是否存在是非常必要的，比如当我们去合并数据的时候，或者对这个数据做一些筛选的时候

```js
var obj = {
    name: "xx"
}
// in 
console.log("name" in obj);    // true
console.log("valueOf" in obj); // true
// hasOwnProperty
console.log(obj.hasOwnProperty("name"));    // true
console.log(obj.hasOwnProperty("valueOf")); // false
```

检测 `name` 字段都没问题返回 *true* ，检测 `valueOf` 字段结果不同，因为 `valueOf` 是所有对象都具有的也就是继承父类的，*in*是可以检测到的，相反*hasOwnProperty*是不会去检测继承的与在原型扩展的属性只检测**自有属性 **

合并数据

```js
var date1 = {a: 1, b: 2， d: 5};
var date2 = {b: 3, c: 4};
// 将date1合并到date2中 相同的合并
for(var k in date1){
    // 做检测是否是date1的自有属性
    if(date1.hasOwnProperty(k)){
    	date2[k] = date1[k]    
    }
}
console.log(date2); // {a: 1, b: 3, c: 4, d: 5};
console.log(Object.assign({}, date1, date2)); // 原生提供的方法，在不破坏原有对象属性的情况下进行合并 自己也可以写一个合并数据的方法

function mixin(target, source){
    // 不定参
    var args = [].slice.call(arguments);
    if(args.length == 1) return target;
    var index = 1;
    while (source = args[index++]) {
        for(var k in source){
            if(source.hasOwnProperty(k)){
                target[k] = source[k];
            }
        };
    };
    return target;
};

function mixin(target, ...source){
    return source.reduce((a, b) => (
        for(var k in b)if(b.hasOwnProperty(k)){
            a[k] = b[k]
        }
        return a;
    ), target);
    // source.forEach(item => {
    //     for(var k in item)if(item.hasOwnProperty(k)){
    //         target[k] = item[k]
    //     }
    // });
    // return target;
}
```



## delete

> 对象的属性和方法是可以被删除的

```js
var a = 1;
b = 2;
delete b;
console.log(b); // not defined
// 定义在window作用域中的属性和方法相当于对 window对象扩展属性和方法 没有var定义的会被delete删除

var obj = new Object();
obj.num = 10;
console.log(obj.num); // 10
delete obj.num;
console.log(obj.num); // undefined
```



## this

> this是javascript中很头疼的一个东西

this，*call*、*apply*、*bind* 的使用与区别

```js
function foo(){
 console.log(this);
}
foo(); // window
console.log(this); // window
```

定义在 *script* 标签下的全局函数相当于给 *window* 对象添加了一个方法上边的调用还可以写成 window.foo()。this的指向为调用该方法的对象。

```js
var a = 20;
var obj = {
   a: 10,
   say: function(){
      console.log(this.a);
   }
}
obj.say(); // 10
window.obj.say(); // 10
// this的指向都是调用这个方法的对象也就是obj
```

但是当把函数的引用指向一个变量的时候就有点问题了

```js
var fn = obj.say;
fn(); // undefined
window.fn(); // 相当于此 this === window this的指向都是调用它的那个对象
```



**this** 指向的偏移，打一个不恰当的比方就相当于移情别恋了。。。

* *call*

```js
var a = {
   name: '如花',
   say: function(){
     console.log("你只能爱我一人。。。"+ this.name);
   }
}

var b = {
   name: '凤姐'
}
a.say();       // a爱着如花
a.say.call(b); // a变心了爱上了凤姐
```

* apply

```js
a.say.apply(b) // 凤姐
```

`call` `apply` 有什么区别呢？现在a想说点其它的，于是乎

```js
var a = {
   name: '如花',
   say: function(param1, param2){
     console.log(param1 + "你只能爱我一人。。。"+ this.name + parma2);
   }
}

var b = {
   name: '凤姐'
}
a.say('皇天在上', '永远不变');            // 皇天在上你只能爱我一人。。。如花永远不变
a.say.call(b, '皇天在上', '永远不变');    // 皇天在上你只能爱我一人。。。凤姐永远不变 
a.say.apply(b, ['皇天在上', '永远不变']); // 皇天在上你只能爱我一人。。。凤姐永远不变 
```

call() 和 apply()，两个方法可用于调用函数，两个方法的第一个参数必须是对象本身。

apply() 方法调用一个函数, 其具有一个指定的this值，以及作为一个数组（或类似数组的对象）提供的参数。

* bind

*bind* 与 *call* 在传递参数上没什么区别，区别在于 bind()方法**创建**一个新的函数， 并且 *bind* 有兼容问题

```js
a.say.call(b, '皇天在上', '永远不变'); // 说话了
a.say.bind(b, '皇天在上', '永远不变'); // 不说话
// 再次调用
a.say.bind(b, '皇天在上', '永远不变')();
// or
a.say.bind(b)('皇天在上', '永远不变');
```



还有 `new` 也可以改变this指向，构造函数

```js
function Person(name, age){
   var obj = new Object();
   obj.name = name;
   obj.age = age;
   return obj;
}
console.log(Person("xxx", 18));

function Person(name, age){
   this.name = name;
   this.age = age;
   console.log(this);  // Person() this指向window
   return this;
}
console.log(new Person('xxx', 18)); // this指向 Person对象 通过new改变了this的指向

function Person(name, age){
   this.name = name;
   this.age = age;
   console.log(this);  // Person() this指向window
}
console.log(new Person('xxx', 18)); // 构造函数默认返回this 所以可以不写
```



