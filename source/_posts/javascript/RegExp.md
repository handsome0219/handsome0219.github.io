---
title: RegExp
tags:
  - js
  - RegExp
categories: Javascript
thumbnail: /img/javascript/RegExp.jpg
abbrlink: 65388
date: 2018-02-12 15:22:34
---

Regular Expression 正则表达式，又称正规表示式。使用单个字符串来描述，匹配一系列符合某个句法规则的字符串。正则表达式通常被用来检索、替换那些符合某个模式(规则)的文本。正则表达式是是一种匹配模式要么匹配字符，要么匹配位置

<!-- more --> 

正则表达式的应用非常的广泛，比如在dos窗口下 *find* 命令

> dir find *.txt

找到当前目录下所有的 `.txt` 结尾的文件。再比如很多 *IDE* 工具中都有正则替换的功能。再比如替换日期格式，匹配请求头替换等等。正则让一些繁琐的处理方式变得简单。重温了一遍黑魔法正则表达式。

![ide](/img/javascript/ide.png)

# RegExp

JavaScript中正则表达式使用包含两个方面字符方法与正则方法。不同的方法调用方式是不同的。下面列举一下

字符方法中可以接受正则表达式的方法有

> split、match、replace、search

正则方法

> test、exec

标识符

> g（global）、i（ignoreCase）、m（multiline）



## 元字符

元字符某些其实就是转义字符，正则表达式中常用的几个元字符

| 字符 | 含义             |
| ---- | ---------------- |
| \d   | 数字             |
| \D   | 非数字           |
| \w   | 数字字母下划线   |
| \W   | 非数字字母下划线 |
| \s   | 空格             |
| \S   | 非空格           |
| \b   | 单词边界         |
| \B   | 非单词边界       |
| ^    | 以xx开头         |
| $    | 以xx结尾         |

```js
var pattern = /\d/;
console.log(pattern.test("abc")); // false
console.log(pattern.test("abc1")); // true

var pattern = /\bis\b/;
console.log('this is a box'.replace(pattern, 'IS')); // this IS a box
```



## 字符集

字符集用来指定某一些符合特性的字符，用 `[]` 来表示可以是单个字符或者是一个范围 例如\[abc\] \[0-9\] 

```js
var pattern = /[0-9a-zA-Z_]/g;        // 与\w意思相同
console.log(pattern.test('*'));      // false
console.log('--abc--'.replace(pattern, 'X')); // --XXX--
```

取反 字符集中使用 `^` 代表非的意思

```js
var pattern = /[^abc]/g;
console.log('abc123'.replace(pattern, 'X')); // XXX123
```

js的正则中 `.` 也代表一个范围和上边的元字符一样代表一个范围 写成字符集可以是 `[^\r\n]` ，`\d` 可以写成 `[0-9]` 。



## 量词

用来判断字符重复出现

| 字符  | 描述                           |
| ----- | ------------------------------ |
| ?     | 出现零次或者一次(最多一次)     |
| +     | 出现一次或者多次(最少出现一次) |
| *     | 出现零次或者多次               |
| {n}   | 出现n次                        |
| {n,m} | 出现n到m次                     |
| {n,}  | 最少出现n次                    |

```js
var pattern = /\d{3}/;
var str = "123";
console.log(pattern.test(str)); // true
```



*贪婪模式*

在满足匹配条件的情况下尽可能的多的去匹配

```js
var pattern = /\d{2,5}/g;
var str = "123456";
console.log(str.replace(pattern, 'x')); // x6

var pattern = /\d{2,5}/;
var str = "123456";
console.log(str.replace(pattern, 'x')); // x6

var pattern = /\d{2,5}/;
var str = "123456";
console.log(str.match(pattern));  // ["12345", index: 0, input: "123456"]
```



*非贪婪模式*

在满足条件的情况下尽可能少的去匹配

```js
var pattern = /\d{2,5}?/g;
var str = "123456";
console.log(str.replace(pattern, 'x')); // xxx

var pattern = /\d{2,5}?/;
var str = "123456";
console.log(str.replace(pattern, 'x')); // x3456

var pattern = /\d{2,5}?/;
var str = "123456";
console.log(str.match(pattern));  // ["12", index: 0, input: "123456"]
```



## 子集

子集也是分组，在正则表达式中分组是一个重要的功能

```js
var pattern = /box{2}/;
var str = "boxx";
console.log(pattern，test(str)); // true  box{2}匹配的是x出现两次 不是box两次

var pattern = /(box){2}/;
var str = /boxbox/;
console.log(pattern.test(str)); // true
```



*反向引用*

```js
var str = 'boxbox';
var pattern = /(box){2}/;
console.log(pattern.test(str)); // true
console.log(/(box)\1/.test(str)); // true
```

反向引用顾名思义就是去引用前边的内容也就是说要一模一样。比如检测一个字符串中是否有连续出现相同的字符

```js
/(\w)\1/.test("pattern");
```



*捕获性分组*

```js
var str = "2018-2-13";
var pattern = /(2018)-(2)-(13)/;
console.log(str.match(pattern)); // ["2018-2-13", "2018", "2", "13", index: 0, input: "2018-2-13"]
console.log(pattern.exec(str));  // ["2018-2-13", "2018", "2", "13", index: 0, input: "2018-2-13"]
```

`match` 与 `exec` 返回一个数组，匹配成功，返回的数组第一项是匹配成功的字符串，其余项为 *分组项* 即 *子集* 



