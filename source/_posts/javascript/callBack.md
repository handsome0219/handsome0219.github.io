---
title: callBack
tags:
  - js
  - function
categories: Javascript
abbrlink: 10115
date: 2018-02-07 16:36:52
thumbnail: /img/javascript/callBack.jpg
---

<!-- more -->

> callBack的存在在javascript中随处可见在没有 *promise* 的年代各种流行的框架随处可见包括原生DOM事件，jQuery中通过callBack可以做很多很多事情。

## callBack?

首先看一下 google 与 百度 的释义

> A callback is a function that is passed as an argument to another function and is executed after its parent function has completed.
>
> 译：回调是一个函数，作为参数传递给另一个函数，并在其父函数完成后执行。

> 通过 **函数指针** 调用的函数。如果你把函数的 **指针**（地址）作为 **参数传递** 给另一个函数，当这个指针被用为调用它所指向的函数时，我们就说这是回调函数。回调函数不是由该函数的实现方直接调用，而是在特定的事件或条件发生时由另外的一方调用的，用于对该事件或条件进行响应。

看一下官方的说法之后并且对语言特性的理解，其实就很容易明白两句话中的一个点 **参数传递**。如果对c或者c++学习的人可能就明白下面百度的释义。毕竟 *javascript* 是 *c++* 开发出来的。

先来看例子

```js
$("#box").click(function(){
    // statement
});
obj.addEventListener('click', function(){
    // statement
})
```

在 *javascript* 中很多地方都可以看到这样的执行方式。它们有一个名字叫做回调函数（callBack）。



## 为什么出现callBack

首先说一下浏览器内部包含 `js解析引擎` `渲染引擎` `http请求` `event事件` `webAPI` ，常驻线程前三个。

js开始设计的时候是一个单线程的，这就省了很多复杂的操作比如线程间的通信，所以浏览器在处理js代码的时候需要一件件去做，产生列队（*这里不讨论浏览器怎么去处理事件列队的*）。经常见到的场景就是用户发起一个请求，等到资源加载完毕执行一些用户的操作，请求的过程大多是一个异步的过程，试想一下如果在发起请求的时候浏览器把当前页面锁住了，是不是很蛋疼？

```js
// 异步请求 success执行 statement语句
$.ajax({
  success: function(){
     // statement
  } 
})
```

也就是说有两个任务A、B在执行的时候需要先执行A等到一定的结果之后在去执行B的时候就必须使用这样的方式

*example*

```js
function A(){
    console.log("hello");
    if(expression){
        B();
    };
}
function B(){
  console.log("world");
}
A();
```

那么问题来了如果A任务完成之后，我们的需求并不是执行B这个任务的时候想换成任务C是不是就需要在去添加一个C任务，等待我们去选择。如果这样子的话那真的要累死了。因为你永远不知道下一次变化是什么。打一个不恰当的比方就好比你女票突然生气但是很多男孩子基本都不知道怎么了，基本都是临场应变，如果是一个有经验的人可能有一些预备措施，看似好像这个男孩子很聪明，如果下次女孩子生气你没有预料到不就惨了。反映到程序上就是，开发者不需要管任务A执行完成，B任务是什么。

*example*

```js
function girl(boy){
    if(expression){
        （boy && typeof boy === 'function'）&& boy();
    }
}
girl(function(){
   // 想干嘛干嘛我都接住
})
```

讲函数做为参数传递是很多语言都具有的能力，js中函数也是一个数据类型所以理所应当可以传递只是区别与其它的数据类型的地方在于**函数可以执行**



## 问题

> 事物都有两面性，当然有好处就有坏处。

*example*

```js
a(function (resultA) {
    b(resultA, function (resultB) {
        c(resultB, function (resultC) {
            d(resultC, function (resultD) {
                e(resultD, function (resultE) {
                    f(resultE, function (resultF) {
                    // 子子孙孙无穷尽也
                    console.log(resultF);
                    });
                });
            });
        });
    });
});
```

现在有很多任务a、b、c、d、e、f……都要依赖上一个任务的结果做为执行的条件。那么就进入了一个无限去嵌套的过程，现在看似很轻松很容易读，但是加上一系列的逻辑判断之后，面目全非……。经过很多年的发展 *jQuery* 以及很多的框架都在努力去解决这个问题，于是 *promise* 诞生了。暂且不谈。只是想告诉大家回调虽好但是在使用的过程中也要注意避免出现这样的代码，首先不便阅读，在者就不易维护与修改。

回调的好处可以简单的总结一下了

* 更加的灵活去处理需求
* DOM事件处理监听
* setTimeout与setInterval中调用得到结果并且返回，setTimeout延迟0的挂起作用
* callBack是一个可以被执行一次或者多次，看情况例如Array中的迭代方法