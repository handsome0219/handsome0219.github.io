---
title: DOM 节点操作
tags:
  - JS
  - DOM
categories: 
    - JavaScript
    - DOM
abbrlink: 52358
date: 2018-03-03 22:27:55
comments: false
---

### DOM 操作 - cold

dom的操作有很多在平时操作中很少用到的，但是却十分好用的API。

#### insert

与 *insertBefore* 类似的几个 *API* 查看 MDN开发者手册，在这里赘述一下。

```
改方法将一个给定的元素节点插入到相对于被调用的元素的给定的一个位置。
1. node.insertAdjacentElement(position, element)

将指定的文本解析为HTML或XML，并将结果节点插入到DOM树中的指定位置。它不会重新解析它正在使用的元素，因此它不会破坏元素内的现有元素。这避免了额外的序列化步骤，使其比直接innerHTML操作更快。
如果只是为了插入文本内容(而不是HTML节点), 不建议使用这个方法, 建议使用node.textContent 或者 node.insertAdjacentText() . 因为这样不需要经过HTML解释器的转换, 性能会好一点.
看到这想说为什么不用innerHTML innerHTML不属于标准DOM 所以并没有被推荐，但是innerHTML的性能也是很好
2. node.insertAdjacentHTML(position, element)

将一个给定的文本节点插入在相对于被调用的元素给定的位置。
3. node.insertAdjacentText(position, element)

参数示意 三个方法都具有四个位置
position
<!-- beforebegin -->
<p>
    <!-- afterbegin -->
    foo
    <!-- beforeend -->
</p>
<!-- afterend -->
```

*example*

```html
<div id="box">
    <div class="line"></div>
</div>
```

```js
var boxDom = document.getElementById("box");
var pDom1 = document.createElement("p");
pDom1.insertAdjacentText('afterbegin', "afterbegin");
var pDom2 = document.createElement("p");
pDom2.insertAdjacentText('beforeend', "beforeend");
boxDom.insertAdjacentElement('afterbegin', pDom1);
boxDom.insertAdjacentElement('beforeend', pDom2);
/*
	change
	<div id="box">
        <p>afterbegin</p>
        <div class="line"></div>
        <p>beforeend</p>
    </div>
*/
```

*example*

```html
<style>
	div {width: 50px; height: 50px; margin: 3px; border: 3px solid black; display: inline-block; background-color: red; float: left}
    section{overflow: hidden;}
</style>
<p>点击彩色框选择它，然后使用下面的前两个按钮在选择之前和之后插入元素。</p>
<section>
    <div></div><div></div><div></div><div></div>
</section>

<button class="before">Insert before</button>
<button class="after">Insert after</button>
<button class="reset">Reset demo</button>
```

```js
var beforeBtn = document.querySelector('.before');
var afterBtn = document.querySelector('.after');
var resetBtn = document.querySelector('.reset');
var container = document.querySelector('section');
var activeElem;
resetBtn.addEventListener('click', function() {
    while (container.firstChild) {
        container.removeChild(container.firstChild);
    }
    for(i = 0; i <=3; i++) {
      var tempDiv = document.createElement('div');
      container.appendChild(tempDiv);
      setListener(tempDiv);
    }
});

beforeBtn.addEventListener('click', function() {
    var tempDiv = document.createElement('div');
    tempDiv.style.backgroundColor = randomColor();
    activeElem.insertAdjacentElement('beforebegin',tempDiv);
    setListener(tempDiv);
});

afterBtn.addEventListener('click', function() {
    var tempDiv = document.createElement('div');
    tempDiv.style.backgroundColor = randomColor();
    activeElem.insertAdjacentElement('afterend',tempDiv);
    setListener(tempDiv);
});

function setListener(elem) {
    elem.addEventListener('click', function() {
        var allElems = document.querySelectorAll('section div');
        for(i = 0; i < allElems.length; i++) {
            allElems[i].style.border = '3px solid black';
        }
        elem.style.border = '3px solid aqua';
        activeElem = elem;
    })
};

function randomColor() {
    function random() {
      var result = Math.floor(Math.random() * 255);
      return result;
    }
    return 'rgb(' + random() + ',' + random() + ',' + random() + ')';
}
function init() {
    var initElems = document.querySelectorAll('section div');
    for(i = 0; i < initElems.length; i++) {
      setListener(initElems[i]);
    }
};
init();
```



