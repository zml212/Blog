---
title: Vue3的Options-API
data: 2023-1-26
tags: [前端]
categories: [Vue3]
---

# Vue3的Options-API


[TOC]

## 计算属性

在模板当中，我们可以使用插值语`{{}}`法来显示data里面的一些数据

但是在某些情况下，我么可能需要将数据进行一些转化之后再显示，或者将多个数据结合起来进行显示

- 比如我们需要将多个data数据进行运算、三元运算符来决定结果、数据进行某种转化之后显示
- 在模板中使用表达式，可以非常方便的实现，但是设计它们的初衷是用于简单的运算，在模板中放入太多的逻辑毁容模板显得臃肿不堪且难以维护，并且如果要在多个地方都使用，这样会有大量重复的代码。

解决办法：

1. 将逻辑抽离到methods当中，放到methods里面的options里面，但是这样有一个弊端，所有的data数据的使用过程都变成了方法的调用
2. 可以使用计算属性computed。

### 什么是计算属性

官方：对于任何包含响应式数据的复杂逻辑，你都应该使用计算属性，计算属性将被混入到组件实例中，所有的getter和setter的this上下文自动绑定为组件实例。

**计算属性的用法：**

选项： compted
类型： { [key: string]: Function | { get: Function, set: Function } }

例子：

```js
computed: {
    getName: function () {
        return this.firstName + this.lastName;
    },
    getResult: function () {
        return this.score >= 60 ? '及格' : '不及格';
    },
    getMsg: function () {
        return this.message.split("").reverse().join('');
    }
}
```

这就是计算属性，这里我们使用的是函数的方式，注意：计算属性看起来是一个函数，但是我们在使用的时候并不需要加上`()`,而是直接使用属性的方式来使用。并且计算属性有缓存，这些函数只会执行一次，除非依赖的响应式数据发生了变化。

计算属性与模板语法，计算属性与methods的区别，代码在这里：1.[模板语法](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/06_%E8%AE%A1%E7%AE%97%E5%B1%9E%E6%80%A7/01_%E4%B8%89%E4%B8%AA%E6%A1%88%E4%BE%8B%E7%9A%84%E5%AE%9E%E7%8E%B0_%E6%8F%92%E5%80%BC%E8%AF%AD%E6%B3%95.html)    2.[methods](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/06_%E8%AE%A1%E7%AE%97%E5%B1%9E%E6%80%A7/02_%E4%B8%89%E4%B8%AA%E6%A1%88%E4%BE%8B%E7%9A%84%E5%AE%9E%E7%8E%B0_methods.html)   3.[计算属性](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/06_%E8%AE%A1%E7%AE%97%E5%B1%9E%E6%80%A7/03_%E4%B8%89%E4%B8%AA%E6%A1%88%E4%BE%8B%E7%9A%84%E5%AE%9E%E7%8E%B0_computed.html)

当计算属性所依赖的数据发生改变的时候，计算属性会重新计算。

### 计算属性的getter与setter

计算属性大多数情况下，只需要一个getter方法即可，所以我们会将计算属性直接写成一个函数。也就是当我们把计算属性里面写成函数的形式，就代表我们使用的计算属性的getter方法。

## 侦听器watch

在开发当中，我们data中返回的对象当中定义了数据，这个数据通过插值语法等方式绑定到template当中，当数据发生变化的时候，template会自动更新来显示最新的数据，但是在某些情况下，我们下网在代码逻辑中监听某个数据的变化，这个时候就需要用到侦听器watch来实现了。

watch也是一个配置项，所以我们写的时候与methods一样，写在vue实例当中。

```js
watch: {
    msg: function (newValue, oldValue) {
        console.log('msg发生了变化');
        console.log('newValue', newValue);
        console.log('oldValue', oldValue);
    }
}
```

我们要监听哪个数据发生了变化，前面就写谁，后面就是一个函数，这个函数有两个参数，第一个参数是新值，第二个参数是旧值。

**注意：**

默认情况下我们侦听器只会针对侦听的数据的本身的改变（内部发生的改变时不会被侦听的）。就比如对象内部的一个属性发生了变化，默认情况下是侦听不到的。

想要侦听到对象内部的属性发生了变化，我们需要在配置项中加上deep:true。深度侦听。

```js
info: {
    handler: function (newValue, oldValue) {
        console.log('info发生了变化', newValue, oldValue);
    },
    deep: true, // 深度侦听，可以监听到对象内部的键值对发生改变
}
```

这样就开启了深度侦听，就算对象里面一个属性发生了变化，也会被侦听到。

下面还有一个立即执行的配置项：也就是页面一加载的时候，就会执行一次这个侦听。

```js
info: {
    handler: function (newValue, oldValue) {
        console.log('info发生了变化', newValue, oldValue);
    },
    deep: true, // 深度侦听，可以监听到对象内部的键值对发生改变
    immediate: true, // 立即执行
}
```

