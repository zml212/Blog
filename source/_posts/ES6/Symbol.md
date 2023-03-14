---
title: Symbol基础
data: [2023-3-9]
tags: [前端]
categories: [ES6]
---

# Symbol

symbol是es6提出了的一种原始数据类型，用来表示独一无二的值。并且不可以通过new关键字创建。

symbol可以通过`symbol（）`这个函数创建。

## Symbol的一些基本知识

### Symbol的创建

```js
var s = Symbol();
console.log(s); // Symbol()
console.log(typeof s); // symbol
```

### 给Symbol附带一个参数进行创建

是不是很奇怪，为什么我们定义了一个Symbol类型的值，但是我们没有给那个symbol变量赋值，最后却输出了一个`Symbol()`。这个其实只是一个标记，如果我们在使用`Symbol`这个函数创建变量的时候，传入一个参数，那么最后输出的时候，这个参数就会带着输出，这样就可以方便我么分别不同的Symbol值。

就像这样：

```js
var s2 = Symbol("我是一个Symbol类型");
console.log(s2);  // Symbol(我是一个Symbol类型)
```

### 不能使用new关键字创建

前面我们提到，Symbol是通过Symbol这个函数进行创建的，Symbol并不是一个对象，而是一个基本数据类型，如果使用new关键字进行创建就会报错。

```js
var s3 = new Symbol();
```

报错信息：

```error
var s3 = new Symbol();
         ^
TypeError: Symbol is not a constructor
    at new Symbol (<anonymous>)
```

这就证明我们创建Symbol不能通过new关键字创建。

### 如果Symbol创建传参为一个对象，则输出时使用toString()方法将内容输出

```js
var obj = {
    toString() {
        return '海绵宝宝';
    }
};
var s4 = Symbol(obj);
console.log(s4); // Symbol(海绵宝宝)
```

最后输出的结果就是：`Symbol(海绵宝宝)`。

### 每一个Symbol都是独一无二的

尽管我们给多个Symbol函数传入相同的参数，但是他们却是不相等的。

```js
var s5 = Symbol("test");
var s6 = Symbol("test");
console.log(s5 === s6);  // false
```

可以看到最后的结果是false，这是因为Symbol出现的原因就是为了解决每个变量都不同的问题。

### Symbol不可以与其他数据类型进行运算

这个Symbol不能与其他数据类型进行运算，包括其本身也不行。

```js
var s6 = Symbol("12");
var s7 = Symbol("34");
console.log(s6 + s7);
```

报错：

```error
console.log(s6 + s7);
               ^
TypeError: Cannot convert a Symbol value to a number
```

### Symbol可以显示转换为字符串

也就是说我们可以直接将Symbol类型的值调用String方法直接转换为字符串。

```js
var s8 = Symbol("海绵宝宝");
console.log(String(s8));  // 'Symbol("海绵宝宝")'
console.log(s8.toString()); // 'Symbol("海绵宝宝")'
```

**注意：**

Symbol作为对象不会出现在 for...in、for...of 循环中，也不会被 Object.keys()、Object.getOwnPropertyNames()、JSON.stringify() 返回

### 创建多个相同的Symbol

如果在开发中我们需要创建多个相同的Symbol，那么我们可以使用`Symbol.for`这个方法。

```js
let s9 = Symbol.for("12");
let s10 = Symbol.for("12");
console.log(s9 === s10); // true
```

## Symbol的常用场景

### 作为属性名使用

在对象中，我们可能需要一个表示独一无二的变量，这个时候就可以用到Symbol。

实例：

```js
// 第一种写法
var oop = {};
let s13 = Symbol('key');
oop[s13] = '海绵宝宝';
// 第二种写法
var oop1 = { [s13]: '海绵宝宝' };
// 第三种写法
var a = {};
Object.defineProperty(a, mySymbol, { value: '海绵宝宝' });
```

这个时候对象里面的该属性就是独一无二的。

### 作为独一无二的变量

有的时候我们并不需要知道这个变量里面存的值是什么，我们只想将这几个变量区分开来，保证他们是独一无二的。这个时候也可以将Symbol派上用场。

### 其他一些使用场景

- 私有属性和方法：在类的实现中，可以使用Symbol来定义私有属性和方法，这样可以避免与其他属性和方法的命名冲突。

- 迭代器：Symbol可以作为迭代器的方法名，用于迭代对象中的值。

- 定义常量：Symbol定义的常量是唯一的，可以用作枚举类型。

- 事件名称：在发布/订阅模式中，可以使用Symbol来定义事件名称，确保事件的唯一性。

- 用作标记：Symbol可以用作标记，用于区分不同的代码块或函数。