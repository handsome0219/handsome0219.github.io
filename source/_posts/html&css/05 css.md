---
title: 初识css
tags: CSS
categories: html&css
abbrlink: 35262
date: 2017-11-15 19:38:29
comments: false
---

![css](https://udemy-images.udemy.com/course/750x422/792484_cc98_3.jpg)

<!-- more -->

> 认识 CSS --- Cascading Style Sheets
>
> CSS的世界是神奇的。随着浏览器WEB标准的日趋统一，CSS在WEB世界中的扮演的角色也越来越重要。
>
> 我们从HTML开始，因为CSS的用途就是为了给HTML标记添样式，所以我们要先知道怎么去写HTML标签



### 什么是CSS

* HTML标记内容是为了给网页赋予纯粹的**语义**。换而言之就是为了让用户可以去理解里边的含义。每一个标签都是对所包含的内容的一种诠释，描述。所以请记住*HTML* 就是`文本`+`标记`的一个文档结构（请不要参杂CSS）。当我们给内容都打上标记，就可以使用CSS给标记添加样式了。添加样式的过程根据标签名、标签属性、标签等等的一些关系来给相对应的标签添加样式，*so！* 先有 **结构**后有**样式**。
* 简单的来说CSS相当于一个神奇的化妆师，它可以去操作文档的整体表现形式，针对布局、文字、颜色、背景、动画效果等等实现精确的控制，让文档的表现更加的美观好看，它的组成是由一系列有含义的单词和数值所构成。
* CSS样式可以直接存储于HTML网页或者单独的样式单文件。无论哪一种方式，样式单包含将样式应用到指定类型的元素的规则。外部使用时，样式单规则被放置在一个带有文件扩展名`.css`的外部样式单文档中。简单的了解这个概念之后我们来学习一些基础知识。





### 添加样式的三种方式

有三种方法可以把CSS样式添加到网页中，分别是行内样式、嵌入样式、链接样式

```html
<!-- 行内样式 -->
<p style="font-size:12px;font-weight:bold;color:red;background:yellow">Hello world</p>
```

```html
<!-- 嵌入样式 -->
<head>
  <style>
    p{font-size:12px;color:red;background:yellow}
  </style>
</head>
```

```html
<!-- 链接样式 -->
<link href="style.css" rel="stylesheet" type="text/css">

<!-- 要注意的是@import指令必须出现在样式表其它样式之前，否则不会被加载 -->
<style>
  @import url(style.css); /* 注意看这里有一个分号 */
</style>
```

`ps` ：网页的解析是从上到下，从左至右。当浏览器遇到开标签&lt;style&gt;时，就会有解析HTML代码切换成为解析CSS样式代码。再次遇到 &lt;/style&gt;时，浏览器会再次切换成为解析HTML代码





### CSS规则

> 构成CSS规则有很多，主要就是选择器。这里我们只需要掌握常用的选择器，碰到特殊的再去查询。



#### 命名惯例

​	选择符 {属性：值；}

​	p { color : red; }



#### 选择器的分类

* 上下文选择器
  * 一般上下文选择器
  * 特殊上下文选择器
    * 子选择器 &gt;
    * 紧邻同胞 +
    * 一般同胞 ~
* id和class选择器
  * \#id{属性：值；}
  * \.class{属性：值；}
  * tag.class{属性：值；}
* 属性选择器
  * tag[属性名]
  * tag[属性名="属性值"]

> `ps`： 什么时候使用id、class、属性选择器？
>
> 1. id 的用途是在页面标记中唯一地标识一个特定的元素。 
> 2. 类是可以应用给任意多个页面中的任意多个 HTML 元素的公共标识符 。简单来说具有相同的特征的元素
> 3. 基于属性名和属性的其它特征选择元素，区别对待相同标签，通过不同的标记找到适合的元素。



#### CSS选择器图解

> 我们可以通过图形来理解一下

1. 一般选择器

   ```html
   <style>
     /* 选择以p为祖先的 所有 span元素 */
     p span{color: red;}
   </style>
   <body>
     <h1><b>一般选择器</b></h1>
     <p>
       <span>this is span</span>
       <a><span>this is a</span></a>
       <span>this is anthor span</span>
     </p>
   </body>
   ```

   ![](/img/css/选择器.png)

2. 特殊选择器

   * 子选择器 &gt;

     ```html
     <style>
       /* 标签1 > 标签2 也就是说1必须是2的父元素 */
       p > span{color: red;}
     </style>
     <body>
       <h1><b>一般选择器</b></h1>
       <p>
         <span>this is span</span>
         <a><span>this is a</span></a>
         <span>this is anthor span</span>
       </p>
     </body>
     ```

     ![](/img/css/子代选择器.png)

   * 紧邻同胞

     ```html
     <style>
       /* 标签1 + 标签2  标签2必须紧跟在标签1后边 */
       h1 + p{color: red;}
     </style>
     <body>
         <h1>紧邻同胞</h1>
         <p>1</p>
         <p>2</p>
         <p>3</p>
     </body>
     ```

     ![](/img/css/紧邻同胞选择器.png)

   * 一般同胞

     ```html
     <style>
       /* 标签1 ~ 标签2  标签2必须跟( 不一定紧跟 )在标签1后边 */
       h1 ~ p{color: red;}
     </style>
     <body>
         <h1>紧邻同胞</h1>
         <p>1</p>
         <p>2</p>
         <p>3</p>
     </body>
     ```

     ![](/img/css/一般同胞选择器.png)

   * id、class选择器 相当于警察叔叔直接查你的身份证（唯一性）、和查你的学生证（你有很多张从小学到大学做为一个特征，你是一个学生）

     > `ps` :  
     >
     > 1. 只不过有一个标签带类选择器 更加精确的定位特定的标签元素 (同理id选择器也具有同样的功能)
     > 2. 多类选择 eg: &lt;p class="a b"&gt;&lt;/p&gt; 可以这样子去写 .a.b{color: red}




#### 伪类 

> 伪类会基于特定的HTML元素的状态应用样式。我们在chrome、firfox开发者工具中任意右键点击一个元素会看到一个菜单。接下来我们介绍一下伪类。*Are you ready ?*

![ui伪类](/img/css/UI伪类.png)



##### 链接伪类

> 在浏览器中样式的时候它们可以帮助我们快速的进行变换。首先介绍一下链接伪类，因为任何一个链接始终都会处于下边四个状态之一

* link	 	链接等着用户点击
* visited     用户点击过这个链接
* hover      鼠标悬停在链接上 
* active      链接正在被点击

```html
<!-- example 使用a标签举例 -->
<style>
  a:link {color: blue;}
  a:visited {color: green;}
  a:hover {color: red;}
  a:active {font-weight: bold;font-size: 20px;}
</style>
<body>
  <a href="#">click me</a>
</body>
```

`tip` : 伪类的写法（：）一个冒号代表伪类，请务必区分和伪元素（：：）的写法，稍后看这个。看到上面的例子，可以看到`a标签` 也就是链接在初始的状态的时候是*blue* ，当鼠标悬停在上方状态为 *red* ，当鼠标点击链接其中的字体变大并且加粗了（为了效果而已），最后呈现的状态*visited* 。不过在这里地方可能会碰到一个很奇怪的问题`link` 当你第一次设置的时候是有效果的第二次在看这个页面的时候样式不对了，请你清除一下浏览器的缓存，并更换一下`href` 。

实际中不会写这么多只需要定义你所需要的， 比如用户悬停的时候给一个鲜艳的颜色，为了告诉用户快tm点我（毕竟是一个妖艳贱货๑乛◡乛๑）。如果这个链接目录很长，那么就应该使用`visited` 状态给一个浅的颜色，对于用户的提示作用有很大的帮助，当然也要看地方。



##### 其它伪类、结构伪类

* focus

  > 获取焦点，表单中使用

* target（不常用）

  > 当用户点击一个指向页面中其它元素（target）的链接时，可以通过此伪类选择

* first-child、last-child

  > 代表同胞组的第一个、最后一个

* nth-child(n)

  > n代表一个数值，或者是`odd奇数`、`even偶数`
  >
  > 可以增强一切列表的可读性，展现不同的效果

```html
<!-- e:target -->
<a href="#info">更多信息</a>
<div id="info">
  <h2>More information</h2>
</div>
```



##### 伪元素

> 顾名思义，伪元素是在你的文档上时有时无的元素。介绍几个常用的，并且区分一下伪类与伪元素的区别，一些小技巧。

`tip`： 请记得和伪类（：）的写法区分，伪元素的写法（：：），虽然浏览器对于一个：也是支持的但是为了避免大家混乱，请遵守规则。

* ::first-letter

  选择首字符

* ::first-line

  选择文本段落第一行

* ::before

  在特定元素前边添加内容

* ::after

  在特定元素后边添加内容(用来清除浮动)

`example` :

```html
<!-- 看到效果了记得告诉我๑乛◡乛๑ -->
<style type="text/css">
  p::first-letter{font-size: 200%;color: red;}
</style>
<body>
  <p>如果你喜欢我，你可以来找我呀！</p>
</body>
```

```html
<!-- first-line会随着屏幕大小改变 -->
<style type="text/css">
  p::first-line{color: red;font-variant:small-caps;/* 以小型大写字母展示 */}
</style>
<body>
  <p>
    I have a dream that one day this nation will rise up and live out the true meaning of its creed: "We hold these truths to be self-evident, that all men are created equal."
    I have a dream that one day on the red hills of Georgia, the sons of former slaves and the sons of former slave owners will be able to sit down together at the table of brotherhood
	</p>
</body>
```

```html
<!-- before、after -->
<style type="text/css">
    p::before{content: '小可爱们，'}
    p::after{content: '小祺'}
</style>
<body>
  <p>晚上好我是</p>
</body>
```

`ps`：伪元素的能做的东西还很多以后我们在去了解。接下来我们来区分一下伪类与伪元素。



##### 区分伪类与伪元素

> 伪类与伪元素是同学们最容易混淆的两个知识点。最直观的请大家通过写法初步区分。1和2的区别

*example*

```html
<!-- 
	1. 通过伪类选择第一个标签并且添加红色 
	2. 同样我们可以使用一个类名给第一个标签添加红色
-->
<style>
	.first{color: red;}
	/*p span:first-child{color: red;}*/
</style>
<body>
  <p>
    <span class="first">HTML</span>
    <span>Javascript</span>
  </p>
</body>
```

```html
<!-- 
	1. 通过伪元素选择第一个字符并且添加红色 
	2. 同样我们可以使用一个类名给第一个字符添加红色
-->
<style>
	.first{color: red;}
	/*p::first-letter{color: red;}*/
</style>
<body>
  <p>
    <span class="first">C</span>ss伪元素
  </p>
</body>
```

到此我相信大家对*CSS* 的人是已经有了一定的了解了。



### CSS盒子模型

> 提到盒子大家应该可以想到很多生活中的例子，例如纸盒、收纳盒、垃圾桶等等立体的盒子。这些盒子和CSS中的盒子有很多的相似点，都有高度宽度都可以装东西。那么我们来一起探究一下什么是盒子模型？
>
> 概念：使用盒子模型来盛放网页中的各种元素，在网页设计中，内容指文字、图片等元素，也可以是盒子之间的嵌套。填充只有宽度高度属性，可以理解为填充物（padding），而盒子的材料厚度就是边框（大小、线性、颜色），盒子与盒子之间的距离（margin）就是间距。
>
> Box Model CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。

![box model](http://www.w3school.com.cn/i/ct_boxmodel.gif)

盒模型的组成由width、height、border、padding、margin来描述。

* margin是什么？

  例如生活中的箱子，两个箱子并排放在一起，他们两个之间的间距（margin）被称为外边距。分为上右下左四个方向。

  ![margin](/img/css/margin.png)

* padding是什么？

  当我们往盒子里边放了一些贵重的物品时（小心易碎！），需要在物品和盒子之间放置一些填充物，这些填充物（padding）被称为内边距。同样内边距分为上右下左四个方向。

  ![padding](/img/css/padding.png)

`tip`：在盒子模型中，width,height指的是**内容区域的**的尺寸。增加内外边距不会影响内容区域的尺寸，但是会影响元素的总尺寸。 所以当我们去计算一个元素应该在页面呈现多大我们需要去合理的设置相关的属性达到我们需要的效果。



> 探究一下padding与margin不同的值所体现的效果。

*example*

```css
/* 四个方向 */
.box{margin: 0;padding: 0;}

/* 第一个10px代表上下 第二个10px代表左右 */
.box{margin: 10px 10px;padding: 10px 10px;}

/* 第一个10px代表上 第二个10代表左右 第三个10px代表下 */
.box{margin: 10px 10px 10px;padding: 10px 10px 10px;}

/* 第一个10px代表上 第二个10代表右 第三个10px代表下 第四个10px代表左 */
.box{margin: 10px 10px 10px 10px;padding: 10px 10px 10px 10px;}
```

**Q&A** 

* 外边距重合？margin有一个很奇怪的效果。

  *example* 

  ```html
  <style type="text/css">
    .box{width: 100px;height: 100px;background: red;margin: 10px 0px;}
    .xq{width: 50px;height: 50px;background: blue;margin: 30px}
  </style>
  <body>
    <div class="box"></div>
    <div class="box"></div>
    
    <!-- 
  	发现问题了吗？
  	解决方法给父级元素添加overflow 或者 添加一个边框解除 
    -->  
    <div class="box">
    	<div class="xq"></div>
    </div>
  </body>
  ```

  ​