---
title: form
tags: HTML
categories: html&css
abbrlink: 55499
date: 2017-11-15 16:38:29
comments: false
---

![表单](http://static.open-open.com/news/uploadImg/20140420/20140420182130_120.jpg)

<!-- more -->

> 认识表单



### 什么是表单？

* 表单是一个包含表单元素的区域,表单元素是允许用户在表单中（比如：文本域、下拉列表、单选框、复选框等等）输入信息的元素。

* 使用form标签定义

  ```html
  <form action="Url" method="get">
        <p>用户名:<input type="text" name="username" /></p>
        <p>密码:<input type="text" name="password" /></p>
  </form>
  ```



#### input 

> input标签的类型有很多种,以下是一些常见的类型

* password
* button
* checkbox
* radio
* file
* hidden
* reset
* submit
* color
* date

```html
<!-- eg -->
<input type="value" value="" placeholder="请输入user">
<input type="password" placeholder="请输入密码">

<input type="checkbox" name="xq" value="1" checked>
<input type="radio" name="sex" value="1" checked>
<!--
	checkbox(多选)
		name 区别于不同选项 用于拼接到url
		value 设置或返回单选按钮的内容
	radio(单选)
		name 相同name只能选择一个 用于拼接到url
		value 设置或返回单选按钮的内容
	checked 选中(写什么都可以选中 一般写true)
-->

<!-- 按钮 -->
<input type="submit" value="提交">
<input type="button" value="注册"/>
<input type="button" value="确定">
<input type="reset" value="重置" />
<input type="hidden" name="six" value="six">
<input type="file">

<input type="date" value="2017-11-15">
<input type="color" value="#ff0000">
```

```html
<!-- 
	label
		增强用户的体验感
-->
<label for="basketball">篮球</label>
<input type="radio" name="hobby" value="basketball" id="basketball"  />
<label for="football">足球</label>
<input type="radio" name="hobby" value="football" id="football"  />
```



#### textarea

```html
<textarea rows="3" cols="30" style="resize: none">
	你们学到了嘛,关于表单的知识
</textarea>
```



#### select

```html
<!-- size默认显示几个 -->
<select name="size" size="3">
    <option value="1">s</option>
    <option value="2">m</option>
    <option value="3">xl</option>
  	<option value="3">xxl</option>
</select>
```



