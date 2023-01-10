---
title: vue监视属性
date: 2022-10-11
tags: [前端]
categories: [Vue]
---
# vue监视属性

在开发中，我们会遇到这样一种情况，我们需要一个属性变化的时候，然后做出一些操作。而检测这个变化的属性，在vue中叫做监视属性。

## 1.监视属性是什么

在vue中，我们可以通过watch属性来监视某个属性的变化，当这个属性发生变化时，我们可以执行一些操作。

- 当监视属性所监视的属性发生变化的时候，回调函数（handler）就会自动调用，并且执行相关的操作
- 监视属性所监视的属性要存在，才能产生作用。

我们这里用一个天气案例来解释什么是监视属性：

首先html代码：

```html
    <div id="app">
        <p>今天天气很{{info}}</p>
        <button v-on:click="change">切换天气</button>
    </div>
```

接下来我们书写js代码：

```js
    var vm = new Vue({
        el: "#app",
        data: {
            isHot: true,
        },
        computed: {
            info: function () {
                return this.isHot ? "热" : "冷";
            }
        },
        methods: {
            change: function () {
                this.isHot = !this.isHot;
            }
        },
        watch: {
            isHot: {
                handler:function (newVal, oldVal) {
                    console.log("isHot属性发生了变化");
                },
            }
        }
    });
```

在代码里面的`handler`这个函数就是我们前面说的回调函数，当`isHot`属性发生变化的时候，这个函数就会自动调用。

当然我们可以在`idHot`对象面添加一个属性：`immediate`，当此属性布尔值为真的时候，`handler`回调函数在初始化的时候就会调用一次。

```js
    watch: {
        isHot: {
            handler:function (newVal, oldVal) {
                console.log("isHot属性发生了变化");
            },
            immediate: true
        }
    }
```

## 2.监视属性的写法

监视属性有两种写法：

- 在vue实例化对象中直接书写：  
  `new Vue({watch:{}})`,然后传入相关配置
- 通过vue实例化对象.$watch('属性名'，回调函数)来书写

这里的第一种写法上面我们已经展现过了，下面我们就展示一下第二种写法：

这里我们假设vue的实例化对象为vm。

```js
    vm.$watch('isHot',function (newVal, oldVal) {
        console.log("isHot属性发生了变化");
    });
```

## 3.监视属性之深度监视

前面我们实现的监视，只能监视vue实例data中直接的简单数据，要是遇到对象或者数组，就无法监视了。

这样做的方法是vue为了提高效率，在vue监视属性中，默认只监视一层，如果要监视多层，就需要我们手动开启深度监视。

```js
    watch: {
        isHot: {
            handler:function (newVal, oldVal) {
                console.log("isHot属性发生了变化");
            },
            immediate: true,
            deep: true
        }
    }
```

其中`deep:true`就开启了深度监视。
深度监视就是监视vue中data中的对象或者数组，当对象或者数组中的属性发生变化的时候，监视属性的回调函数就会自动调用。

在vue中其实是可以检测对象内部值的变化，那为什么vue监视属性不默认开启深度监视呢？

因为vue监视属性的回调函数是在数据发生变化的时候才会调用，如果开启深度监视，那么vue就要监视对象内部的所有属性，这样会大大降低vue的效率。

在我们使用监视属性的时候，我们根据具体的业务需求，来判断要不要开启深度监视。