---
title: Vue中webpack基础打包
data: [2023-2-21]
tags: [前端]
categroies: [Vue3]
---

# Vue中Webpack基础打包

## 什么是webpack

在前端开发中，我们经常听到工程化这个词语，并且在前端开发中，我们经常会使用一些预编译的框架，比如scss等等，并且还会有导包。这些如果直接放在浏览器中是无法正常运行的。所以我们需要一个工具来帮助我们将这些文件进行打包，这个工具就是webpack。

打包工具不仅仅可以在这些文件转换为浏览器能运行的文件，还可以对这些文件进行压缩，合并等操作。

## Webpack在Vue中需要打包一些什么文件

- 对javascript的打包
    - 将ES6的语法转换为ES5的语法
    - 对TypeScript的处理
- 对CSS的处理
    - 对CSS文件模块进行加载处理
    - 对预编译css文件进行处理
- 资源文件
    - 图片img的加载
    - 字体font的加载
- HTML的处理
    - 打包HTML文件
- 处理Vue项目SFC文件
    - 对.vue文件进行打包

## Webpack的基本配置

在