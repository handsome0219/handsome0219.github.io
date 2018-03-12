---
title: javascript XML
tags:
  - XML
  - DOM
categories: XML
abbrlink: 24945
date: 2018-03-03 23:26:29
---

XML曾经做为存储和通过网络传输结构化数据的标准。微型的结构化的数据库，保存一些小型数据。从这里边可以看到 *web* 的发展 *DOM* 的发展。DOM的出现使得浏览器都内置了对 *XML* 的支持。

**XML DOM**

在没有正式规范之前，浏览器对 *XML* 的解析支持度参差不齐，*DOM2* 增加了对 *XML DOM* 的方法。*DOM3* 也进一步加强了。谈到标准就一定不能忘记老 **IE** 了。

### IE 中 XML

*IE* 浏览器是第一个原生支持XML的浏览器，而它是通过 *ActiveX* 对象实现的。这个对象，只有IE有，一般是IE9之前采用。

```
创建 XMLDOM对象  IE
MSXML2.DOMDocument.3.0  在JavaScript中使用，这是最低的建议版本
MSXML2.DOMDocument.6.0  脚本能够可靠处理的最新版本
MSXML2.DOMDocument      仅针对IE5.5之前的版本
这三个版本常用其它的不稳定
```

采取向下兼容的方式写兼容。

```JS
function createXmlDom() {
	var version = [
        'MSXML2.DOMDocument.6.0',
        'MSXML2.DOMDocument.3.0',
        'MSXML2.DOMDocument'
	];
	for (var i = 0; i < version.length; i ++) {
		try {
			var xmlDom = new ActiveXObject(version[i]);
			return xmlDom;
		} catch (e) {
			//跳过
		}
	}
	throw new Error('您的系统或浏览器不支持MSXML！');		//循环后抛出错误
}
```

创建了xmlDom对象之后，载入xml，两种方式。第一种加载外部的xml文件，并序列化

```js
var xmlDom = createXmlDom();
xmlDom.load('src');
xmlDom.xml
```

第二种载入 xml字符串，并序列化

```js
var xmlDom = createXmlDom();
xmlDom.loadXML("<root><user>Kobe</user></root>");
xmlDom.xml
```

需要注意的是 *load* 方法用于服务器端，所以存在 同步 or 异步, 服务器端默认异步加载

```js
xmlDom.async = false; // 同步
xmlDom.load('src.xml');
xmlDom.xml; // 打印
```

```js
xmlDom.async = true;      //同步设置false，异步设置true，默认是异步
xmlDom.load('src.xml');
xmlDom.onreadystatechange = function () {    
    if (xmlDom.readyState == 4) {
        if (xmlDom.parseError.errorCode == 0) {
            //alert(this === xmlDom);               //this执行的是window
            xmlDom.xml // 打印
        } else {
            throw new Error(
                '错误代号：' + xmlDom.parseError.errorCode + '\n' +
                '错误行号：' + xmlDom.parseError.line + '\n' +
                '错误位置：' + xmlDom.parseError.linepos + '\n' +
                '错误解释：' + xmlDom.parseError.reason + '\n' +
            );
        }
	}
}
```

parseError是微软提供的如果说出现了解析错误，帮助排错的。

| 属性      | 说明                             |
| --------- | -------------------------------- |
| errorCode | 发生的错误类型的数字代号         |
| filepos   | 发生错误文件中的位置             |
| line      | 错误行号                         |
| linepos   | 遇到错误行号那一行上的字符的位置 |
| reason    | 错误的解释信息                   |

### DOM2 XML

在支持 *DOM2* 级的浏览器中可以创建一个空白的 *XML* 文档，实际中很少会创建一个空白的 *XML* 文档。只有火狐支持 `load` 载入 xml文件

```js
// DOM2 创建
var xmlDom = document.implementation.createDocument('', 'root', null);
console.log(xmlDom.documentElement.nodeName); // root
// 节点操作
var user = xmlDom.createElement('user');
xmlDom.documentElement.appendChild(user);
// 序列化
var serializer = new XMLSerializer();
var xml = serializer.serializeToString(xmlDom);
console.log(xml);

// ---
// 加载 xml  firfox 支持load!!!!!! 并且不放在服务器上还是默认异步的
xmlDom.async = false;
xmlDom.load("src.xml");
var serializer = new XMLSerializer();
var xml = serializer.serializeToString(xmlDom);
console.log(xml);
```

```js
xmlDom.async = true;
xmlDom.load("src.xml");
xmlDom.onload = function(){
    var serializer = new XMLSerializer();
    var xml = serializer.serializeToString(xmlDom);
    console.log(xml);
}
```

由于只有火狐支持 `load` 载入，所以还可以通过解析 xml字符

```js
var parser = new DOMParser();
var xmlStr = '<root><user>Lee</user></root>';
// parseFromString 第二个参数为解析成什么类型 不能解析html
var xmlDom = parser.parseFromString(xmlStr, 'text/xml');
// 解析 序列化
var serializer = new XMLSerializer();
var xml = serializer.serializeToString(xmlDom);
alert(xml);
```

错误解析。如果 `parseFromString` 解析错误会返回一个`parsererror` 元素，通过对它的判断

```js
var parser = new DOMParser();
var xmlDom,
    errors;
try{
	xmlDom = parser.parseFromString();
    errors = xmlDom.getElementsByTagName('parsererror');
    if (errors.length > 0) {
        throw new Error('XML格式有误：' + errors[0].textContent);
    }
}catch(e){
    alert('error');
}
var serializer = new XMLSerializer();
var xml = serializer.serializeToString(xmlDom);
```

 跨浏览器处理 *XML*

```js
function parseXML(xml){
    var xmlDom = null;
    if(typeof DOMParser != 'undefined'){
        xmlDom = (new DOMParser()).parseFromString(xml, 'text/xml');
        var errors = xmlDom.getElementsByTagName('parsererror');
        if (errors.length > 0) {
            throw new Error('XML格式有误：' + errors[0].textContent);
        }
    }else if(typeof ActiveXObject != 'undefined'){
        var xmlDom = createXmlDom();
        xmlDom.loadXML(xml);
        if(xmlDom.parseError.errorCode != 0){
            throw new Error(
                '错误代号：' + xmlDom.parseError.errorCode + '\n' +
                '错误行号：' + xmlDom.parseError.line + '\n' +
                '错误位置：' + xmlDom.parseError.linepos + '\n' +
                '错误解释：' + xmlDom.parseError.reason + '\n' +
            );
        }
    }else{
        throw new Error("No xml available");
    }
     return xmlDom;
};
```

序列化 *XML*

```js
function serializeXML(xmlDom) {
	var xml = '';
	if (typeof XMLSerializer != 'undefined') {
		xml = (new XMLSerializer()).serializeToString(xmlDom);
	} else if (typeof xmlDom.xml != 'undefined') {
		xml = xmlDom.xml;
	} else {
		throw new Error('无法解析XML！');
	}
	return xml;
}
```

