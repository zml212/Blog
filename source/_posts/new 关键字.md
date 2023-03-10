---
title: new关键字
date: 2022-8-30
tags: [前端]
categories: [JavaScript]
---
# new 关键字

在JavaScript中使用构造函数的时候，我们就会用到`new`关键字。

## 定义：

`new`关键字是用来创建一个用户自定义的对象类型实例或者具有构造函数的内置对象的实例

## js中的`new`做了什么？

假设我们要创建一个`user`的新实例，这个时候我们需要使用`new`关键字，那我们以这种方式调用构造函数，会经历一下四个步骤：

1. 创建一个新的对象。
2. 将构造函数的作用域赋给新对象（所以`this`就指向了这个新的对象）。
3. 执行构造函数中的代码（为我们这个新对象添加相应的属性）。这些属性就是构造函数里面的属性，我们通过这一步将构造函数的属性添加到新对象里面。
4. 最后返回这个新对象。

## 我们来看看`new`的例子

    function Test(name) {
        this.name = name
    }
    Test.prototype.sayName = function () {
        console.log(this.name)
    }
    const newTest = new Test('海绵宝宝')
    console.log(newTest.name) // '海绵宝宝'
    newTest.sayName() // '海绵宝宝'

在这个例子中，我们可以看到：

- `new`关键字创建了一个新的对象。
- 并且这个新对象可以访问到构造函数的属性。
- 这个新对象还可以访问到构造函数的方法。
- 也就是说，我们通过`new`操作符将构造函数与实例对象通过原型链链接起来了。

但是上面那个例子，构造函数并没有返回任何值（默认返回undefined）,要是我们让他返回值，会发生什么情况呢？

这个分为两种情况：

1. 返回值为原始值。
2. 返回值为对象。

我们一种一种的来看

首选我们看看返回值为原始值的时候：

    function Test(name) {
        this.name = name;
        return '111';
    }
    const newTest = new Test('海绵宝宝');
    console.log(newTest.name); // '海绵宝宝'

上面这个例子，我们可以看到，即使我们最后返回了一个字符串，但是最后实例对象还是输出了构造函数里面的属性值。这说明这个原始值的返回类型没有任何用处。结果还是和没有返回值一样的。

接下来我们看看返回值为对象的时候，会发生什么：

    function Test(name) {
        this.name = name
        console.log(this)
        return {
            name: '蟹老板'
        }
    }
    const Test1 = new Test('海绵宝宝')
    console.log(Test1.name)  // '蟹老板'
    console.log(Test1) // {name: '蟹老板'}

通过这个例子我们知道，当构造函数里面将返回值设置为一个对象的时候，返回的对象会作为实例对象的属性。这个返回值将会被正常的使用。

**总结：**

构造函数尽量不要使用返回值，因为原始值的没有任何作用，返回对象的话会导致`new`操作符失效。