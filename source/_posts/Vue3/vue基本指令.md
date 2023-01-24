---
title: Vue3当中的一些基本指令
date: 2023-1-23
tags: [前端]
categories: [Vue3]
---
# Vue3当中的一些基本语法
[TOC]

## 插值语法

在vue的模板里面，我们可以使用花括号来实现一个插值语法：

`<div>{{count}}</div>`

在插值语法里面可以使用vue实例对象data里面的数据，并且可以写一些基本的表达式，不是语句！

里面可以只是一个变量，也可以对变量进行一些运算（加减乘除等等），也可以使用函数，还可以使用三元表达式。

但是不能使用语句。

## v-once指令

- v-once指令用于指定元素或者组件只渲染一次：
	- 当属局发生变化的时候，元素或者组件以及其所有的子元素，都会被视为静态内容并且跳过。
	- 该指令可以用于性能优化
	- 如果是里面的子节点也只会被渲染一次：
```html
	<div v-once>
		<h2>{{counter}}</h2>
	</div>
```

## v-text指令

用于更新元素的textContent:

```html
<div v-text = "msg"></div>
// 等价于
<div>{{msg}}</div>
```

## v-html指令

默认情况下，如果我们展示的内容本身是html的，那么vue并不会对其进行特殊的解析。

如果我们希望这个html内容可以被vue解析出来，那么我们可以使用v-html来展示。

用法：

```html
	 <div>{{msg}}</div>
     <div v-html="msg"></div>
```

## v-pre指令

v-pre用于跳过元素和他的子元素的编译过程，显示原始的Mustache标签。

跳过不需要编译的节点，加快编译的速度。

`<div v-pre>{{message}}</div>`这样的运行效果就是只会出现message。

## v-cloak指令

这个指令保持在元素上直到关联组件实例结束编译

和css规则如[v-cloak]{display:none}一起使用时，这个指令可以隐藏未编译的Mustache标签知道组件实例准备完毕。

## v-bind的绑定属性 

在实际开发当中,除了会动态绑定一些内容之外，我们可能还会需要动态绑定一些属性。这个时候就需要用到v-bind这个属性。

- 我们可以动态绑定某个标签的class属性，或者a标签的href属性。 

绑定属性我们使用v-bind：

- 缩写：：
- 预期： any(with argument)|Object(without argument)
- 参数： attr Or Prop(optional)
- 修饰符：camel --> 将kebab-case attribute 名转换为camelCase
- 用法: 动态地绑定一个或者多个attribute，或者一个组件prop到表达式

例子：

`<a v-bind:href="url">{{msg}}</a>`

```js
data: function () {
        return {
            msg: '我的博客',
            url: "https://www.zmlblog.top"
        }
    },
```

这样我们就把url与超链接的地址给绑定起来了。

上面是v-bind 的基础使用，也就是原始的写法，但是这样写起来有点麻烦，vue给我们提供了一个语法糖：

`<a :href="url">{{msg}}</a>`

也就是在v-bind可以直接简写为一个`:`。

实例在这里：[这里]()

### v-bind绑定class属性

在实际开发当中，可能某个元素的class是动态的，处于某个状态的时候，字体颜色为黑色；处于另一个状态的时候，字体颜色为红色。

这个时候就需要使用v-bind来绑定class，绑定class有两种方式：

- 对象语法
- 数组语法

**对象语法：`{'active':boolean}`**

可以看到在对象语法当中前面用引号括起来的就是可以绑定的类名，后面跟上了一个布尔值，当这个布尔值为真的时候，该类名就会被绑定到该元素；如果布尔值为假，那么这个类名就不会被绑定到这个元素上。

既然是一个对象，那么里面也可以是多个键值对，就像这样：`<div v-bind:class="{'fontColor':isTrue,'title':isTrue}">哈哈哈哈哈哈哈</div>`。
示例代码地址：[这里](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/03_v-bind%E5%92%8Cv-on%E7%9A%84%E4%BD%BF%E7%94%A8/02_v-bind%E7%BB%91%E5%AE%9Aclass-%E5%AF%B9%E8%B1%A1%E8%AF%AD%E6%B3%95.html)

