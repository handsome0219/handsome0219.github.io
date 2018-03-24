---
title: Event
tags:
  - JS
  - event
categories: 
    - JavaScript
    - Event
date: 2018-03-12 13:45:54
comments: false
---

## delegate 事件委托

事件中一个很重要的概念 *事件委托* ,对于事件处理程序过多,绑定过多的重复事件损耗性能.事件委托其实就是指定一个处理程序,管理某一类型所有的事件.利用了事件冒泡的原理.

在event事件对象中两个属性`currentTarget`  `target` ,分别为当前事件对象、目标事件对象.

```html
<div id="box">box</div>
<div id="wrap"><a>click</a></div>
<script>
    box.onclick = function(e){
        console.log(e.currentTarget === e.target);  // true
    }
    wrap.onclick = function(e){
        console.log(e.currentTarget === e.target);  
        // 如果点级的是 a   ---> false
        // 如果点击的是 div ---> true
    }
</script>
```

 如果wrap中有很多a标签都需要设置点击一下改变字体样式,肯定会写很多的重复的点击事件,利用委托就很容易做到,减少DOM添加事件次数

```js
wrap.onclick = function(e){
    if(e.target.tagName === 'A'){
        // 代码
    }
}
```

过去当不知道利用事件委托的时候,有一个场景出错率很大.简单的说明一下

```html
<button id="add">add p element</button>
<div id="box">
    <p>1</p>
    <p>2</p>
    <p>3</p>
    <p>4</p>
</div>
<script>
	var pDoms = document.querySelectorAll('#box p');
    var boxDom = document.getElementById("box");
    var btnDom = document.getElementById("add");
    for(var i=0;i<pDoms.length;i++){
        pDoms[i].onclick = function(){
            alert(this.innerHTML);
        }
    }
    btnDom.onclick = function(){
        boxDom.innerHTML += '<p>'+Math.random()+'</p>';
    }
    
    // 没有去点击add时, 之前绑定的事件都存在.当点击add之后前边的事件全丢失了.这个是innerHTM去添加节点出现的问题,点击添加放上去的节点已非原来的节点了.
    // 解决方式1: 创建节点并追加, 这样子可以保留原来的
    btnDom.onclick = function(){
        var p = document.createElement("p");
        p.innerText = Math.random();
 		p.onclick = function(){ alert(this.innerHTML)}
        boxDom.append(p);
    }
    
    // 方式2: 把事件绑定到标签中 原来的事件自然也要绑定到标签中
    btnDom.onclick = function(){
        boxDom.innerHTML += '<p onclick="fn(this)"></p>';
    }
    // 方式3: innerHTML添加之后在重新绑定一次事件 麻烦到家
    // 方式4: 委托
    boxDom.onclick = function(e){
        if(e.target.tagName === 'P'){
            alert(e.target.innerHTML);
        }
    }
</script>
```

需要说明一下,这里绑定事件中使用事件监听的方式当然是可以的.在jq的使用中如果不知道使用 `on` 绑定并且使用了方式3,那么就会出现重复绑定事件的情况了.需要注意一下.



## 事件触发器

dispatchEvent是作为高级浏览器(如chrome、Firfox等)的事件触发器来使用的，那么什么是事件触发器？就是触发事件的东西。可能有人觉得有点莫名其妙，触发事件不是在交互中被触发的吗？的确，通常情况下，事件的触发都是由用户的行为如点击、刷新等操作实现，但是，其实有的情况下，事件的触发也可以由程序来实现，比如ajax框架的一些自定义事件。和正常事件的绑定一样。对于浏览器而言，绑定事件分为高级浏览器和IE浏览器两派，事件触发器也是分为,高级浏览器和IE两派。

在非ie中可以使用 `createEvent` 创建event对象，接受一个参数表示要创建的事件类型

* UIEvents：一般化的 UI 事件。 鼠标事件和键盘事件都继承自 UI 事件。 DOM3 级中是 UIEvent。
* MouseEvents：一般化的鼠标事件。 DOM3 级中是 MouseEvent。 
* MutationEvents：一般化的 DOM 变动事件。 DOM3 级中是 MutationEvent。
* HTMLEvents：一般化的 HTML 事件。没有对应的 DOM3 级事件 

```js
// 非ie ie9+
// 创建事件对象实例
var haha = document.createEvent('HTMLEvents');  // UIEvents
// 初始化参数 事件名 是否冒泡 是否阻止默认事件
haha.initEvent('haha', true, true);
// 添加一些属性
haha.name = '老狗'；
haha.age = 3;
// 给doucment绑定一下
document.addEvenetListener('haha', function(e){
    alert(e.name +":"+ e.age);
})
// 怎么触发呢？没见过这个事件啊
document.dispatchEvent(haha);
```

```js
// ie 不能自定义事件
var haha = document.createEventObject();
haha.name = '小狗';
haha.age = 3;
document.attachEvent('onclick', function(e){
    alert(e.name +":"+ e.age);
})
document.fireEvent('onclick',haha);
```

通常用来做一些自定义的事件，模拟一些事件。使用jq对应 `trigger` 



