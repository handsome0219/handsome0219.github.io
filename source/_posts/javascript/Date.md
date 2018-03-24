---
title: Date
tags: JS
categories: JavaScript
thumbnail: /img/javascript/Date.jpg
abbrlink: 1442
date: 2018-02-10 16:47:49
comments: false
---

> Date类型使用自UTC（Coordinated Universal Time，国际协调时间）。从1970.1.1 00:00:00开始

<!-- more -->

# Date

日期对象的定义

```js
var date = new Date;

// 可以传递参数
var date = new Date(year, month, day, hours, minutes, seconds);
var date = new Date(milliseconds);
```

js还定义另外两种方法 `Date.parse` `Date.UTC` 同样可以接受参数返回毫秒数

```JS
var date = new Date(Date.parse("2018,02,09"));
// Fri Feb 09 2018 00:00:00 GMT+0800
var date = new Date(Date.UTC(2018,02,09));
// Fri Mar 09 2018 08:00:00 GMT+0800
// 可以发现两个日期的月份不一样 UTC格式中传入的月份 0-11 parse月份 1-12 时间差了8个小时
```



## 格式化

| 方法                 | 描述                                                   |
| -------------------- | ------------------------------------------------------ |
| toTimeString()       | 把 Date 对象的时间部分转换为字符串。                   |
| toDateString()       | 把 Date 对象的日期部分转换为字符串。                   |
| toUTCString()        | 根据世界时，把 Date 对象转换为字符串。                 |
| tolocaleString()     | 根据本地时间格式，把 Date 对象转换为字符串。           |
| toLocaleTimeString() | 根据本地时间格式，把 Date 对象的时间部分转换为字符串。 |
| toLocaleDateString() | 根据本地时间格式，把 Date 对象的日期部分转换为字符串。 |

## Date对象方法

| 方法                | 描述                                            |
| ------------------- | ----------------------------------------------- |
| getDate()           | 从 Date 对象返回一个月中的某一天 (1 ~ 31)。     |
| getDay()            | 从 Date 对象返回一周中的某一天 (0 ~ 6)。        |
| getMonth()          | 从 Date 对象返回月份 (0 ~ 11)。                 |
| getFullYear()       | 从 Date 对象以四位数字返回年份。                |
| getHours()          | 返回 Date 对象的小时 (0 ~ 23)。                 |
| getMinutes()        | 返回 Date 对象的分钟 (0 ~ 59)。                 |
| getSeconds()        | 返回 Date 对象的秒数 (0 ~ 59)。                 |
| getTime()           | 返回 1970 年 1 月 1 日至今的毫秒数。            |
| getTimezoneOffset() | 返回本地时间与格林威治标准时间 (GMT) 的分钟差。 |

*tip* : Date.now() 可以获取当前的毫秒。IE不支持