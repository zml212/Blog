---
title: jQuery中自定义动画
date: 2022-10-24
tags: [前端]
categories: [jQuery]
---
# jQuery中自定义动画

在上一篇文章当中，我们讲述了jQuery中自带的一些动画属性，但是在开发中，产品经理提出的一些需求靠这些自带的动画，往往是实现不了的，这个时候，我们就需要自定义动画了。

## 1. 自定义动画

在jQuery中，自定义动画的方法是通过animate()方法来实现的，这个方法的参数是一个对象，对象中的属性是需要改变的样式，属性值是改变后的样式值。

`$(selector).animate({params},speed,callback);`

**解释：**

- selector：必需。规定要添加动画效果的元素。
- params：必需。规定形成动画效果的 CSS 属性集。[对象]
- speed：可选。规定完成动画的时长。它可以取以下值："slow"、"fast" 或毫秒。
- callback：可选。在动画完成时所执行的函数名称。

注意看，在动画第二个参数中，我们传入的是一个对象，这个对象中的属性是需要改变的样式，属性值是改变后的样式值。  
并且我们可以传入多个样式，来实现多个样式的动画。

### 1.1动画操作多个属性

我们可以通过animate()方式来为一个动画操作该元素的多个属性。

```js
    $('button').click(function(){
        $('div').animate({
            width:'300px',
            height:'300px',
            opacity:'0.5'
        },1000);
    });
```

代码解释：

在上面的代码当中，我们为button绑定了一个点击事件，当点击button的时候，我们就会让div元素的宽度、高度、透明度都发生变化。  
这里让这个动画在一秒内同时操作了div元素的宽度、高度、透明度，这样就实现了一个动画操作多个属性的效果。

### 1.2动画中属性使用相对值

是不是觉得每次为动画设置属性值很麻烦，因为这些都是基于浏览器默认的样式值，如果我们想要的效果是基于当前的样式值，那么我们就需要使用相对值来实现。

使用相对值的方法就是在元素属性值前面加上+=或者-=，这样就可以实现相对值的效果。

- +=：表示相对于当前元素增加
- -=: 表示相对于当前元素减少

```js
    $('button').click(function(){
        $('div').animate({
            width:'+=300px',
            height:'+=300px',
        },1000);
    });
```

代码解释：

在上面的代码中，我们将div元素的宽度和高度都设置为相对值，将这个div元素的宽度和高度都增加300px。

### 1.3动画中的队列功能

其实在jQuery中，我们还可以同时为一个元素设置多个动画，这样就可以实现一个元素的多个动画效果。

并且这些多花按照从上之下的顺序依次执行。

```js
    $('button').click(function(){
        var div=$('div')；
        div.animate({
            width:'300px',
            height:'300px',
            opacity:'0.5'
        },1000);
        div.animate({
            width:'600px',
            height:'600px',
            opacity:'1'
        },1000);
    });
```

这样极大的提高了我们的开发效率。

## 2.动画停止

在动画执行的过程中，我们可以随时停止。

停止动画我们需要用到`stop`方法。

`$(selector).stop(stopAll,goToEnd);`

- 什么参数也没有：停止当前正在执行的动画，但是不清除队列中的动画。
- 只有一个stopAll参数，当参数为true：停止当前正在执行的动画，并且清除队列中的动画；当参数为false:停止当前正在执行的动画，但是不清除队列中的动画。
- goToEnd参数：当参数为true：停止当前正在执行的动画，并且清除队列中的动画，并且将动画的属性值设置为最终的值；当参数为false:停止当前正在执行的动画，但是不清除队列中的动画。

```js
$(document).ready(function(){
    $("#btn1").click(function(){
        $("#panel").slideDown(5000);
    });
    $("#bnt2").click(function(){
        // $("#panel").stop();
        // $("#panel").stop(true);
        // $("#panel").stop(false);
        // $("#panel").stop(true,true);
        $("#panel").stop(false,true);
    });
});
```

上面停止动画的代码中，从上至下，我们分别使用了不同的参数来停止动画，可以看到不同的效果。

## 3.动画回调函数

在一些开发中，我们希望在动画完成之后，来执行一些操作，这时候我们就需要使用到动画回调函数。

动画里面的回调函数，只会在动画执行完毕之后，才会被调用，这就避免了在动画执行的过程中，执行回调函数。

```js
    $('button').click(function(){
        $('div').animate({
            width:'300px',
            height:'300px',
            opacity:'0.5'
        },1000,function(){
            alert('动画执行完毕');
        });
    });
``` 

代码解释：

上面的代码中，在动画执行完毕之后，会弹出一个提示框，提示动画执行完毕。