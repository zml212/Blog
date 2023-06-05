---
title: Storage、正则表达式
date: 2023-06-06
tags: [前端]
categories: [JavaScript]
---
# Storage、正则表达式

- Storage存储
- 正则表达式
- 认识防抖

# 1. Storage存储

## 1.1认识Storage

WebStorage提供了一种更加直观的存储方式。

- localStorage：本地存储，提供的是一种永久性的存储方法。（关闭网页再打开，依然存在）
- sessionStorage：会话存储，提供本次会话的存储（关闭会话时，存储内容被清除）

**localStiorage与sessionStorage的区别：**

- 关闭网页再打开，localStorage数据会被保留，但是sessionStorage数据会被删除。
- 在页面内进行跳转，两者的数据都会被保存。
- 在页面外进行跳转（打开新网页），localStorage会保存，sessionStorage不会被保留。

Storage常见的方法、属性：

- length属性：(获取存储的个数)

```jsx
console.log(localStroage.length);
```

- 使用localStorage保存数据：

```jsx
const name = "海绵宝宝";
// 保存数据到本地

localStorage.setItem("userName", name);
```

- 从localStorage获取数据：
    
    方式一：
    

```jsx
let name = localStorage("userName");
```

       方式二：

```jsx
let name = localStorage.key(0);
```

- 从localStorage删除数据

删除指定数据

```jsx
localStorage.removeItem("userName")
```

删除所有数据

```jsx
localStorage.clear();
```

Storage一般不能存放对象。

我们可以将对象类型使用`JSON.stringify(对象)`转换为字符串。

取值之后使用`JSON.parse(结果)`转换为对象。

# 2.正则表达式

帮助我们匹配字符串。

正则表达式主要由两部分组成：模式与修饰符。

## 2.1 创建一个正则表达式

```jsx
// 创建正则表达式
const re1 = new RegExp("abc","ig");
const re2 = /aaa/ig;
```

## 2.2 使用正则表达式

1. 使用正则对象上的实例方法。
2. 使用字符串的方法，传入一个正则表达式。

```jsx
const re1 = new RegExp("abc", "ig");

const str = "fAbc asdaaBc asdABC dfabC";

// 将所有的abc替换为cba,忽略大小写

// 使用字符串上的方法
const newStr = str.replaceAll(/abc/ig, "cba");
console.log(newStr); 
```

正则表达式的常用方法：

![](https://raw.githubusercontent.com/zml212/FigureBed/main/%E6%AD%A3%E5%88%99%E7%9A%84%E5%B8%B8%E7%94%A8%E6%96%B9%E6%B3%95.png)


上面两个方法为正则对象的方法，下面的都是字符串的方法。

`matchAll()`这个方法传入的正则表达式必须包含全局匹配（g）。

- 常见的修饰符

![](https://raw.githubusercontent.com/zml212/FigureBed/main/%E4%BF%AE%E9%A5%B0%E7%AC%A6%E7%9A%84%E4%BD%BF%E7%94%A8.png)


- 匹配规则字符类：

![](https://raw.githubusercontent.com/zml212/FigureBed/main/%E6%AD%A3%E5%88%99%E5%8C%B9%E9%85%8D%E5%AD%97%E7%AC%A6%E7%B1%BB.png)

<img src="[https://raw.githubusercontent.com/zml212/FigureBed/main/正则匹配字符类.png](https://raw.githubusercontent.com/zml212/FigureBed/main/%E6%AD%A3%E5%88%99%E5%8C%B9%E9%85%8D%E5%AD%97%E7%AC%A6%E7%B1%BB.png)"/>