在对象语法里面`{'active':boolean}`,active是用引号括起来的，当然我们也可以不使用引号，这样的话这个类名就会去data里面寻找，看在data里面有没有active这个变量，然后找到里面存放的类名。

我们使用v-bind绑定class还可以与静态的class进行结合：

```js
<div class="abc" :class="{'fontColor':isTrue,'title':isTrue}">
    123123
</div>
```

最后渲染出来的结果就是类名有：abc fontColor  title

**数组语法：[className,className,三元表达式.....]**

`<div :class="['fontColor','title']">hhhhhhh</div>`

这样绑定的就是两个类名：fontColor  title

当然我们也可以去data里面寻找我们需要的类名：

`<div :class="['fontColor',title]">hhhhhhh</div>`

这段代码其中第二个值中代表的类名就需要在data里面寻找

我们还可以使用三元运算符：

```js
<div :class="['fontColor',isTrue?'title':'']">
    hhhhhhh
</div>

```

在这里我们就会去判断isTrue是否为真，如果为真，那么就把title这个类名绑定到元素上，否则就绑定一个空的类名（也就是不绑定任何类名）。

但是这样写三元表达式，代码不优雅，更优雅的方式是，在数组里面我们写上一个对象，就像这样：

```js
<div :class="['fontColor',{title:isTrue}]">
    hhhhhhh
</div>
```

这样是不是就更加优雅了呢？

示例代码在：[这里](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/03_v-bind%E5%92%8Cv-on%E7%9A%84%E4%BD%BF%E7%94%A8/03_v-bind%E7%BB%91%E5%AE%9Aclass%E6%95%B0%E7%BB%84%E8%AF%AD%E6%B3%95.html)

### v-bind绑定style

CSS Property名可以使用驼峰式(camelCase)或者短横线分隔(kebab-case,记得用引号括起来)的方式来命名。

同样的绑定方式也有两种：
- 对象语法
- 数组语法

**对象语法：**

用法：`:style="{属性名:'属性值'}"`

可以看到在css属性值哪里有一个单引号，这是必须的，不然vue就会将属性值看做一个变量来解析。

命名方式：

- 驼峰： fontSize
- 短横线：font-size,使用短横线这种方式的时候，必须将属性名用引号引起来。

实例代码：[这里](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/03_v-bind%E5%92%8Cv-on%E7%9A%84%E4%BD%BF%E7%94%A8/04_v-bind%E7%BB%91%E5%AE%9Astyle-%E5%AF%B9%E8%B1%A1%E8%AF%AD%E6%B3%95.html)

**数组语法：**

在开发当中，我们想要把多个对象里面的css属性写到一起，我们就可以使用数组的方式，将前面的对象作为数组的元素写到一起：`<div :style="[styleObj,styleObj2]">哈哈哈哈</div>`

注意：

如果使用数组语法的时候，如果数组里面对象的css属性有重复的，那么浏览器会根据最后一个（最右边的）属性来渲染。

示例代码在：[这里](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/03_v-bind%E5%92%8Cv-on%E7%9A%84%E4%BD%BF%E7%94%A8/05_v-bind%E7%BB%91%E5%AE%9Astyle%E6%95%B0%E7%BB%84%E8%AF%AD%E6%B3%95.html)

## 动态绑定属性

在某些情况下，我们属性的名称可能也是不固定的：

- 前端我们无论绑定src,href,class,style属性名都是固定的。如果属性名称不是固定，我们可以使用<font color=red>:[属性值]='值'</font>的格式来定义，这种绑定的方式，我们称之为动态绑定属性。 

就像这样：

`div :[name]="styleObj">hhhhhh</div>`
```js
name: 'style',
styleObj: {
color: 'red',
fontSize: '40px',
    },
```

里面的`name`可以换成其他的变量。

