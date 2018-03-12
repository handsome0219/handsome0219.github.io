---
title: 列表，表格标签
tags: HTML
categories: html&css
abbrlink: 9288
date: 2017-11-10 19:39:21
comments: false
---

![](https://wallpapers.wallhaven.cc/wallpapers/full/wallhaven-407979.jpg)

<!-- more -->

> HTML标签之列表，表格，iframe

### 有序列表，无序列表

- ul 无序列表标签

  ```html
  <ul>
    <h3>ul无序列表</h3>
    <li>蒹葭苍苍，白露为霜</li>
    <li>所谓伊人，在水一方</li>
    <li>我就是这么的可爱!</li>
  </ul>
  ```

  ​

- ol有序列表标签

  ```html
  <ol>
    <h3>ol有序列表</h3>
    <li>蒹葭萋萋，白露未晞</li>
    <li>所谓伊人，在水之湄</li>
    <li>我就是这么的迷人!</li>
  </ol>
  ```

  *PS*：列表之间是可以相互去嵌套的，互相搭配干活不累。





### 列表

- dl标签

  ```html
  <dl>
    <h3>dl列表</h3>
    <dt>HTML</dt>
    <dd>超文本标记语言</dd>
    <dt>CSS</dt>
    <dd>层叠样式表</dd>
    <dt>Javascript</dt>
    <dd>脚本语言</dd>
    <dt>JAVA</dt>
    <dd>Coffice</dd>
  </dl>
  ```

  > dt：**定义列表中的项目** 
  >
  > dd：**描述列表中的项目** 





### 表格

| 表格          | 描述       |
| ----------- | -------- |
| < table >   | 定义表格     |
| < caption > | 定义表格标题。  |
| < th >      | 定义表格的表头。 |
| < tr >      | 定义表格的行。  |
| < td >      | 定义表格单元。  |
| < thead >   | 定义表格的页眉。 |
| < tbody >   | 定义表格的主体。 |
| < tfoot >   | 定义表格的页脚。 |



```html
<!-- 垂直的表头 -->
<table border="1" style="border-collapse: collapse;">
    <tr>
        <th> name </th>
        <th> Mr.老司机寻常 </th>
    </tr>
    <tr>
        <th> size </th>
        <th> 18 </th>
    </tr>
</table>
```

```html
<!-- 横向的表头 -->
<table border="1" style="border-collapse: collapse;">
  <tr>
    <th> name </th>
    <th> age </th>
    <th> mail </th>
  </tr>
  <tr>
    <td> Mr.xq </td>
    <td> 18 </td>
    <td> 2039971852@qq.com </td>
  </tr>
</table>
```


### iframe框架

```html
<!-- 写法 -->
<iframe src="Url"></iframe>
<!-- Url指的是iframe显示的内容地址 -->
```

| 属性          | 值           | 描述                              |
| ----------- | ----------- | ------------------------------- |
| width       | px/%        | 规定 iframe 的宽度。                  |
| height      | px/%        | 规定 iframe 的高度。                  |
| frameborder | 1/0         | 规定是否显示框架周围的边框。                  |
| scrolling   | yes/no/auto | 规定是否在 iframe 中显示滚动条。            |
| src         | URL         | 规定在 iframe 中显示的文档的 URL。         |
| seamless    | seamless    | 规定在 < iframe > 中显示的页面的 HTML 内容。 |

`ps`：内嵌网页，加载更多，付款成功（不想跳转页面的话）