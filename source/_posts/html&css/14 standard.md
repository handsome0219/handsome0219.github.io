---
title: 规范说明
tags: HTML+CSS
categories: html&css
abbrlink: 21179
date: 2017-12-05 23:36:07
thumbnail: http://img-cdn2.luoo.net/pics/vol/52c14db094079.jpg!/fwfh/640x452
comments: false
---

![layout](http://img-cdn2.luoo.net/pics/vol/52c14db094079.jpg!/fwfh/640x452)

<!-- more -->


# layout

> 上一篇内容相信大家应该掌握了，了解css中的属性值及其特性，分析问题与需求选择合适的解决方案。这一节讨论一下规范。


## HTML规范

html的书写相对来说想写错真的太难了但是同样我们需要一个规范。无论从团队配合还是约定内容都是必须要遵守的。

### 代码规范

* 声明html5的doctype

* 所有的html代码都使用小写

* 页面编码采用gbk，在系统允许的前提下可以使用utf-8

* 页面内容lang=`zh-Hans-CN` 以前的zh-CN已经被废除，当然废除不代表不生效了。

* 在head中添加对页面相关人员团队的注释

  ```html
  <!-- 页面设计：xxx | 页面制作：xxx | 团队博客：http://tgideas.qq.com/ -->
  ```

* 内容性质的图片必须加上alt属性，修饰性的图片可以不加，width与height值为原始大小

* 注释的写法 IE条件注释

  ```html
  <!-- [if IE]>
  这里只有ie浏览器才可以显示
  <![endif]-->
   
  <!-- [if !IE]>
  这里只有非ie浏览器才可以显示
  <! <![endif]-->
   
  <!--[if IE 6]>
  这里只有ie6浏览器才可以显示
  <![endif]-->
   
  <!--[if lt IE 9]>
  这里只有ie9以下浏览器才可以显示
  <![endif]-->
   
  <!--[if lte IE 8]>
  这里只有ie8以及ie8以下浏览器才可以显示
  <![endif]-->
  ```

  对注释中的空白区域用等号替换

  ```html
  <!-- header======header -->
  ```

* 对于需要转义的字符使用转义字符 `&lt; ` `&gt;` 等等

* 单标签不要写 `/` 闭合，声明为html5页面会自动处理 img、br、hr、input

* 元素嵌套段落标签嵌套内联标签，块级标签嵌套内联标签

* 对标签的特性熟悉对应的内容使用怎样的标签，及对网页标签的合理应用



## CSS规范

### 命名规范

+ ID 在页面中具有唯一性，也就是说以ID做为选择器来写css就无法重用，使用类名选择器定义样式，避免使用ID定义。



### 以字母开头

* 不允许单个字母的类选择器出现，单词名字使用有意义的名字，结构化的名字与作用相关的名字
* 不允许命名带有广告等英文的单词，例如ad,adv,adver,advertising，已防止该模块被浏览器当成垃圾广告过滤掉。
* 全部小写，使用 `-` 连接多个单词的class，不要使用下划线，禁止使用驼峰



### 文件名

* 基本样式 `base.css`

* 全局样式 `global.css`

* 布局样式 `layout.css`

* 字体样式 `font.css`

  ​

### 常用名

整页`.wrap` 页眉`.header` 页脚 `.footer` 导航 `.nav ` 主体内容 `.main`  侧边栏`.side`  标志 `.logo` 搜索 `.search` 登录 `.login`  注册 `.reg` 标题 `.tit` 按钮`.btn-`  背景图片 `.bg-`  列表 `.list-` 表格 `.tb-` 标签 `.tag-`  视频 `.video-` 联系 `.contact`



### 书写风格

+ 单个css选择器或新申明开启新行

  ```css
  .container,
  .header,
  .home,
  .nav{ font-size: 12px}
  ```

  ​

+ 内容属性顺序

  ```css
  /*  这些属性只是最常用到的, 并不代表全部 */

  /* 布局定位属性 */
  display / list-style / position / float / clear  / visibility / overflow

  /* 自身属性 */
  width / height / margin / padding / border / background

  /* 文本属性 */
  color / font / text-decoration / text-align / vertical-align / white- space / break-word

  /* 其他（CSS3）  */
  content / cursor / border-radius / box-shadow / text-shadow / background:linear-gradient
  ```

  ​

+ 属性时候换行随意个人习惯，最后都要被压缩，写一个属性后边冒号空一格 width: 100px

+ 尽可能利用css控制交互视觉变换JS操作的只需要添加删除类名

+ 注释

  ```
  /*=====头部=====*/
  .header {background-color: #333;height: 100px;}
  /*=====头部结束=====*/
  ```

  ​

## last

这几天的练习抓住几个点

* 布局看高度颜色，间距看文字距离。
* 类名组合写法
* 一个页面不要很多的父级元素 虽然说会产生一些代码嵌套 但是总的来说结构会比较清晰
* 类名组合使用
* 结构是结构 样式是样式
* 有时候可能会改变标签书写顺序，导致增加一些标签。考虑到以后可能的改变利大于弊
* 最后一定要细心 出了问题 别着急了 重新打开一个备份 标签是否闭合 可能会导致浏览器解析的时候出错
* 掌握每个css属性的特性，特性使然

讲道理其实就是一个慢工出细活，如果这点耐心都没有是不行的哦。么么哒~