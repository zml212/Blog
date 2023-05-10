---
title: JSON对象
tags: [前端]
categories: [JavaScript]
---
# JSON对象
## 1.JSON

在JSON对象当中，每个对象只有一个值，这个值可以是数组或者对象，也有可能是一个原始类型的值。整个对象只能是一个值，不能是两个或者多个值。

JSON对象对值的类型和格式有着严格要求：

- 复合类型的值只能是数组或者对象，不能是函数、正则表达式、日期对象。
- 原始类型的值只有四种：字符串、数值（必须为十进制）、布尔值、null（不能使用NaN、Infinity、-Infinity、undefined）
- 字符串必须使用双引号表示、不能使用单引号
- 对象的键名必须放在双引号里面
- 数组或者对象的最后一个成员后面，不能加逗号

```JavaScript
["one", "two", "three"]

{ "one": 1, "two": 2, "three": 3 }

{"names": ["张三", "李四"] }

[ { "name": "张三"}, {"name": "李四"} ]
```

null、空数组、空对象都是合法的JSON对象

## 2JSON对象的一些方法

### 2.1 JSON.stringify()

使用`JSON.stringify()`方法将一个值转换为JSON对象，并且可以使用`JSON.parse()`方法还原。

```javascript
var str = "abc";
console.log(str);// abc
var json = JSON.stringify(str);
console.log(json); // "abc"
var obj = JSON.parse(json);
console.log(obj); // abc
```

注意：
- 经过`JSON.stringify()`转换的原始类型字符串，转换结果会带有双引号

```javasscript
JSON.stringify('foo') === "foo" // false
JSON.stringify('foo') === "\"foo\"" // true
```

这里可以看到，转换之后的结果与原始值加双引号并不相等，而是等于原始值加`"\""\"`。那这是因为什么呢？

这是因为，内层的双引号告诉引擎：这是一个字符串，不然后面还原的时候，引擎就不知道这是一个布尔值还是一个字符串。

如果某个对象的属性值是函数、undefined、XML对象，那么这些属性不能作为JSON对象的值，所以使用这个方法的时候，这些属性会被`JSON.stringify()`自动过滤。

如果某个数组的成员是`undefinded`或者函数，那么这些都会被`JSON.stringify()`转换为`null`。

正则对象会被转换成空对象。

`JSON.stringify()`对象还会忽略对象不可遍历的属性。

### 2.2 JSON.stringify()的第二个参数

`JSON.stringify()`方法还可以接受一个数组，作为第二个参数，指定参数对象的哪些属性需要转成字符串。但是这第二个参数只对对象有作用。

```js
var obj = {
    "a": 1,
    "b": 2,
    "c": 3
}

var arr = ['a', 'b'];

var json = JSON.stringify(obj, arr);
console.log(json);// {"a":1,"b":2}
```