---
title: font字体
tags: CSS
categories: html&css
abbrlink: 65362
date: 2017-11-18 19:14:49
thumbnail: /img/css/float.png
comments: false
---

![font](http://htm5.oss-cn-beijing.aliyuncs.com/175576/1511003918466.jpg)

<!-- more -->

# CSS属性值 字体与文本

> 网页设计中有很多的文字要去处理，标题、段落、文章、列表以及表单中的文本。这一篇章我们讨论一下*HTML*中的字体与文本



## 字体

> 首先要有一个认识字体和文本不是一个东西哦。字体是不同的文本体式或者可以说是字的形体结构。对于英文来说有很多种不同的样式包括字母、数字和符号组成的。
>
> 文本指的是通过文本属性描述对文本的处理方式。行高，字符间距，缩进等。
>
> 那么网页中的字体是哪里来的呢？其中有哪些属性？文本属性中有哪些小秘密呐？



### 来源

* 用户机器中安装的字体
* 保存在第三方网站上的字体（link）
* 保存在服务器上的字体。这些字体可以使用@font-face规则随网页一起发给浏览器（一般字体图片都会放到一个单独的服务器上，更加的优化）



## 字体属性

### font-family

> 字体族

font-family用于设定元素中的文本使用什么字体。通常给一个文档页面设置一个主字体（因为`font-family`是可以继承的），然后针对那些需要使用不用样式的字体在单独应用font-family。

因为字体来源我们已经知道了，两条路径要么是用户机器，要么是网上，那么就存在某种字体不能再某个网页中使用的可能。所以需要给出一组字体，这组字体叫做**字体栈** 。 

简单的来说就是就是预备队，如果用户机器上没有某种字体，预备的字体就用作用了，用户还可以使用另一种字体阅读。

```css
/* 
	font-family的值不区分大小写 但是如果引入的是在线字体库请不要随意修改
	有可能导致无法使用提供的定制字体
*/
body{font-family: "Helvetica Neue", Helvetica, Arial, sans-serif；}
```



### font-size

> 字体大小

浏览器样式表默认为每个HTML元素都设定了font-size，言外之意是我们在设置font-size的时候其实是在修改默认值。字体大小的单位px、%、em。但是有一个很重要的点是字体的大小也是可以去继承的这个地方可能会出现一些未知的错误。

`px`是一个很常见的单位，`em`也是一个单位有什么区别呢？

`px`绝对单位，`em`是一种相对单位与百分比是一样的，浏览器默认样式表在设定所有元素的字体大小时使用的都是相对单位em，h1被设定为2em，h2是1.5em，p是1em。默认情况下1em = 16px。这也是font-size的基准大小。

![font-size](/img/css/font-size.png)

*example 1* 

```html
<style>
  /* h1此时2em */
  body{font-size: 200%;}
  /* h1此时应该是多少呢？ */
  body{font-size: 20px;}
  p{font-size: 16px;}
</style>
<body>
  <h1>我是小祺</h1>
  <p>font-size</p>
</body>
```

`tip`：其它的以绝对单位设定的会重新设定字体的大小，不会产生继承。同时我们在设定的时候可以选择关键字值，比如`x-small`、`medium` 、`x-large` 等等 ，当然用的很少，你会在浏览器看到medium感兴趣的可以去观察一下吧。



*example 2*

```html
<style>
  /* span的字体大小是多少呢? */
  p{font-size： 80%;}
</style>
<body>
  <p>我是<span>小祺</span></p>
</body>
```



*example 3*

```html
<style>
  /* h1的字体大小是多少呢？ */
  /* span的字体大小是多少呢? */
  body{font-size: 150%;}
  p{font-size： 80%;}
</style>
<body>
  <h1>我是小祺</h1>
  <p>我是<span>小祺</span></p>
</body>
```

> 答案可以写在评论 ๑乛◡乛๑

`tip`：使用绝对单位的好处是，在祖先元素的字体大小变化时，不会出现意外的连锁反应。



### font-style

> 字体样式

| 值       | 描述                                |
| ------- | --------------------------------- |
| normal  | 默认值。浏览器显示一个标准的字体样式。               |
| italic  | 浏览器会显示一个斜体的字体样式。（斜体代表强调含义所以还是用em） |
| oblique | 浏览器会显示一个倾斜的字体样式。                  |
| inherit | 规定应该从父元素继承字体样式。                   |



### font-weight

> 字体粗细

这个貌似没什么好说的，还是过了吧。主要一点最好使用`bold`、`normal` 当然`strong`标签是加粗的状态，你们应该懂我的意思。



### font-variant

> 字体变化

| 值          | 描述                            |
| ---------- | ----------------------------- |
| normal     | 默认值。浏览器会显示一个标准的字体。            |
| small-caps | 浏览器会显示小型大写字母的字体。              |
| inherit    | 规定应该从父元素继承 font-variant 属性的值。 |



### font

> 简写，复合写法
>
> 强调两个规则

**rule** 

* 必须声明size与family。
* 顺序 font-weight、font-style、font-variant随意调换，其次font-size，font-family。
* 同时还可以设置行高 font:bold italic small-caps  16px/1.5 'Microsoft yahei'  16px/1.5的这个1.5代表的是倍数。



## 文本属性

> 字体了解之后，接下来我们了解一下文本属性。缩进、角标、间距、排版等。



### text-decoration

> 文本装饰

| 值            | 描述                               |
| ------------ | -------------------------------- |
| none         | 默认。定义标准的文本。                      |
| underline    | 定义文本下的一条线。                       |
| overline     | 定义文本上的一条线。                       |
| line-through | 定义穿过文本下的一条线。                     |
| blink        | 定义闪烁的文本。                         |
| inherit      | 规定应该从父元素继承 text-decoration 属性的值。 |



### text-transform

| 值          | 描述                              |
| ---------- | ------------------------------- |
| none       | 默认。定义带有小写字母和大写字母的标准的文本。         |
| capitalize | 文本中的每个单词以大写字母开头。（仅针对首字母）        |
| uppercase  | 定义仅有大写字母。                       |
| lowercase  | 定义无大写字母，仅有小写字母。                 |
| inherit    | 规定应该从父元素继承 text-transform 属性的值。 |



#### text-indent

> 文本缩进，设置文本的起点，一般我们进行首行缩进。

```css
p{text-indent: 2em;}
```

`tip`：这个玩意也会继承。

*example*

```html
<style type="text/css">
    .box{text-indent: 2em;width: 400px;border: 1px solid #333;}
</style>
<body>
    <div class="box">
        一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题
        <p>一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题</p>
        <p>一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题</p>
        <p>一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题一个很重要的问题</p>
    </div>
</body>
```



### text-algin

> 文本对齐方式

| 值       | 描述                   |
| ------- | -------------------- |
| left    | 把文本排列到左边。默认值：由浏览器决定。 |
| right   | 把文本排列到右边。            |
| center  | 把文本排列到中间。            |
| justify | 实现两端对齐文本效果           |



### letter-spacing

> 字符间距

这个貌似也没啥说的还是过了吧。。。๑乛◡乛๑



### word-spacing

> 单词间距，亲啊一定要区分清楚啊。 Hi girl！我只能说很少用，毕竟我们不写英文。



### line-height

> 行高，数字。
>
> 在css里边最tm难理解的两个东西它就是其中一个。尴尬...

先来个图！

![ji](/img/css/jixian.png)

> green：top（顶线）
>
> blue：middle（中线）
>
> red：baseline（基线）
>
> pink：bottom（底线）



*example* 

```html
<style>
  /* 
  	设置字体我们可以同时设置行高，意思就是可以采用数字 2的含义是基于字体大小的倍数 
  	通常我们不会这样子写，因为会设置字体的大小，如果是浏览器默认的值当然可以这样子写。
  */
  /*
  	css中行高平均分布在一行文本的上下。
  	这里字体的默认大小是16px，line-height为2那么就是32px,多出来的	
  	16px怎么会在文本的上下各填充8px。但是这样子好像有点... 直接line-height: 32px多好费劲 ⊙︿⊙.
  */
  .p{width: 200px;height: 32px;text-align: center;line-height: 2;border: 1px solid red;}
</style>
<body>
  <p>行高，没错就是我</p>
</body>
```



### vertical-align

> 垂直对齐 以基线为参照上下移动文本，但是这个玩意只能影响行内元素
>
> 在css里边最tm难理解的另一个。

*example*

```html
<style>
	p sub{font-size:60%; vertical-align:-.4em;}
</style>
<body>
	<p>H<sub>2</sub>O. 10<sup>5</sup></p>  
</body>
```

`tip`：vertical-align通常用来让两个行内元素对齐，虽然我们有时候设置了大小行高，行内的元素有时候也无发正常居中毕竟line-height是对文字的。这里line-height与vertical-align大家知道怎么去使用碰到问题解决问题，不要深究，**会死**。



## last

最后给大家看一下字体与文本的结合，英文的排版样式布局。欣赏一下！

```css
@import url(http://fonts.googleapis.com/css?family=Pinyon+Script);
*{margin: 0;padding: 0;}
body{
    font-family:"CrimsonRoman", georgia, times, serif;
    background-color:#fff;
    margin:100px 10% 0;
}

/* 设置h2的字体 文本*/
h2{
    font-size:18px;
    line-height:24px;
    font-weight:bold;
    text-align:center;
    font-variant:small-caps;
    word-spacing:.5em;
    letter-spacing:.6em;
}

/* 设置h1的字体 文本 */
h1{
    font-size: 60px;
    line-height:96px;
    font-family:"Pinyon Script", cursive;
    margin:4px 0 -4px;
    text-align:center;
    font-weight:normal;
}

p {
    font-size:18px;
    line-height:24px;
}

/* 设置长引用的边距 */
blockquote {margin:0px 20%;}

/* 设置引用字体的样式 */
q {
    font-size:18px;
    font-style:italic;
    line-height:24px;
}

/* 首字母下沉 */
h1 + p::first-letter {
    font-family:times, serif;
    font-size:90px;
    float:left; 
  	line-height:.65;
    padding:.085em 3px 0 0;
}

/* 修改第一行的文本 小型大写字母 */
h1 + p::first-line {
    font-variant:small-caps;
    letter-spacing:.15em;
}

/*只缩进跟在段落后面的段落*/
p + p {text-indent:14px;}
/*引用内容前面的引号*/
q::before {content:"\201C"}
/*引用内容后面的引号*/
q::after {content:"\201D"}
```



```html
<!-- q标签 文本两端插入引号 -->
<!-- blockquote标签 标记长引用 会给元素的前后添加边距 -->
<h2>an excerpt from</h2>
<h1>The Hound of the Baskervilles</h1>
<p>Holmes stretched out his hand for the manuscript and flattened it upon his knee. &ldquo;You will observe, Watson, the alternative use of the long s and the short. It is one of several indications which enabled me to fix the date.&rdquo; At the head was written: &ldquo;Baskerville Hall,&rdquo; and below in large, scrawling figures: &ldquo;1742.&rdquo;</p>
<p>&ldquo;It appears to be a statement of some sort.&rdquo;</p>
<p>&ldquo;Yes&mdash;it is a statement of a certain legend which runs in the Baskerville family.&rdquo;</p>
<blockquote>
  <q>Of the origin of the Hound of the Baskervilles there have been many statements, yet as I come in a direct line from Hugo Baskerville, and as I had the story from my father&hellip;</q>
</blockquote>
<p>When Dr. Mortimer had finished reading this singular narrative he pushed his spectacles up on his forehead and stared across at Mr. Sherlock Holmes.</p>
```



## Q&A

```html
<input type="button" value="按钮">
<input type="button" value="按钮">
<input type="button" value="按钮">

<button>6</button>
<button>6</button>
<button>6</button>

<div style="border: 1px solid #333;width:300px;">
  <img src="url" width="300">
</div>
```

> 发现问题了吗?  行内元素在换行显示之后回车被html解析了。

```html
<input type="button" value="按钮"
><input type="button" value="按钮"
><input type="button" value="按钮">

<button>6</button><button>6</button><button>6</button>  

<div style="font-size:0">
  <button>6</button>
  <button>6</button>
  <button>6</button>
</div>

<div style="border: 1px solid #333;width:300px;font-size:0;">
  <img src="url" width="300" style="display:block">
</div>
```

