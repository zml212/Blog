# Vue中的列表循环

在电商项目的开发中，会用到这样一个问题：商品的陈列我们如果使用原生的js会显得十分臃肿。但是在Vue中，我们可以直接使用列表渲染来实现这样的效果。

## 1. v-for

v-for指令用于渲染一个列表，它接受一个数组或者对象，然后使用一个模板来渲染每个元素。

v-for指令需要使用item in items的语法，其中items是源数据数组，而item则是被迭代的数组元素的别名。

在vue中，使用v-for指令不仅可以渲染数组，还可以渲染对象。当使用v-for渲染对象时，它会遍历对象的属性名。

### 1.1 数组

这里我们使用一个数组来渲染一个列表。

```html
<div id="app">
    <ul>
        <li v-for="item in items">{{item}}</li>
    </ul>
```

在上面我们写了一个ul标签，然后使用v-for指令来渲染一个数组。这里我们使用了item in items的语法，其中items是源数据数组，而item则是被迭代的数组元素的别名。

```js
    var vm = new Vue({
        el: '#app',
        data: {
            items: [
                11,22,33
            ],
        }
    });
```

渲染结果：

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c8da912e898e4234b52b168a52b65dc9~tplv-k3u1fbpfcp-watermark.image?)

当然在使用v-for来遍历数组的时候，还可以添加另外一个属性。这里我们使用index作为数组的索引。

```html
<div id="app">
    <ul>
        <li v-for="(item,index) in items">{{index}}:{{item}}</li>
    </ul>
```

渲染结果：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6d51089fd6c24e95b870a093b27f9416~tplv-k3u1fbpfcp-watermark.image?)

**tips：**

记得两个属性之间要添加逗号。

### 1.2 对象

前面我们看了数组的渲染，那么我们也可以使用v-for来渲染对象。

```html
<div id="app">
    <ul>
        <li v-for="(value,key,index) in object">{{index}}:{{key}}:{{value}}</li>
    </ul>
```

```js
    var vm = new Vue({
        el:'#app',
        data:{
            obj:{
                name:'张三',
                age:18，
            }
        },
    });
```

结果如下：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/01bb9cf61aff4a759ff401c23d1026f3~tplv-k3u1fbpfcp-watermark.image?)

在遍历对象的时候，我们这里填写了三个属性，分别是value,key,index。其中value是对象的值，key是对象的键，index是索引。

通过结果也显而易见。

## 2. v-for的key

在使用v-for来渲染列表的时候，我们需要为每一个节点添加一个唯一的key属性。这样做的好处是可以提高渲染的效率。

众所周知，在vue中有一个虚拟dom，通过虚拟dom可以极大的提高渲染的效率。当我们使用v-for来渲染一个列表的时候，vue会根据key来判断节点是否需要更新。如果key不变，那么vue会复用节点，如果key变了，那么vue会重新渲染节点。

也就是说，如果我们需要给对象或者数组添加/删除元素，vue会先比较哪些元素变了，哪些元素没变。变了的元素，dom才会更新，没有发生改变的，dom就直接拿过来使用。这样就极大的提高了渲染的效率。