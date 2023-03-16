---
title: Set的基本使用
data: [2023-3-15]
tags: [前端]
categories: [ES6]
---

# Set的基本使用

Set作为ES6新出的一种数据类型，它是一种特殊的集合，它里面没有键名，并且在Set中，相同的元素只会出现一次。

## Set的增删改查

### Set的创建

Set不是原始数据类型，所以我们同样需要使用`new`关键字来创建。

就像这样：

```js
let s1 = new Set([1, 2, 3, 4, 5]);
```

在创建Set的时候，我们使用`New`关键字然后后面跟上`Set()`在这里面，我们可以选择个ISet传入一个参数，当然这个参数一般都是数组类型的，这个传入的数组将被作为Set的初始值，当然，我们也可以不给Set传入参数，此时Set的值为0。

### Set添加元素

在Map中我们使用`map.set(key,value)`给Map添加元素，在Set中，我们将会使用`set.add(value)`。就像这样：

```js
let s3 = new Set();
s3.add(2);
console.log(s3); // 2
```

使用这个方法返回的是Set本身。

### Set删除元素

我们使用`set.delete(value)`来删除Set中指定的元素，如果Set中存在该元素，那么就返回true，删除成功；否则Set中不存在该元素，删除失败，返回false.

```js
let s4 = new Set([1, 2])
var isTrue = s4.delete(1);
var isTrue2 = s4.delete(3);
console.log(isTrue, isTrue2); // true false
```

在删除`1`这个元素的时候，找到了该元素，删除成功，返回一个布尔值`true`，但是在删除`3`这个元素的时候，在该Set里面并没有，删除失败，返回一个布尔值`false`。

### 判断Set是否含有指定元素

我们需要判断某个元素是否在Set中时，我们可以使用`set.has(value)`。它的返回值也是一个布尔值，包含返回`true`，不包含返回`false`。

```js
let s5 = new Set(["2","3"]);
let flag = s5.has("3");
let flag2 = s5.has(3);
console.log(flag,flag2); // true  false
```

### 清空整个Set

如果我们需要将整个Set进行清空，那么我们只需要使用`set.clear()`方法即可，这个使用方法与Map一致。

```js
let s6 = new Set([1,2,3,4]);
console.log(s6); // Set(4) { 1, 2, 3, 4 }
s6.clear();
console.log(s6); // Set(0) {}
```

### 获取Set元素的个数

我们也可以使用`set.size`来获取Set元素的个数，这个`size`是Set的一个属性，而不是方法，所以我们在使用的时候可以不用加上`()`。

```js
let s7 = new Set([1, 2, 3, 4, 5, 6]);
var length = s7.size;
console.log(length);  // 6
```

## Set的迭代

Set的迭代其实与Map相差不大，Map上面的迭代方法在Set这里也能使用。这里是我写的另一篇[Map使用技巧](https://juejin.cn/post/7210301826939764792)。

在Set中我们可以使用下面三种方法进行遍历。

- set.keys()： 遍历并返回一个包含所有值的可迭代对象，
- set.values()：所得到的结果与`set.keys()`一样，这里主要是为了与Map兼容
- set.entries()：遍历并返回一个包含所有的实体 [value, value] 的可迭代对象，它的存在也是为了兼容 Map。

## Set的一个使用场景

因为Set是不包含重复元素的，所以我们可以使用Set来给数组去重，但盘【】哦、2图 是如果数组里面是一些对象类型的数据，比如这样`[{"anme":1},{"name":1}]`这样并不能通过Set去重，因为在Set看来，这两个是不一样的。

数组去重：

```js
let arr = [1,2,2,3,4,4,5,5,6,7,8,9,9];
let set = new Set();
for (const i of arr) {
    set.add(i);
}
arr = Array.from(set);
console.log(arr);

// 结果

// [
//   1, 2, 3, 4, 5,
//   6, 7, 8, 9
// ]
```

这样我们就通过Set成功将数组给去重了。