---
title: position定位
tags: CSS
categories: html&css
abbrlink: 55465
date: 2017-11-28 23:51:23
thumbnail: http://htm5.oss-cn-beijing.aliyuncs.com/175576/1511886634890.jpg
comments: false
---

![position](http://htm5.oss-cn-beijing.aliyuncs.com/175576/1511886634890.jpg)

<!-- more -->

`ps`：图片和内容无关只是觉得好看，找不到图了。。。



# 定位

> CSS中定位是布局中经常会用到的。需要大家对文档流有一定的了解。



## position属性

- static，默认值。位置设置为static的元素，它始终会处于文档流给予的位置。
- absolute，生成绝对定位的元素，**相对于距该元素最近的已定位的祖先元素**进行定位。此元素的位置可通过 “left”、”top”、”right” 以及 “bottom” 属性来规定。
- relative，生成相对定位的元素，**相对于该元素在文档中的初始位置进行定位**。通过 “left”、”top”、”right” 以及 “bottom” 属性来设置此元素相对于自身位置的偏移。
- fixed，生成绝对定位的元素。默认情况下，可定位于**相对于浏览器窗口**的指定坐标。元素的位置通过 “left”, “top”, “right” 以及 “bottom” 属性进行规定。不论窗口滚动与否，元素都会留在那个位置。
- sticky, 粘性定位(relative+fixed)。在一定阈值下是 `relative` 超过了变成 `fixed` 。

**不管是哪种定位，都必须有一个参照物。找对了参照物，就成功了一半。**



## 相对定位

*relative* 给定的元素，会相对与原来的正常位置进行定位。使用top、left、right、bottom来偏移。 

`tip`：相对定位的元素会保留原来的位置，并且relative的元素没有脱离文档流，毕竟还占据原来的位置。



## 绝对定位

*absolute* 给定的元素，与相对定位的元素最大的区别就是，这个元素将**脱离文档流的束缚**，其它的元素会认为这个元素不存在而去填充它原来的位置，绝对定位的元素会参照定位上下文（定位父级） 来移动自己的位置。如果没有设定定位父级默认就是body。

设定了定位父级按照最近的父级做为参照。

```html
<div class="box1" style="position: relative">
  <div class="box2" style="position: absolute">
    <div class="box3" style="position: absolute">
        box3的定位父级是最近的box2 不是box1
    </div>
  </div>
</div>
```

*over* 遮罩层 

> 注意点：这里设定的宽度高度是根据window的窗口大小来的，并不是body和html的大小，因为body和html的默认的大小是根据内容来撑开的。通过iframe设定高度可以得到验证。设置z-index遮罩层的层级一定是最高的所以一般可能会看到一些网站的遮罩层会设定99或者999当然这要能让它显示在最上边就好了。

```html
<style>
  .over{position: absolute;top: 0;left: 0;z-index: 99;width: 100%;height: 100%;}
</style>
<body>
  <div class="over"></div>
</body>
```

多个元素设定absolute时候会发生重叠，与单个元素设定定位不一样（与relative一样，暂时保持当前位置，只是脱离了文档流的束缚）。一些特定的场景需要设定absolute，比如二级菜单，banner等等。



## 固定定位

*fixed* 给定的元素，定位上下文始终是浏览器窗口，不会随着body的移动而改变位置（这里讲错了，上课的时候说的是body，但是仔细想一下如果是相对与body的话，那么body在滚动的时候肯定也会发生位置的改变，注意一下）。

一般需要固定在页面上的元素，不会随着页面的滚动而发生偏移的元素，请使用 *fixed* 定位。



## 粘性定位

*example*

```html
<style>
  .wrap{width: 400px;height: 300px;margin: 30px auto;overflow: auto}
  dl {
    margin: 0;
    padding: 10px 0 0 0;
  }

  dt {
    background: #B8C1C8;
    border-bottom: 1px solid #989EA4;
    border-top: 1px solid #717D85;
    color: #FFF;
    font: bold 18px/21px sans-serif;
    margin: 0;
    padding: 2px 0 0 12px;
    position: -webkit-sticky;
    position: sticky;
    top: -1px;
  }

  dd {
    font: bold 20px/45px sans-serif;
    margin: 0;
    padding: 0 0 0 12px;
    white-space: nowrap;
    border-top: 1px solid #CCC
  }
</style>
<body>
  <div class="wrap">
    <dl>
      <dt>A</dt>
      <dd>Andrew W.K.</dd>
      <dd>Apparat</dd>
      <dd>Arcade Fire</dd>
      <dd>At The Drive-In</dd>
      <dd>Aziz Ansari</dd>
    </dl>
    <dl>
      <dt>C</dt>
      <dd>Chromeo</dd>
      <dd>Common</dd>
      <dd>Converge</dd>
      <dd>Crystal Castles</dd>
      <dd>Cursive</dd>
    </dl>
    <dl>
      <dt>E</dt>
      <dd>Explosions In The Sky</dd>
    </dl>
  </div>
</body>
```

测试下就明白了



## 层级顺序

> 页面文档中的元素会按照一定的规则去显示。

![层级顺序](http://on-img.com/chart_image/5857e077e4b04ce3879eca88.png)

内容是页面的很重要的实体，因此层叠水平较高。



*example* 

```html
<style>
  div{width: 200px;height: 100px;font-size: 20px;}
  .a{background: red;}
  .b{background: blue;margin: -35px 0 0 10px;}
</style>
<body>
  <div class="a">box1</div>
  <div class="b">box2</div>  
</body>
```

上边的例子可以看到一些现象

1. 盒子 box2 会挡住盒子 box1 ，元素产生了覆盖，颜色的覆盖。页面中的排布遵循元素排布规则。相同的元素后来居上。
2. 文字覆盖也是后来居上的。



*example* 

```html
<style>
  div{width: 200px;height: 100px;font-size: 20px;}
  .a{display: inline-block;background: red;}
  .b{background: blue;margin: -35px 0 0 10px;}
  .c{background: black;}
  .d{display: inline-block;background: green;margin: -35px 0 0 10px;}
</style>
<body>
  <div class="a">box1</div>
  <div class="b">box2</div>
  <div class="c">box3</div>
  <div class="d">box4</div>
</body>
```

当设定了元素的显示属性的时候按照上边的层叠规则可发现，此时元素没有按照后来居上的原则来了。这里还可以加一个浮动元素去验证。

1. 此时盒子之间的覆盖，背景颜色的覆盖始终都是遵循层叠顺序。
2. 文字还是后来居上。再次强调一下文字是内容实体很重要。



### 关于定位元素的层叠顺序

> 首先要了解两个东西

1. 层叠上下文：页面根节点（body），是根层叠上下文（参考对象），请把它理解成容器，容器就是盛放物品的，物品在其中是有层叠顺序的。
2. 层叠水平：层叠上下文的元素，以及普通的元素都具有层叠水平。层叠水平决定了元素在同一个层叠上下中显示的顺序（学生和老师之间的距离）。



*example1*

```html
<style>
  .outer{width: 300px;height: 300px;border: 1px solid red;}
  .inner{width: 200px;height: 100px;position: absolute;/* float: left */}
</style>
<body>
  <div class="outer">
    <div class="inner"></div>
    <!-- 很多的文字 -->
  </div>
</body>
```

文字被inner元素所覆盖，因为设定了定位的元素具有默认的z-index： auto。和浮动比较，两者遵循层叠顺序 z-index： auto 的元素会排列在float元素之上。



*example2* 

```html
<style>
  .outer{width: 100px;height: 150px;position: relative;background: red;/* z-index: 1; */}
  .inner{width: 200px;height: 100px;position: absolute;z-index: -1;background: black;}
</style>
<body>
  <div class="outer">
    <div class="inner"></div>
  </div>
</body>
```

给inner设定了z-index：-1时inner元素跑到了outer元素的后边。遵循负值z-index排列在block元素之后。

如果此时给父级元素设定z-index: 1;这里任意数值，会发现outer显示到了后边，这个时候再去调整inner的z-index值就算再大也不会超越outer的显示层级。什么原因呢？

`tip`：定位元素具有z-index默认值auto，但是auto不会是元素成为层叠上下文。设定数值之后定位元素就会成为层叠上下文，inner此时在outer中就是遵循层叠顺序来排列，outer即变成了盛放内容的容器了。inner设定的z-index只是会影响outer内部的层叠水平。不会影响影响与outer之间的层叠顺序。

*example3* ：最后一点就不举例子了。当有互相嵌套的时候并且父级也有z-index值的时候，层叠顺序的比较止步于父级的层叠上下文（父级在其层叠上下中的排列顺序）。



总结：

1. 定位元素默认具有z-index：auto（可以看成0）。
2. z-index：auto不会创建定位上下文。
3. z-index层叠顺序的比较止步与父级层叠上下文。





## last

打完收工！有问题留言补充，再修改。晚安，么么哒~