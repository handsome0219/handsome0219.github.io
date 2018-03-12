---
title: background
tags: CSS
categories: html&css
abbrlink: 17546
date: 2017-11-20 19:05:34
comments: false
---

![background](http://www.wzsky.net/img2015/uploadimg/20150902/10060221.jpg)

<!-- more -->



# background

> 背景（background）
>
> 每一个盒子元素都是有图层组成，前景包含文本图片边框，元素的背景层可以使用颜色填充，背景图片层在元素的背景层之上。
>
> 简单的来说在css model中我们用一个图层的概念来理解背景。文本（图片或边框）-- 背景颜色 -- 背景图片



## 背景属性

### background-color

> 背景颜色

没什么说的。只需要思考一个问题，为什么不直接`background:red` 而要 `background-color:red`



### background-image

> 背景图片

和上边的问题一致。



### background-repeat

> 背景重复

控制背景重复的属性有四个值。*repeat*（默认） 、*repeat-x*、*repeat-y*、*no-repeat* 

![repeat](/img/css/bg-repeat.png)

默认重复，x方向重复，y方向重复，不重复。但是在使用的时候可能会碰到一些头疼的情况。比如容器尺寸和背景图片的个数恰好会让边界的背景显示的不全。CSS3提供了另外的两个属性`round`、`space`

* round：确保图片不被剪切，调整图片的大小适应背景区域。
* space：确保图片不被剪切，添加空白来使用背景区域。





### background-size

> 背景尺寸

CSS3规定的属性，用来控制背景图片的尺寸。

```html
<style>
  .bg{
    width: 500px;height: 300px;margin: 100px auto;background-image: url(xxx.jpg);
    background-size: cover;
    /* background-size: contain; */
    /* background-size: 50%; */
    /* background-size: 100% 50%; */
    /* background-size: 500px 300px; */
  }
</style>
<body>
  <div class="bg"></div>
  <div class="bg"></div>
  <div class="bg"></div>
  <div class="bg"></div>
</body>
```

* cover：拉伸，完全填满背景区，保持宽高比。
* contain：缩放，**恰好**适应背景区，保持宽高比。
* 50%：缩放，填充一半，保持宽高比。（给两个就不会保持了）
* 500px 300px：宽度500px，高度300px。





### background-position

> 背景位置

控制背景图片的位置，css中提供了top、left、right、bottom、center属性，当然也可以使用px去调整。雪碧图运用。

可以去组合

*example* 

```css
/* 需要注意的是 top right与right top 是没有区别的 */
/* 只设置一个center代表水平，另一个方向会设置成center所以center center的组合和写一个center是一样的 */
/* 100px 200px则是距离div左上角的位置 left 100px top 200px位置 */
div{background-image: url(url);background-position: center;}
div{background-image: url(url);background-position: top left;}
div{background-image: url(url);background-position: right bottom;}
div{background-image: url(url);background-position: 100px 200px;}
```





### background-attachment

> 背景粘附

默认属性是scroll，即背景图片不会随着元素滚动。设置成fixed，背景图片将不会随着页面滚动。

`tip`：一般用来设置水印，不随着元素的滚动而滚动。



### background

> 简写

```html
<style>
  body{
    background-image: url(src);
    background-color: #fff;
    background-position: center;
    background-repeat: no-repeat;
    background-size: cover;
    background=attachment: fixed;
  }
  
  /* 相当于 */
  background: color  url(src)  no-repeat  fixed center / cover ;
</style>
<body>
  
</body>
```

`tip`：background可以设置多个背景图片以逗号分割。



## 背景渐变

> 渐变就是在一定的范围内让颜色自然的过渡。以前是不支持的所以这个是CSS3中的属性。
>
> 分为两种一种是线性渐变（linear-gradient），一种是放射性渐变（radial-gradient）。



### 线性渐变

> 线性渐变从元素的一端延伸到另外一端

*example*

```html
<style>
  /* 默认从上到下 */
  div{width: 300px;height: 300px;margin: 20px auto;border: 1px solid #ddd;}
  .bg1{background: linear-gradient(#DC2EC5, #879BEA)}
  .bg2{background: linear-gradient(red, black)}
  .bg3{background: linear-gradient(#ff6600, #6CD236)}
  
  /* 同时可以使用rgba也可以 */
  .bg3{background: -webkit-linear-gradient(left, rgba(255,255,255,.5), rgba(100,200,150,.5))}
  
  /* 使用百分号也是可以的 百分号代表的是位置 */
  .bg3{background: -webkit-linear-gradient(left,#e86a43, #fff 25%, #64d1dd 25%, #64d1dd 75%,
#fff 75%, #e86a43);}
  
  .bg4{background: linear-gradient(45deg,#ff6600, #6CD236)}
  .bg5{background: linear-gradient(left,#ff6600, #6CD236)}
  
  /* 其实我们在设置渐变色的时候也可以设置背景图片,因为没有找到合适的图。。所以就没做出好看的 */
  .class{
    background: -webkit-linear-gradient(right, rgba(255,55,255,0), rgba(255,255,255,1)),url(src) right top no-repeat;
  }
</style>
<body>
    <div class="bg1"></div>
    <div class="bg2"></div>
    <div class="bg3"></div>
  	<div class="bg4"></div>
  	<div class="bg5"></div>
</body>
```



`tip`：可以看到在bg5中需要加-webkit去兼容要不然会不起作用，在很多css3的属性中需要去声明，谷歌（webkit），火狐（moz），opera（o），IE比较特殊（ms）低版本的IE9（包括9）使用。

```css
body{
  filter: progid:DXImageTransform.Microsoft.gradient(GradientType=0, startColorstr=#fff000, endColorstr=#333333);
}
```

渐变色还有很多不一样的用法有兴趣可以自己去尝试。





### 放射性渐变

> 放射听到这个词就应该知道的是发散状态，区别很显然。

*example*

```html
<style>
  /* 
  	默认的渐变形状，即渐变效果会填充元素，这里的元素是矩形。如果元素是正方形，那渐变就是圆形。 
  */
  .bg1 {
    background: -webkit-radial-gradient(#fff, #64d1dd, #70aa25);
  }
  
  /* 
  	设定形状cirle,渐变形状变得均匀 (其实没看出来很大的区别)
  */
  .bg2 {
    background: -webkit-radial-gradient(circle, #fff, #64d1dd, #70aa25);
  }
  
  /*
  	自定义位置
  */
  .bg3 {
    background: -webkit-radial-gradient(50px 30px, circle, #fff, #64d1dd, #4947ba);
  }
</style>
<body>
  <div class="bg1"></div>
  <div class="bg2"></div>
  <div class="bg3"></div>
</body>
```



