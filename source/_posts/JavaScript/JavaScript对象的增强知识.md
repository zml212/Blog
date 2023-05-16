---
title: JavaScript对象的增强知识
date: 2023-5-16
tags: [前端]
categories: [JavaScript]
---
# JavaScript对象的增强知识

1. Object.defineProperty
2. 数据属性描述符
3. 存取属性描述符
4. Object.defineProperties
5. 对象的其他方法补充

# 1. 对象属性的控制

针对对象某一个对象，我们可以对里面的一些属性进行查看修改。

我们可以通过某种方式对这些行为进行限制。

## 1.1 对属性操作的控制

默认情况下，对对象某一个属性操作是没有限制的。

如果我们香香对一个属性进行比较精准的操作控制，我们可以使用属性描述符。

- 通过属性描述符可以精准的添加或者修改对象的属性
- 属性描述符需要使用Object.defineProperty来对属性进行修改或者添加

# 2 Object.defineProperty

- Object.defineProperty()方法直接在对象上定义一个新属性，或者修改一个对象的现有属性，并且返回该对象。

```jsx
Object.defineProperty(要操作的对象,要修改的属性(名称或者Symbol),属性描述符)
```

## 2.1 属性描述符分类

- 数据属性
- 存取属性

数据属性描述符有以下特征：

1. [Configurable]：表示属性是否可以通过delete删除属性，是否可以修改它的特性，或者是否可以将它次该为存取属性描述符。

直接在对象上定义一个属性，[Configurable]默认情况下为`true`

通过属性描述符定一个一个属性时，[Configurable]默认为`false`

```jsx
Object.defineProperty(obj, "name", {
    configurable: false, // 不可以配置该属性
})
```

1. [Enumerable]:表示属性是否可以通过for in或者Object.keys()返回该属性

直接在对象上定义一个属性，[Configurable]默认情况下为`true`

通过属性描述符定一个一个属性时，[Configurable]默认为`false`

```jsx
Object.defineProperty(obj, "name", {
    configurable: true,
    enumerable: true, // 可枚举
})
```

1. [writeable]：表示是否可以修改属性的值

直接在对象上定义一个属性，[Configurable]默认情况下为`true`

通过属性描述符定一个一个属性时，[Configurable]默认为`false`

```jsx
 Object.defineProperty(obj, "name", {
    configurable: true,
    enumerable: true, // 可枚举
    writable: true, // 可以写入
})
```

1. [value]:修改属性的值

```jsx
Object.defineProperty(obj, "name", {
    configurable: true,
    enumerable: true, // 可枚举
    writable: true, // 可以写入
    value: 'qweqwe',
})
```

存取属性描述符： 

1. [setter]：设置属性时会执行的函数，监听设置属性修改赋值的事件

```jsx
Object.defineProperty(obj, "name", {
    set: function () {
        console.log("name属性被修改了");
    }
})
obj.name = "12212"; // name属性被修改了
```

1. [getter]：获取属性时会执行的函数，监听获取属性值的事件

```jsx
Object.defineProperty(obj, "name", {
    set: function () {
        console.log("name属性被修改了");
    },
    get: function () {
        console.log("name属性被获取了");
    }
})
obj.name = "12212";
let name = obj.name; // name属性被获取了
```

# 3. 多个属性的描述符

`Object.deineProperties()`

```jsx
Object.defineProperties(对象名称,{
	要设置的属性名
},
{
	属性描述符
})
```

# 4. 对象 方法的补充

- getOwnPropertyDescriptor:获取某个属性的默认属性描述符。
- getOwnPropertyDescriptors:获取某个对象所有属性的默认属性值

```jsx
Object.getOwnPropertyDescriptor(对象名，属性名)
Object.getOwnPropertyDescriptors(对象名)
```

- 阻止对象的扩展（严格模式下报错）

preventExtensions()