---
title: 什么是path路径模块
date: 2022-12-17
tags: [前端]
categories: [node]
---
# path路径模块

## 什么是path路径模块

`path路径模块`是`node.js`官方提供的用来处理路径的模块。

比如：

- `path.join()`:将多个路径片段拼接成为一个完整的路径片段
- `path.basename()`:可以从文件的路径字符串里面，将文件的名字解析出来

引入模块：

`const path = require('path');`

## 路径拼接

`path.join()`语法格式：

`path.join([...paths])`: