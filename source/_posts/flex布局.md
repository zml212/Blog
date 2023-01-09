---
title: Flex布局
date: 2022-8-22
tags: [前端]
categories: [css]
---
# flex布局

在传统的网页布局模式中，我们使用的是盒模型，这种盒模型往往依赖dispaly属性，position属性，float属性，但是对于一些特殊布局这样的布局方式会非常的不方便，比如在垂直居中的时候，使用position布局就会非常不方便，而且使用float布局的话，后面我们还需要清除浮动，非常的不方便。

## 什么是flex布局

flex是Flexible Box的缩写，意思为弹性布局，在html中任何一个容器都可以被指定为flex布局。

*注意：*

html中，设置为flex布局之后，它的子元素**float、clear、vertical-align**属性将会失效

## flex的基本概念

采用flex布局的元素，我们称之为容器，这个容器里面的所有子元素我们称之为容器成员，也就是flex项目，我们简称为项目。

在flex容器中，存在两根轴，主轴和交叉轴；在主轴的开始位置我们称之为main start,结束位置我们称之为main end；交叉轴的开始位置叫做cross start,结束位置我们称之为cross end。

主轴默认水平排列，交叉轴默认竖直排列，项目默认沿着主轴排列。单个项目占用主轴的空间叫做main size，单个项目占用交叉轴的空间叫做cross size。

## 容器的属性

下面列出容器的属性：

- flex-direction
- flex-wrap
- flex-flow
- justify-content
- align-items
- align-content

### flex-direction属性

flex-direction属性可以决定主轴的方向（也就是我们项目的排列方向），当主轴的排雷方向发生改变的时候，交叉轴的方向随之发生改变。

    .div{
        flex-direction:row | row-reverse | colum | colum-reverse;
    }

这个属性可能有四个值：

- row(默认值):主轴为水平方向，起点在左端。
- row-reverse:主轴为水平方向，起点在右端。
- column:主轴为垂直方向，起点在上面。
- colum-reverse:主轴为垂直方向，起点在下面。

### flex-wrap属性

在默认情况下，项目都是排列在主轴上的，当主轴的长度不够时，项目会换行。换行的方式我们可以通过flex-wrap属性来指定。

    .div{
        flex-wrap:nowrap | wrap | wrap-reverse;
    }

这个属性可以有三个值：

- nowrap(默认值):项目不会换行。
- wrap:换行，第一行会在上方。
- wrap-reverse:换行，第一行会在下方。

### flex-flow属性

flex-flow属性可以同时指定flex-direction和flex-wrap属性。默认值是row nowrap。

    .div{
        //flex-flow:<flex-direction> <flex-wrap>;
        flex-flow:row nowrap;
    }

### justify-content属性

justify-content定义了项目在主轴上的对齐方式。

    .div{
        justify-content:flex-start | flex-end | center | space-between | space-around;
    }

这个属性可以有五个值：(具体对齐方式与主轴方向有关)

- flex-start:。从主轴的起点开始往终点排列项目。
- flex-end:。从主轴的终点开始往起点排列项目。
- center:。项目排列在主轴的中间。
- space-between:。项目排列在主轴的中间，两端对齐，项目之间的间隔都相等。
- space-around:。项目排列在主轴的中间，两端对齐，项目之间的间隔不相等(项目之间的距离是项目距离边框的两倍)。

### algin-items属性

algin-items定义了项目在交叉轴上的对齐方式。

    .div{
        align-items:flex-start | flex-end | center | baseline | stretch;
    }

这个属性可以有五个值：(具体对齐方式同样与交叉轴方向有关)

- flax-start:。项目在交叉轴的起点开始往终点排列。
- flex-end:。项目在交叉轴的终点开始往起点排列。
- center:。项目在交叉轴的中间。
- baseline:。项目在交叉轴的基线(也就是与项目第一行文字的基线对齐)。
- stretch:。项目在交叉轴的中间，并且占满交叉轴的空间(也就是如果项目没有设置高度或者设置为auto那么这个项目将会占满整个容器)。
  
### align-content属性

align-content定义了**多根轴线**的对齐方式。如果项目只有一根轴线，那么这个属性就不会起作用。

    .div{
        align-content:flex-start | flex-end | center | space-between | space-around | stretch;
    }

这个属性可以有六个值：

- flex-start:从交叉轴的起点开始往终点排列。
- flex-end:从交叉轴的终点开始往起点排列。
- center:项目在交叉轴的中间。
- space-between:项目在交叉轴的中间，两端对齐，项目之间的间隔都相等。
- space-around:项目在交叉轴的中间，两端对齐，项目之间的间隔不相等(项目之间的距离是项目距离边框的两倍)。
- stretch:轴线占满整个交叉轴。

## 项目属性

前面我们说了容器的属性，这里我们再来了解一下项目的属性。

- order　　项目的排列顺序。数值越小，排列越靠前，默认为0。
- flex-grow　　项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
- flex-shrink　　项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
- flex-basis　　在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。
- flex　　是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。
- align-self　　允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。
