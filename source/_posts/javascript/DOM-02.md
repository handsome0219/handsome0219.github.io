---
title: DOM 属性和方法
tags:
  - JS
  - DOM
categories: 
    - JavaScript
    - DOM
abbrlink: 60597
date: 2018-02-28 14:00:40
comments: false
---

> DOM （ Document Object Model ）是针对HTML和XML文档的一个API.DOM是一个具有丰富层次的节点树,我们可以对其进行添加、删除、修改. 所以来认识一下DOM中的属性和API以及兼容问题.



## 节点层次

DOM将 *HTML* 与 *XML* 文档绘制成由一个 **根节点** 发散出来的多层节点构成的树状结构（DOM树）。 所以存在很多的节点， 不同的节点在文档中表示的信息也是不同的， 每个节点有自己的特点、属性、方法。节点与节点之间存在一定的关系。直观的看一下w3c中的DOM树

![w3c](http://www.w3school.com.cn/i/ct_htmltree.gif)



## Node 类型

DOM1级定义了一个 *Node* 接口，将DOM中的所有节点类型实现，在JavaScript中做为 *Node* 类。（IE中访问不到，IE使用COM对象去实现的）。所有的节点都继承自 *Node* 类 。打开浏览器可以在开发工具中看到一个列表

![node](/img/javascript/dom/node.png)

每一种节点都有一个 `nodeType` 属性表明节点的类型，这个值用数字表示。常见有

* 元素节点（ELEMENT_NODE）  nodeType = 1
* 属性节点（ATTRIBUTE_NODE）nodeType = 2
* 文本节点（TEXT_NODE）nodeType = 3
* 注释节点（COMMENT_NODE）nodeType = 9
* fragment片段节点（DOCUMENT_FRAGMENT_NODE） nodeType = 11

另外还有nodeName nodeValue属性

| node | nodeType | nodeName | nodeValue |
| ---- | -------- | -------- | --------- |
| 元素 | 1        | 标签名称 | null      |
| 属性 | 2        | 属性名称 | 属性值    |
| 文本 | 3        | \#text   | 文本值    |

### 关系属性

节点与节点之间存在一定的联系，例如 *html* 可以看成是 *body* 的父级，*body* 中的标签就都是 *body* 的子孙。所以每一个节点都有一个 `childNodes` 属性

![node_attr](/img/javascript/dom/node_attr.png)

列举一下常见的节点关系属性

| 属性                   | 描述                                                         |
| ---------------------- | ------------------------------------------------------------ |
| parentNode             | 返回指定的节点在DOM树中的父节点，parentNode不能被append      |
| parentElement          | 返回当前节点的父元素节点,如果该元素没有父节点,或者父节点不是一个元素节点.则 返回 null |
| childNode              | 返回包含指定节点的子节点的集合，该集合为即时更新的集合       |
| children               | 返回 一个Node的子elements 是一个动态更新的 HTMLCollection    |
| firstChild             | 返回树中节点的第一个子节点，如果节点是无子节点，则返回 null  |
| firstElementChild      | 同上 IE9+                                                    |
| lastChild              | 返回树中节点的最后一个子节点，如果节点是无子节点，则返回 null |
| lastElementChild       | 同上 IE9+                                                    |
| nextSibling            | 返回node节点紧跟在其后面的节点，如果指定的节点为最后一个节点，则返回 null |
| nextElementSibling     | 同上 IE9+                                                    |
| previousSibling        | 返回当前节点的前一个兄弟节点,没有则返回 null                 |
| previousElementSibling | 同上 IE9+                                                    |
| ownerDocument          | 返回当前节点的顶层的 document 对象，如果在document 自身上使用此属性，返回null |

*example 1* 

```html
<div id="box">
    <div></div>
    <div></div>
    <div></div>
</div>
```

```js
var boxDom = document.getElementById("box");
boxDom.childNodes.length  			// 7
boxDom.children.length    			// 3
boxDom.firstChild.nodeType 			// 3
boxDom.firstElementChild.nodeType	// 1
```

*example 2*

```html
<div id="box"></div>text
<div></div>
```

```js
var boxDom = document.getElementById("box");
boxDom.nextSibling.nodeType           // 3
boxDom.nextElementSibling.nodeType    // 1
```

*ps* : childNodes 会获取除了元素节点的其它节点 推荐使用 children。firstChild、lastChild、nextSibling、previousSibling同样会获取到非元素节点，推荐使用另外一种但是有兼容问题需要手动做兼容

```js
// 兼容处理
function firstChild(el){
    if(el.firstElementChild){
        return el.firstElementChild;
    }else{
        var f = el.firstChild;
        // 直到找到nodeType是1的停止
        if(f && f.nodeType != 1){
            // f = f.nextSibling;
            f = next(f);
        }
        
        // while(f && f.nodeType != 1){
        //     f = f.nextSibling;
        // }
        return f;
    }
}

function lastChild(el){
    if(el.lastElementChild){
        return el.lastElementChild;
    }else{
        var l = el.lastChild;
        // 直到找到nodeType是1的停止
        if(l && l.nodeType != 1){
            l = prev(l);
        }
        return l;
    };
}

// 另一种思路
function lastChild(el){
    var index = el.children.length-1;
    var l = el.children[index];
    while (l) {
        if(l.nodeType == 1)break;
        l = el.children[index--];
    }
    return l;
}


function next(el){
    if(isElement(el))return;
    if(el.nextElementSibling){
        return el.nextElementSibling;
    }else{
        // ie 678
        var nxt = el.nextSibling;
        if(nxt && nxt.nodeType != 1)nxt = next(nxt);

        // while(nxt.nodeType != 1){
        //     nxt = nxt.nextSibling;
        // }
        return nxt;
    };
};

function prev(el){
    if(!isElement(el))return;
    if(el.previousElementSibling){
        return el.previousElementSibling;
    }else{
        // ie 678
        var pre = el.previousSibling;
        if(pre.nodeType != 1)pre = prev(pre);
        return pre;
    };
};

function siblings(){
    var arr = [];
    var child = el.parentElement.children;
    for(var i=0;i<child.length;i++){
        if(child[i] != el){
            arr.push(child[i])
        }
    };
    return arr;
}

// 获取所有同级元素 不包含自己
function siblings(el){
    var pDom = el.parentNode;
    var child = pDom.children;
    return [].slice.call(child).filter(function(item){
        return item != el;
    });
}

function siblings(el){
    var pDom = el.parentNode;
    var child = [].slice.call(pDom.children);
    if(child.indexOf(el) != -1)child.splice(child.indexOf(el),1);
    return child;
}

function siblings(el){
    var arr = [];
    var prev = el.previousElementSibling;
    while (prev) {
        arr.unshift(prev);
        prev = prev.previousElementSibling;
    };
    // 向下找到所有的
    var next = el.nextElementSibling;
    while (next) {
        arr.push(next);
        next = next.nextElementSibling;
    };
    reutrn arr;
}；


// 获取指定元素中的某子元素
function eq(el, index){
    return el.children[index];
}

// 查找指定元素中的某子元素根据 标签 id class
function find(el, param){
	return [].slice.call(el.children).filter(item=>( item.id == param.slice(1) || item.className.indexOf(param.slice(1)) != -1 || item.tagName.toLowerCase() == param));
}
```

```js
// 把零散的整理起来 定义一个 domUtils对象
var domUtils = {
    first: firstChild,
    last: lastChild,
    next: next,
    prev: prev,
    siblings: siblings,
    eq: eq,
    find: find
}
```

*ps* : 兼容的方法很多可以多多去尝试



### 关于text 的属性

*Node* 节点除了关系属性之外还包含关于 *Node* 自己的一些属性，关于文本的。当然一般都是标签才具有这类属性，会生效。

*tagName*，*localName* 这两个属性返回的都是标签自己独有的名称，后者为小写

*innerText* *innerHTML* *textContent* 区别

* innerText 返回元素的文本内容可读写
* innerHTML 返回元素标签中的内容，可读写（标签可以被解析）innerHTML不属于规范
* textContent IE9+使用 规范DOM 
  * innerText受 CSS 样式的影响，它会触发重排（reflow），但textContent不会
  * textContent通常具有更好的性能，因为文本不会被解析为HTML

*outerText* *outerHTML* 用法和上边一样，区别在于写的时候都会把父元素删除，基本不会使用这个属性



### 关于位置大小的属性

> offsetWidth/Height clientWidth/Height scrollWidth/Height 
>
> offsetParent offsetLeft offsetTop 

元素大小 自动取整round 只读属性

* offsetWidth/Height = width/height + padding + border + scrollbar 
* clientWidth/Height = width/height + padding
* scrollWidth/Height = width/height + padding + 溢出

元素位置

```js
// 获取元素到页面的绝对距离
function offset(el){
	var cleft = 0, ctop = 0;
    do{
        ctop += elem.offsetTop || 0;
        cleft += elem.offsetLeft || 0;
        elem = elem.offsetParent;
    } while (elem);
    return {ctop, cleft}
};

// 获取指定父元素 不指定到页面的绝对距离
function offsetxq(el,parent,position){
    var pos = position || {top:0,left:0};
    if(el == parent)return pos;
    pos.top += el.offsetTop;
    pos.left += el.offsetLeft;
    return offsetxq(el.offsetParent,parent,pos);
};
```

### 关于节点的样式

*style* 属性用来操作节点的 *css* 样式 *className* 属性用来获取节点的类名

*style* 获取的是行内样式，同样设置的也是行内样式

```js
el.style.color = 'red';
el.style.marginLeft = '100px';
```

*cssText* 属性可以一次性写入多个css样式，连字符的样式需要转为驼峰

```js
el.style.cssText += 'width:100px;height:100px;backgroundColor:red';
```

这里用了 `+=` 因为cssText会覆盖原有的样式，这样子为了保存原有的,如果相同被覆盖。

问题来了怎么获取行间样式？

```js
function getStyle(el, attr){
 	return el.currentStyle? el.currentStyle[attr] : window.getComputedStyle(el, null)[attr];
}
```

*currentStyle* 针对IE *getComputedStyle* 针对非IE。后者还可以获取伪元素，如果要获取伪元素（after，before）第二个参数为伪元素。getComputedStyle(el, 'after')  getComputedStyle(el, 'before')

*PS*：上边的关于元素的大小位置的获取与 *getStyle* 获取的区别在于前者会取整不会取到小数，两者都没有px单位。所以使用的过程中如果需要小数则选取后者。

*API* 中有一个方法也是可以获取的 `getBoundingClientRect` 这个方法返回了6个属性

```js
var bound = Element.getBoundingClientRect()
bound.width / bound.height / bound.top / bound.left / bound.right / bound.bottom
```

top属性获取的是 Element距离当前窗口的距离而非绝对距离，可以写一个更加完美的获取元素绝对位置的方法

```js
var getOffsetPosition=function(elem){
    if ( !elem ) return {left:0, top:0};
    var top = 0, left = 0;
    if ( "getBoundingClientRect" in document.documentElement ){
        var box = elem.getBoundingClientRect(),
            doc = elem.ownerDocument,
            body = doc.body,
            docElem = doc.documentElement,
            // clientTop 获取元素顶部边框的宽度
            clientTop = docElem.clientTop || body.clientTop || 0,
            clientLeft = docElem.clientLeft || body.clientLeft || 0,
            top  = box.top  + (self.pageYOffset || docElem && docElem.scrollTop  || body.scrollTop ) - clientTop,
            left = box.left + (self.pageXOffset || docElem && docElem.scrollLeft || body.scrollLeft) - clientLeft;
    }else{
        do{
            top += elem.offsetTop || 0;
            left += elem.offsetLeft || 0;
            elem = elem.offsetParent;
        } while (elem);
    }
    return {left:left, top:top};
}
```

`getBoundingClientRect` 在做图片惰性加载的时候就显得很容易了,这里就写一下判断时候需要显示的函数

```js
// 只考虑向下滚动加载
// 如果图片出现在小于等于可视窗口300的位置时候就应该让该图片显示 
function isInSight(el) {
  const bound = el.getBoundingClientRect();
  const clientHeight = window.innerHeight;
  return bound.top <= clientHeight - 300;
}
```



### 关于document的属性

document.all 非标准

document.title

document.body 返回body

document.compatMode

document.documentElement 返回文档对象

document.documentElement.scrollTop/Left 获取滚动条

document.documentElement.clientWidth/clientHeight 获取窗口大小 兼容 window.innerWidth/innerHeight