示例代码在:[这里](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/03_v-bind%E5%92%8Cv-on%E7%9A%84%E4%BD%BF%E7%94%A8/06_v-bind%E5%8A%A8%E6%80%81%E7%BB%91%E5%AE%9A%E5%B1%9E%E6%80%A7.html)

还有一个v-bind直接绑定一个对象：示例看[这里](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/03_v-bind%E5%92%8Cv-on%E7%9A%84%E4%BD%BF%E7%94%A8/07_v-bind%E7%9B%B4%E6%8E%A5%E7%BB%91%E5%AE%9A%E4%B8%80%E4%B8%AA%E5%AF%B9%E8%B1%A1.html)

## v-on指令

**v-on绑定事件：**

前面我们学习了这么多的指令，但是都是一些静态的，在前端开发当中，一个重要的特性就是与用户的交互。这个时候我们就需要用到v-on指令。

用来监听胡勇大声的时间，比如点击、拖拽、键盘点击事件等等

**v-on的使用：**
- 简写：@
- 预期：Function|Inline Statement | Object
- 参数： event
- <div id="v-on修饰符">修饰符：</div>
```
 .stop  调用event.stopPropagation()
 .prevent  调用 event.preventDefault()
 .capture  添加事件侦听器时使用capture模式
 .self  只当事件是从侦听器绑定的元素本身触发时才触发回调
 .{keyAlias}  仅当事件是从特定触发时才触发回调
 .once   只触发一次回调
 .left  只当鼠标点击左键时触发
 .right  只当鼠标点击右键时触发
 .middle  只当鼠标点击中键时触发
 .passive  {passive:true} 模式添加侦听器
```
- 绑定事件监听

### v-on基本使用

一个简单的例子：

`<button v-on:click="add">点我加一</button>`

这个时候就是一个完整的v-on写法了，在v-on后面加上一个冒号，然后加上事件的类型（此时我们添加的点击事件），然后在后面加上事件要调用的事件函数。

也有语法糖：

`<button @click="add">点我加一</button>`:也就是将v-on简写为`@`.

如果我们要处理的事件不是很复杂，我们可以直接写一个表达式在等号后面：`<button @click="this.count++">点我加一</button>`

我们还可以绑定一个对象:

在绑定对象的时候，语法是这样的：`v-on={'事件类型':'事件函数',......}`

示例在[这里](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/03_v-bind%E5%92%8Cv-on%E7%9A%84%E4%BD%BF%E7%94%A8/08_v-on%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8.html)

### v-on的参数传递

在函数事件当中，可能会需要传递一些参数，这个时候就需要使用到v-on的参数传递。

在获取事件对象的时候，如果一个事件函数不需要传入其他参数，可以直接不写参数，在事件函数里面可以直接获取到事件对象，就像这样：

```html
    <button @click="btn1Click">按钮1</button>
```
```js
btn1Click: function (event) {
                console.log(event);
        },
```

这样就可以拿到事件对象。如果需要传入多个参数，那么这样就不行了。

就需要像下面这样写：

```html
<button @click="btn2Click($event,'海绵宝宝')">按钮2</button>
```
```js
btn2Click: function (event, name) {
    console.log(event);
    console.log(name);
}
```

可以看到在模板里面我们获取事件对象使用的是`$event`，这是一个固定写法。

示例代码在:[这里](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/03_v-bind%E5%92%8Cv-on%E7%9A%84%E4%BD%BF%E7%94%A8/09_v-on%E7%9A%84%E5%8F%82%E6%95%B0%E4%BC%A0%E9%80%92.html)

### v-on修饰符

v-on支持修饰符，修饰符相当于对事件进行了一些特殊处理：

具体修饰符见[上面](#v-on修饰符)

示例代码在[这里](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/03_v-bind%E5%92%8Cv-on%E7%9A%84%E4%BD%BF%E7%94%A8/10_v-on%E4%BF%AE%E9%A5%B0%E7%AC%A6.html)