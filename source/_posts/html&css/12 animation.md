---
title: animation动画
tags: CSS3
categories: html5&css3
abbrlink: 46345
date: 2017-12-01 14:22:16
thumbnail: http://www.alonely.com.cn/d/file/Html-CSS/2016-09-27/git1csitzbk.jpg
comments: false
---

![animation](http://www.alonely.com.cn/d/file/Html-CSS/2016-09-27/git1csitzbk.jpg)

<!-- more -->

> CSS3的animation动画，结合transform元素可以被转换（translate）、旋转（rotate）、缩放（scale）、倾斜（skew），可以做出很多有趣的效果。



## transform、transition

首先CSS3的 **transform **属性 , 只对 **block **级元素生效！*transform* 是 *CSS3* 中一个可以针对2D或3D的变化属性 。transform只能定义几何变化，不能改变颜色，背景，透明度等**非几何**变化。列举一些常用的变化属性。

- translate，translateX, translateY, translateZ
- rotate, rotateX, rotateY, rotateZ
- scale, scaleX, scaleY
- skew, skewX, skewY



指定了 **transform** 的元素会立即发生改变，需要配合 **transition** 一起来使用，用transition属性为transform变化指定属性、持续时间、运动曲线、 延迟时间。添加了必要的动画参数才是一个完整的动画，否则单独的transform不会有动画效果。

> transition: < property > < duration > < timing-function > < delay >;

`tip`：transition与transform不同的地方在于前者可以改变非几何属性，即transform无法变化的属性。



*example1*

```css
.box{width: 100px;height: 100px;background-color: red;margin: 100px auto;transition: width 2s;}
.box:hover{width: 200px;}
```

并且可以变化多个属性通过逗号分隔。



*example2*

```css
.box{width: 100px;height: 100px;background-color: red;margin: 100px auto;transition: width 2s, height 2s, background 2s, opacity 2s, transform 2s;}
.box:hover{width: 200px;height: 200px;background: blue;opacity: .3;transform: rotate(180deg) translateX(100px);}
```

> 提醒一下transform如果在别的浏览器不兼容记得加浏览器兼容的前缀！
>
> -webkit-transform: rotate();
>
> -moz-transform: rotate();
>
> -ms-transform: rotate();
>
> -o-transform: rotate();



*example3*

```html
<style>
.box{
  /*transition-property: background-color, color;
  transition-duration: 1s;
  transition-timing-function: ease-out;*/
  /* 可以一句话写完 */
  transition: background-color, color, 1s ease-out;
  /* transition: all 1s ease-out; */
  background-color: grey; 
  width: 60px;
  height: 26px;
  color: white;
  font-size: 20px;
  text-align: center;
  box-shadow: 2px 2px 1px black;
  padding: 2px 4px;
}
.box:hover{
  background-color:#AA7EF6;
  color:black;
  box-shadow: 2px 2px 1px black;
}
</style>
<body>
  <div>Css3</div>
</body>
```



*example4*

```css
#heart {
  position: relative;
  width: 100px;
  height: 90px;
  transition: all 1s ease-out;
}
#heart::before, #heart::after {
  position: absolute;
  content: "";
  left: 50px;
  top: 0;
  width: 50px;
  height: 80px;
  background: red;
  border-radius: 50px 50px 0 0;
  transform: rotate(-45deg);
  transform-origin: 0 100%;
}
#heart:after {left: 0;transform: rotate(45deg);transform-origin :100% 100%;}
#heart:hover{transform: scale(1.2);}
```

这个例子看到了一个新的属性 `transform-origin` 可以用来控制元素的旋转基点。x轴、y轴两个方向，可以使用top、left、right、bottom、%来设定基点位置。除了百分数其它的四个都是位置。设定一个就是对应的边的中点



## animation

> @keyframes关键祯 是专门用来做动画的，它可以指定具体到某一帧的状态是什么样子的，以整数百分比来指定帧数，再给定CSS属性，就组成了一组状态的变化。

```css
@keyframes move{
  from{width: 100px;} 
  to{width: 200px;}
}

@keyframes move{
  0%{width: 100px;}
  50%{width: 150px;}
  100%{width: 200px;}
}
```

同样在不同的浏览器如果没有效果的记得加前缀

> @-webkit-keyframes
>
> @-moz-keyframes
>
> @-ms-keyframes
>
> @-o-keyframes

`@keyframes` 是制作动画的过程，`animation` 就是一台放映机，规定动画的播放时间、速率、延迟、次数、反向轮播。

*example1*

```html
<style>
.view {
  background-color:black;
  width:500px;
  height:20px;
  margin: 100px auto;
  overflow: hidden;
}

.eye {
  color:white;
  height: 100%;
  width: 25%;
  background-color: red;
  background-image: -webkit-linear-gradient(left, rgba( 0,0,0,0.9 ) 25%, rgba( 0,0,0,0.1 ) 50%, rgba( 0,0,0,0.9 ) 75%);
  animation: move 4s linear 0s infinite alternate;
}
  
@keyframes move {
  from {margin-left:-20%;}
  to {margin-left:100%;}
}
</style>
<body>
  <div class="view">
    <div class="eye"></div>
  </div>
</body>
```



## 3D

两个属性

* perspective: 800
* transform-style: preserve-3d;

景深的大小决定的是元素距离你的视距的长短

*example*  翻书demo 

```html
<style>
  .list{width: 400px;height: 200px;background-color: white;margin: 100px auto;position: relative;-webkit-perspective: 800;transform-style: preserve-3d;transform: rotateX(30deg);}
  .list li{position: absolute;width: 200px;height: 195px;top: 2px;background: white;left: 200px; transform-origin: left;text-align: center;line-height: 195px;}
  .list li:nth-child(1){
    animation: move 3s linear infinite;
  }

  .list li:nth-child(2){
    animation: move 3s linear 1.5s infinite;
  }

  @keyframes move{
    from{ transform: rotateY(0deg);}
    to{ transform: rotateY(-180deg);}
  }
</style>
<body>
  <ul class="list">
    <li>我爱你</li>
    <li>么么哒</li>
  </ul>
</body>
```

