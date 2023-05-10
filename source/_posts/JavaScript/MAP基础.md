---
title: MAP基础
date: 2022-10-12
tags: [前端]
categories: [ES6]
---
# MAP基础

map不同于普通的对象，普通对象的键值对，键只能是数值、字符串或者符号，而map的键可以是任意类型的值，包括对象、函数、数组等。

## 创建map

- 使用Map构造函数

    const map1 = new Map();

我们可以在使用Map构造函数创建的时候，顺带进行初始化：

    const map2 = new Map([
      ['name', '张三'],
      ['age', 18]
    ]);

## Map常见方法

- set(key, value)设置键值对

    map.set(key, value);

在map初始化之后，我们使用`set`方法，可以往map中添加键值对。

    const m = new Map();
    m.set('name', '海绵宝宝');// 添加了一个键值对

- has(key)判断是否存在某个键

    map.has(key);

在实际开发中，我们有时候需要查询，map里面是否存在这样一个键，此时，我们就需要用到`has`方法：

    const m = new Map();
    m.set('name', '海绵宝宝');
    alert(m.has('name'));// true

若是存在，返回`true`，否则返回`false`。

- get(key)获取键值对

在上面我们知道了如何查询一个元素在map中是否存在，那么，如果我们想要获取这个元素的值，我们就需要使用`get`方法：

    map.get(key);

`get`方法：如果存在，返回对应的值，否则返回`undefined`。

    const m = new Map();
    m.set('name', '海绵宝宝');
    alert(m.get('name'));// 获取到了键值对的值 海绵宝宝


- delete(key)删除某个键值对

    map.delete(key);

前面我们知道了可以增加、查询、判断是否存在某个键，那么如何删除呢？使用`delete`方法。

    const m = new Map();
    m.set('name', '海绵宝宝');
    m.delete('name');// 删除name键值对
    alert(m.has('name'));// false 

在删除之后，我们再次查询，发现已经不存在了。

- clear()清空map

    map.clear();

若是我们想直接清空整个map，可以使用`clear`方法。

    const m = new Map();
    m.set('name', '海绵宝宝');
    m.clear();// 清空map
    alert(m.has('name'));// false

使用`clear`之后，这个map里面的所有内容将会被删除，我们再次查询，发现已经不存在了。

## 如何选择

前面我们说道，map和object的区别有在于键：

- map的键可以是任意类型的值，而object的键只能是数值、字符串或者符号。

那么在实际开发中，我们如何来选择呢?

1. 内存

在固定内存大小的情况下，map可以比object更好的存储数据（map可以多储存50%的键值对）。

2. 查找

两者差别不大，在键值对较少的情况下，object的速度也快一点，如果object的键是当做数组的索引一样使用的话，那么object的速度会更快。

3. 插入

map比object稍微快一点，但是差别不大，如果是插入大量的键值对，那么map的速度会更快。

4. 删除

在大多数浏览器中，map的删除速度更快，如果涉及大量删除键值对的情况，那么毫无疑问map是首选。