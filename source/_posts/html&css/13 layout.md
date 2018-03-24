---
title: layout布局
tags: layout
categories: html&css
abbrlink: 21184
date: 2017-12-02 23:14:04
thumbnail: https://wallpapers.wallhaven.cc/wallpapers/full/wallhaven-375080.jpg
comments: false
---

![layout](https://wallpapers.wallhaven.cc/wallpapers/full/wallhaven-375080.jpg)

<!-- more -->

## layout

> 布局是UI设计师将有限的视觉元素进行有机的排列组合。将理性的思维个性化的表现出来是一种具有个人风格和艺术特色的视觉传达方式。

分栏又称为分列，常见的布局有一列、两列、三列、**混合布局**。我们需要通过css中的浮动定位完成UI设计中的布局要求，所以前端工程师就是将艺术与技术完美融合的岗位。



## 多列布局

### 一列布局

单页的应用没有太多的内容，文字内容较少。

```html
<style>
  .header{height: 80px;background: #ddd;}
  /* 主体main高度肯定是要靠内容去撑起来的实际中不用给 */
  .main{width: 800px;height: 800px;background: #111;}
  .footer{height: 80px;background: #333;}
</style>
<body>
  <div class="header"></div>
  <div class="main"></div>
  <div class="footer"></div>
</body>
```



### 两列布局

两列布局很常见，具体的可以通过浮动或flex

*example 1*

```html
<style>
  /* 两列固定宽度 float */
  .clearfix::after{content: '';display: table;clear: both;}
  .wrap{width: 1000px;margin-left: auto;margin-right: auto;}
  .left{float: left;width: 300px;background: red;}
  .right{float: right;width: 700px;background: blue;}
</style>
<body>
  <div class="wrap clearfix">
    <div class="left"></div>
    <div class="right"></div>  
  </div>
</body>
```



*example 2*

```html
<style>
  /* 一列定 一列自适应 table */
  /*   
    简单解释一下这个原理 table设定的容器内部的元素按照表格排列，并且每列单元格的宽度和一定等于总的宽度
    table-cell设定的元素不会超出父元素的宽度 一般给2000 当然如果你是4k屏幕分辨率高的惊人可以设置成9999
    另外table-cell设定后margin失效 需要间距设定padding
  
    这里也可以不用外层的 如果需要居中则嵌套一层 
    table-layout: fixed 这个属性用来加速浏览器的渲染
  */
  .parent{display: table;table-layout: fixed;width: 100%;}
  .left{float: left;width: 400px;height: 400px;background: red;}
  .right{display: table-cell;width: 2000px;height: 400px;background: blue;}
</style>
<body>
  <div class="parent">
    <div class="left"></div>
    <div class="right"></div>
  </div>
</body>
```



*example 3*

```html
<style>
  /* float + overflow 缺点溢出不可见 需要居中加一层 */
  .left{float: left;width: 300px;height: 400px;background-color: red;}
  .right{overflow: hidden;background-color: blue;}
  .right div{width: 100px;height: 90px;background-color: yellow;margin:10px 0 0 -10px;}
</style>
<body>
  <div class="left"></div>
  <div class="right">
    <div></div>
    <div></div>
    <div></div>
    <div></div>
  </div>
</body>
```



*example 4*

```html
<style>
  /* flex 除了兼容问题没毛病 */
  .parent{display: flex;}
  .left{width: 400px;height: 400px;background-color: blue;flex:0 0 400px;}
  .right{width: 2000px;height: 400px;background-color: red;}
</style>
<body>
  <div class="parent">
    <div class="left"></div>
    <div class="right"></div>
  </div>
</body>
```



### 三列布局

三列布局的一般为左右两边定宽 中间自适应 方法手段很多只是每一种html结构差异

*example 1*

```html
<style>
  /*
    两边定位 中间用margin定位 
    缺点：当父元素有margin的时候 中间的会被挤下去
  */
  .left{width: 300px;height: 400px;background-color: red;position: absolute;top: 0;left: 0;}
  .middle{height: 400px;background-color: blue;margin: 0 210px 0 310px;}
  .right{width: 200px;height: 400px;background-color: black;position: absolute;top: 0;right: 0;}
</style>
<body>
  <div class="left"></div>
  <div class="middle"></div>
  <div class="right"></div>
</body>
```



*example 2*

```html
<style>
  /*
    两边浮动 中间margin定位 html结构产生了变化 
    缺点：两列和小于width之和右边会掉下去
  */
  .left{float: left;width: 300px;height: 400px;background-color: red;}
  .right{float: right;width: 200px;height: 400px;background-color: black;}
  .middle{height: 400px;background-color: blue;margin: 0 210px 0 310px;}
</style>
<body>
  <div class="left"></div>
  <div class="right"></div>
  <div class="middle"></div>
</body>
```



*example 3* 

```html
<style>
  /*
    flex 布局 除了兼容问题
  */
  .wrap{display: flex;width: 1200px;margin: 0 auto;}
  .left{width: 200px;height: 300px;background: green;flex: 0 0 200px;}
  .middle{width: 100%;background: red;margin:0 10px;}
  .right{width: 200px;height: 300px;background: yellow;flex: 0 0 200px;}
</style>
<body>
  <div class="wrap">
    <div class="left"></div>
    <div class="middle"></div>
    <div class="right"></div>
  </div>
</body>
```



*example 4*

```html
<style>
  /* 
    圣杯布局 一个流传很久的布局 有兴趣自行查看
  */
  #container {
    padding-left: 200px;      /* LC width */
    padding-right: 150px;     /* RC width */
  }
  
  #container .column {
    position: relative;
    float: left;
    padding-top: 1em;
    text-align: justify;
  }

  #center {
    width: 100%;
    background: #DDD;
  }

  #left {
    width: 200px;             /* LC width */
    height: 400px;
    right: 200px;             /* LC width */
    margin-left: -100%;
    background: #66F;
  }

  #right {
    width: 150px;             /* RC width */
    height: 400px;
    margin-right: -100%;
    background: #F66;
  }

  #footer {
    clear: both;
  }
</style>
<body>
  <div id="header">This is the header.</div>
    <div id="container">
      <div id="center" class="column">
        <h1>This is the main content.</h1>
        <p>center</p>
      </div>
      <div id="left" class="column">
        <h2>This is the left sidebar.</h2>
        <p>left</p>
      </div>
      <div id="right" class="column">
        <h2>This is the right sidebar.</h2>
        <p>right</p>
      </div>
    </div>
    <div id="footer">footer.</div>
</body>
```



### 居中

#### 水平居中

子元素在父元素中居中且宽度均可变

```css
/* inline-block + text-align */
.outer{text-align: center;}
.inner{display: inline-block;}

/*
  margin
  .inner{margin-left: auto;margin-right: auto;}
*/

/*
  flex
  .outer{display: flex;justify-content: center;}
*/

/*
  table + margin 兼容性最好
  .inner{display: table;margin-left: auto;margin-right: auto;}
*/

/*
  absolute + transform
  .outer{position: relative;}
  .inner{position: absolute;left: 50%;transform: translateX(-50%);}
*/
```



#### 垂直居中

```css
.outer {display: table-cell;vertical-align: middle;}

/*
  absolute + transform
  .outer{position: relative;}
  .inner{position: absolute;top: 50%;transform: translateY(-50%);}
*/

/*
  .outer{display: flex;align-item: center;}
*/
```



#### 水平垂直居中

```css
.outer {text-align: center;display: table-cell;vertical-align: middle;}
.inner {display: inline-block;}

/*
  absolute + transform
  .outer{position: relative;}
  .inner{position: absolute;top: 50%;left: 50%;transform: translate(-50%, -50%);}
*/

/*
  .outer{position: relative}
  .inner{width: 100px;height: 100px;position: absolute;top: calc(50% - 50px);left: calc(50% - 50px);}
*/

/*
  flex
  .outer{display: flex;justify-content: center;align-items: center;}
*/
```



### last

基础真的很重要，不能只关注深层的东西而忽略了它，很多时候我们都只关注这个新的技术，各种各样的框架组件。感叹的同时却还在原地踏步！挖掘简单事物背后的不简单，想信我们就真的进步了！

风起于青萍之末，浪成于微澜之间。生活多姿多彩等待你我去发现！么么哒~