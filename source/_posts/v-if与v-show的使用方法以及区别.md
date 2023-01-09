# v-if与v-show的使用方法以及区别

在vue里面有两种方式来控制元素的显示与隐藏，分别是v-if和v-show，这两种方式都可以控制元素的显示与隐藏，那么如何进行使用呢？

## v-if

在vue中使用v-if来控制元素的显示与隐藏，v-if是一个指令，当v-if的值为true时，元素显示，当v-if的值为false时，元素隐藏。

使用一个栗子来说明：

```html

  <div id = "app">
    <button @click="show = !show">切换</button>
    <div v-if="show">我是v-if</div>
  </div>
```

```javascript
    var app = new Vue({
        el: '#app',
        data: {
        show: true,
        }
    })
```

当点击按钮时，show的值会发生改变，当show的值为true时，元素显示，当show的值为false时，元素隐藏。

## v-show

在vue中使用v-show来控制元素的显示与隐藏，v-show是一个指令，当v-show的值为true时，元素显示，当v-show的值为false时，元素隐藏。

使用一个栗子来说明：

```html

  <div id = "app">
    <button @click="show = !show">切换</button>
    <div v-show="show">我是v-show</div>
  </div>
```

```javascript
    var app = new Vue({
        el: '#app',
        data: {
        show: true,
        }
    })
```

在这个例子中，我们点击按钮的时候，show的值会发生改变，当show的值为true时，元素显示，当show的值为false时，元素隐藏。

## 两种方式的区别

上面我们看到，两种方式都可以控制元素的显示与隐藏，那么这两种方式有什么区别呢？

### v-if

其实真正的隐藏元素是通过`v-if`来实现的，当v-if的值为false时，元素并不会被页面渲染，当v-if的值为true时，元素才会开始渲染。

当`v-if`的值为false时，元素并不会出现在页面中。
*true:*
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ee372a34472749cabdb49c8c995a4428~tplv-k3u1fbpfcp-watermark.image?)

*false:*

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0b896278430b482ca738e85175626b0b~tplv-k3u1fbpfcp-watermark.image?)

如果页面不需要频繁的控制同一元素的显示与隐藏，那么使用v-if来控制元素的显示与隐藏是最好的选择。  
因为这样可以更好的提高页面的性能。

### v-show

在`v-show`中，元素一开始就会被渲染，就类似于css中的display属性，当v-show的值为false时，元素的display属性为none，当v-show的值为true时，元素的display属性为block。

当`v-show`的值为false时，元素的display属性为none：

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d2951857028044d59df6046bccdeb160~tplv-k3u1fbpfcp-watermark.image?)

当`v-show`的值为true时，元素的display属性为block：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5ac4fa7fe1c4412c9d97556c1d26ffe3~tplv-k3u1fbpfcp-watermark.image?)

也就是当我们使用v-show来控制元素的显示与隐藏时，元素一直都存在与页面当中，当我们页面中需要经常切换元素的显示与隐藏的时候，我们就可以使用v-show来控制元素的显示与隐藏。