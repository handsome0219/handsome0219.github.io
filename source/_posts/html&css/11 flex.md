---
title: flex 弹性盒模型
tags: CSS3
categories: html5&css3
abbrlink: 59381
date: 2017-11-30 17:36:36
comments: false
---

![flex](http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071321.png)

<!-- more -->



# flex讲解

- 采用Flex布局的元素，称为Flex容器（flex container），简称"容器"。它的所有子元素自动成为容器成员，称为Flex项目（flex item），简称"项目"。
- 容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做`main start`，结束位置叫做`main end`；交叉轴的开始位置叫做`cross start`，结束位置叫做`cross end`。项目默认沿主轴排列。单个项目占据的主轴空间叫做`main size`，占据的交叉轴空间叫做`cross size`

<img src="http://www.ruanyifeng.com/blogimg/asset/2015/bg2015071004.png">

- 容器的属性

  1. flex-direction 

     排列方向	row | row-reverse | column | column-reverse

  2. flex-wrap

     nowrap | wrap | wrap-reverse

  3. justify-content

     主轴对齐方式	flex-start | flex-end | center | space-between | space-around

  4. align-items

     交叉轴对齐方式	flex-start | flex-end | center | baseline | stretch


- 子元素的flex属性：`flex` 属性是 `flex-grow`， `flex-shrink`  和  `flex-basis` 的简写，默认值为`0 1 auto`。后两个属性可选。
  1. flex-grow		定义项目的放大比例
    2. flex-shrink定义项目的缩小比例
  2. flex-basis         定义了在分配多余空间之前，浏览器根据这个属性，计算主轴是否有多余空间 默认auto

`tip`： 在基准值之内grow分配之外之外多余的部分按照grow的比例分配，超出的量按照shrink比例收缩并且basis失效。

