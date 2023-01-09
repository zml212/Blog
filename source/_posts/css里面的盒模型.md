---
title: CSS盒模型
date: 2022-10-17
tags: [前端]
categories: [css]
---
# 教你5分钟搞懂css里面的盒模型

## 什么是盒模型

在html中，我们写的标签，很多都是有盒模型的，也就是在css里面，我们为这些标签设置样式的时候，实际上就是为这些盒子设置样式。

可能这样不是很好理解，我们来看一个例子：

在日常生活中，我们经常使用一些盒子来装东西，此时我们将盒子想象成从上往下看的二维平面：  
那么盒子将会包含以下东西：

- 盒子里面装的东西【内容】
- 盒子内容与盒子包装的距离【内边距】
- 盒子的厚度：【边框】
- 盒子距离其他东西的距离【外边距】

css里面的盒模型就包含内容`content`,内边距`padding`,边框`border`,外边距`margin`
如下图所示：

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8bf44e4c1d4e4aa1806ec005354e67aa~tplv-k3u1fbpfcp-watermark.image?)

- 白色框框里面的就是内容`content`
- 绿色箭头的距离就是内边距`padding`
- 再往外的黑色边框就是盒子的边框`border`
- 最外面的的蓝色箭头就是外边距`margin`

## 盒模型的分类

在如今css的标准里面，盒模型有两种，一种是标准盒模型，一种是IE盒模型。

css默认使用的是ie盒模型，这就导致了一些初学者在设置css样式的时候出现一些出乎意料的问题。

至于是什么问题呢？  
接着往下看。

### ie盒模型

首先我们来讲讲ie盒模型是怎么样子的。

我们在ie盒模型中，设置的宽度以及高度，只是包含了内容`content`的宽度以及高度，不包含内边距`padding`以及边框`border`的宽度以及高度。

```css
.box{
    width: 100px;
    height: 100px;
    padding: 10px;
    border: 1px solid #000;
    margin: 19px;
}
```
![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6e263168872d44beade22cf89637312b~tplv-k3u1fbpfcp-watermark.image?)

在浏览器开发者工具中，我们可以看到，该盒子的内容内容长宽都为100px,但是盒子的实际长宽为130px，这就是因为ie盒模型长宽只包含内容的长宽。

此时我们设置的长宽度计算方式为：

`width = content.width`  
`height = content.height`

这时候，问题就来了：  
一些初学者明明只给盒子设置了100px的宽度，为什么盒子的实际宽度却是130px呢？

看来上面的例子，就很明了了，这就是ie盒子长宽不包含内边距以及边框的宽度导致的问题。

### 标准盒模型

标准盒模型，就是我们常说的盒模型，它的长宽包含了内容`content`，内边距`padding`，边框`border`的宽度以及高度。

**注意：**

- 标准盒模型并不包含外边距`margin`的宽度以及高度。
- 在使用标准盒模型之前，我们需要通过`box-sizing: border-box;`来设置盒模型的类型。

```css
.borderBox {
            box-sizing: border-box;
            width: 100px;
            height: 100px;
            padding: 10px;
            border: 1px solid #000;
            margin: 19px;
        }
```

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/054c4c4110094f78911e7579839d7a4f~tplv-k3u1fbpfcp-watermark.image?)

在上面的图片我们可以看到，盒子里面的内容明显比ie盒模型的内容要小，这是因为标准盒模型的长宽包含了内容`content`，内边距`padding`，边框`border`的宽度以及高度。

此时的我们设置的长宽度计算方式为：  
`width = content.width + padding.width + border.width`  
`height = content.height + padding.height + border.height`

