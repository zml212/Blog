---
title: CSS中的盒子模型
date: [2023-4-4]
tags: [前端]
categories: [CSS]
---

# CSS中的盒子模型

在网页中，可以把每一个元素都看做一个盒子。

每个盒子包含内容、内边距、边框、外边距这四个部分。

## 边框（border）

边框的方向有四个：top、right、bottom、left。

边框属性：

- 边框的厚度：`border-width` 
	- value: 与之前设置长度的方式一样
- 边框的样式：`border-style`
	- none(无) dotted（虚线） solid（实线） double（双实线）
- 边框的颜色：`border-color`
	- 与其他设置颜色的方式一样

简写方式：

- border-width: value1 value2 value3 value4;
	- 此时的作用顺序为:上右下左。
- border-width: value1 value2;
	- 此时的作用顺序为：value1 上下、value2 左右。
- border-width: value1 value2 value3;
	- 此时的作用顺序为： value1  上部、value2  左右 、value3 下部。 

border的综合写法：

```CSS
border: width style color;
```
