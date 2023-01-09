---
title: 纯css实现跑马灯效果
date: 2022-12-3
tags: 前端
categories: [css,html]
---
# 纯CSS实现跑马灯效果

又是一个安静的晚上，又是没有灵感的一天，又只能去逛逛`codepen`。看到一个流光的边框效果，我就想如何通过自己的方式来现这样一个效果。

突然又想起路边夜市的招牌，不就是一个流光效果加一个广告词吗。这下我的兴趣一下就激发起来了。

首先上一个效果图

![跑马灯.gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b7bd3aa2a13e4bf1be984a32560e72e7~tplv-k3u1fbpfcp-watermark.image?)

那这里的广告词就写我的名字吧，这都不重要，接下来让我们看看是怎么实现的吧。

## 结构分析

首先很明显我们使用了一个盒子将文字装起来，并且将文字进行了居中，然后我们看到这盒子的周围围绕了两条光带，那么这两条光带是怎么实现的呢？

其实这是用四个小盒子实现的，就是我们将这四个小盒子分别放在大盒子的周围（紧贴这大盒子的内壁），然后使用这几个小盒子实现跑马灯效果。

接下来我们就看看具体的实现效果

## 代码实现

### 外部大盒子

首先准备一个`div`盒子，用来装我们的广告语，还有跑马灯的四个小盒子。
```html
<div class="main">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        海绵宝宝
</div>
```

这个盒子就准备好了  我们给这个何止设置一点样式，让他在浏览器中可以被我们看到

```html
.main{
    width: 100px;
    height: 40px;
    background-color: transparent;
    /* 盒子居中 */
    position: absolute;
    top: 50%;
    margin-top: -20px;
    left: 50%;
    margin-left: -50px;
    /* 文字居中 */
    text-align: center;
    line-height: 40px;
    font-family: '宋体';
    color: aquamarine;
    overflow: hidden;
}
```

我们给这个大盒子设置了宽高，以及背景颜色，这样使得我们的盒子在浏览器中显得不那么突兀，并且我们将盒子在浏览器中居中，并且将盒子里面的文字也进行居中，不要问我为什么一定要居中，纯属我的强迫症。

到现在为止，我们就已经把大盒子准备好了，效果就是这样的：

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/32d4a7803a2e4520904bc5cd03ecfdfe~tplv-k3u1fbpfcp-watermark.image?)

接下来我们给大盒子设置灯光效果。

### 内部灯光效果

既然是灯光效果，那颜色就必须得亮，这里我选着`#0FF`配色，在效果图上，我们可以看到灯光尾部的颜色比较淡，展现出一种过渡效果，组件从头部慢慢过渡到小盒子的原本颜色。

那这样的效果是怎么实现的呢？

这里就需要用到`background`的一个新的属性值:`linear-gradient`,通过这个属性，我们可以将多个颜色设置为一个盒子的背景颜色，并且还有过渡效果，更强大的地方在于，这个属性值可以设置渐变的方向。

语法：

`linear-gradient(渐变方向,color1,color2,color3...)`

那就开始使用这个属性值，我们写其中一个小盒子的效果，其他的效果实现方法都类似。

```css
.main :nth-child(1){
    background: linear-gradient(to right, transparent , #0FF);
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 1px;
}
```

我们这里设置的是顶部的灯光效果，这里我们使用`top`和`left`的属性，设置这个盒子的位置；使用`width:100%`，使得这个盒子的宽度与大盒子保持一致，然后使用`height:1px`,将灯光条的宽度设置为1px,当然你也可以根据实际需求，改变灯光条的宽度。

接着剩下三条灯光的实现原理基本相同，只有位置和背景颜色不同。

剩下背景颜色的渐变方向分别是`to bottom`、`to left`、`to top`。

最后的效果就是这样：

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3c902a90d4444931a0dae4dd13fc3c91~tplv-k3u1fbpfcp-watermark.image?)

是不是离我们最终的效果越来越像了，对的，我们还差最后一部，给灯光加上动画效果。

### 灯光的动画效果

说道动画效果，那必定离不开`@keyframes`,我们使用该属性为我们的灯光添加动画效果。

同样我们也设置顶部的灯光效果作为示范：

动画：

```css
@keyframes run1{
    0%{
        transform: translateX(-200px);
    }
    100%{
        transform: translateX(200px);
    }
}
```

盒子：

给盒子加上动画

```css
.main :nth-child(1){
    animation: run1 1s linear infinite;
}
```

前面我们没有设置动画之前，可以看到颜色最深的部分在最右边，但是跑马灯的实现需求是需要颜色最什么部分从左边出现，然后移动到右边，所有我们这里将动画的初识位置向左移整个盒子的长度。这样就实现了需求。

其他几个小盒子实现的方式大同小异。

接下来我们来看看效果：

![跑马灯.gif](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/14c2f986d6f74376bd515021cf4ec0b4~tplv-k3u1fbpfcp-watermark.image?)

此时我们看到该动画显得有点乱，我们通过使用动画延时来实现我们想要的动画效果。

我们从第二个小盒子开始添加延时：每个盒子比前一个盒子多`0.5`的延时。

```css
.main :nth-child(2){
    animation-delay: .5s;
}
.main :nth-child(3){
    animation-delay: 1s;   
}
.main :nth-child(4){
    animation-delay: 1.5s;
}
```

最后效果就出来了。



接下来放上码上掘金的地址，这里有全部代码：↓

[代码片段](https://code.juejin.cn/pen/7172800105433530400)