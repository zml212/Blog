---
title: BOM基础
date: 2022-9-23
tags: [前端]
categories: [BOM]
---
# BOM基础

## BOM概述

`BOM`就是浏览器对象模型，它提供了独立于内容与浏览器窗口进行交换的对象，其核心是`window`。

BOM是一系列的对象构成，每个对象都提供了一些特定的功能，这些对象都是`window`的属性。

BOM是没有标准的，这就会导致BOM的兼容性比较差。 

## BOM的组成

- document
- location
- navigation
- screen
- history

## window对象

`window`对象是BOM的核心，它代表浏览器的一个实例，它是全局对象，所有的`全局变量`都是它的属性，所有的`全局函数`都是它的方法。

之前使用的`alert()`、`confirm()`、`prompt()`、`setTimeout()`、`setInterval()`等方法都是`window`对象的方法。

**注意**

在声明全局变量的时候，尽量不要使用`name`作为变量名。因为在`window`对象里面，包含一个特殊的属性`window.name`。如果使用`name`作为变量名，那么`window.name`就会被覆盖。

## location对象

`location`对象是`window`对象的属性，它代表当前窗口的URL信息。并且他可以解析URL，返回的值也是一个对象。

### location对象的属性

| location对象属性  |               返回值                |
| :---------------: | :---------------------------------: |
|   location.href   |         获取或者设置整个URL         |
|   location.host   |          返回主机（域名）           |
|   location.port   | 返回端口号，如果未填写 返回空字符串 |
| location.pathname |              返回路径               |
|  location.search  |              返回参数               |
|   location.hash   |  返回片段 #后面内容 常见于链接锚点  |

### location对象的方法

|  location对象方法  |                          返回值                          |
| :----------------: | :------------------------------------------------------: |
| location.assign()  | 跟href一样，可以跳转页面（也成为重定向页面）（记录历史） |
| location.replace() |      替换当前页面，因为不记录历史，所以不能后退界面      |
| location.reload()  |  重新加载页面，相当于刷新，如果参数为true  则为强制刷新  |

## navigator对象

`navigator`对象是`window`对象的属性，它代表浏览器的信息。  
我们最常用的就是useAgent属性，它可以返回由客户机发送服务器的user-agent头部的值。

## history对象

window对象给我们提供了一个history对象，它可以让我们操作浏览器的历史记录。该对象包含了用户（在浏览器窗口中）访问过得URL。

| history对象方法 |                           作用                           |
| :-------------: | :------------------------------------------------------: |
|     back()      |                       可以回退功能                       |
|    forward()    |                         前进功能                         |
|    go(参数)     | 前进后退功能，参数如果是1 前进一个页面  -1  后退一个页面 |