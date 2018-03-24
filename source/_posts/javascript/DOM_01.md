---
title: DOM 发展
tags:
  - JS
  - DOM
categories: 
    - JavaScript
    - DOM
abbrlink: 4276
date: 2018-02-28 13:46:29
comments: false
---

### 网页生成过程、DOM发展

#### 1. 理解网页的性能,先了解一下网页的生成过程

> 1. HTML代码转化成DOM
> 2. CSS代码转化成CSSOM（CSS Object Model）
> 3. 结合DOM和CSSOM，生成一棵渲染树（包含每个节点的视觉信息）
> 4. 生成布局（layout），即将所有渲染树的所有节点进行平面合成
> 5. 将布局绘制（paint）在屏幕上



**webkit渲染流程**

![webkit流程](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015091502.png)

**Geoko渲染过程**

![Geoko](/img/javascript/dom/Geoko.jpg)



从第一步到第三步都是很快速的, 第四步与第五步是很耗时的

**"生成布局"（flow）和"绘制"（paint）这两步，合称为"渲染"（render）。** 

> **ps: 火狐把Layout过程成为回流是一个意思**



#### 2. 重排与重绘

**网页生成的时候，至少会渲染一次。用户访问的过程中，还会不断重新渲染。**

以下情况, 会导致网页的重新渲染

- 修改DOM
- 修改样式表
- 用户事件（比如鼠标悬停、页面滚动、输入框键入文字、改变窗口大小等等）
- 添加、删除元素(回流+重绘)
- 隐藏元素，display:none(回流+重绘)，visibility:hidden(只重绘，不回流)

1. 移动元素，比如改变top,left(jquery的animate方法就是,改变top,left不一定会影响回流)，或者移动元素到另外1个父元素中。(重绘+回流)
2. 对style的操作(对不同的属性操作，影响不一样)

**重新渲染，就需要重新生成布局和重新绘制。前者叫做"重排"（reflow），后者叫做"重绘"（repaint）。**

需要注意的是，**"重绘"不一定需要"重排"**，比如改变某个网页元素的颜色，就只会触发"重绘"，不会触发"重排"，因为布局没有改变。但是，**"重排"必然导致"重绘"**，比如改变一个网页元素的位置，就会同时触发"重排"和"重绘"，因为布局改变了。

