---
title: BOM
tags: BOM
categories: BOM
abbrlink: 4436
date: 2018-03-05 13:55:20
---

*ECMAScript* 是 *JavaScript* 语法的核心规范，如果在 *web* 中使用 *JavaScript*，那么 *BOM* 则是一个很重要的核心。BOM提供了很多的对象，用于访问浏览器的功能。BOM最早没有一个统一的规范导致浏览器厂商都有自己的想法随意的去扩展。后来把浏览器共有的对象做为事实上的标准。这些对象在浏览器中得以存在。W3C为了把浏览器中基本的部分标准化，将 *BOM* 纳入了 *H5* 的规范中。

## BOM

![bom](/img/javascript/dom/bom.jpg)

BOM（Browser Object Model），核心对象 *window* 对象，它表示浏览器的一个实例。

在全局中定义的变量函数，都会被归纳在 *window* 对象名下

```js
var a = 1;
console.loa(a);
console.log(window.a);
```

因为全局变量是归属于 *window* 对象，但是与直接扩展在 *window* 上的属性有点区别：直接定义在window上的属性可以被 `delete` 删除，通过 `var` 定义的变量是不会被删除的.

### screen

说screen对象之前先说一下确定浏览器可视窗口大小的属性

浏览器确定一个窗口的大小不是简单的事情。有四个属性 innerWidth，innerHeight，outerWidth，outerHeight。IE8 及更早版本没有提供取得当前浏览器窗口尺寸的属性；不过，它通过 DOM 提供了页面可见区域的相关信息

* IE9+、 Safari 和 Firefox中， outerWidth 和 outerHeight 返回浏览器窗口本身的尺寸 
* Chrome 中， outerWidth、 outerHeight 与innerWidth、 innerHeight 返回相同的值 
* document.documentElement.clientWidth 和 document.documentElement.clientHeight 中保存了页面视口的信息。在 IE6 中，在标准模式下才有效；如果是混杂模式，就必须通过document.body.clientWidth 和 document.body.clientHeight 取得相同信息。而对于Chrome，则无论通过 document.documentElement 还是 document.body 中的 clientWidth 和clientHeight 属性，都可以取得视口的大小。 

