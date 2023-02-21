---
title: Vue3组件化开发（一）
data: [2023-2-20]
tags: [前端]
categories: [Vue3]
---

# Vue3组件化开发（一）

## 为什么需要组件化开发

在前端工程化中，流行的框架都是使用的组件化开发模式，这是因为在前端开发逻辑中，有许多重复的代码，我们将这些重复的代码封装进一个组件里面，提高代码的复用性。同时，我们也可以将一些公共的代码抽离出来，这样我们在开发的时候，只需要引入这个组件就可以了，这样就可以减少我们的代码量，同时也可以使得我们的代码更加的简洁。

## Vue3中的组件化开发

### 组件的定义

在vue中，我们页面有很多板块，比如一个企业的官网，里面有导航栏，轮播图，新闻列表，底部等等，我们可以将这些板块封装成一个个的组件，然后在页面中引入这些组件，这样就可以实现页面的复用。

这是Vue官网的一幅图：

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fdc39bc668b74022a6dde247f7d3c306~tplv-k3u1fbpfcp-watermark.image?)

可以看到在一个页面中。可以将页面分成很多板块，比如头部、主体部分、侧边栏、或者还可以有底部。在主题部分又可以有子组件等等。

在Vue中，我们可以将这些板块封装成一个个的组件，然后在页面中引入这些组件，这样就可以实现页面的复用。

### 组件的注册

在Vue中注册有两种方式：

- 全局注册
- 局部注册

首先我们来全局注册：

首先准备一个要挂载的模板：

```html
<template id="template">
<!-- <div>{{msg}}</div> -->
    <my-component></my-component>
    <compont-b></compont-b>
</template>
```

可以看到里面有两个组件，一个是my-component，一个是compont-b。

接下来就是这两个组件的代码：

```html
<template id="compont-b">
    <h1>标题</h1>
    <h2>{{msg}}</h2>
</template>
```

```html
<template id="component">
    <h2>我是componted组件</h2>
</template>
```

下面我们来注册这两个组件：

第一步：创建一个Vue应用

```js
const app = Vue.createApp({
    template: '#template',
})
```

这时候app就是一个Vue的实例，我们可以把这个实例挂载到html中的某个元素上。比如：`<div id="app"></div>`。

第二步：注册组件

```js
App.component('my-component',{
    template: '#component',
});
App.component('compont-b',{
    template: '#compont-b',
    data(){
        return {
            msg: '我是compont-b组件'
        }
    }
});
```

注册组件的时候，我们使用的是`component`，这个方法有两个参数，第一个参数是组件的名称，第二个参数是组件的配置。组件名称也就是我们等会在页面中使用的组件名称。

在组件的配置里面我们还可以写data,methods等等，这些都是我们平时写的Vue的配置。

第三步：挂载

```js
App.mount('#app');
```

这段代码表示我们将名为app的Vue实例挂载到id为app的元素上。


小知识：

- 定义组件名称组件有两种：
    - 驼峰命名法：`MyComponent`(仅在脚手架中生效)
    - 中划线命名法：`my-component`

接下来就是局部注册组件：

全局组件有一个缺点，就是在页面一加载的时候，就会将所有的全局组件进行加载，但是有时候我们并不需要将所有组件都加载出来，这样就会造成性能浪费。

并且使用打包工具进行打包的时候，如果该全局组件没有用到，依然会进行打包。导致文件多余。

所以就出现了局部组件来解决这个问题。

在前面进行全局注册的时候，我们使用的是`App.component`，这个方法是全局注册组件的方法，那么局部注册组件的方法是什么呢？

`components`来定义一个组件，这个方法有一个参数，就是组件的配置。

之前我们vue实例里面有`data`，`methods`等等属性，compoments选项是一个对象，里面的键值对是：`组件名称:组件对象`。

就像这样：

```vue
    const app = {
        template: "#template",
        // 注册局部组件
        components:{
            // key:value
            // key:组件名称
            // value组件对象
            "component-a":{
                template:"#component-a",
                data: function () {
                    return {
                        msg:"局部",
                    }
                }
            }
        }
    };
    const App = Vue.createApp(app);
```

我们在这里注册了一个局部组件，与之前全局注册同的是：全局注册使用的是`App.component`，而局部注册使用的是`components`。也就是在根组件里面使用components选项来注册组件。在里面我们以键值对的形式来注册组件。