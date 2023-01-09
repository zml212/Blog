---
title: CSS动画
date: 2022-10-6
tags: [前端]
categories: [css]
---
# CSS动画

## 1. CSS动画的基本概念

动画是一种使元素从一种样式逐渐转变为另一种样式的效果。CSS动画是通过改变元素的样式来实现的，这些样式可以是元素的位置、大小、颜色、背景、边框等等。

CSS动画包含两个部分：

- 描述动画的样式规则
- 描述动画开始、中间、结束的关键帧

## 2.动画需要用到`@keyframes`

`@keyframes`是CSS3中新增的一个规则，用来描述动画的关键帧。关键帧是动画的开始、中间、结束的状态。关键帧的名称可以自定义，但是必须以`%`结尾，表示动画的进度。(也可以直接from[起点] to[终点])

```css
    @keyframes example1 {
        from {background-color: red;}
        to {background-color: yellow;}
    }

    @keyframes example2 {
        0% {background-color: red;}
        25% {background-color: yellow;}
        50% {background-color: blue;}
        100% {background-color: green;}
    }
```

我们使用上面两段代码演示了一下这两种定义动画的方式，一种是from...to...,一种是百分比的方式。

很明显使用百分比的方式可以更加精确的控制动画的效果。

## 3.css动画的属性

- animation-name：动画的名称，必须与`@keyframes`中的名称一致
- animation-duration：动画的持续时间
- animation-delay：动画的延迟时间
- animation-iteration-count：动画的播放次数
- animation-direction：动画的方向
- animation-timing-function：动画的速度曲线
- animation-fill-mode：动画的填充模式
- animation：设置动画的所有简写属性

### 3.1 animation-duration

动画的持续时间，也就是动画从开始到结束的时间，可以使用`s`或者`ms`来设置，如下：

```css
    animation-duration: 5s;
    animation-duration: 5000ms;
```

### 3.2 animation-delay

动画的延迟时间，也就是动画开始前的延迟时间，同样也是使用`s`或者`ms`来设置，如下：

### 3.3 animation-iteration-count

该属性用于设置动画播放的次数，可以是一个数字，也可以是`infinite`，表示无限次播放，如下：

```css
    animation-iteration-count: 3;// 只播放三次动画
    animation-iteration-count: infinite;// 无限循环
```

### 3.4 animation-direction

该属性可以设置动画的播放方向，可以是`normal`、`reverse`、`alternate`、`alternate-reverse`，如下：

- normal: 正常播放(从0%-->100%)
- reverse: 反向播放(从100%-->0%)
- alternate: 交替播放(从0%-->100%-->0%)
- alternate-reverse: 交替反向播放(从100%-->0%-->100%)

### 3.5 animation-timing-function

该属性用来设置动画的播放速度曲线，可以是`linear`、`ease`、`ease-in`、`ease-out`、`ease-in-out`、`cubic-bezier()`，如下：

- linear: 动画从头到尾的速度是相同的
- ease: 默认值，动画以低速开始，然后加快，在结束前变慢
- ease-in: 动画以低速开始
- ease-out: 动画以低速结束
- ease-in-out: 动画以低速开始和结束
- cubic-bezier(n,n,n,n): 在 cubic-bezier 函数中自己的值。可能的值是从 0 到 1 的数值

### 3.6 animation

前面我们每一个属性就写一行代码的话，显得十分臃肿，这里我们可以使用简写属性的方式来解决问题。

```css
    animation: name duration timing-function delay iteration-count direction fill-mode;
```

可以看到，书写的属性顺序为：动画名字、持续时间、速度曲线、延迟时间、播放次数、播放方向、填充模式。

不需要的属性我们可以直接省略，但是顺序不能改变。