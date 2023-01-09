---
title: EventEmitter类
---
# EventEmitter 类

`eventEmitter`是`events`模块里面的一个对象，它的核心功能就是事件触发与事件监听器功能的封装。

## 引入event模块

代码：

```js
	var events = require('events');
```

我们通过`require`引入这个events模块之后，我们实例化一个`eventEmitter`对象

```js
	var eventEmitter = new events.EventEmitter();
```

这里注意：

- 实例化对象使用`new`关键字
- 后面的`EventEmitter`两个单词首字母都需要大写


我们有了eventEmitter对象之后，就可以为指定的事件添加监听器了。

在此之前，我们先定义两个监听器：

```js
	var listener1 = function(){
		console.log('我是第一个监听器');
	}
	var listener2 = function(){
		console.log('我是第二个监听器');
	}
```

接下来就是为eventEmitter绑定监听器了。

## 绑定监听器

1. 使用`on`方法绑定：
	`eventEmitter.on('事件', 监听器);`
	使用`on`方法进行绑定的时候，可以多次使用从而绑定多个监听器，执行的时候按照绑定顺序进行执行。
2. 使用`addListener`方法绑定：
	`eventEmitter.addListener('事件',监听器);`
	这个方法是将一个监听器添加到监听器数组的尾部。也就是将这个监听器添加到原来事件监听器数组的最后一个。
3. 使用`once`方法绑定：
	`eventEmitter.once('事件',监听器);`
	看方法名字，我们就大致可以知道这个监听器只会执行一次，执行完毕之后就会解绑。

### 栗子：

```js
	// 默认事件名字为test
	
	// 方法一绑定监听器
	eventEmitter.on('test',listener1);
	eventEmitter.on('test',listener2);
	// 方法二绑定监听器
	eventEmitter.addListener('test',listener1);
	// 方法三绑定监听器
	eventEmitter.once('test',listener1);
```

上面我们使用了三种方式来绑定监听器，方法一中的监听器会按照绑定顺序执行；方法二中的监听器会最后一个执行，因为它位置事件监听器数组的最后一个，所以此时绑定的监听器会最后一个执行；方法绑定的监听器只会执行一次。

## 解绑监听器

前面我们知道了给事件绑定监听器，那么如何给事件解绑监听器呢？

这里node.js为我们提供了两种方法：

1. 	`removeListener(event, listener)`
2. 	`removeAllListeners([event])`

从名字我们就可以看出两个方法的不同，第一个只是移除指定事件上的指定监听器；第二则是移除指定事件上的所有监听器。

### 栗子：

```js
	// 方法一：
	eventEmitter.removeListener('test',listener1);
	// 方法二：
	eventEmitter.removeAllListener('test');
```
