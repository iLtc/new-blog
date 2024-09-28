---
title: '通过 JavaScript 获取地址栏参数'
date: 2015-12-24
tags:
- JavaScript
- CODE
---

在网页前端编写 JavaScript 代码时，偶尔需要获取地址栏内的参数来响应不同的操作。在这里了列出两种常用的办法。

<!--more-->

### 1. 获取简单参数

如果需要传递的参数只有一个，可以直接通过 hash 来传递。

``` JavaScript
var getHash = function(){
    return window.location.hash.substr(1);
}
```

若页面地址为 `abc.html#test` ，则 `getHash()` 的结果为 `test` 。

### 2. 获取复杂参数

如果需要传递的参数有多个，则传递参数的地址形式类似于 ```abc.html?id=1&text=test``` 。

此时可以通过正则表达式的方式来获取参数。

``` JavaScript
var getQuery = function(name){
    var reg = new RegExp("(^|&)"+ name +"=([^&]*)(&|$)");
    var r = window.location.search.substr(1).match(reg);
    if(r!=null)return  unescape(r[2]);
    return null;
}
```

若页面地址为 `abc.html?id=1&text=test` ，则 `getQuery('id')` 的结果为 `1` 。

本文参考：
[http://www.cnblogs.com/fishtreeyu/archive/2011/02/27/1966178.html](http://www.cnblogs.com/fishtreeyu/archive/2011/02/27/1966178.html)