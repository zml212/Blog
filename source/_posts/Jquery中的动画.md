---
title: jQuery中的自带动画
date: 2022-10-23
tags: [前端]
categories: [jQuery]
---
# jQuery中的自带动画

jQuery是一个非常强大的JavaScript库，它提供了很多非常有用的功能，其中包括动画。

在前端的实际开发中，我们经常会遇到一些动画效果，比如：鼠标移入移出，点击，滚动等等。这些动画效果，我们可以使用jQuery来实现。下面我们就来看看jQuery中的动画。

## jQuery自带的动画

在jQuery中内置了一些动画，比如元素的隐藏与显示、淡入淡出、滑动等等。这些动画都是通过jQuery提供的方法来实现的。

### 1. 隐藏与显示

语法：
`$(selector).hide(speed,callback);`隐藏元素
`$(selector).show(speed,callback);`显示元素
`$(selector).toggle(speed,callback);`切换元素的显示与隐藏

根据语法，我们不难看出，这两个方法都是通过选择器来选择元素，然后通过speed参数来设置动画的速度，最后通过callback参数来设置动画完成后的回调函数。

代码演示：

```js
$(document).ready(function(){
    $("#btn1").click(function(){
        $("p").hide();
    });
    $("#btn2").click(function(){
        $("p").show();
    });
});
```

上面我们获取了两个按钮，第一个按钮控制元素的隐藏，第二个按钮控制元素的显示。

### 2. 淡入淡出

在jQuery中，有四种方法来实现淡入淡出的效果，分别是：

1. `fadeIn()`：淡入
2. `fadeOut()`：淡出
3. `fadeToggle()`：淡入淡出切换
4. `fadeTO()`：淡入淡出到指定的不透明度

上面的前种方法都支持speed和callback两个参数，其中speed参数用来设置动画的速度，callback参数用来设置动画完成后的回调函数。

其中第三种方法可以切换元素淡入淡出的效果，如果元素是隐藏的，那么就会淡入，如果元素是显示的，那么就会淡出。

`$(selector).fadeIn(speed,callback);`
`$(selector).fadeOut(speed,callback);`
`$(selector).fadeToggle(speed,callback);`



代码演示：

```js
$(document).ready(function(){
    $("#btn1").click(function(){
        $("#div1").fadeIn();
        $("#div2").fadeIn("slow");
        $("#div3").fadeIn(3000);
    });
});
```

第四种方法呢：参数与前面三种有点区别，它除了speed和callback两个参数外，还有一个参数：opacity，用来设置元素淡入淡出到指定的不透明度。

`$(selector).fadeTo(speed,opacity,callback);`

代码演示：

```js
$(document).ready(function(){
    $("#btn1").click(function(){
        $("#div1").fadeTo("slow",0.15,1000);
    });
});
```

### 3. 滑动

在jQuery中，我们还可以通过其自带的方法实现滑动的效果。

jQuery实现滑动的方法也有三种：

1. slideDown()：向下滑动
2. slideUP()：向上滑动
3. slideToggle()：向上向下滑动切换

`$(selector).slideDown(speed,callback);`
`$(selector).slideUp(speed,callback);`
`$(selector).slideToggle(speed,callback);`

参数：

在这三种方法中，speed和callback参数的含义与前面的方法是一样的。都是定义动画的速度有完成动画之后的回调函数.

代码演示：

```js
$(document).ready(function(){
    $("#btn3").click(function(){
        $("#div1").slideToggle();
        $("#div2").slideToggle("slow");
        $("#div3").slideToggle(3000);
    });
});
```

这里我们只展示了`slideToggle()`方法，其他两种方法的使用方法与`slideToggle()`方法是一样的。