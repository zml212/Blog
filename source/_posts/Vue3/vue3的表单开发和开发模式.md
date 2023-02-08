---
title: Vue3的表单开发和开发模式
data: 2023-1-27
tags: [前端]
categories: [Vue3]
---

# Vue3的表单开发和开发模式

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