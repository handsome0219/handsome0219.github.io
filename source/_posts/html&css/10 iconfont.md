---
title: 阿里图标
tags: HTML+CSS
categories: html&css
abbrlink: 10583
date: 2017-11-30 15:16:07
comments: false
---

![阿里图标](/img/css/ali-icon.png)

<!-- more -->



# 阿里图标

> 阿里矢量图标不是图片，是一种字体。



## 优点

* 可以通过font的css样式美化修改图标，不需要考虑背景位置（图片、雪碧图需要）。
* 放大缩小屏幕的时候不会失真。因为只需要修改字体大小就可以，如果使用图片的时候我们就可能需要一大一小两张图片了。
* 加载字体文件和图片文件相比较，字体的体积更加的小，而且图片自身也需要加载时间。
* 相比较于图片，图片不能很好地进行缩放，当图片进行放大时会失真（即变模糊），当图片进行缩小时就会浪费掉像素。而且加载张图片都需要发起请求，因此也拖慢了整个加载页面的时间。如果需要对图片进行编辑、处理等操作，没有专门的图片工具也很不好去修改。




## 使用

阿里图标的使用有三种形式unicode、font-class、symbol。首推Unicode毕竟官网也是这么推荐的。三种的兼容性略有不同分别支持的范围IE6+、IE8+、IE9+。

**在使用前请务必把所需要的图标添加到自己的项目文件夹。** 一般来不推荐下载使用，推荐在线使用切换，修改相比较都容易。

* unicode引用

  使用font-face引入在线的阿里图片链接，并且设定iconfont样式，最后挑选对应图片的unicode代码（类似&#xe666;）。

  ```html
  <style>
    /* 这里的地址需要在项目文件夹中生成！ */
    @font-face {font-family: 'iconfont';
      src: url('iconfont.eot');
      src: url('iconfont.eot?#iefix') format('embedded-opentype'),
        url('iconfont.woff') format('woff'),
        url('iconfont.ttf') format('truetype'),
        url('iconfont.svg#iconfont') format('svg');
    }

    .iconfont{
      font-family:"iconfont" !important;
      font-size:16px;font-style:normal;
      -webkit-font-smoothing: antialiased;
      -webkit-text-stroke-width: 0.2px;
      -moz-osx-font-smoothing: grayscale;
    }
  </style>
  <body>
    <!-- 推荐使用i标签，做为图标标签 切记加上iconfont类名 -->
    <i class="iconfont">&#xe605;</i>
  </body>
  ```

* font-class

  与unicode的区别就是在于语义明确的程度，使用同样在我的项目文件中生成css链接

  ```html
  <head>
    <!-- 这里的地址需要在项目文件夹中生成！-->
    <link rel="stylesheet" href="http://at.alicdn.com/t/font_8d5l8fzk5b87iudi.css">
  </head>
  <body>
    <!-- 推荐使用i标签，做为图标标签 切记加上iconfont类名 -->
    <i class="iconfont icon-xxx"></i>
  </body>
  ```

* symbol

  symbol的引用和前两者最大的区别其实在于颜色，但是只支持高级浏览器以及IE9+。当我们需要使用多色图片的时候可能需要使用多个图片互相组合。上面的两种也只能使用一些css颜色样式不能做到多色的图片，那么这个时候就需要使用到symbol（svg）

  同样在我的项目中生成对应的 `js` 链接。看清楚了哦js文件，在style中添加样式，固定格式添加。如果觉得小可以在项目中修改图标的大小。

  ```html
  <style type="text/css">
    .icon {
      width: 1em; height: 1em;
      vertical-align: -0.15em;
      fill: currentColor;
      overflow: hidden;
    }
  </style>
  <script src="http://at.alicdn.com/t/font_8d5l8fzk5b87iudi.js"></script>
  <body>
    <svg class="icon" aria-hidden="true">
      <use xlink:href="#icon-xxx"></use>
    </svg>
  </body>
  ```



`tip`：三种方式各有优缺点。但是需要注意的是每次往项目中添加了新的图标，无论上述哪一种都需要重新生成一次链接，否则新添加的图片是不会有效果的。



## 下载

当我们的项目的结束的时候所有的图标都确定了之后我们可以选择下载，在过程中或许会去调整图片每次都下载是一件很麻烦的事情，所以在没有结束，没有确定所需要的图标的时候，推荐使用在线的图标。方便修改替换。