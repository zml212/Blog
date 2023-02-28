---
title: Vue中的插槽
data: [2023-2-28]
tags: [前端]
categories: [Vue]
---

# Vue中的插槽

在Vue的开发中，我们对于某一个组件，其中某个位置所摆放的元素是什么，是充满不确定性的。比如在手机购物软件中，我们顶部的导航栏，可以分为三个部分，比如左侧部分，右侧部分。

但是我们不知道这个左侧部分和右侧部分的内容是什么，这个时候我们就可以使用插槽来解决这个问题。

## 插槽的基本使用

在Vue中，我们定义插槽使用的是`slot`。

```html
<template>
  <div>
    <h2>我是组件开始</h2>
    <slot></slot>
    <h2>我是组件结束</h2>
  </div>
</template>
```

这就是一个很简单的插槽组件，我们暂时命名这个组件为`MySlot`。在定义好这个组件之后，我们需要将这个组件注册到Vue中。

就像这样：

```js
import MySlot from "./MySlot.vue";

export default {
  name: "app",
  components: {
    MySlot,
  },
};
```

我们这里将插槽组件进行局部注册之后，我们就可以在`app`组件里面使用这个插槽组件了。

```html
<template>
  <my-slot>
    <button>按钮</button>
  </my-slot>
  <my-slot>
    <h5>我是文本</h5>
    <button>按钮2</button>
  </my-slot>
</template>
```

在使用的时候，我们将注册的组件写到`app`的模板里面，然后我们这个组件里面写上自己需要放进去的内容，内容可以写按钮，文本框等等一些元素。包括其他一些组件。

以上就是插槽的一些基本使用方法。

## 具名插槽的使用

在前面的示例中，我们插槽组件只有一个插槽，如果某个插槽组件包含多个插槽，那我们使用的时候就需要使用具名插槽。

首先具名插槽，需要在插槽组件中进行定义（给每个插槽加上属于他们自己的名字），就像这样：

```html
// name : NavBar
<template>
  <div class="nav-bar">
    <div class="left">
      <slot name="left"></slot>
    </div>
    <div class="center">
      <slot name="center"></slot>
    </div>
    <div class="right">
      <slot name="right"></slot>
    </div>
  </div>
</template>
```

上面我们定义了三个插槽,我们分别命名为`left`，`center`，`right`。

然后将这个组件在app组件里面注册，我们就开始使用这个插槽：

```html
<template>
  <nav-bar>
    <template v-slot:left>
      <button>左边按钮</button>
    </template>
    <template v-slot:center>
      <div>中间文字</div>
    </template>
    <template v-slot:right>
      <li>列表</li>
    </template>
  </nav-bar>
</template>
```

在使用具名插槽的时候，我们需要将插槽的名字写在`v-slot`里面，然后我们需要将每个插槽的东西