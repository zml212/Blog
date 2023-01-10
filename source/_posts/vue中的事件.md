---
title: vue中的事件
date: 2022-10-9
tags: [前端]
categories: [Vue]
---
# vue中的事件

## 1. 事件绑定
在vue中，事件的绑定使用vue指令来完成，我们需要使用v-on指令来完成事件的绑定，v-on指令可以简写为@，如下：

```html
    <div id="app">
    <button v-on:click="handleClick">按钮</button>
```

如上面代码所示：

vue绑定事件的写法为 `v-on:事件名 = "事件处理函数"`,当然我们也可以使用简写形式：`@事件名 = "事件处理函数"`

前面是事件在html中的写法，后面是事件在vue中的写法。

在vue实例化对象中，我们需要将事件处理函数写在`methods`对象中，最终这个方法会出现在vue实例化的对象中，如下：

```javascript
    var vm = new Vue({
        el: '#app',
        data: {
            msg: 'hello world'
        },
        methods: {
            handleClick: function () {
                alert('hello world')
            }
        }
    })
```

那为什么我们不直接将方法写在`data`中，这样不是更方便吗？其实这样写是不行的，因为vue会将`data`中的数据进行数据劫持，当数据发生变化时，会通知视图进行更新，但是方法是不会进行数据劫持的，所以当方法发生变化时，视图不会进行更新。

也就是说，如果我们将事件处理函数写在`data`中，那么这些函数vue会给我们做一个数据代理，这样极大地浪费了资源，导致页面运行缓慢。

**注意：**

在前面我们提到了两种遭html中绑定事件的写法，那么这两种写法有什么区别呢？

- `v-on:click = "methods"`这个写法我们不能再传递参数，但是`@click = "methods()"`可以传递参数。

## 2. 事件修饰符

在vue中常见的事件修饰符有：

- prevent: 阻止默认事件(如：阻止a标签的跳转)
- stop:阻止事件的冒泡
- once:只执行一次事件处理函数

```html
    <div id="app">
    <button v-on:click="handleClick">按钮</button>
    <a href="https://juejin.cn" v-on:click.prevent="handleClick">百度</a>
    <button v-on:click = "handleClick1">
            <button v-on:click.stop="handleClick">按钮</button>
    </button>
    <button v-on:click.once="handleClick">按钮</button>
```

在上面的代码中，我们可以看到，我们使用了三种事件修饰符，分别是`prevent`、`stop`、`once`，这三种事件修饰符的作用分别是：

- prevent:阻止了a标签的跳转页面
- stop：阻止了按钮的冒泡（因为在vue中，事件默认是冒泡）
- once:设定了这个按钮只能点击一次，点击一次之后再点击就没有反应了

这三种修饰符是比较常见的，还有几种我们不是经常用到的，这里我们提几句：

- self:只有当事件是从该元素本身触发时才会触发
- capture:使用事件捕获模式(改变事件的触发顺序)
- passive:被动事件监听器(提高性能：事件的默认行为立刻执行，无需等到vue的事件处理函数执行完毕)

如果在同一个事件上需要多种修饰符，那么我们可以这样写：

```html
    <button v-on:click.stop.prevent="handleClick">按钮</button>
```

这样就可以在点击按钮的时候阻止事件的冒泡，又可以阻止默认事件。

## 3.键盘事件

在vue中，键盘事件有三种：

- keydown:按下键盘时触发
- keypress:键盘按住时触发
- keyup:键盘抬起时触发

```html
    <div id="app">
    <input type="text" v-on:keydown="handleKeydown">
    <input type="text" v-on:keypress="handleKeypress">
    <input type="text" v-on:keyup="handleKeyup">
```

在键盘事件中，同样也有着事件修饰符，下面我会列举一些常见的键盘修饰符：
| 键盘修饰符 |  解释   |        备注         |
| :--------: | :-----: | :-----------------: |
|   enter    | 回车键  |                     |
|    tab     |  tab键  | 必须配合keydown使用 |
|   delete   | 删除键  |  delete/backspace   |
|    esc     |  esc键  |                     |
|   space    | 空格键  |                     |
|     up     | 上箭头  |                     |
|    down    | 下箭头  |                     |
|    left    | 左箭头  |                     |
|   right    | 右箭头  |                     |
|    ctrl    | ctrl键  |                     |
|    alt     |  alt键  |                     |
|   shift    | shift键 |                     |
|    meta    | meta键  |                     |

上面这些就是vue根据我们常用的键位设置的属性，我们可以直接在事件绑定的时候使用。

就像这样`@keyup.enter = "methods"`，这样就可以监听到回车键的事件了。

但是需要注意的是：

- 修饰符可以串联，如：`@keyup.enter.alt = "methods"`
- 系统修饰键比较特殊：
    (1).配合keyup使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才可以触发
    (2).配合keydown使用：正常触发事件

虽然vue给我们准备了很多常见的键位，但是在实际开发中，难免会有一些特殊的键位，需要我们自定义：  
不要担心，vue提供了一个方法，可以让我们自定义键位：

```html
    <div id="app">
    <input type="text" v-on:keyup.自定义按键名="handleKeyup">
```

```js
    Vue.congig.keyCodes.自定义按键名 = 按键值;
```
