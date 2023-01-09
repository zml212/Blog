---
title: CSS中的定位
date: 2022-10-15
tags: [前端]
categories: [css]
---
# CSS中的定位

在页面布局中，我们经常会对一些盒子调整位置，普通的浮动或者一些其他方法，但是这些方法要么达不到我们想要的效果要么就是实现起来太复杂了。  
今天我们要讲到的定位就可以很好的解决这个问题。

技术发展就是这样的：出现什么问题，过不了多久就会出现新的解决语法。

## position(定位)

position属性用来设置元素的定位方式，有以下几种：

* static：默认值，元素在正常的流中，top、right、bottom、left和z-index属性无效。
* relative：元素在正常的流中，相对于其在正常流中的位置进行定位，top、right、bottom、left和z-index属性有效。
* absolute：元素脱离正常的流，相对于其最近的已定位祖先元素进行定位，如果没有已定位的祖先元素，则相对于body元素进行定位，top、right、bottom、left和z-index属性有效。
* fixed：元素脱离正常的流，相对于浏览器窗口进行定位，top、right、bottom、left和z-index属性有效。
* sticky：当页面滚动或者改变后，该盒子未超出目标区域，它的表现就像absolute;超出目标区域之后，它就表现为fixed定位。

### static 

static是默认值，元素在正常的流中，top、right、bottom、left和z-index属性无效。
```js
    .box{
    position: static;
}
```

### relative

relative是相对定位，元素在正常的流中，相对于其在正常流中的位置进行定位，top、right、bottom、left和z-index属性有效。

```js
    .box{
    position: relative;
    top: 100px;// 距离顶部100px
    left: 100px;// 距离左边100px
}
```

### absolute

absolute是绝对定位，元素脱离正常的流，相对于其最近的已定位祖先元素进行定位，如果没有已定位的祖先元素，则相对于body元素进行定位，top、right、bottom、left和z-index属性有效。

```js
    .box{
    position: absolute;
    top: 100px;// 距离顶部100px
    left: 100px;// 距离左边100px
}
```

这个属性值定位的参考点是最近的已定位的祖先元素，如果没有已定位的祖先元素，则相对于body元素进行定位。

### fixed

fixed是固定定位，元素脱离正常的流，相对于浏览器窗口进行定位，top、right、bottom、left和z-index属性有效。

```js
    .box{
    position: fixed;
    top: 100px;// 距离顶部100px
    left: 100px;// 距离左边100px
}
```

不管这个页面如何滚动（改变），这个盒子都会固定在浏览器窗口的某个位置。

### sticky

sticky是粘性定位，它的特性是当页面滚动或者改变后，该盒子未超出目标区域，它的表现就像absolute;超出目标区域之后，它就表现为fixed定位。

```js
    .box{
    position: sticky;
    top: 100px;// 距离顶部100px
    left: 100px;// 距离左边100px
}
```

这段代码就是让box盒子在滚动到距离上面100px或者左边100px的时候，就固定在浏览器窗口的这个位置。

## z-index(层叠顺序)

z-index属性用来设置元素的层叠顺序，它的值是一个整数，数值越大，层叠顺序越靠上。

```js
    .box{
    position: relative;
    z-index: 1;
}
```

z-index可以设置为负值，但是不建议这么做。

## 总结

- 在使用绝对定位的时候，一定要注意定位的参考点，如果没有已定位的祖先元素，则相对于body元素进行定位。（子绝父相）
- 在使用sticky粘性定位的时候，必须使用top,botton,left,right中的一个或者多个属性，否则不会生效。
- 使用固定定位的时候，在其父级元素中，如果有overflow:hidden;overflow:auto;overflow:scroll;这些属性，那么这个盒子就会被裁剪掉，所以在使用固定定位的时候，一定要注意这个问题。
- 父级元素的高度如果没有设置，那么子元素的固定定位是不会生效的，所以在使用固定定位的时候，一定要注意父级元素的高度。（父元素高度 > 子元素高度）
- 固定定位的生效范围只在父元素内。