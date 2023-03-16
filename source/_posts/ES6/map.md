---
title: Map数据类型
date: [2023-3-17]
tags: [前端]
categroies: [ES6]
---
[TOC]
# Map

在Map这种数据类型里面，我们可以使用各种数据类型作为Map的键名，打破了之前对象只能使用一部分数据类型作为键名的限制。

## 创建Map

Map不是一种原始数据类型，所以我们需要使用`new`关键字来创建，在创建Map数据类型的时候，需要将Map大写。

```js
let m = new Map();
```

就像这样，我们创建出了Map对象。

## Map的增删改查

### 给Map对象赋值

给Map对象赋值，我们使用`set`方法，并且这个方法返回的是Map对象本省，所以如果需要对Map对象赋多个值，我们可以链式赋值。

就像这样：

```js
p.set("2", "q")
   .set("3", "w")
   .set("4", "e");
```

我们这样就给Map对象赋值了三个。

### 获取Map对象指定key的value

通过get方法可以获取到指定键名的值，`map.get(键名)`就可以得到对应的值，如果不存在该key,那么就返回undefined。

```js
p.get("2"); // "q"
p.get("-1"); // undefined
```

### 判断Map对象是否拥有这个`key`

我们使用`has(key)`方法来判断Map对象是否拥有这个`key`，如果存在那么返回`true`，如果不存在就返回`false`。

例如：

```js
p.has("2"); // true
p.has("-1");  // false
```

### 删除Map对象指定的键名及其数据

我们可以使用`delete(key)`来删除我们指定的`key`的数据。

```js
p.delete("2"); // 删除key值为“2”的元素
p.has("2"); // false
```

### 清空整个Map

我们使用`clea()r`直接将这个Map清空。

```js
p.clear();
var i = p.size; // 0
```

### 获取整个Map的长度

我们使用`size`属性来获取Map的数据长度。

```js
let count = p.size;
```

## Map迭代

在Map中，我们可以直接使用for   of变量，但是这样遍历的结果是数组形式的。

如果我们需要一些其他的遍历。

这里有三种方法：

- map.keys():返回一个包含所有键的可遍历对象
- map.value():返回一个包含所有值（value）的可遍历对象
- map.enties():返回一个包含所有实体（[key:value]）的可遍历对象

我们举一个例子：

```js
let mp = new Map();
mp.set("1", "q")
    .set("2","w")
    .set("3","e");
let x1 = mp.keys();
let x2 = mp.values();
let x3 = mp.entries();
for (const i of x1) {
    console.log(i);
}
console.log("--------------");
for (const i of x2) {
    console.log(i);
}
console.log("--------------");
for (const i of x3) {
    console.log(i);
}
```

最后运行结果：

```结果
1
2
3
--------------
q
w
e
--------------
[ '1', 'q' ]
[ '2', 'w' ]
[ '3', 'e' ]
```

**注意：**

在Map中，遍历的顺序适合我们插入值的顺序是有关的，也就是说遍历Map是按照插入顺序进行遍历的。这一点也与Object不同。

## 从对象创建Map

在创建Map的时候，我们可以给Map传入一个数组的参数，里面包含一些键值对的数组。就像这样：

```js
let mp = new Map([
	["1","q"],
	["2","w"]
])
```

这样创建出来的Map就已经初始化了。

在对象中有这样一个方法：`Object.entries(对象)`这个方法可以将对象里面的每一个元素都转换为键值对的数组，并且返回一个数组，我们可以利用这个方法将对象转换为Map。

```js
var obj = {
    "1": "q",
    "2": "w",
    "3": "e"
}

let m1 = new Map(Object.entries(obj));
let x4 = m1.entries();
for (const i of x4) {
    console.log(i);
}

// 结果

// [ '1', 'q' ]
// [ '2', 'w' ]
// [ '3', 'e' ]
```

## 从Map创建对象

前面我们使用`Object.entries(obj)`的方法创建了对象，我们现在可以使用`Object.fromEntries(arr)`来创建对象。

`Object.fromEntries(arr)`需要传入的参数是一个键值对的数组。就类似于这样的：`[[q:w],[e:r],[t:y]]`。

这样的格式是不是很眼熟，是的，我们在Map中使用`map.entries()`得到的结果就是这样的。

所以从Map创建对象，我们可以这样做：

```js
let mp1 = new Map();
mp1.set("1", "q")
    .set("2", "w")
    .set("3", "e");

let obj1 = Object.fromEntries(mp1.entries());
console.log(obj1);

// 结果

{ '1': 'q', '2': 'w', '3': 'e' }
```