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