#### scrollIntoView

- MDN官网注解

  scrollIntoView：让当前的元素滚动到浏览器窗口的可视区域内。

- ​参数 （*boolean*  |  *object*）

  boolean

  * 如果为`true`，元素的顶端将和其所在滚动区的可视区域的顶端对齐。
  * 如果为`false`， 元素的底端将和其所在滚动区的可视区域的底端对齐。

  object 

  * {block: 'start'} 对应boolean `true`
  * {block:'end'} 对应boolean ` false`
  * object中还有一个`key` behavior 三种值 auto、instant、smooth 其中 smooth 支持动画效果 obj.scrollIntoView( {block: true, behavior: 'smooth'} )

```html
<style>
#up,#down{width: 120px;height: 30px;text-align: center;line-height: 30px;position: fixed;right: 10px;top: 330px;background: red;color: #fff;}
#down{top: 380px}
#wrap{height: 20vh;background: skyblue;margin-top: 100vh;}
</style>
<div id="wrap"></div>
<div id="up">up</div>
<div id="down">down</div>
```

```js
var wrap = document.getElementById("wrap");
var up = document.getElementById("up");
var down = document.getElementById("down");
up.addEventListener('click',function(){
    // wrap.scrollIntoView(true)
    wrap.scrollIntoView({
        block: 'start',
        behavior: 'smooth'
    });
});

down.addEventListener('click',function(){
    // wrap.scrollIntoView(false)
    wrap.scrollIntoView({
        block: 'end',
        behavior: 'smooth'
    });
});
```

 ####  scrollIntoViewIfNeeded

- MDN官方注解

  用来将不在浏览器窗口的可见区域内的元素滚动到浏览器窗口的可见区域。 如果该元素已经在浏览器窗口的可见区域内，则不会发生滚动。 此方法是标准的 Element.scrollIntoView() 方法的专有变体。

  *PS*：MDN官方说一个说明 这个是非标注的一个方法。其中有些小问题

- 参数 （*booean*）

  true   则元素将在其所在滚动区的可视区域中居中对齐。

  false   可视区顶部，或者底部

- 补充说明

  scrollIntoViewIfNeeded是比较懒散的，如果元素在可视区域，那么调用它的时候，页面是不会发生滚动的其次是scrollIntoViewIfNeeded只有Boolean型参数，没有动画。

  不是全部可见或者全部不可见的情况下，调用scrollIntoViewIfNeeded时，无论参数是`true` 还是`false` ，都会发生滚动而且效果是滚动到元素与可视区域顶部或底部对齐，元素离哪端更近，往那边靠。

  *PS*：scrollIntoViewIfNeeded 完全不可见的情况下 元素距离那边近 `true`，`false` 效果都一样就近原则

  移动端，以及原生的使用页面滚动的时候采用jQuery动画操作，现在的原生API都这么好用。当然如果不考虑兼容问题（IE 移动端的UC浏览器不支持）这个是不错的选择

```html
<style>
    #up,#center,#down{width: 120px;height: 30px;text-align: center;line-height: 30px;position: fixed;right: 10px;top: 330px;background: red;color: #fff;}
    #center{top: 380px}
    #down{top: 430px}
    #wrap{height: 20vh;background: skyblue;margin-top: 100vh;}
</style>
<div id="wrap"></div>
<div id="up">up</div>
<div id="center">center</div>
<div id="down">down</div>
```

```js
var wrap = document.getElementById("wrap");
var up = document.getElementById("up");
var down = document.getElementById("down");
up.addEventListener('click',function(){
    wrap.scrollIntoView(true);
});

center.addEventListener('click',function(){
    wrap.scrollIntoViewIfNeeded(true);
});

down.addEventListener('click',function(){
    wrap.scrollIntoViewIfNeeded(false);
});
```

