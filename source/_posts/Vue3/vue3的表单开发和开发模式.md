---
title: Vue3的表单开发和开发模式
data: 2023-1-27
tags: [前端]
categories: [Vue3]
---

# Vue3的表单开发和开发模式

[TOC]

## 1.表单开发

### v-model的使用

v-model指令可以在表单input textarea select组件上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。

在官方的说法当中，v-model其实背后有两个操作：

- v-bind绑定value属性的值
- v-on绑定input事件监听到函数中，函数会获取到最新的值赋值到绑定的属性当中。

v-model语法：

`v-model="要绑定的属性"`

比如：

```html
<input type="text" v-model="msg">
<div>{{msg}}</div>
```

在这里我们将`msg`的属性双向绑定，也就是说我们在input输入框中输入的值会实时的显示在div标签中。

### v-model使用场景

1. 可以在文本框内使用v-model来实现双向数据绑定

```html
<textarea name="text" id="test" cols="30" rows="10" v-model="info"></textarea>
```

在下面的data中，我们定义了一个info变量，用来存储文本框中的值。

```js
info: 'hello world!',
```

此时我们默认在文本框中显示的是hello world!，我们可以在文本框中输入任意内容，此时info的值也会随之改变。

2. 可以在单选框中使用v-model来实现双向数据绑定

```html
<input type="radio" v-model="isAgree" name="1">同意协议
<input type="radio" v-model="isAgree" name="1">不同意协议
```

在下面的data中，我们定义了一个isAgree变量，用来存储单选框中的值。

```js
isAgree: false,
```

3. 可以在复选框中使用v-model来实现双向数据绑定

```html
<label for="a">
    <input type="checkbox" v-model="hobbies" value="篮球">篮球
</label>
<label for="b">
    <input type="checkbox" v-model="hobbies" value="足球">足球
</label>
<label for="c">
    <input type="checkbox" v-model="hobbies" value="羽毛球">羽毛球
</label>
```

在下面的data中，我们定义了一个hobbies数组，用来存储复选框中的值。

```js
hobbies: [],
```

4. 可以在下拉框中使用v-model来实现双向数据绑定

```html
<select name="shuiguo" id="sg" v-model="shuiguo">
    <option value="juzi">橘子</option>
    <option value="taozi">水蜜桃</option>
    <option value="xiangjiao">香蕉</option>
</select>
```

在下面的data中，我们定义了一个shuiguo变量，用来存储下拉框中的值。

```js
shuiguo:'',
```

### v-model的值绑定

在上面的例子中，我们都是对数据进行写死处理，但是在实际开发中我们需要渲染的数据，往往是从服务器返回回来的数据。

比如我们有一个下拉框，需要从服务器获取数据，然后渲染到下拉框中。

```html
<select name="test" id="test">
    <option v-for="item in list" value="item.value">{{item.name}}</option>
</select>
```

### v-model的修饰符

v-model有一些修饰符，可以对v-model的值进行修饰。

1. lazy

在前面的例子中，我们发现，只要输入框的数据一发生变化，这边就会侦听到，然后发生改变。但是我们不希望改变得这么频繁，这个时候我们就需要用到lazy这个修饰符。

就像这样：

```html
<input type="text" v-model.lazy="text">
<div>result:{{text}}</div>
```

这时候我们只有在提交（回车/输入框失去焦点时）的时候，数据才会发生改变。

`lazy`的本质其实是将原来v-model用于双向绑定的事件从`input`改为`change`。

2. number

为什么需要这个修饰符呢，这是因为在输入框中，我们输入的是数字，但是v-model进行绑定的时候，他默认是字符串类型。

这个时候我们就需要用到number这个修饰符。

```html
<input type="text" v-model.number="number">
```

在这种情况下，我们在输入框输入的除数字之外的字符，一律不会被绑定在number上。

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cd0b8ba7b01e4e3b840d67607c480b71~tplv-k3u1fbpfcp-watermark.image?)