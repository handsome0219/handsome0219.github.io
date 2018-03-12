---
title: event target
date: 2018-03-09 18:40:18
tags: [js,event]
categories: Javascript
---

## 事件

页面与用户之间的交互是通过事件完成的。事件是用户自身的操作或者浏览器自身的一些动作。比如常见的用户的点击，浏览器load事件。在事件发生的过程中会记录一些用户的操作信息或者浏览器的动作信息。学习事件需要了解事件的机制（冒泡、捕获），不同浏览器对dom事件的写法以及delegate，onload、domReady等等。

DOM处理事件的方式，可以写在标签中执行。也可以通过js添加属性事件例如 `onclick`，或者事件监听方式

```
<div onclick = "fn()"></div>
divDom.onclick = fn;
divDom.addEventListener('click', fn);
```



### 事件流

页面上执行事件时候有一个顺序

![event](/img/javascript/event.png)

DOM流中当点击了一个div，实际上也相当于点击了body、html、window。在非IE给dom添加监听事件的方法最后一个参数代表是否捕获，默认不捕获。还是参照上边的图做一个简单的说明

```js
divDom.addEventListener('click', function(){ console.log('div1')}, false)
document.body.addEventListener('click', function(){ console.log('body')}, false)
document.documentElement.addEventListener('click', function(){ console.log('document')}, false)
window.addEventListener('click', function(){ console.log('window')}, false)

divDom.addEventListener('click', function(){ console.log('div2')}, true)
document.body.addEventListener('click', function(){ console.log('body')}, true)
document.documentElement.addEventListener('click', function(){ console.log('document')}, true)
window.addEventListener('click', function(){ console.log('window')}, true)
```

当点击了 div 会优先触发父元素的捕获事件依次向子元素执行，其次在从子元素依次执行不捕获事件（冒泡阶段）。针对事件源，哪个事件写在前优先执行哪个事件。

阻止冒泡。有时候可能我们不需要事件产生冒泡，这个时候就要断绝父子关系了js中有两种方式。但是冒泡也是有好处的，不要觉得冒泡不好例如 *delegate* 就是利用的冒泡。

```js
event.cancelBubble = true;   // 非标准  无兼容问题 推荐使用（真的是比另外一个好记多了）
event.stopPropagation();     // 标准
```



### 事件监听

IE浏览器的监听方式和其它浏览器不同 `attachEvent` `addEventListener` 两种。

```js
el.attachEvent(type, fn)
el.addEventListener(type, fn, boolean);
```

两者的 *type* 有一点区别 IE的事件和属性添加的一样 `on+type` ，跨浏览器事件处理

```js
var eventHandle = function(obj, type, fn){
    if(obj.addEventListener){
        obj.addEventListener(type, fn, false);
    }else if(obj.attachEvent){
        obj.attachEvent('on'+type, fn);
    }else{
        obj[on+'type'] = fn;
    }
}
```

虽然可以跨浏览器使用这个方法。但是有一个问题，IE中绑定事件的传入的fn，`this` 指向不是当前的 *obj* 。

```html
<div id="box">点我</div>
<script>
	function fn(){alert(this.innerText)};
    box.attachEvent('onclick', fn);  // undefined
</script>
```

嫌弃IE可能这一点占了很大一部分，可以通过 `call` `apply` `bind` 来改变 `this` 的指向。但是低版本的IE不支持`bind` 绑定。修改一下

```js
var eventHandle = function(obj, type, fn){
    if(obj.addEventListener){
        obj.addEventListener(type, function(){
            fn.apply(obj, arguments)
        }, false);
    }else if(obj.attachEvent){
        obj.attachEvent('on'+type, function(){
            fn.apply(obj, arguments);
        });
    }else{
        obj[on+'type'] = fn;
    }
};
```

很好，但是有点觉得这个东西有点重复把fn在提取出来

```js
var eventHandle = function(obj, type, fn){
    var handle = function(){
        fn.apply(obj, arguments)
    }
    if(obj.addEventListener){
        obj.addEventListener(type, handle, false);
    }else if(obj.attachEvent){
        obj.attachEvent('on'+type, handle);
    }else{
        obj[on+'type'] = fn;
    }
    return handle
};
```

基本上没有问题， 事件绑定之后可能回去解除绑定的函数，这个写法找不到对应的函数，需要把这个函数return出去。所以重新写一个事件处理函数添加解除绑定的方法。为了形成一个完整的处理函数，里边的每一个功能分开

