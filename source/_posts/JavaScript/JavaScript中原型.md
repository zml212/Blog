---
title: JavaScript中原型
date: 2022-10-3
tags: [前端]
categories: [JavaScript]
---
# JavaScript中原型

## 什么是原型

众所周知，JavaScript是一门面向对象的编程语言，但是JavaScript中并没有类的概念，那么JavaScript是如何实现面向对象的呢？这就要从原型说起。

首先我们要知道：原型存在于对象中。

在JavaScript中，每个构造函数内部都有一个（prototype）属性，这个属性的值为对象，也就是原型对象。原型对象中包含了可以被对象共享的属性和方法。当我们访问一个对象的属性或者方法时，如果这个对象本身没有这个属性或者方法，那么JavaScript就会去它的原型对象中寻找，如果原型对象中也没有，那么就会去原型对象的原型对象中寻找，直到找到为止，如果最终都没有找到，那么就会返回undefined。

## 原型链

原型链就是原型对象的原型对象的原型对象……一直到Object.prototype为止，这就是原型链。

这种一层一层的查找属性的方法就是原型链。

前面我们说道JavaScript是一门没有`类`概念的编程语言，所以他就不能通过模板来创建对象。所以就出现了原型链，通过原型链来实现对象的继承。

下面我们来详细解释原型链的组成。

### Object.prototype

Object.prototype是所有对象的原型对象，也就是说所有对象都可以访问到Object.prototype中的属性和方法。

前面我们提到每个构造函数里面都有`prototype`属性，这个属性的值就是原型对象，而原型对象的原型对象就是Object.prototype。

在构造函数的实例里面又有一个`__proto__`属性，这个属性的值就是构造函数的原型对象，也就是说`__proto__`属性的值就是`prototype`属性的值。

简单来说就是构造函数的`prototype`指向原型对象，构造函数的实例`__proto__`属性指向Object.prototype。
![avatar](D://桌面/插图1.png)

### constructor

每个原型对象都有一个`constructor`属性，这个属性指向关联构造函数。

我们可以通过代码来验证这一说法：

```javascript
    function Person() {}
    console.log(person. === Person.prototype.constructor); // true
```

既然实例对象是由构造函数构造得到的，那么是不是构造函数也有一个`constructor`呢？

我们也可以通过代码来验证一下：

```js
    function Person() {}
    var p = new Person();
    console.log(P.constructor === Person.prototype.constructor); // true
```

但是实际上实例化对象并没有该属性，它的这个属性是从原型对象那里得到的。

### 原型链顶层

既然js通过原型链查找属性，前面我们也提到了如果查找到最后依旧没有找到该属性，就返回为undefined。那么原型链的顶层是什么呢？

其实答案就是Object对象，既然object是对象，理所应当它也拥有__proto__属性，只是它的值比较特殊，它的值为null。

```js
    console.log(Object.prototype.__proto__); // null
```

## 总结

最后：

**总结一下:**

- JavaScript中每个函数都有一个原型属性prototype，这个属性是一个对象，这个对象就是原型对象。
- 普通函数的构造函数是Object(),所以所有函数都是Object的实例。`Person.prototype.__proto__ === Object.prototype`
- 每个原型对象都有一个constructor属性，这个属性指向关联构造函数。
- 原型链的顶层是Object对象，它的__proto__属性的值为null。