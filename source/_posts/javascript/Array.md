---
title: Array
tags: JS
categories: JavaScript
thumbnail: /img/javascript/array.jpg
abbrlink: 7791
date: 2018-02-10 14:07:59
comments: false
---

> 数组是 *ECMAscript* 中除了object之外最常用的类型了

<!-- more -->

# Array

数组是什么？一个盒子，一个容器，还是一个垃圾桶。

*Array* 类型是ECMAScript最常用的类型。而且ECMAScript中的Array类型和其他语言中的数组有着很大的区别。虽然数组都是有序排列，但ECMAScript中的数组每个元素可以保存任何类型。ECMAScript中数组的大小也是可以调整的。

* 数组的定义

```js
// 使用字面量写法创建数组
var arr = [1,2,3]; 					// 创建了一个数字并且分配了元素
// PS: var arr = [1,2,];			// 禁止使用IE会识别错误

// 使用new关键字创建数组
var arr = new Array();				// 创建了一个空数组
var arr = new Array(7);				// 创建了一个包含7个元素的数组
var arr = new Array(1,2,3);         // 创建了一个数组并且分配了元素

// 直接Array创建
var arr = Array(7); 				
```

* 数组的数据类型

```js
var arr = [];
console.log(typeof arr); 		 // object
console.log(Array.isArray(arr)); // true 准确判断是不是数组
```

* 注意点

```js
var arr = [1,2,3];
arr[5] = 'xq';
console.log(arr[4]);	// undefined 第3,4位没有元素被undefined所占位
console.log(arr.length);// 5 使用索引也可以增加数组的长度
```



## method

> 数组中的方法很多，栈方法、列队方法、重排序方法、操作方法、位置方法、迭代方法

| 方法           | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| push(param)    | 往数组的末尾添加元素                                         |
| unshift(param) | 往数组的前端添加元素                                         |
| pop()          | 从数组的末尾删除元素,并返回                                  |
| shift()        | 从数组的前端删除元素,并返回                                  |
| sort()         | 数组排序(默认按照ASCII值排序),可以传递一个**function**参数   |
| slice()        | 复制数组，并返回                                             |
| splice()       | 剪切数组元素，并返回                                         |
| concat()       | 拼接数组,返回一个数组                                        |
| join()         | 按照param拼接数组,返回字符串                                 |
| reverse()      | 数组反转 (也可以反转字符串)                                  |
| indexOf()      | 返回匹配的项,并返回索引,找不到返回-1                         |
| lastIndexOf()  | 返回匹配的项,并返回索引,找不到返回-1                         |
| find()         | 找到第一个满足测试函数的元素并返回那个元素的值，如果找不到，则返回 undefined |
| findIndex()    | 找到第一个满足测试函数的元素并返回那个元素的索引，如果找不到，则返回 -1 |
| forEach()      | 遍历数组                                                     |
| filter()       | 对数组的每一项执行指定函数，返回该函数会返回true的项组成的数组 |
| map()          | 对数组的每一项执行指定函数，返回该函数调用结果组成的数组     |
| some()         | 对数组的每一项执行指定函数，如果每一项都返回true则返回该数组 |
| reduce()       | 迭代方法                                                     |
| fill()         | 用一个固定值填充一个数组中从起始索引到终止索引内的全部元素   |
| entries()      | 返回一个迭代器                                               |

*ps*

数组中一些方法会改变原有数组的排列

> push、unshift、pop、shift、splice、sort、reverse、fill

会返回的一个数组或者期望值的方法

> sort、reverse、splice、slice、concat、join、indexOf、lastIndexOf、find、findIndex

会涉及到遍历数组的方法

> forEach、filter、find、findIndex、some、map、reduce



## example

*reverse*

```js
var str = "hello world";
console.log(str.split("").reverse().join(""));

var arr = [1,2,3];
console.log(arr.reverse());
for(var start=0,end=arr.length-1;start<end;start++,end--){
    arr[start] ^= arr[end];
    arr[end] ^= arr[end];
    arr[start] ^= arr[end];
}
```

*sort*

```js
var arr = [1,5,4,3,6,8,16,11];
arr.sort();
// sort 默认排序使用ASCII值比较
console.log(arr); 	// [1,11,16,3,4,5,6,8];
// 字符串之间的比较
console.log('2' > '199');	// true 一个道理
// 怎么样完成数字大小的排序
function compare(a,b){
  	// a < b <==> a-b<0
    return a -b;
};
// 完成正常排序需要 通过回调函数完成
arr.sort(compare);
arr.sort(function(a,b){
    // return a-b; 		// 升序
  	// return b-a;		// 降序
});

function sortArr(arr) {
    var entries = arr.entries(),temp;
    // while(temp = entries.next().value){
	//     temp[1].sort((a ,b) => a - b);
	// }
    for(var e of entries)e[1].sort((a ,b) => a - b);
    return arr;
}
var arr = [[1,34],[456,2,3,44,234],[4567,1,4,5,6],[34,78,23,1]];
sortArr(arr);
// ### 元素重排序
```

*数组升维降维* 

