---
title: Vue3中的过渡动画
data: [2023-3-13]
tags: [前端]
categories: [Vue3]
---

# Vue3中的过渡动画效果

在日常网页的开发中，难免会有一些动画的交互效果。

首先我们将要有动画效果的盒子使用`transition`标签包裹起来，然后给这个标签给予一个`name`属性，这个属性我们在后面css的类名中会用到。

比如：

```html
<transition name="hmbb">
<div v-if="isTrue" class="style">我是组件的内容</div>
</transition>
```

这就是一个用`transition`标签包裹起来的盒子，我们给其name属性赋值为`hmbb`。

这个时候我们就可以在`style`标签里面给这个盒子设置相应的动画样式了。

在vue3中，有这样几个状态：

- v-enter-from：定义组件进入过渡的开始状态，在元素被插入之前生效，在元素插入后的下一帧失效。
- v-enter-acticve：定义过渡生效时的状态，在整个过渡阶段中应用，在元素插入之前生效，在过渡动画效果完成后移除，在这里面我们可以定义一些过渡的效果，延迟以及速度变化的曲线。
- v-enter-to：组件过渡开始
- v-leave-from
- v-leave-active
- v-leave-to

