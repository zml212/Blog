---
title: BOM常见的方法
date: 2022-9-22
tags: [前端]
categories: [BOM]
---
# BOM常见的方法

## window对象常见的事件

### 窗口加载事件`onload`

    window.onload = function(){}

    或者

    window.addEventListener('load', function(){})

window.onload是窗口（页面）加载事件，党文档内容（图像、脚本文件、css、文件）完全加载完成之后，才会触发该函数。

通过`onload`事件，我们就可以将js代码写在页面元素的上面了，这样就不会因为页面元素还没有加载完毕，导致js代码失效的问题。

**注意：**

`window.onload`传统注册事件的方法只能写一次，如果有多个，那么只会一最后一个`window.onload`为准生效。

但是如果使用方法监听绑定事件的方式，那么就可以写多个`window.onload`。不会有限制。

### 窗口加载事件`DOMContentLoaded`

    window.addEventListener('DOMContentLoaded', function(){})

`DOMContentLoaded`是DOM内容加载完成之后触发的事件，不会等待样式表、图像、子框架的加载。

在实际开发中，我们可能会遇到这样一个问题：某一个页面的图片特别多，要是等待页面全部加载完毕之后，再加载用户的交互代码，这样等待的时间太长，会造成不好的用户体验。

而`DOMContentLoaded`就是为了解决这个问题而出现的。

### 窗口大小改变事件`onresize`

    window.onresize = function(){}

    或者

    window.addEventListener('resize', function(){})

当浏览器的窗口大小发生变化的时候，就会触发该事件（函数处理）。

我们可以利用这个事件来完成事件响应式布局。window.innerWidth(宽度)和window.innerHeight(高度)可以获取到当前窗口的宽度和高度。

### 定时器

- `setTimeout()`：定时器，只执行一次

    window.setTimeout(function(){}, 1000)// 1000毫秒后执行

    setInterval(function(){}, 1000)

setTimeout()方法用于设置一个定时器，该定时器到时间之后执行调用函数。(window可以省略)

setTimeout()的函数可以直接写函数，也可以写函数名来调用函数。

setTimeout()调用的这个函数我们称作`回调函数`，普通的函数，我们都是按照代码顺序直接调用，但是在`setTimeout()`里面，这个函数需要等待的时间结束之后，采取调用这个函数，就是时间结束之后，再回过头去调用这个函数。

页面中，可能有多个定时器，为了防止混淆，我们经常会将定时器的id保存起来，以便后面使用。

    var timer = setTimeout(function(){}, 1000)

**停止定时器**

```
    window.clearTimeout(定时器名字)// 清除定时器
    (window可以省略)
```

- `setInterval()`：定时器，可以重复执行

```
    window.setInterval(回调函数,间隔时间);
```
每隔设定的间隔时间，就去调用一次回调函数。(重复调用这个函数)

**停止定时器**

```
    window.clearInterval(定时器名字)// 清除定时器
    (window可以省略)
``` 