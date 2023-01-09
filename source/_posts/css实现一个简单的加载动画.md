---
title: CSS实现一个简答的加载动画
date: 2022-10-27
tags: [前端]
categories: [css,html]
---
# css实现一个简单的加载动画

在网页的页面中，我们有时候需要等待服务器返回数据给我们，我们再将其渲染到页面上。

但是服务器返回数据给我们的时候，这个是有延迟的；为了让用户得到更好的使用体验。我们需要在用户等待这段时间给用户一个加载动画。

今天我们要实现的是一个圆圈旋转加载效果。

**效果图**：
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/146696d099ba4c2383c5ea1615861016~tplv-k3u1fbpfcp-watermark.image?)

## HTML框架

既然有一个圆圈，那么必定就会有一个盒子。  
但是圆圈不能是全封闭的，得留一个缺口出来，那么这个缺口怎么实现呢？

我们这里的解决方法就是设置一个盒子，覆盖在圆圈的上面，背景颜色与圆圈里面的背景颜色一样。

话不多说，上代码：

```html
    <div class = "border"> // 外部圆圈
        <div class = "box"></div> // 遮住圆圈的盒子
    </div>
```

## 加载动画的圆圈

圆圈我们可以通过边框来实现。首先给边框设置一下：边框厚度，边框样式，边框颜色。

然后给边框设置一个`border-radius`属性。

```css
    .border{
        box-sizing:border-box;
        width:50px;
        height:50px;
        border:5px solid #0ff;
        border-radius:50%;
    }
```
我们这里使用`border-box`盒模型，主要是防止边框的问题将这个盒子撑大。

因为我们要通过圆角边框来实现圆圈。所以我们首先将`.border`盒子设置相同的宽高，使之成为一个正方形；然后通过`border-radius`属性设置为50%。  
圆圈就出来了。

为了使这个加载动画更加美观，我们使用定位将其放到页面中央：

```css
.border{
    position: absolute;
    top: 50%;
    margin-top: -50px;
    left: 50%;
    margin-left: -50px; 
}
```

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/54478ba8ee4945239063586e2be6a90b~tplv-k3u1fbpfcp-watermark.image?)

## 圆圈的缺口

在上面的效果图里面，加载动画圆圈是由一个缺口的，我们在这里使用一个同背景色的盒子来遮住圆圈实现。

```css
.border>.box {
    position: relative;
    top: 50%;
    left: 50%;
    width: 25px;
    height: 25px;
    background-color: inherit;
}
```

我们还需要将这个用于遮罩的盒子，移动到圆圈的旁边，所以我们使用到了绝对定位。  
在背景颜色上面，我们选择继承父盒子的颜色。

为了尽量不影响其他元素。我们将这个遮罩的宽高设置为圆圈直径的（父盒子宽高）一半。

最后出现的效果就是这样：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2a6d97fcb9ea49118d2c5ea04e95c014~tplv-k3u1fbpfcp-watermark.image?)

接下来就是添加动画让圆圈动起来：

## 动画设置

为了让这个加载动画看起来更加丝滑，我们不设置用百分比的方式来设置，使用`from to`的方式来设置动画。

然后我们还要设置动画旋转的中心点，默认的中心点是盒子的左上角，但这不是我们想要的效果，我们希望从盒子中心旋转。于是我们使用`transform-origin: center center;`的方式将旋转中心设置为盒子中央。

```css
    @keyframe rotate{
        from{
            transform-origin: center center;
            transform: rotate(0deg);
        }
        to{
            transform-origin: center center;
                transform: rotate(360deg);
        }
    }
```

我们这里就把加载动画设置完毕了，我么将这个动画加在`.border`的盒子上。

```css
    .border{
        animation: rotate 1s linear infinite;
    }
```

最后的效果展示：

![动画1.gif](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/9f826fdec7974092906cf5b7aa55b087~tplv-k3u1fbpfcp-watermark.image?)