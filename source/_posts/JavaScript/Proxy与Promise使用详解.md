---
title: Proxy-Promise使用
date: 2023-5-25
tags: [前端]
categories: [JavaScript]
---
# Proxy-Promise使用

- Proxy-Reflect使用
    - 监听对象的操作
    - Proxy类基本使用
    - Proxy常见捕获器
    - Reflect介绍和作用
    - Reflect的基本使用
    - Reflect的receiver
- Promise使用（重要）
    - 异步代码的困境
    - 认识Promise作用
    - Promise基本使用
    - Promise状态变化
    - Promise实例方法
    - Promise的类方法

# 1. Proxy-Reflect使用详解

## 1.1 监听对象的操作

什么叫对对象属性的操作？

```jsx
const obj = {
    mame: "zml",
    age: 18,
    height: 1.88,
}

console.log(obj);
obj.name = "hmbb";
```

对对象属性的获取以及修改，添加删除等等操作，就是对对象属性的操作。

在Vue中，就是监听对象中每一个属性的所有操作，但是我们在对象中单独写getter与setter太过于麻烦。

我们可以使用遍历来进行设置。

`Object.definedProperty`主要是用于设置某个属性的监听，而`Proxy`更适合监听整个对象。

## 2.1 Proxy基本使用

Proxy是ES6新增的一个类，我们可以先创建一个代理对象，对该对象的所有操作，都由代理对象来完成。

- 首先，`new Proxy`对象，并且传入要监听的对象以及一个处理对象，称之为`handler`。
    
    ```jsx
    const p = new Proxy(target,handler);
    ```
    
- 其次，我们之后的操作都是直接对Proxy的操作，不是原有的对象，因此我们需要在handler进行监听。

在handler里面编写的每一个函数，都是一个捕获器。

get与set

**分别对象的是函数模型：**

- set函数有四个参数
    - target：目标对象（被监听的对象）
    - property：将被设置的属性key
    - value：新的属性值
    - receiver：调用的代理对象
- get函数有三个参数
    - target：目标对象（被监听的对象）
    - property：将被设置的属性key
    - receiver：调用的代理对象

```jsx
const obj = {
    name: "as",
    age: 18,
    height: 1.99,
}

// 创建一个Proxy对象
const objProxy = new Proxy(obj, {
    // 捕获器
    set: function (target, key, value) {
        console.log(`监听：设置${key}的值:${value}`);
        target[key] = value;
    },
    get: function (target, key) {
        console.log(`监听：监听${key}的获取`);
        return target[key];
    }
});

// 对obj的所有操作应该去操作objProxy
console.log(objProxy.name);
objProxy.name = "jmbb";
console.log(objProxy.name);

objProxy.address = "重庆";

// 结果
//监听：监听name的获取
//as
//监听：设置name的值:jmbb
//监听：监听name的获取
//jmbb
//监听：设置address的值:重庆
```

## 2.2 Proxy所有的捕获器

![image.png]([https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bbe8f6b8c7964bcfbe26ed57d976c824~tplv-k3u1fbpfcp-watermark.image?)](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bbe8f6b8c7964bcfbe26ed57d976c824~tplv-k3u1fbpfcp-watermark.image?))

- Proxy的construct与apply。

监听对象的new操作与apply操作。

# 3. Reflect基本使用

Reflect是一个对象，不能通过new创建。直接调用对象里面的一些方法。

提供了操作JavaScript对象的一些方法。

出现Reflect的原因：

- 早期JS的设计没有考虑到对象本身操作的规范，将这些方法放到Object，但是Object是一个构造函数，这些操作放在Object不合适。还有一个方法使用时感觉不对劲。
- 另外使用Proxy时，可以做到不操作原对象。

**Reflect常见的方法：**

Reflect方法与Proxy是一一对应的。

![image.png]([https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0a290c3d8df14a1b98c56592c09b564d~tplv-k3u1fbpfcp-watermark.image?)](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0a290c3d8df14a1b98c56592c09b564d~tplv-k3u1fbpfcp-watermark.image?))

Reflect的好处：

1. 操作代理对象，不直接操作原对象
2. Reflect.set方法有返回Boolean值，可以判断本次操作是否成功

Reflect中Reciver中的作用：

- Reflect使用setter与getter中，有一个参数为reciver，这个参数的作用就是改变setter或者getter中的this指向。

# 4. Promise基本使用

- Promise是一个类。
- 通过new创建Promise的时候，需要传入一个回调函数，称之为executor.
    - 这个executor函数会被立即执行，并且这个回调函数还会传入两个回调函数，作为失败/成功调用的回调函数(resolve,reject)。
    - 调用resolve回调函数时，执行Promise对象的then方法传入的回调函数。
    - 调用reject回调函数时，执行Promise对象的catch方法传入的回调函数。

大致结构：

```jsx
const p = new Promise((resolve, reject) => {
    // 执行异步任务
    // 异步任务代码
    // 异步任务完成，调用回调函数
    // 调用成功回调函数
    if (success) {
        resolve();
    } else {
        // 调用失败回调函数
        reject();
    }
});

// 外部的回调函数
p.then(() => {
    console.log("执行成功");
}).catch(() => {
    console.log("执行失败");
})
```

## 4.1 Promise状态的解析

Promise有三种状态：

