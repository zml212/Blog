---
title: await、async、事件循环、错误
date: 2023-06-05
tags: [前端]
categories: [JavaScript]
---
# await、async、事件循环

- async、await
- 浏览器进程、线程
- 宏任务、微任务队列
- Promise面试题解析
- throw、try、catch
- 浏览器存储Storage

# 1. 异步函数

async、await

在函数定义的时候前面添加一个async。

```jsx
async function foo(){
	// 函数体
}
```

异步函数内部的代码与普通函数一样，默认情况下是同步执行的。

## 1.1 异步函数的返回值

当异步函数有返回值时：

- 异步函数的返回值相当于被包裹在Promise.reslove中。

```jsx
async function foo() {
    return 321;
}

console.log(foo()); // Promise { 321 }
```

- 如果异步函数返回的也是一个Promise，那么状态会由Promise决定

```jsx
async function foo1() {
    return new Promise((reslove, reject) => {
        setTimeout(() => {
            reslove("321");
        }, 1000)
    })
}
console.log(foo1()); //  Promise { <pending> }
```

- 如果异步函数返回的是一个对象并且实现了thenable，那么就会由then方法来决定。

```jsx
async function foo2() {
    return {
        then: function (reslove, reject) {
            reject("错误");
        }
    }
}

foo2().catch(res => {
    console.log(res); // 错误
});
```

异步函数返回的是一个Promise。

## 1.2 异步函数的异常

如果在执行函数代码的时候产生了异常，这个异常不会立即被浏览器处理，会Promise.reject()。（作为Promise.reject传递）

也就是我们可以在返回值调用catch捕获到。 

## 1.3 await关键字的使用

await这个关键字只能在异步函数里面才能使用（ES13里面新增可以在顶层模块使用）

通常使用await的时候，在这个关键字后面跟上一个表达式，这个表达式一般返回一个Promise。

这个await会等待Promise状态变为fulfilled状态之后，才会继续执行下面的代码。

```jsx
async function foo() {
    const res = await new Promise((reslove) => {
        setTimeout(() => {
            reslove("111");
        }, 2000)
    })
    console.log(res); 
}

foo(); // 两秒之后输出111
```

await处理连续的异步请求：

```jsx
function requestData(url) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve(url)
        }, 2000);
    })
}

async function foo1() {
    const res1 = await requestData('1111');
    console.log(res1);
    const res2 = await requestData('4444');
    console.log(res2);
}

foo1();
```

如果在await处理时，Promise返回的是一个reject，

1. 那么就会向外抛出异常，我们可以在异步函数的返回值调用catch捕获异常。并且后面的代码不会执行

```jsx
function requestData(url) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (url >= 2222) {
                resolve(url)
            } else {
                reject("err:" + url)
            }
        }, 2000);
    })
}

async function foo1() {
    const res1 = await requestData('1111');
    console.log(res1);
    const res2 = await requestData('4444');
    console.log(res2);
}

foo1().catch(err => {
    console.log(err);
});
```

1. 我们还可以使用try catch 捕获异常

```jsx
function requestData(url) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (url >= 2222) {
                resolve(url)
            } else {
                reject("err:" + url)
            }
        }, 2000);
    })
}

async function foo1() {
    try {
        const res1 = await requestData('1111');
        console.log(res1);
        const res2 = await requestData('4444');
        console.log(res2);
    } catch (err) {
        console.log("捕获到异常：" + err);
    }
}

foo1().catch(err => {
    console.log(err);
});
```

# 2. 进程和线程

**进程**：计算机已经运行的程序，是操作系统管理程序的一种方式

**线程**：操作系统内能够运行运算调度的最小单位，通常情况下它被包含在进程中。

## 2.1 浏览器中的JavaScript线程

JavaScript运行是有自己的容器：浏览器和node.js。

JavaScript代码在执行的过程之中都是在一个单独的线程之中执行的，也就是JavaScript在同一时刻内，只能做一件事。

## 2.2 浏览器的事件循环

JavaScript代码中的一些异步任务 ，并不会直接放在上下文执行栈中，而是会被方法哦浏览器中，将这些事件放到一个事件队列当中，执行到哪一个事件的时候再将该事件放到上下文执行栈中。

![](https://raw.githubusercontent.com/zml212/FigureBed/main/%E4%BA%8B%E4%BB%B6%E5%BE%AA%E7%8E%AF.png)

## 2.3 微任务和宏任务

在事件队列中，又分为微任务和宏任务。

为什么要进行区分呢？

事件循环中并非只维护着一个队列，事实上是有两个队列：

- 宏任务队列：ajax、setTimeout、setInterval、DOM监听、UI Rendering等
- Promise的then回调、MutationObserverAPI、queueMicrotask()。

宏任务与微任务的执行顺序：

1. 在main script中的代码优先执行（编写在顶层的script代码）
2. 在执行任何一个宏任务之前，都会查看微任务队列里面是否还有任务需要执行。 

# 3. 异常

## 3.1 抛出异常

通过throw抛出异常。

使用throw会中断代码的执行。

```jsx
function throwErr() {
    throw new Error("抛出错误");
}

throwErr();
```

这段代码就会抛出一个错误。

throw关键字可以跟上以下几种数据类型：

1. 基本数据类型：number、string、Boolean
2. 对象类型。

JavaScript中有一个自带的Error类。

**Error**包含三个属性：

1. message:创建Error对象时传入的message（）错误信息。
2. name ：Error的名称，通常和类的名称一致。
3. stack：整个Error错误信息，包括函数的调用栈。

Error的子类：（了解）

- RangeError：下标值越界时使用的错误类型
- SyntaxError：解析语法错误时的错误类型
- TypeError：出现类型错误时的错误类型

## 3.2 异常的捕获

抛出的异常如果不进行捕获，它会继续往上抛出，直到被顶部（window或者global）捕获。

所以一般我们要对异常进行捕获。

使用try catch进行异常捕获。

```jsx
function foo() {
    throw new Error("错误信息");
}

try {
    foo();
} catch (error) {
    console.log(error);
}
```

最后错误被捕获：

![](https://github.com/zml212/FigureBed/blob/main/%E5%BC%82%E5%B8%B8%E7%9A%84%E6%8D%95%E8%8E%B7.png)

如果代码中有一些必须要执行的代码，我们可以在catch中添加一个finally，finally里面的代码无论如何都会执行。