# vue绑定样式

总所周知，vue是一个响应式的框架，当数据发生改变的时候，页面会自动响应这个变化。那么当元素的样式发生改变的时候，也应该动态的发生改变，要实现这样一种效果，我们可以使用vue的绑定样式的方式来实现。

在vue中，我们可以使用v-bind来绑定元素的属性，那么这里我们就需要使用到v-bind去绑定元素的class属性，来实现元素的样式绑定。

具体如何操作我们接着往下看：

## 1. 绑定class

我们使用v-bind绑定class属性，来实现元素的样式绑定。

```html
<div id="app">
    <div v-bind:class="classObject">我是一个div</div>
</div>
```

```javascript
var app = new Vue({
    el:'#app',
    data:{
        classObject:{
            'class1':true,
            'class2':false
        }
    }
})
```

```css
.class1{
    color:red;
}
.class2{
    color:blue;
}
```

代码解释：  
在上面我们分别将html、css、JavaScript分开写，我们可以看到，我们在html中使用v-bind绑定了class属性，然后在JavaScript中定义了一个classObject对象，这个对象中有两个属性，分别是class1和class2，这两个属性的值分别是true和false，那么我们可以看到，当class1的值为true的时候，div的颜色为红色，当class2的值为true的时候，div的颜色为蓝色，当两个值都为true的时候，div的颜色为蓝色，当两个值都为false的时候，div的颜色为黑色。

## 2.绑定内联样式

前面我们演示了如何绑定class，那么我们也可以使用v-bind绑定style属性，来实现元素的样式绑定。

```html
<div id="app">
    <div v-bind:style="backGroundColor,fontSize">我是一个div</div>
</div>
```

```js
    var vm = new Vue({
        el:'#app',
        data(){
            return {
                backGroundColor:'background-color:red',
                fontSize:'font-size:20px',
            }
        }
    })
```

在上面的代码中，我们使用vue绑定了一个style样式，在代码里面我们可以看到，内联样式我们属性名使用的是驼峰命名，而不是外部css样式采用`-`连接的方式。

## 3. 扩展


在实际的开发中，我们经常会遇到统一个元素需要绑定多个样式的情况，vue中同样可以做到。

### 数组语法（绑定多个样式）

使用数组语法，就是将多个样式传进一个数组里面，最后在模板里面使用`v-bind:style`绑定这个数组。

```html
<div id="app">
    <div v-bind:class="[bgc,color]">我是一个div</div>
</div>
```

```js
    var vm = new Vue({
        el:"#app",
        data(){
            return {
                bgc:'background-color':'red',
                color:'color':'blue',
            }
        }
    })
```
