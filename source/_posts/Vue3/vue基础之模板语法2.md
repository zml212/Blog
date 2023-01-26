---
title: vue基础之模板语法2
data: 2023-1-25
tags: [前端]
categories: [Vue3]
---

# Vue基础之模板语法（二）

[TOC]

## 条件渲染

在某些情况下，我们需要根据当前的条件决定某些元素或者组件是否渲染，这个时候我们就需要进行条件判断。

vue提供了下面的几个指令来进行条件判断：

- v-if
- v-elseb
- v-else-if
- v-show

### v-if  v-else  v-else-if

这三个指令都是根据条件来渲染某一块的内容，这些内容只有条件为true的时候，才会渲染出来；这三个指令与JavaScript的条件语句if  else  else if类似。

多个条件判断例子：[这里](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/04_%E6%9D%A1%E4%BB%B6%E6%B8%B2%E6%9F%93/02_%E5%A4%9A%E4%B8%AA%E6%9D%A1%E4%BB%B6%E7%9A%84%E6%B8%B2%E6%9F%93.html)

**v-if的渲染原理：**

- v-if是惰性的
- 当条件为false的时候，他里面内容不会被渲染或者被销毁掉
- 当条件为true的时候，才会真正的渲染里面的内容

### Template元素

因为v-if是一个指令，所以必须将其添加到一个元素上面：

- 但是如果我们希望切换的是多个元素的时候，我们之前使用的div来切换，但是我们并不希望div这种元素被渲染，这个时候就可以用到template元素。

示例代码在：[这里]()

### v-show

v-show与v-if的用法看起来是一样的，同样也是根据一个条件决定是否显示元素或者组件

示例代码地址：[这里](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/04_%E6%9D%A1%E4%BB%B6%E6%B8%B2%E6%9F%93/04_v-show%E7%9A%84%E6%9D%A1%E4%BB%B6%E6%B8%B2%E6%9F%93.html)

### v-if与v-show的区别：

当他们判断条件都为真的时候没有什么区别，真正的区别在于他们判断条件为false的时候，我们可以看到在v-if中，源代码是找不到这个html标签的，但是在v-show里面，我们可以看到源代码里面有这样一个标签，并且设置的样式为`display:none`，也就是说使用v-show的时候，这个标签一直都存在。

**注意：**

- 用法上的区别：
  - v-show是不支持template的
  - v-show不可以与v-else一起使用
- 本质的区别：
  - v-show元素无论是否需要渲染到浏览器上，它的DOM实际上一直都在渲染，只是通过CSS的display属性隐藏起来了
  - v-if条件为false的时候，其对应的元素压根不会渲染到DOM当中

也就是说如果v-if的条件为假，那么这个元素根本不会渲染，那么如果某个元素对于显示与隐藏的切换不频繁，那么使用v-if更加节约性能；如果某个元素切换比较频繁，那么使用v-show更加节约性能。

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d69bb0d345ac465ca8e3ae4a01366f7a~tplv-k3u1fbpfcp-watermark.image?)

示例代码在：[这里](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/04_%E6%9D%A1%E4%BB%B6%E6%B8%B2%E6%9F%93/05_v-if%E4%B8%8Ev-show%E7%9A%84%E5%8C%BA%E5%88%AB.html)

## 列表渲染

在实际的开发当中，我们可能会从服务器拿到一些数据，需要用列表的形式展示出来，这个时候我们就需要使用列表渲染。

在vue当中，列表渲染需要使用到v-for，v-for就类似于JavaScript中的for循环，可以用来遍历一组数据。

### v-for基本使用

**v-for遍历数组：**

v-for的基本格式是：`item in 数组`

- 数组通常是来自data或者prop，也可以是其他方式
- item是我们给每项元素起的一个别名，这个别名可以自定义

语法示例：

```html
<ul>
    <li v-for="num in arr">{{num}}</li>
</ul>
```

这里我们只获取到了数组里面的内容，要是我们还想获取索引怎么办呢？我们可以这样操作：

