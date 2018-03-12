---
title: String Number Boolean
tags: js
categories: Javascript
abbrlink: 45359
date: 2018-02-08 14:27:55
thumbnail: /img/javascript/package-class.jpg
---

<!-- more -->

> 为了便于操作，*javascript* 提供了三个基本包装类型。在使用中不需要声明再去调用不同的方法。

# Package Class

看一个 *example*

```js
var str = "String";
str.substring(2);
```

`str` 固然是一个字符串基本类型，那怎么可以去调用 `substring` 这个方法的。其实是这样的

```js
var str = "String";
var temp = new String(str);
temp.substring(2);
temp = null;
```

经过这样子的处理之后基本类型变得和对象一样了。其它的 *Number*、 *Boolean* 同样适用。基本包装类型的**生命周期**，当调用对象的方法时，就会自动创建一个相应的引用实例，然后执行结束后又会马上销毁的。这也就意为不能为基本类型去添加属性。

```js
var str = "xq";
str.color = "red";
console.log(str.color); // undefined

// 实际上
var str = "xq";
var temp = new String(str);
temp.color = 'red';
temp = null;
console.log(str.color); // undefined
```



> 类型检测

```js
var str = "String";
var object = new String("String");
console.log(typeof str); // string
console.log(typeof object); // object
console.log(str instanceof String); // false
console.log(object instanceof String); // true
```

手动创建一个基本包装类类型为 *object* 。不推荐这么去做，会傻傻分不清楚。搞清楚了基本包装类那么来看一下基本包装类底下的常用方法有哪些。



## String

| 方法                     | 描述                                                         |
| ------------------------ | ------------------------------------------------------------ |
| chaAt(n)                 | 返回指定索引位置的字符                                       |
| charCodeAt(n)            | 以Unicode编码形式返回指定索引位置的字符                      |
| fromCharCode(ascii)      | 返回ASCII对应的字符                                          |
| indexOf(str, index)      | 从左往右查找字符str是否在字符串中,找到返回索引,找不到返回-1  |
| lastIndexOf(str, index)  | 从右往左查找字符str是否在字符串中,找到返回索引,找不到返回-1  |
| substring(n,m)           | 返回区间[n,m]之间的字符串,不包括m索引位                      |
| slice(n,m)               | 返回区间[n,m]之间的字符串,不包括m索引位                      |
| substr(n,m)              | 返回n索引之后的m个字符                                       |
| toLowerCase()            | 全部转换成小写                                               |
| toUpperCase()            | 全部转换成大写                                               |
| split(pattern)           | 按照pattern匹配来切分原始字符串                              |
| repalce(str, repalceStr) | 被用来在正则表达式和字符串直接比较，然后用新的子串来替换被匹配的子串。 |
| trim()                   | 从字符串的开始和结尾去除空格                                 |
| trimLeft()               | 去除字符串左边空格                                           |
| trimRight()              | 去除字符串右边空格                                           |
| match(pattern)           | 使用正则表达式与字符串相比较。                               |

**新增语法**

| 方法                      | 描述                                                         |
| ------------------------- | ------------------------------------------------------------ |
| includes(str)             | 判断一个字符串里是否包含其他字符串                           |
| endsWith(search, index)   | 判断一个字符串的结尾是否包含其他字符串中的字符，可以调整位置 |
| startsWith(search, index) | 判断一个字符串的起始是否包含其他字符串中的字符，可以调整位置 |
| repeat(n)                 | 返回指定重复次数的由元素组成的字符串                         |

*ps*：

* 需要注意一下substring和slice 参数的**正负**
  * substring() `n,m` 如果小于0 就变成0。  `n>m` ，`n,m` 交换位置
  * slice() `n,m` 如果小于0 就变成n/m+str.length。`n>m` ，结果null
* substr 在IE传递负值会返回原始字符串
* str[index]、charAt(index) 效果一样的 但是str[index] 在IE会返回undefined



## Number

> 属性

| 属  性            | 描述                   |
| ----------------- | ---------------------- |
| MAX_VALUE         | 表示最大数             |
| MIN_VALUE         | 表示最小值             |
| NaN               | 非数值                 |
| NEGATIVE_INFINITY | 负无穷大，溢出返回该值 |
| POSITIVE_INFINITY | 无穷大，溢出返回该值   |

> 方法

| 方  法                | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| toString()            | 将数值转化为字符串，并且可以转换进制                         |
| toFixed()             | 将数字保留小数点后指定位数并转化为字符串                     |
| toExponential(param)  | 将数字以指数形式表示，保留小数点后指定位数并转化为字符串     |
| toPrecision(param)    | 指数形式或点形式表述数，保留小数点后面指定位数并转化为字符串 |
| isNaN(param)          | ES6 判断到底是不是NaN  区别isNaN方法                         |
| toLocaleString(param) | 返回一个字符串                                               |

## pactice

这里挑几个有意思的练习一下，因为上述方法需要结合很多不同的内容场景使用。

```js
// 随机数
function randomNumber(start, end){
    return Math.floor(Math.random() * (end - start + 1) + start);
}

// rgb
function randomRgb(){
    return 'rgb(' + randomNumber(255) + ',' + randomNumber(255) + ',' + randomNumber(255) + ')';
}

// hex
function randomHex(){
    return '#' + Math.random().toString(16).substring(2).substr(0, 6);
}

function randomHex(){
    var color = '#';
    for (var i = 0; i < 6; i++) {
        color += '0123456789abcdef'[randomNumber(15)];
    }
    return color;
}

// 重复
str.repeat(n);

// 替换
var str = 'xq很帅,xq很帅,xq很帅';
function replaceStr(str, search, rep){
    var temp = str.slice();
	while(temp.includes(search)){
        temp = temp.replace(search, rep);
    }
    
    // while(temp.indexOf(search) != -1){
    //    temp = temp.replace(search, rep);
    // }
    return temp;
}
```

```js
var arr = [7,2,3,4,5];
function getMinNum(arr){
  var max = Number.MAX_VALUE;		// 最大数
  var index = -1;
  for(var i=0;i<arr.length;i++){
    if(max > arr[i]){
      max = arr[i];
      index = i;
    }
  }
  return index;
}


// toLocaleString
var number = 123456.12；
// 逗号隔开
console.log(number.toLocaleString())；// 123,456.123
// 中文
console.log(number.toLocaleString('zh-Hans-CN-u-nu-hanidec'))；
// 一二三,四五六.一二三

// 货币
console.log(number.toLocaleString('en', { style: 'currency', currency: 'USD'}));
// $ 123,456.12
console.log(number.toLocaleString('en-IN', { style: 'currency', currency: 'EUR'}));
// € 123,456.12
console.log(number.toLocaleString('en-IN', { style: 'currency', currency: 'CNY'}));
// ￥123,456.12 
```

