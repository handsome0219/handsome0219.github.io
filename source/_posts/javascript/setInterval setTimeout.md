---
title: setInterval setTimeout（一）
tags: JS
categories: JavaScript
abbrlink: 28450
date: 2018-03-06 16:45:54
comments: false
---

## js中的定时器

JavaScript是单线程语言，但它允许通过设置超时值，间歇时间值来调度代码在特定时刻执行。这两个分别是*window* 对象的 `setTimeout` 与 `setInterval` 方法，返回值是一个 `intervalID` 可以传递给 `clearTimeout` 与 `clearInterval` 

### 简单的认识

```js
var intervalID1 = setTimeout(func, delay);
var intervalID2 = setTimeout(func, delay, param...);
var intervalID3 = setInterval(func, delay);
var intervalID4 = setInterval(func, delay, param...);
```

先来看一下定时器的写法

```js
function fn(){ alert(1) };
setTimeout(fn, 1000);
setTimeout("fn()", 1000);  // 不推荐写法 evel
setTimeout(function(){ fn() }, 1000); // 通过回调的方式调用 多了不必要的开销 但是可以去传递参数
```

其中要注意一点

```js
window.onload = function(){
 	setTimeout("fn()", 1000);  // 报错 fn is not defined 如果fn写在全局是可以的 所以这里个人认为可能是eval去解析了 字符fn() 但是fn必须是 window作用域底下的函数 局部作用域会找不到fn 虽然有点牵强但是知道有这个坑就好了
    function fn(){ alert(1) };
}
```

当调用的函数有参数的时候

```js
function fn(a, b){ console.log(a, b) };
setTimeout("fn(1, 2)", 1000);  // 不推荐 eval
setTimeout(fn, 1000, 1, 2);    // IE9-不支持第一种语法中向延迟函数传递额外参数的功能
setTimeout(function(num){console.log(num)}, 1000, 10);
```

**this**

定时器中的this也是一个很容易出错的地方

```js
var obj = {
    a: 10,
    printa(){
       alert(this.a);
    }
}
var a = 20;
obj.printa(); // 10  obj
setTimeout(obj.printa, 1000); // 20  window
```

`setInterval` 与 `setTimeout` 中执行的代码在一个单独的执行上下文中运行到它所调用的函数中。因此，被调用函数的this关键字将被设置为window对象，它不会与调用的函数最初的上下文环境相同

有方式解决嘛? 当然

```js
setTimeout(obj.printa.bind(obj), 1000);  // 通过bind改变
// 因为bind不兼容 可能有人在这里想这样写 setTimeout.call(obj, obj.printa, 1000), 很遗憾报错 Uncaught TypeError: Illegal invocation
// 无法通过call把对象传递给回调函数
```

```js
// 自己动手封装一下好了
function setTime(callBack, delay){
    var content = this,
        args = Array.prototype.slice.call(arguments, 2);
    return setTimeout(function(){
        callBack.apply(content, args);
    }, delay)
} 

// MDN上给出了一中方法是 还是重新写了一遍
var __nativeST__ = window.setTimeout, __nativeSI__ = window.setInterval;
window.setTimeout = function (vCallback, nDelay ) {
    var oThis = this,
        aArgs = Array.prototype.slice.call(arguments, 2);
      return __nativeST__(vCallback instanceof Function ? function () {
            vCallback.apply(oThis, aArgs);
      } : vCallback, nDelay);
};
window.setInterval = function (vCallback, nDelay ) {
    var oThis = this,
        aArgs = Array.prototype.slice.call(arguments, 2);
      return __nativeSI__(vCallback instanceof Function ? function () {
            vCallback.apply(oThis, aArgs);
      } : vCallback, nDelay);
};
```



### 区别

简单的区别两者就是 `setTImeout` 仅会在超时值执行一次，但是这个超时值不一定是我们所设置的那个值。`setInterval` 会间歇性的调用，但是这个间歇时间可能会让程序出现问题。

```js
setTimeout(_=>{ alert(1) }, 500); // 有意思的事情就来了 弹窗并非很快的执行
for(var i=0;i<10e8;i++){}
```

这个是因为window对象底下的定时器属于异步语句，js单线程的执行。当程序自上而下的去执行的时候，优先执行同步代码，浏览器会把异步的语句放入一个 **异步语句列队**(定时器，webAPI，DOM，ajax)，当js主线程的任务执行完毕之后在依次执行异步语句。

怎么去依次执行的异步语句呢？这个就是 `delay` 参数起作用了。延迟时间实际上的意思相当于 *当js主线程的任务完成之后异步列队中的语句推入主线程执行的时间* 

```js
setTimeout(_=>{ console.log(1) }, 500); 
setTimeout(_=>{ console.log(2) }, 100); 
setInterval(_=>{ console.log(3)}, 50)
for(var i=0;i<10e8;i++){}
/*
   3
   2
   1
   3...
*/
```

通过 `setTimeout` 可以模拟 `setInterval` 。更多的可能会用 *out* 来模拟 *interval* 

```js
// 简单的示例
var fn = function(){
    console.log(1);
    setTimeout(fn, 1000)
}
fn();
```

`setTimeout` 会在fn执行从上而下执行完成，在调用下一次定时器。而 `setInterval` 不会等待fn执行完成而是会

到达调用时间间隔就会插入事件列队去执行。所以在做某些场景的时候不要去影响业务就好了。 



### 定时器休眠

浏览器是很智能的程序。当用户切换了当前显示的页面，或者最小化窗口的时候 `setInterval` 会暂时进入休眠状态，但是并不是停止，而是很缓慢，并且在这个时间段的执行语句都会放入一个列队，当用户再次进入页面的时候会一次性全部执行，在很多 *canvas* 的效果中这个现象很普遍，以及在做无缝的轮播的时候可能也会碰到这样子的问题。贴一个链接可以去查看。[Canvas实现会跳舞的时间动画](http://www.html5tricks.com/demo/html5-canvas-dance-time/index.html) 切换到其它页面等一会在切换回去就会发现问题了

```js
var index = 0;
setInterval(function(){
    document.title = ++index;
}, 30)
// 更改title打开页面可以看到快速的变化，切换到其它的页面会发现 title的改变会变得很缓慢。就是 setInterval的休眠机制 很有趣 h5 API另外一个 window.requestAnimationFrame()
function fn(){
    document.title = ++index;
    window.requestAnimationFrame(fn);
}
```

`window.requestAnimationFrame()` 比起 `setInterval` 会更加的稳定因为它会随着浏览器刷新频率的变化而变化做动画会更加的平滑。上边的执行当切换到其它的页面。title会停止改变，也就是说会更加的节约浏览器的开支消耗。这个不是重点。
