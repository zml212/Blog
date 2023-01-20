---
title: Vue3当中的一些基本指令
date: 2023-1-19
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

### v-bind绑定class属性

在实际开发当中，可能某个元素的class是动态的，处于某个状态的时候，字体颜色为黑色；处于另一个状态的时候，字体颜色为红色。

这个时候就需要使用v-bind来绑定class，绑定class有两种方式：

- 对象语法
- 数组语法

对象语法：`{'active':boolean}`

可以看到在对象语法当中前面用引号括起来的就是可以绑定的类名，后面跟上了一个布尔值，当这个布尔值为真的时候，该类名就会被绑定到该元素；如果布尔值为假，那么这个类名就不会被绑定到这个元素上。

既然是一个对象，那么里面也可以是多个键值对，就像这样：`<div v-bind:class="{'fontColor':isTrue,'title':isTrue}">哈哈哈哈哈哈哈</div>`。
示例代码地址：[这里](https://github.com/zml212/vue3_learn/blob/master/learn_vue3/03_v-bind%E5%92%8Cv-on%E7%9A%84%E4%BD%BF%E7%94%A8/02_v-bind%E7%BB%91%E5%AE%9Aclass-%E5%AF%B9%E8%B1%A1%E8%AF%AD%E6%B3%95.html)