---
title: require函数
data: 2023-3-5
tags: [前端]
categories: [node]
---

# require函数

在node.js中，我们经常会导入一些模块，这其中包括我们自己所写的一些模块，也包含一些node的核心模块，比如fs、http等等。

这个时候我们就需要用到require函数了。

既然这是一个函数，我们在使用的时候，就需要在函数名后面加上一对小括号，然后在小括号里面写上我们需要导入的模块的路径。

## 导入node核心模块

在node中，我们可以直接导入node的核心模块，比如fs、http等等。

`const fs = require("fs");`