- 待定状态（padding）：此时没有调用reslove或者reject.
- 成功状态(fulfilled)：调用reslove函数，此时为成功状态
- 失败状态(rejected)：调用reject函数，此时为失败状态

**注意：**

promise的状态只能从待定状态转向成功状态或者失败状态。并且这个过程不可逆，并且 不能从其他方式进行转换。

- 转换方式一： padding —> fulfilled
- 转换方式二：padding —> rejected

## 4.2 Reslove的值

- 普通值

reslove里面可以返回任何的数据类型，比如字符串，数组，对象等等。

- resolve(promise)

如果reslove回调函数返回的值是一个Promise,那么当前resolve的Promise的状态由resolve函数所返回的Promise来决定。

```jsx
const promise = new Promise((reslove, reject) => {
    setTimeout(() => {
        reslove("2秒之后成功");
    }, 2000);
})

const p = new Promise((reslove, reject) => {
    reslove(promise);
})
p.then((res) => {
    console.log("成功:", res);
}).catch(() => {

})
```

这段代码2秒之后输出：`成功: 2秒之后成功`.

- reslove(thenable对象)

```jsx
const p = new Promise((reslove, reject) => {
    reslove({
        name: "azml",
        then: function (resolve) {
            resolve(111);
        }
    });
})
p.then((res) => {
    console.log("成功:", res);
}).catch(() => {
  
})
```

输出`成功：111`.

## 4.3then的返回值

Promise本身支持链式：then方法本身是由返回值的，它的返回值是一个Promise。

- 当then方法中的回调函数本身在执行的时候，那么它处于p ending状态
- 当then方法中的回调函数返回一个结果的时候，那么它处于fulfilled状态，并且会将结果作为reslove的参数：
    - 情况一： 返回一个普通的值
    - 情况二：返回一个Promise
    - 情况三：返回一个thenable值
- 当then抛出异常的时候，它处于reject状态

then方法传入的回调函数的返回值类型：

## 4.4 catch方法的返回值

catch方法返回的也是一个promise值。

在catch方法之后也可以正常使用then方法或者catch方法。

同样catch之后的一些方法也是等待catch决议之后再执行。

如果在链式调用的时候，比如多个then调用，出现reject的时候，会直接跳到最近的一个catch方法。

<img src="https://raw.githubusercontent.com/zml212/FigureBed/main/202305251422972.png"/>

## 4.5 finally方法

不论Promise最后是fulfilled还是rejected状态，都会执行finally方法。

```jsx
const p = new Promise((resolve, reject) => {
    
});

p.then()
    .catch()
    .finally()
```

## 4.6 Promise的类方法

- resolve方法：

一般用于已经有一个值，我们想将其转换为Promise使用。

`Promise.resolve(”Hello”);`

相当于：（new一个Promise并且立即执行resolve）

```jsx
new Promise(resolve => { resolve("Hello") });
```

- reject方法：

`Promise.reject(”err”);`

相当于：(new一个Promise并且立即执行reject)

```jsx
new Promise(reject => { reject("err") });
```

**注意：**

下面几个方法传入多个Promise，要用可迭代的对象包裹起来。

- all方法

它的作用就是将多个Promise包裹在一起形成一个新的Promise。

新的Promise状态由被包裹的所有Promise决定：

- 当所有的Promise状态变为fulfilled时，新的Promise状态为fulfilled，并且会把所有Promise的返回值组成一个数组。
- 当有一个Promise状态为reject，新的Promise状态为reject，并且会将第一个reject的返回值作为参数

```jsx
const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log("p1");
    }, 2000)
});

const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log("p2");
    }, 3000)
});

const p3 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log("p3");
    }, 4000)
});

Promise.all([p1, p2, p3])
```

结果：

```jsx
p1
p2
p3
所有Promise结果: [ 'p1', 'p2', 'p3' ]
```

当有reject的Promise：

```jsx
const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log("p1");
        resolve("p1");
    }, 2000)
});

const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log("p2");
        // resolve("p2");
        reject("p2");
    }, 3000)
});

const p3 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log("p3");
        resolve("p3");
    }, 4000)
});

Promise.all([p1, p2, p3]).then(res => {
    console.log("所有Promise结果:", res);
}).catch(err => {
    console.log("出错了：", err);
})
```

结果：

```jsx
p1
p2
出错了： p2
p3
```

- allSettled方法：

该方法会在所有的Promise都有结果（settled），无论是fulfilled还是rejected，才会有最终的状态。

这个Promise的结果一定是fulfilled。

```jsx
const p1 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log("p1");
        resolve("p1");
    }, 2000)
});

const p2 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log("p2");
        // resolve("p2");
        reject("p2");
    }, 3000)
});

const p3 = new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log("p3");
        resolve("p3");
    }, 4000)
});

Promise.allSettled([p1, p2, p3]).then((res) => {
    console.log(res);
}).catch((err) => {
    console.log(err);
})
```

结果：

```jsx
p1
p2
p3
[
  { status: 'fulfilled', value: 'p1' },
  { status: 'rejected', reason: 'p2' },
  { status: 'fulfilled', value: 'p3' }
]
```

最后输出的是一个对象，这个对象包含status状态，以及对应的value值。

- race方法：

多个Promise互相竞争，谁先出结果，就用谁的结果。（不论fulfilled还是rejected）