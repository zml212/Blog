---
title: JavaScript的深拷贝与浅拷贝
date: 2022-8-26
tags: [前端]
categories: [JavaScript]
---
# JavaScript的浅拷贝与深拷贝

首先这个浅拷贝与深拷贝是针对引用值复制来谈的。

我们知道在JavaScript中，数据类型分为基本数据类型和引用（对象）数据类型。

1. 基本数据类型的特点就是他们的数据是直接存储在栈（stack）里面的.
2. 引用数据类型的就不一样了，引用数据类型他们在栈里面存放的是该对象的地址，而不是该对象的值。这个地址指向的是一个堆的内存地址，这个内存地址里面存放的是该对象的值。

## 赋值与浅拷贝的区别

前面我们有讲述过原始值复制，那么这个复制与引用复制，这两种复制的区别是什么呢？

其实我们只要知道，两者复制最后都是复制栈里面的值。在原始值中，复制的是变量的值；但是在引用值复制里面，复制的是栈里面的地址，但是真正的数据没有被复制。

引用值这样复制就会导致一个问题：复制之后的原始值与之前的原始值是相互独立的，这就是说，两者的改变不会影响到另一个；但是引用值则不同，因为他们在栈里面的地址相同，意味着他们指向的对象是同一个对象。这就导致明明修改的是另一个对象数据，但是实际上之前那个对象也被修改了。

例如：

    let message = {
        name: '海绵宝宝',
        age: 18,
    };
    let message2 = message;
    message2.name = '小海绵';
    console.log(message); // { name: '小海绵', age: 18 }

这里我们明明修改的是message2，但是message里面的`name`也被修改了，因为message2和message本质上都是指向内存里面的同一个对象。

## 浅拷贝

为了解决上面出现的这种问题，我们就需要用到浅拷贝。

那么什么是浅拷贝呢？

前面我们看到引用值在使用`=`复制时，并没有达到两个对象互相独立的效果，这是因为我们复制的是栈里面的地址，而不是对象的值。既然如此，那么我们可以直接将对象里面的各个属性单独复制出来吗？

答案是可以的。

- 第一种方法：for···in···
  
我们使用for循环，遍历对象的属性，然后再将每个属性复制出来。

    let message = {
        name: '海绵宝宝',
        age: 18,
    };
    let message2 = {};
    for (let key in message) {
        message2[key] = message[key];
    }
    console.log(message2); // { name: '海绵宝宝', age: 18 }

- 第二种方法：Object.assign

这里我们可以Object.assign，将对象的属性复制出来。

**语法：**

    Object.assign(dest,[src1,src2,...])

- dest：目标对象(我们要复制到的对象)
- `scr1,scr2,...`：源对象(我们要复制的对象)
- 返回值：返回目标对象

例如：

    ```
    let message = {
        name: '海绵宝宝',
        age: 18,
    };
    let message2 = Object.assign({}, message);
    console.log(message2); // { name: '海绵宝宝', age: 18 }
    ```

**注意：**

- 如果复制的属性已经存在，那么该属性将会被覆盖。

## 深拷贝

前面我们复制的都是对象里面全是原始值的情况，那么要是对象里面还存在一个引用值。我们使用之前的方法，会出现什么情况呢。

例子：

    let message = {
        name: '海绵宝宝',
        age: 18,
        address: {
            city: '北京',
            street: '朝阳',
        },
    };
    let message2 = {};
    for (let key in message) {
        message2[key] = message[key];
    }
    message2.address.city = '上海';
    console.log(message2); // { name: '海绵宝宝', age: 18, address: { city: '上海', street: '朝阳' } }

这里我们看到，对象里面的引用值还是没有被完全复制，这就需要使用深拷贝了。

我们可以使用lodash库的cloneDeep方法，它会深拷贝对象。

## 一个小知识点

使用const声明的对象也是可以被修改的。之前我们了解的const是声明常量的，这也就意味着它的值不能被修改。，但是在对象里面可以实现修改。

    const user = {
        name = "user",
        weight = 60,
    }
    user.weight = 70;
    alert(user.weight); // 70

也就是说只有我们将const声明作为一个整体进行修改时，才会报错。