```js
var eventHandle = function(){
    var fire = function(el, type, fn){
        var handle = function(){
            fn.apply(obj, arguments)
        }
        if(obj.addEventListener){
            obj.addEventListener(type, handle, false);
        }else if(obj.attachEvent){
            obj.attachEvent('on'+type, handle);
        }else{
            obj['on'+ type] = fn;
        }
        return handle;
    }
    
    var remove = function(el, type, handle){
        if(obj.removeEventListener){
            obj.removeEventListener(type, handle, false);
        }else if(obj.deatchEvent){
            obj.deatchEvent('on'+type, handle);
        }else{
            obj[on+'type'] = null;
        }
    }
    return {
        fire: fire,
        remove: remove
    }
}();

// test
var handle = eventHandle.fire(boxDom, 'click', function(){
    alert(this.innerText);
})
eventHandle.remove(boxDom, 'click', handle);
```

```js
~function($){
    if(document.addEventListener){
        $.addEvent = function(el,type,callBack){
            el.addEventListener(type,callBack,false);
            el[type+'callBack'] = el[type+'callBack'] || [];
            el[type+'callBack'].push(callBack);
            return callBack;
        }
        $.off = function(el,type,fn){
            if(typeof fn != 'undefined'){
                el.removeEventListener(type,fn,false);
            }else{
                el[type+'callBack'].forEach(function(item){
                    el.removeEventListener(type,item);
                })
            }
        };
    }else{
        $.attachEvent = function(el,type,callBack){
            var bound = function(){
                return fn.apply(el,arguments);
            }
            el.attachEvent('on'+type,bound);
            return bound;
        };
        $.off = function(el,type,fn){
            el.detachEvent('on'+type,fn);
        };
    };
}(window);
```

另外尝试，因为事件监听的方式不会产生覆盖，如果想一次性清除所有的相同类型的事件可以采用上边的写法。都是简单的尝试，更多的细节也可以去查看一下jq的源码，其中对事件的处理方式很多东西值得学习。



### 事件对象

在DOM上触发某个事件的时候会产生一个事件对象，用来记录与这个事件相关信息。非IE中这个事件对象存在于绑定函数的参数中，IE则不能直接使用这个参数而是通过访问window对象底下的event对象。

```js
el.onclick = function(e){
	e = e || window.event;
    alert(e);
};
```

阻止冒泡也是通过事件对象底下属性和方法去做到的。事件对象还能做什么呢？

> 鼠标位置可以用来做很多事情。IE与非IE鼠标位置的获取不同 `clientX/Y` `pageX/Y` 

区别：pageX/Y 不兼容ie9-。获取的值clientX/Y获取到可视区的距离，pageX/Y获取到浏览器的绝对距离，但是如果要在IE下获取到距离浏览器的绝对距离就需要手写了。

```js
function posXY(e){
    e = e || event;
    var pos = {x:0, y:0};
    if(e.pageX){
        pos.x = e.pageX;
        pos.y = e.pageY;
    }else{
        pos.x = e.clientX + document.documentElement.scrollLeft;
        pos.y = e.clientY + document.documentElement.scrollTop;
    }
    return pos;
}
```

> 阻止默认事件 

如果调用事件的对象 `cancelable` 的值为true，那就表明存在默认事件。a链接，表单提交，浏览器滚动条，有时候都需要阻止其中的默认事件 w3c的方法是`e.preventDefault()`，IE则是使用 `e.returnValue = false` js中使用`return false` 也可以做到当然只是高级浏览器。

> 键码

玩过游戏的都知道键盘上我们可以控制游戏人物的方向，键盘事件中 `event.keyCode` 用来监听按了哪一个按键，查询对应的键码就知道按下了哪一个键。

```js
document.onkeydown = function(e){
    e = e || event;
    if(e.keyCode == 13){  // 回车键
        // 操作
    }
}
```

> 鼠标按键

```js
// 阻止右键菜单
document.oncontextmenu = function(){
    return false;
}
// 自定义右键菜单  0--左键  1--滚轮  2--右键
document.onmousedown = function(e){
    e = e || event;
    var x = e.pageX;
    var y = e.pageY;
    if(e.button == 2){
        var divDom = document.createElement("div");
        with(divDom.style){
          	width = '100px';
            height = '100px';
            background = 'red';
            position = 'absoulte';
            left = x + 'px';
            top = y + 'px';
        } 
    }
}
```



