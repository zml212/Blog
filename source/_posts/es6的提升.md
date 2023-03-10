---
title: ES6的提升
date: 2022-10-16
tags: [前端]
categories: [ES6]
---
# es6的提升

在es6之前，我们定义定义变量的时候，只能使用var关键字来定变量，这样有一个问题，var定义的变量会成为全局变量。  
但是在我们实际开发的过程中，我们应该尽量少的将变量或者函数暴露在全局作用域下，这是一种好的软件设计原则。

为了解决var的变量泄露问题，es6引入了let和const关键字。  

## 什么是提升

提升就是变量或者函数在定义之前就可以使用，这是因为js引擎在编译的时候，会将变量和函数的声明提升到作用域的顶部。

注意这里我们讲到了函数与变量都存在提升的现象：

### 变量提升

```js
    console.log(a); // undefined
    var a = 1;
```

在这里并不会出现`ReferenceError`的错误，因为js引擎在编译的时候，会将变量的声明提升到作用域的顶部，所以在这里我们可以使用变量a，但是值为undefined。

也就是说，变量提升只会提升变量的声明，而不会提升变量的赋值。  
变量提升，提升的变量的声明，而变量的运行逻辑还是停留在原地。

那么es6新增的let和const关键字，是否也存在变量提升呢？

我认为是会提升的，但是：由于let与const暂时性死区的问题，导致在变量声明之前使用变量会报错。
*这里看法不统一，有人认为会提升，有人认为不会提升*

```js
    console.log(a); // ReferenceError: a is not defined
    let a = 1;
```

### 函数提升

```js
    console.log(a); // function a() {}
    function a() {};
```

**这里注意：**
我们上面这段代码里面书写的是函数声明，而不是函数表达式。  
这点很重要，因为在函数提升中，只有函数声明才会被提升，函数表达式是不会被提升的。

其实很好理解，这就像变量提升相似，提升的只是函数的声明，而不会提升函数的赋值。

```js
    console.log(a); // TypeError
    var a = function() {};
```

其实上面的代码就类似于下面的样子：
    
```js
    var a;
    console.log(a); // TypeError
    a = function() {};
```

## 函数提升优先

要是函数和变量提升两者同时存在的时候，谁会更先提升呢？

```js
    console.log(a); // function a() {}
    var a = 1;
    function a() {};
```

到这里答案就很明显了，函数优先。

## 总结

不管是函数还是变量，被提升的只有声明，并没有赋值。

**注意**

- 函数提升优先
- 函数只会提升函数声明，不会提升函数表达式