*ps* ：虽然最终无法确定浏览器窗口本身的大小，但却可以取得页面视口的大小。关于浏览器的渲染模式[参考](https://www.cnblogs.com/imxiu/p/3541932.html)

```js
var pageWidth = window.innerWidth,
	pageHeight = window.innerHeight;
if (typeof pageWidth != "number"){
    if (document.compatMode == "CSS1Compat"){
        pageWidth = document.documentElement.clientWidth;
        pageHeight = document.documentElement.clientHeight;
    } else {
        pageWidth = document.body.clientWidth;
        pageHeight = document.body.clientHeight;
    }
}
// 通常我们可能会这么写
var pageWidth = document.documentElement.clientWidth || window.innerWidth
```

screen对象其实没有很大的用处，screen对象可以表明浏览器的显示器信息。以像素表示

```js
window.screen.availWidth  // 可用的屏幕宽度 减去界面特性，比如窗口任务栏
window.screen.availHeight // 可用的屏幕高度 减去界面特性，比如窗口任务栏
```



### location

location是BOM中很重要的对象。提供了与当前窗口相关的信息。而且 *location* 对象既是 window对象也是document对象 

```js
window.location === document.location
```

以 https://www.baidu.com/s?wd=bom 举例说明

| 属性     | 例子                           | 描述                                                         |
| -------- | ------------------------------ | ------------------------------------------------------------ |
| hash     | "#content"                     | 返回URL中的hash（#号后跟零或多个字符），如果URL中不包含散列，则返回空字符串 |
| host     | "www.baidu.com"                | 返回服务器名称和端口号（如果有）                             |
| hostname | "www.baidu.com"                | 返回不带端口号的服务器名称                                   |
| href     | https://www.baidu.com/s?wd=bom | 返回当前加载页面的完整URL。而location对象的toString()方法也返回这个值 |
| pathname | "/s"                           | 返回URL中的目录和（或）文件名                                |
| port     | ""                             | 返回URL中指定的端口号。如果URL中不包含端口号，则这个属性返回空字符串 |
| protocol | "https"                        | 返回页面使用的协议。通常是http:或https:                      |
| search   | "?wd=bom"                      | 返回URL的查询字符串。这个字符串以问号开头                    |

查询参数，通过location对象可以审查到URL中的参数返回一个对象

```js
function getUrlParam(){
    var qs = location.search;
    var str = qs.length > 0 ? qs.substring(1) : "";
    var param = {};
    var items = str.split("&");
    for(var i=0,len = items.length;i<len;i++){
        item = items[i].split("=");
        param[decodeURIComponent(item[0])] = decodeURIComponent(item[1]);
    }
    return param;
}

function getUrlParam(){
    var qs = location.search;
    var str = qs.length > 0 ? qs.substring(1) : "";
    var items = qs.match(/(\w+)=(\w+)/g);
    return items.reduce((a, b) => {
        var index = b.indexOf("=");
        return a[decodeURIComponent(b.slice(0,index))] = decodeURIComponent(b.slice(index+1)),a;
    },{});
} 
```

另外location对象中除了hash属性修改不会重新加载其它的属性修改之后都会重新加载，而且会在 *history* 产生一条记录，因此可以通过回退返回。可以禁止这个行为  `location.replace(url);` 用户不能点击返回，并且不会在  *history* 产生记录。

还有一个相关的方法 `reload()` 。重新加载当前的页面。无参数的时候会议最有效的方式加载。也就是会从浏览器缓存中加载。如果需要强制从服务器加载添加参数 `true` 

```js
location.reload();      // 可能从缓存加载
location.reload(true);  // 从服务器加载
```

由于是重新载入页面所以位于 `reload` 之后的代码可能不在执行。所以请记得写在最后一行。



### navigator

Navigator 对象包含有关浏览器的信息，每个浏览器都有自己的一套属性。

| 属性或方法     | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| appCodeName    | 浏览器的名称。通常都是Mozilla，即使在非Mozilla浏览器中也是如此 |
| appName        | 完整的浏览器名称                                             |
| appVersion     | 浏览器的版本。一般不与实际的浏览器版本对应                   |
| cookieEnabled  | 表示cookie是否启用                                           |
| language       | 浏览器的主语言                                               |
| onLine         | 表示浏览器是否连接到了因特网                                 |
| oscpu          | 客户端计算机的操作系统或使用的CPU                            |
| platform       | 浏览器所在的系统平台                                         |
| plugins        | 浏览器中安装的插件信息的数组                                 |
| systemLanguage | 操作系统的语言                                               |
| userAgent      | 浏览器的用户代理字符串                                       |
| userLanguage   | 操作系统的默认语言                                           |
| userProfile    | 借以访问用户个人信息的对象                                   |
| vendor         | 浏览器的品牌                                                 |
| javaEnabled()  | 表示当前浏览器中是否启用了Java                               |

这里给出一个书上的检测用于代理字符串，包括呈现引擎、平台、操作系统、移动设备和游戏系统的代码。

```js
var client = function () {
    //呈现引擎
    var engine = {
        ie: 0,
        gecko: 0,
        webkit: 0,
        khtml: 0,
        opera: 0,
        //完整的版本号
        ver: null
    };
    //浏览器
    var browser = {
        //主要浏览器
        ie: 0,
        firefox: 0,
        safari: 0,
        konq: 0,
        opera: 0,
        chrome: 0,
        //具体的版本号
        ver: null
    };
    //平台、设备和操作系统
    var system = {
        win: false,
        mac: false,
        x11: false,
        //移动设备
        iphone: false,
        ipod: false,
        ipad: false,
        ios: false,
        android: false,
        nokiaN: false,
        winMobile: false,
        //游戏系统
        wii: false,
        ps: false
    };
    //检测呈现引擎和浏览器
    var ua = navigator.userAgent;
    if (window.opera) {
        engine.ver = browser.ver = window.opera.version();
        engine.opera = browser.opera = parseFloat(engine.ver);
    } else if (/AppleWebKit\/(\S+)/.test(ua)) {
        engine.ver = RegExp["$1"];
        engine.webkit = parseFloat(engine.ver);
        //确定是 Chrome 还是 Safari
        if (/Chrome\/(\S+)/.test(ua)) {
            browser.ver = RegExp["$1"];
            browser.chrome = parseFloat(browser.ver);
        } else if (/Version\/(\S+)/.test(ua)) {
            browser.ver = RegExp["$1"];
            browser.safari = parseFloat(browser.ver);
        } else {
            //近似地确定版本号
            var safariVersion = 1;
            if (engine.webkit < 100) {
                safariVersion = 1;
            } else if (engine.webkit < 312) {
                safariVersion = 1.2;
            } else if (engine.webkit < 412) {
                safariVersion = 1.3;
            } else {
                safariVersion = 2;
            }
            browser.safari = browser.ver = safariVersion;
        }
    } else if (/KHTML\/(\S+)/.test(ua) || /Konqueror\/([^;]+)/.test(ua)) {
        engine.ver = browser.ver = RegExp["$1"];
        engine.khtml = browser.konq = parseFloat(engine.ver);
    } else if (/rv:([^\)]+)\) Gecko\/\d{8}/.test(ua)) {
        engine.ver = RegExp["$1"];
        engine.gecko = parseFloat(engine.ver);
        //确定是不是 Firefox
        if (/Firefox\/(\S+)/.test(ua)) {
            browser.ver = RegExp["$1"];
            browser.firefox = parseFloat(browser.ver);
        }
    } else if (/MSIE ([^;]+)/.test(ua)) {
        engine.ver = browser.ver = RegExp["$1"];
        engine.ie = browser.ie = parseFloat(engine.ver);
    }
    //检测浏览器
    browser.ie = engine.ie;
    browser.opera = engine.opera;
    //检测平台
    var p = navigator.platform;
    system.win = p.indexOf("Win") == 0;
    system.mac = p.indexOf("Mac") == 0;
    system.x11 = (p == "X11") || (p.indexOf("Linux") == 0);
    //检测 Windows 操作系统
    if (system.win) {
        if (/Win(?:dows )?([^do]{2})\s?(\d+\.\d+)?/.test(ua)) {
            if (RegExp["$1"] == "NT") {
                switch (RegExp["$2"]) {
                    case "5.0":
                        system.win = "2000";
                        break;
                    case "5.1":
                        system.win = "XP";
                        break;
                    case "6.0":
                        system.win = "Vista";
                        break;
                    case "6.1":
                        system.win = "7";
                        break;
                    default:
                        system.win = "NT";
                        break;
                }
            } else if (RegExp["$1"] == "9x") {
                system.win = "ME";
            } else {
                system.win = RegExp["$1"];
            }
        }
    }
    //移动设备
    system.iphone = ua.indexOf("iPhone") > -1;
    system.ipod = ua.indexOf("iPod") > -1;
    system.ipad = ua.indexOf("iPad") > -1;
    system.nokiaN = ua.indexOf("NokiaN") > -1;
    //windows mobile
    if (system.win == "CE") {
        system.winMobile = system.win;
    } else if (system.win == "Ph") {
        if (/Windows Phone OS (\d+.\d+)/.test(ua)) {
            system.win = "Phone";
            system.winMobile = parseFloat(RegExp["$1"]);
        }
    }
    //检测 iOS 版本
    if (system.mac && ua.indexOf("Mobile") > -1) {
        if (/CPU (?:iPhone )?OS (\d+_\d+)/.test(ua)) {
            system.ios = parseFloat(RegExp.$1.replace("_", "."));
        } else {
            system.ios = 2; //不能真正检测出来，所以只能猜测
        }
    }
    //检测 Android 版本
    if (/Android (\d+\.\d+)/.test(ua)) {
        system.android = parseFloat(RegExp.$1);
    }
    //游戏系统
    system.wii = ua.indexOf("Wii") > -1;
    system.ps = /playstation/i.test(ua);
    //返回这些对象
    return {
        engine: engine,
        browser: browser,
        system: system
    };
}();
console.log(client);
/*
	{engine: {…}, browser: {…}, system: {…}}
*/
```



### history

*history* 对象保存着用户上网的历史记录。*history* 存在一个 `go()` 方法，用于前进或后退

```js
history.go(-1);  // 后退一页
history.go(1)；  // 前进一页
```

此外还有另外两个方法 `forward()` `back()` 与 `go()` 方法的前进后退一致。这里可以利用JavaScript的阻塞原理做一些小的功能。alert、confirm、prompt都可以让页面阻塞执行

```js
// 现在有一个小的功能点击某个链接 但是需要验证一些信息
var submit = document.getElementById("submit");
submit.onclick = function(){
    var flag = confirm("你是不是我的小可爱,请回答是或者不是");
    if(flag){
        window.location.href = "https://www.baidu.com";
    }else{
        // window.history.go(-1);
        window.history.back();
    }
}
```