```js
var arr = [1,2,3,4,5,6];
function chunk(arr, len){
    var temp = [];
    var index = 0;
    while(arr[index]){
        temp.push(arr.slice(index, index+=len));
    }
    return temp;
}
function chunk(array, size){
    if(!Array.isArray(array)){
        return [];
    }
    size = size || 1;
    var index = 0,
        resIndex = 0,
        length = array === null ? 0 : array.length,
        resLen = Math.ceil(length / ( size|0));
        result = Array(resLen);
    while(index < resLen){
        result[index++] = array.slice(resIndex, resIndex+=size);
    }
    return result;
}
console.log(chunk(arr,2));

var flattened = [[0, 1], [2, 3], [4, 5]].reduce(function(a, b) {
    return a.concat(b);
});
console.log(flattened); 	// [0, 1, 2, 3, 4, 5]
```

*indexOf*

```js
var arr = [1,2,3];
console.log(arr.indexOf(2)); 	// 1

// 同样indexOf 也有去重的作用
var arr = [1,1,2,2,3,3];
var foo = [];
for(var i=0;i<arr.length;i++){
    if(foo.indexOf(arr[i]) != -1){
        foo.push(arr[i]);
    };
};
console.log(foo); 			// [1,2,3]
```

*filter*

```js
/*
	filter(item,index,array)
	item 	遍历的当前元素
	index	遍历的当前元素索引
	array	当前所遍历的数组
	做为数组的高级方法filter的过滤作用有很多的用处
*/
var arr = [1,2,3,4,5,6];
// 返回满足条件的项 这里返回了偶数
var foo = arr.filter(item=> item %2 === 0)
console.log(foo); 		// [2,4,6]

// 数组去重
arr.filter(function(item,index,array){
  	//检索当前的索引是不是唯一 indexOf总返回第一个元素 后续的重复元素位置与 indexOf 返回的位置不相等
  	return array.indexOf(item) === index;

  	//反过来思考 查看是否重复 返回-1的留下
  	return array.indexOf(item,index+1) == -1;
});

// 保留非空的非undefined
var arr = [1,,2,3,,4,undefined,5];
var foo = arr.filter(function(item){
   return item && item.toString().trim() && typeof item != 'undefined';
});
```

*find*

```js
/*
	find(fn) findIndex(fn)
	fn(item,index,array)
*/ 

var arr = [{name:'xq'},{name:'vk'},{name:'afei'}];
var obj = arr.find(function(x){return x.name == 'xq'}));
var idx = arr.findIndex(function(x){return x.name == 'xq'}));
console.log(obj); 	// {name: 'xq'}
console.log(idx); 	// 0

// 查看是不是质数
function isPrime(element, index, array) {
  	var start = 2;
  	while (start <= Math.sqrt(element)) {
    	if (element % start++ < 1) {
      		return false;
    	}
  	}
  	return element > 1;
}
console.log([4, 5, 8, 12].find(isPrime)); // 5
```

*map*

```js
/*
	map 方法会给原数组中的每个元素都按顺序调用一次  callback 函数
	map(fn);
	fn(item,index,array);
*/

// 格式化时间 
function formatTime(date) {
  	var year = date.getFullYear(),
  		month = date.getMonth() + 1,
        day = date.getDate(),
		hour = date.getHours(),
  		minute = date.getMinutes(),
  		second = date.getSeconds();
  	return [year, month, day].map(formatNumber).join('/') + ' ' + [hour, minute, second].map(formatNumber).join(':')
}

function formatNumber(n) {
  	n = n.toString()
  	return n[1] ? n : '0' + n
};

// 获取字符串的ASCII值
var str = 'I love you';
[].map.call(str,function(n){
    return n.charCodeAt();
});

// 遍历元素的属性
var inputDoms = document.getElementsByTagName('input');
[].map.call(inputDoms,function(val){
    return val.value;
});

// 反转字符串
var str = 'hello xq';
[].map.call(str,function(x){
    return x;
}).reverse().join('');

// 类似的数字操作
var arr = [10,21,32,4,2];
var foo = arr.map(function(x){
  return x % 10;
});
console.log(foo);   // 0, 1, 2, 4, 2

// 问题
["1", "2", "3"].map(parseInt);
// 可能觉的会是[1, 2, 3]
// 但实际的结果是 [1, NaN, NaN]
// parseInt有两个参数默认 parseInt做为回调函数传入map会给其默认传递三个参数 会把索引值当成进制使用

// 改进
['1', '2', '3'].map(Number);
// [1, 2, 3]
```

*reduce*

```js
// 迭代求和
var arr = [1,2,3,4];
var sum = 0;
for(var k in arr){
    sum += arr[k];
}
var sum1 = arr.reduce((a, b) => a + b);

// 统计字符出现次数
var str = "aabbcc";
var count = str.split("").reduce((a, b) => ( a[b]++ || (a[b]=1),a ), {});
console.log(count);
// 判断 a出现的次数
var count = str.split("a").length - 1;
// 判断数组中某一个元素出现次数
var count = (arr, value) => arr.reduce((a, b) => b === value ? a + 1 : a + 0, 0);

// 转换中文数字
var num = 123450;
function convert(num){
    var arr = ["零", "一", "二", "三", "四", "五", "六", "七", "八", "九"];
    return num.toString().split("").reduce((a, b) => ( a + arr[b] ), "");
}
console.log(convert(num));
```

