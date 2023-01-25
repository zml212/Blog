---
title: vue基础之模板语法2
data: 2023-1-25
tags: [前端]
categories: [Vue3]
---

# Vue基础之模板语法（二）

## 条件渲染

在某些情况下，我们需要根据当前的条件决定某些元素或者组件是否渲染，这个时候我们就需要进行条件判断。

vue提供了下面的几个指令来进行条件判断：

- v-if
- v-elseb
- v-else-if
- v-show

### v-if  v-else  v-else-if

这三个指令都是根据条件来渲染某一块的内容，这些内容只有条件为true的时候，才会渲染出来；这三个指令与JavaScript的条件语句if  else  else if类似。