传送门 [浏览器内部工作原理](http://kb.cnblogs.com/page/129756/)



#### 3. 对性能的影响

> 重排和重绘会不断触发，这是不可避免的。但是，它们非常耗费资源，是导致网页性能低下的根本原因。

**提高网页性能，就是要降低"重排"和"重绘"的频率和成本，尽量少触发重新渲染。**

前面提到，DOM变动和样式变动，都会触发重新渲染。但是，浏览器已经很智能了，会尽量把所有的变动集中在一起，排成一个队列，然后一次性执行，尽量避免多次重新渲染。

```js
div.style.color = 'blue';
div.style.marginTop = '30px';
```

上述代码会触发浏览器一次的重排重绘



**如果写的不好就会导致浏览器执行多次**

```js
div.style.color = 'blue';
// 这里再次出现了元素的位置 浏览器不得不重新排列一次
var margin = parseInt(div.style.marginTop);
div.style.marginTop = margin + 10 + 'px';
```



一般来说浏览器对于一些样式的**写**操作之后,出现一下的读属性操作,都会引发浏览器的重新渲染

- offsetTop/offsetLeft/offsetWidth/offsetHeight
- scrollTop/scrollLeft/scrollWidth/scrollHeight
- clientTop/clientLeft/clientWidth/clientHeight
- getComputedStyle()

**尽量不要把读写操作混到一起**

```js
// bad
div.style.left = div.offsetLeft + 10 + 'px';

// good
var l = div.offsetLeft;
div.style.left = l + 10 + 'px';
```

一般的规则是:

- 样式表越简单，重排和重绘就越快。


- 重排和重绘的DOM元素层级越高，成本就越高。


- table元素的重排和重绘成本，要高于div元素

#### 4. 提高性能的几种方式

1. DOM 的多个读操作（或多个写操作），应该放在一起。不要两个读操作之间，加入一个写操作
2. 如果某个样式是通过重排得到的，那么最好缓存结果。避免下一次用到的时候，浏览器又要重排。
3. 不要一条条地改变样式，而要通过改变class，或者csstext属性，一次性地改变样式。
4. 尽量使用离线DOM，而不是真实的网面DOM，来改变元素样式。比如，操作Document Fragment对象，完成后再把这个对象加入DOM。再比如，使用 cloneNode() 方法，在克隆的节点上进行操作，然后再用克隆的节点替换原始节点。 documentFragment ( 文档片段:虚拟的节点在重复的渲染过程中避免产生渲染回流,其实可以理解为重排 )
5. 先将元素设为`display: none`（需要1次重排和重绘），然后对这个节点进行100次操作，最后再恢复显示（需要1次重排和重绘）。这样一来，你就用两次重新渲染，取代了可能高达100次的重新渲染。
6. position属性为`absolute`或`fixed`的元素，重排的开销会比较小，因为不用考虑它对其他元素的影响。
7. 只在必要的时候，才将元素的display属性为可见，因为不可见的元素不影响重排和重绘。另外，`visibility : hidden`的元素只对重绘有影响，不影响重排。
8. 虚拟DOM框架 vue、react
9. 使用 window.requestAnimationFrame()、window.requestIdleCallback() 这两个方法调节重新渲染

[详见阮一峰大大文章](http://www.ruanyifeng.com/blog/2015/09/web-page-performance-in-depth.html)



#### 5. 什么是DOM

DOM，文档对象模型（Document Object Model）。DOM是 W3C（万维网联盟）的标准，DOM定义了访问HTML和XML文档的标准。在W3C的标准中，DOM是独于平台和语言的接口，它允许程序和脚本动态地访问和更新文档的内容、结构和样式。

DOM（文档对象模型）是针对xml经过扩展用于html的应用程序编程接口，我们又叫API。DOM把整个页面映射为一个多层的节点结构，html或xml页面中的每个组成部分都是某种类型的节点，这些节点又包含着不同类型的数据。



#### 6. DOM的地位

一个网页是由html来搭建结构的，通过css来定义网页的样式，而JavaScript赋予了页面的行为，通过它我们可以与页面进行交互，实现页面的动画效果等等。那javascript究竟通过什么来实现的呢？通过ECMAScript这个标准，我们可以编写程序让浏览器来解析，利用ECMAScript，我们可以通过BOM对象（即browser object model）来操作浏览器窗口、浏览器导航对象(navigator)、屏幕分辨率(screen)、浏览器历史(history)、cookie等等。但这个通过BOM来实现的交互远远不够。要实现页面的动态交互和效果，操作html才是核心。那如何操作html呢？对，就是DOM，简单的说，DOM给我们提供了用程序来动态控制html的接口，也就是早期的DHTMl的概念。因此，DOM处在javascript赋予html具备动态交互和效果的能力的核心地位上。



#### 7. DOM的发展 DOM1、DOM2、DOM3的区别

- DOM0

  JavaScript在早期版本中提供了查询和操作Web文档的内容API（如：图像和表单），在JavaScript中定义了定义了'images'、'forms'等，因此我们可以像下这样访问第一张图片或名为“user”的表单

  这实际上是未形成标准的试验性质的初级阶段的DOM，现在习惯上被称为DOM0，即：第0级DOM。由于DOM0在W3C进行标准备化之前出现，还处于未形成标准的初期阶段，这时Netscape和Microsoft各自推出自己的第四代浏览器，自此DOM遍开始出各种问题。

  ​

- DOM1

  在浏览器厂商进行浏览器大站的同时，W3C结合大家的优点推出了一个标准化的DOM，并于1998年10月完成了第一级 DOM，即：DOM1。W3C将DOM定义为一个与平台和编程语言无关的接口，通过这个接口程序和脚本可以动态的访问和修改文档的内容、结构和样式

  DOM1级主要定义了HTML和XML文档的底层结构。在DOM1中，DOM由两个模块组成：DOM Core（DOM核心）和DOM HTML。其中，DOM Core规定了基于XML的文档结构标准，通过这个标准简化了对文档中任意部分的访问和操作。DOM HTML则在DOM核心的基础上加以扩展，添加了针对HTML的对象和方法，如：JavaScript中的Document对象.

  ![](/img/javascript/dom/dom_1.jpg)

- DOM2

  在DOM1的基础上DOM2引入了更多的交互能力，也支持了更高级的XML特性。DOM2将DOM分为更多具有联系的模块。DOM2级在原来DOM的基础上又扩充了鼠标、用户界面事件、范围、遍历等细分模块，而且通过对象接口增加了对CSS的支持。DOM1级中的DOM核心模块也经过扩展开始支持XML命名空间。在DOM2中引入了下列模块，在模块包含了众多新类型和新接口：

  1. DOM视图（DOM Views）：定义了跟踪不同文档视图的接口
  2. DOM事件（DOM Events）：定义了事件和事件处理的接口
  3. DOM样式（DOM Style）：定义了基于CSS为元素应用样式的接口
  4. DOM遍历和范围（DOM Traversal and Range）：定义了遍历和操作文档树的接口

  ![](/img/javascript/dom/dom_2.jpg)

  百度百科完整DOM2标准

  ![](/img/javascript/dom/dom_22.jpg)


- DOM3

  DOM3级：进一步扩展了DOM，引入了以统一方式加载和保存文档的方法，它在DOM Load And Save这个模块中定义；同时新增了验证文档的方法，是在DOM Validation这个模块中定义的。

  DOM3进一步扩展了DOM，在DOM3中引入了以下模块：

  1. DOM加载和保存模块（DOM Load and Save）：引入了以统一方式加载和保存文档的方法
  2. DOM验证模块（DOM Validation）：定义了验证文档的方法
  3. DOM核心的扩展（DOM Style）：支持XML 1.0规范，涉及XML Infoset、XPath和XML Base

  ![](/img/javascript/dom/dom_3.jpg)

#### 8. 认识DOM

DOM可以将任何HTML描绘成一个由多层节点构成的结构。节点分为12种不同类型，每种类型分别表示文档中不同的信息及标记。每个节点都拥有各自的特点、数据和方法，也与其他节点存在某种关系。节点之间的关系构成了层次，而所有页面标记则表现为一个以特定节点为根节点的树形结构。

![](/img/javascript/dom/dom_tree1.jpg)

```html
<!DOCTYPE html>
  <html>
  <head>
     <meta charset="utf-8">
      <title>DOM</title>
  </head>
  <body>
      <h2><a href="http://www.baidu.com">javascript DOM</a></h2>
      <p>对HTML元素进行操作，可添加、改变或移除css样式等</p>
      <ul>
          <li>Javascript</li>
          <li>DOM</li>
          <li>CSS</li>
      </ul>
  </body>
  </html>
```

将HTML代码分解成DOM节点层次

![](/img/javascript/dom/dom_tree.jpg)



- 文档节点的发展历史

  > 我们说DOM文档对象模型是从文档中抽象出来的，DOM操作的对象也是文档，因此我们有必要了解一下文档的类型。文档随着历史的发展演变为多种类型，如下:

  ![](/img/javascript/dom/dom_his.jpg)

  ​

- DOM节点的类型

  > DOM1级定义了一个Node接口，这个Node接口在javascript中是作为Node类型来实现的。除了IE以外，其他所有浏览器都可以访问这个类型。每个节点都有一个nodeType属性，用于表明节点的类型。节点类型通过定义数值常量和字符常量两种方式来表示，IE只支持数值常量。节点类型一共有12种，这里介绍常用的7种类型。如下图：

  ![](/img/javascript/dom/dom_type.jpg)

  1. #### Element(元素节点)

     > 是组成文档树的重要部分，它表示了html、xml文档中的元素。通常元素因为有子元素、文本节点或者两者的结合，元素节点是唯一能够拥有属性的节点类型。

     ​

  2. #### Attr(属性节点)

     > 代表了元素中的属性，因为属性实际上是附属于元素的，因此属性节点不能被看做是元素的子节点。因而在DOM中属性没有被认为是文档树的一部分。换句话说，属性节点其实被看做是包含它的元素节点的一部分，它并不作为单独的一个节点在文档树中出现。

     ​

  3. #### Text(文本节点)

     > 是只包含文本内容的节点，在xml中称为字符数据，它可以由更多的信息组成，也可以只包含空白。在文档树中元素的文本内容和属性的文本内容都是由文本节点来表示的。


- ### DOM的nodeType、nodeName、nodeValue

  > 通过DOM节点类型，我们可知，可以通过某个节点的nodeType属性来获得节点的类型，节点的类型可以是数值常量或者字符常量。示例代码如下：

  ```html
  <!DOCTYPE html>  
  <html>  
  <head lang="en">  
      <meta charset="UTF-8">  
      <title>nodeName,nodeValue</title>  
  </head>  
  <body>  
      <!--nodeName,nodeValue实验-->  
      <div id="container">这是一个元素节点</div>  
      <script>  
          var divNode = document.getElementById('container');  
          console.log(divNode.nodeName + "/" + divNode.nodeValue);     
          //结果:    DIV/null  
          
          var attrNode = divNode.attributes[0];  
          console.log(attrNode.nodeName + "/" + attrNode.nodeValue);      
          //结果：   id/container  
          
          var textNode = divNode.childNodes[0];  
          console.log(textNode.nodeName + "/" + textNode.nodeValue);      
          //结果：   #text/这是一个元素节点  
          
          var commentNode = document.body.childNodes[1];  
          //表示取第二个注释节点，因为body下面的第一个注释节点为空白符。  
          console.log(commentNode.nodeName + "/" +commentNode.nodeValue);  
          //结果：  #comment/nodeName,nodeValue实验  
          
          console.log(document.doctype.nodeName + "/" + document.doctype.nodeValue);   
          //结果： html/null  
          
          var frag = document.createDocumentFragment();  
          console.log(frag.nodeName + "/" + frag.nodeValue);    
          //结果： #document-fragment/null  
      </script>  
  </body>  
  </html>  
  ```

  总结:

  ![](/img/javascript/dom/nodeType.jpg)

  ​

#### 9. 关于domReady、onload

> html是一种标记语言，它告诉我们这个页面有什么内容，但行为交互是需要通过DOM操作来实现的。我们不要以为有两个尖括号就以为它是一个DOM了，html标签要通过浏览器解析才会变成DOM节点，当我们向地址栏传入一个url的时候，我们开始加载页面，就能看到内容，在这期间就有一个DOM节点构建的过程。节点是以树的形式组织的，当页面上所有的html都转换为节点以后，就叫做`DOM树构建完毕`，简称为`domReady`。

上边已经介绍了浏览器的渲染引擎的基本渲染流程

> 浏览器渲染要做的事情就是把css、html、图片等静态资源显示到用户的面前

![](/img/javascript/dom/load.jpg)

> 渲染引擎首先通过网络获得所请求文档的内容，通常以8k分块的方法来完成：

![](/img/javascript/dom/render.jpg)

> 上图就是html渲染的基本过程，但这并不包含解析过程中浏览器加载外部资源，比如图片、脚本、iframe等的一些过程。说白了，**上面的4步仅仅是html结构的渲染过程**。而外部资源的加载在html结构的渲染过程中是贯彻始终的，即便绘制DOM节点已经完成，而外部资源仍然可能正在加载或者尚未加载。



然而我们并不是要在去了解浏览器渲染引擎到底是怎么来工作的、我们需要了解的是domReady的实现策略

- 实例1

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Dom not ready</title>
    <script>
      document.getElementById("header").style.color = "red";
    </script>
  </head>
  <body>
    <h1 id="header">这里是h1元素包含的内容</h1>
  </body>
</html>
```

这里文字并没有变成红色,开始学习DOM的操作多少都会碰到这个问题,原因就在于没有区分清楚`HTML标签`与`DOM节点`的区别, 浏览器通过解析器解析html语义标签,解析css样式表构建生成一个DOM节点树,就叫做DOM树构建,结束的过程代表`domReady`

1. 这里可以通过一个定时器来不断的去执行也可以实现效果

```js
setInterval(function(){
    document.getElementById('header').style.color = 'red';
},30);
```

虽然也可以实现效果但是用户界面总会有一个短暂的闪烁,颜色在随之改变.不利于用户的体验.

1. 另外一个解决方式

```js
window.onload = function(){
	document.getElementById('header').style.color = 'red';  
}
```

有一定了解的这里肯定会去使用`window.onload` 来解决这个问题,没有任何问题

> window.onload方法，表示当页面所有的元素都加载完毕，并且所有要请求的资源也加载完毕才触发执行function这个匿名函数里边的具体内容。这样肯定保证了代码在domReady之后执行。使用window.onload方法在文档外部资源不多的情况下不会有什么问题，但是当页面中有大量远程图片或要请求的远程资源时，我们需要让js在点击每张图片时，进行相应的操作，如果此时外部资源还没有加载完毕，点击图片是不会有任何反应的，大大降低了用户体验。那既然window.onload方法不可行，又该怎么做呢？

- 实例2

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Dom not ready</title>
  </head>
  <body>
    <h1 id="header">这里是h1元素包含的内容</h1>
    <script>
      document.getElementById("header").style.color = "red";
    </script>
  </body>
</html>
```

> 这里并没有考虑 domReady 程序也可以正常运行, 因为浏览器是从上而下,从左到右渲染元素的, js代码肯定会在domReady之后去执行, 那为什么还要去写domReady呢? 在编写大型项目的时候，js文件往往非常多，而且之间会相互调用，大多数都是外部引用的，不把js代码直接写在页面上。这样的话，如果有个domReady这个方法，我们想用它就调用，不管逻辑代码写在哪里，都是等到domReady之后去执行的。

jQuery中的readey事件其实就是domReady完成执行其中的代码块,算是弥补了window.onload的短板, 用的是w3c提供的`DOMContentLoaded`事件, 但是DOMContentLoaded不支持低版本的IE([DOMContentLoaded](https://developer.mozilla.org/zh-CN/docs/Web/Events/DOMContentLoaded))MDN文档

简单的来说,在页面的DOM树创建完成后（也就是HTML解析第一步完成）即触发，而无需等待其他资源的加载。即domReady实现策略:

> ```
> 1. 支持DOMContentLoaded事件的，就使用DOMContentLoaded事件。
> 2. 不支持的就用来自Diego Perini发现的著名Hack兼容。兼容原理大概就是通过IE中的document，
> documentElement.doScroll('left')来判断DOM树是否创建完毕。
> ```



#### 10. javascript对DOMReady的实现

```js
// 1.
function ready(fn){
    // 目前Mozilla、Opera和webkit 525+内核支持DOMContentLoaded事件
    if(document.addEventListener) {
        document.addEventListener('DOMContentLoaded', function() {
            document.removeEventListener('DOMContentLoaded',arguments.callee, false);
            fn();
        }, false);
    }
    // 如果IE
    else if(document.attachEvent) {
        // 确保当页面是在iframe中加载时，事件依旧会被安全触发
        document.attachEvent('onreadystatechange', function() {
            if(document.readyState == 'complete') {
                document.detachEvent('onreadystatechange', arguments.callee);
                fn();
            }
        });

        // 如果是IE且页面不在iframe中时，轮询调用doScroll 方法检测DOM是否加载完毕
        if(document.documentElement.doScroll && typeof window.frameElement === "undefined") {
            try{
                document.documentElement.doScroll('left');
            }
            catch(error){
                return setTimeout(arguments.callee, 20);
            };
            fn();
        }
    }
};

// 2.
function myReady(fn){
    //对于现代浏览器，对DOMContentLoaded事件的处理采用标准的事件绑定方式
    if ( document.addEventListener ) {
        document.addEventListener("DOMContentLoaded", fn, false);
    } else {
        IEContentLoaded(fn);
    }
    //IE模拟DOMContentLoaded
    function IEContentLoaded (fn) {
        var d = window.document;
        var done = false;

        //只执行一次用户的回调函数init()
        var init = function () {
            if (!done) {
                done = true;
                fn();
            }
        };
        (function () {
            try {
                // DOM树未创建完之前调用doScroll会抛出错误
                d.documentElement.doScroll('left');
            } catch (e) {
                //延迟再试一次~
                setTimeout(arguments.callee, 50);
                return;
            }
            // 没有错误就表示DOM树创建完毕，然后立马执行用户回调
            init();
        })();
        //监听document的加载状态
        d.onreadystatechange = function() {
            // 如果用户是在domReady之后绑定的函数，就立马执行
            if (d.readyState == 'complete') {
                d.onreadystatechange = null;
                init();
            }
        }
    }
}

// jQuery实现的过程
function bindReady(){
    if ( readyBound ) return;
    readyBound = true;
    // Mozilla, Opera and webkit nightlies currently support this event
     if ( document.addEventListener ) {
        // Use the handy event callback
         document.addEventListener( "DOMContentLoaded", function(){
            document.removeEventListener( "DOMContentLoaded", arguments.callee, false );
            jQuery.ready();
        }, false );

    // If IE event model is used
    } else if ( document.attachEvent ) {
        // ensure firing before onload,
        // maybe late but safe also for iframes
        document.attachEvent("onreadystatechange", function(){
            if ( document.readyState === "complete" ) {
                document.detachEvent( "onreadystatechange", arguments.callee );
                jQuery.ready();
            }
        });
        // If IE and not an iframe
        // continually check to see if the document is ready
        if ( document.documentElement.doScroll && typeof window.frameElement === "undefined" ) (function(){
            if ( jQuery.isReady ) return;
            try {
                // If IE is used, use the trick by Diego Perini
                // http://javascript.nwbox.com/IEContentLoaded/
                document.documentElement.doScroll("left");
            } catch( error ) {
                setTimeout( arguments.callee, 0 );
                return;
            }
            // and execute any waiting functions
            jQuery.ready();
        })();
    }
    // A fallback to window.onload, that will always work
    jQuery.event.add( window, "load", jQuery.ready );
}
```

[各大主流JS框架中对DOMReady事件的实现](http://www.cnblogs.com/JulyZhang/archive/2011/02/12/1952484.html)

本文借鉴了很多大神的blog，自己也对DOM有了更深的理解，希望可以帮助到更多人。