非捕获性分组*

```js
var pattern = /(?:a)(b)(c)/;
var str = "abc";
console.log(str.match(pattern)); // ["abc", "b", "c", index: 0, input: "abc"]
```

`?:` 这个分组将不会被返回。为了整体的匹配只返回需要的



*前瞻模式*

```js
var str = 'google';    
var pattern = /goo(?=gle)/;
console.log(patter.test(str));  // true
console.log(pattern.exec(str)); // ["goo", index: 0, input: "google"]
```

*goo* 后边必须跟的是 *gle* 。同样捕获返回分组内容，只会返回匹配成功的字符



*负向前瞻*

```js
var str = "ab123";
var pattern = /a(?![a-zA-Z])/g;
console.log(str.replace(pattern, "X")); // aX123

var str = "a123*7vv";
var pattern = /\w(?!\d)/g;
console.log(str.replace(pattern, 'X')); // a12X*XXX
```

如何实现数字的千位分隔符表示？*123456789 => 123,456,789*

```js
'123456789'.replace(/(?!^)(?=(\d{3})+$)/g, ',');
```



*或者*

```js
var str = "box is xob";
var pattern = /box|xob/g;
console.log(str.replace(pattern, 'X'));
```



## 方法

正则表达式提供了两个方法 `test` `exec` 这个两个方法有些地方需要注意一下

*test example*

```js
var str = 'a';
var pattern = /\w/g;
console.log(pattern.test(str));  // true
console.log(pattern.test(str));  // false
console.log(pattern.test(str));  // true
console.log(pattern.test(str));  // false
```

有点奇怪是吧~

```js
var str = 'a';
var pattern = /\w/;
console.log(pattern.test(str));  // true
console.log(pattern.test(str));  // true
```

两个的区别在于加了 `global` 标识符的正则表达式的 `lastIndex` 属性会每次作用于正则表达式本身。lastIndex 属性为匹配文本的最后一个字符的下一个位置，所以出现了一次 *true* 一次 *false* 奇怪的现象好像看着正则表达式一点都不靠谱。

```js
var str = 'a';
var pattern = /\w/g;
console.log(pattern.test(str), pattern.lastIndex);  // true  1
console.log(pattern.test(str), pattern.lastIndex);  // false 0
console.log(pattern.test(str), pattern.lastIndex);  // true  1
```

只有加了 `global` 标识符 `lastIndex` 才会生效



*exec example* 

```js
var str = '1ab2cc3dd4';
var pattern = /\d\w\w\d/;
var ret1 = pattern.exec(str);
var ret2 = str.match(pattern);
console.log(ret1, pattern.lastIndex);  // ["1ab2", index: 0, input: "1ab2cc3dd4"] undefined
console.log(ret2, pattern.lastIndex);  // ["1ab2", index: 0, input: "1ab2cc3dd4"] undefined
```

加上 `global` 标识符

```js
var str = '1ab2cc3dd4';
var pattern = /\d\w\w\d/g;
var ret1 = pattern.exec(str);
var ret2 = str.match(pattern);
// 第一次
console.log(ret1, pattern.lastIndex);  // ["1ab2", index: 0, input: "1ab2cc3dd4"] 4
// 第二次
console.log(ret2, pattern.lastIndex);  // ["1ab2", "3dd4"] 0
```

同样加了全局标识符之后 `lastIndex` 会随着匹配过程改变，变成全局的正则表达式 *match* 方法 `lastIndex` 是不会改变的，也不会像 *exec* 返回一个详细的信息包括匹配成功的位置，匹配的字符串，下一次匹配的起始位置。选择时候如果不需要这些信息完全可以使用 *match* 获取结果就ok，如果需要位置信息就要选择 *exec*

全局正则 `lastIndex` 改变那么就可以反复调用 *exec* 方法来遍历字符串中的所有匹配文本。

```js
var str = '1ab2cc3dd4';
var pattern = /\d\w\w\d/g;
var ret;
while(ret = pattern.exec(str)){
    console.log(ret);
    console.log(pattern.lastIndex + '\t' + ret.index + '\t' + ret.toString())
}
```



## RegExp.$1-$9

> $1 - $9 属性是包含括号子集匹配的正则表达式的静态和只读属性。通过RegExp直接访问

```js
var re = /(\w+)\s(\w+)/;
var str = 'John Smith';
str.replace(re, '$2, $1'); // "Smith, John"
RegExp.$1; // "John"
RegExp.$2; // "Smith"
```

```js
var date = '2018-01-10';
var pattern = /(\d{4})-(\d{2})-(\d{2})/;
console.log(date.replace(pattern, '$2/$3/$1'))
```

```js
var str = '我是{{name}}，年龄{{age}}，性别{{sex}}';
var person = {
    name:'张三',
    age: 20,
    sex: '男'
}
var result = str.replace(/\{\{(.+?)\}\}/g,function (match, m1) {
    return person[m1]
})
console.log(result); // 我是张三，年龄20，性别男
```

```js
// 中文
var pattern = /^[\\u4e00-\\u9fa5]{0,}$/;
// 颜色
var pattern = /^#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})$/;
```



## last

正则表达式图形化界面，对于理解很有帮助

* [Regexper: 可视化正则表达式](https://regexper.com/)
* [在线正则表达式使用](https://regex101.com/)