```html
<ol>
    <li v-for="(num,index) in arr">{{num}},{{index}}</li>
</ol>
```

就是在前面加上一个括号，括号里面有两个参数，第一个就是我们获取的内容，第二个参数就是我们的索引。

**v-for遍历对象：**

前面我们了解了如何使用v-for遍历一个数组，其实v-for还可以用来遍历对象：

`v-for="item in Obj"`

众所周知，在对象里面有key-value键值对，那么我们v-for第一个参数(item)拿到的是什么呢？

答案是对象里面的value。

想要拿到key，那么我们就可以这样操作：

```html
<ol>
    <li v-for="(value,key) in obj">{{key}}:{{value}}</li>
</ol>
```

这样就拿到了key，如果想要拿到索引，我们可以接着看：

```html
<ol>
    <li v-for="(value,key,index) in obj">{{index}}--{{key}}:{{value}}</li>
</ol>
```

这样第一个参数代表值，第二个参数代表键名，第三个参数代表索引。

**v-for遍历数字：**

也就是我们把原来放数组/对象那个位置，换成一个数字，那么v-for就会从1开始，一直遍历到你指定的那个数字：

`<div v-for="number in 10">{{number}}</div>`

这段代码就是从1开始，一直遍历到10结束。

同样的，我们想要获取索引，和之前的方法一样：`<div v-for="(number,index) in 10">{{index}}--{{number}}</div>`

v-for示例代码在这里：[这里](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/05_%E5%88%97%E8%A1%A8%E6%B8%B2%E6%9F%93/01_v-for%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.html)

### v-for里面的template元素

在渲染过程中，我们需要一个父级元素来遍历，然后用子元素来展示内容分，这个时候我们就会把那个父级元素用template来替换：

```html
<template v-for="(value,key) in message">
    <div>{{key}}:</div>
    <div>{{value}}</div>
    <hr>
</template>
```

示例代码：[这里](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/05_%E5%88%97%E8%A1%A8%E6%B8%B2%E6%9F%93/02_v-for%E4%B8%8Etamplate.html)

### v-for一些小补充

1. 数组更新检测

在vue中，vue将数组一些方法进行了包裹，也就是说我们在vue中使用了这些方法，vue的视图会自动更新数组，然后重新渲染到页面上。这些被包裹的方法包括：

- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()

上面的方法会直接修改原来的数组，但是某些方法不会替换员阿里的数组，而是会生成新的数组，比如：filter()、concat()、slice()

2. v-for中key的作用

在使用v-for进行列表渲染的时候，我们通常会给元素或者组件绑定一个key属性

首先我们来看什么是VNode：

- 目前我们还没有学习到组件，所以我们现在可以把VNode理解为HTML元素创建出来的VNode。
- VNode的全称是 Virtual Node，也就是虚拟节点，真实节点就是我们html文档里面DOM
- 事实上，无论是组件还是元素，他们在vue中表现出来的都是一个一个的VNode。
- VNode的本质就是一个JavaScript的对象，但是这个对象可以用来描述这个DOM
- vue中template会先转换成VNode然后在渲染成真实DOM，最后展示到页面上。

再来看看什么是虚拟DOM

- 如果我们vue模板里面不止一个简单的div，还有一些其他的标签，这样一大堆元素，就会形成一个VNode Tree

这里我们来看一个数组插入元素的案例：

我们想在数组中间插入一个元素：

原数组是这样的:`arr: ['a', 'b', 'c', 'd']`，然后我们给数组中间插入一个`f`，我们定义一个方法：

```js
 insert: function () {
    this.arr.splice(2, 0, 'f');
}
```

vue是如何去更新这个视图的呢？

我们可以猜测一下，是把整个数组重新渲染吗？这样难免太消耗性能了，其实vue使用的是diff算法，就是将旧的VNodes与新的VNodes进行对比，将不同的地方改掉，而不需要将整个数组重新渲染。

在vue中会根据有没有key调用两个不同的方法：

- 有key，那么就使用patchKeyedChildren方法
- 没有key,那么就使用patchUnKeyedChildren方法