---
title: fs文件系统模块
date: 2022-12-17
tags: [前端]
categories: [node]
---
# fs文件系统模块

## 什么是fs文件系统模块

fs模块是node.js官方提供的、操作文件的模块。

例如：

- fs.readFile():用于读取指定文件的文件内容
- fs.writeFile()：用于向指定的文件写入内容

引入fs模块：

`const fs = require('fs');`

## 读取指定文件的内容

- fs.readFile()语法：
	`fs.readFile(path[,options],callback)`
	参数：
		- path:必选参数，字符串格式，表示文件的路径
		- options:*可选* 参数，表示以什么编码格式来读取文件
		- callback:必选参数：文件读取完成之后，通过这个回调函数返回读取的结果（失败：失败信息；成功：读取的结果）

栗子：

首先我先创建一个名为`test.txt`的文档，并且里面的内容是：
`12341234`

然后我们使用node.js里面的fs模块来读取文件：

```js
	// 引入fs模块
	const fs = require('fs');
	
	// 读取文件
	fs.readFile('./test.txt','utf-8',function(err,data){
		console.log(err);// null
		console.log(data);// 12341234
	})
```

我们可以看到在读取文件的时候，后面回调函数里面有两个参数：

1. 第一个参数代表着读取失败的参数，此时我们这里读取成功，所以结果为null
2. 第二个参数代表着读取成功之后的结果，这里我们读取到文件的内容，所以输出的就是文件的内容。

我们可以根据读取文件回调函数的第一个参数返回的值来判断文件是否读取成功：如果返回的null，代表文件读取成功；否则读取失败。

## 向指定文件写入内容

- fs.writeFile()语法：
	`fs.writeFile(file,data[,options],callback);`
	参数：
		- 参数1：必选参数，字符串格式，表示文件的路径
		- 参数2：必选参数，表示写入的内容
		- 参数3：*可选* 参数，表示以什么编码格式写入内容
		- 参数4：必选参数，文件写入后的回调函数

栗子：

```js
	const fs = require('fs');

	fs.writeFile('text.txt', '海绵宝宝', 'utf-8', function(err) {
    	console.log(err);// null
	})
```

这段代码执行完毕之后，输出一个`null`，那是不是就表示已经写入成功了呢？

是的，在同级文件夹下，我们可以看到生成了一个新的`text.txt`文件，打开发现正式我们刚才写入的`海绵宝宝`。

那么我们要是再执行一遍代码，只是写入的内容发生改变，那么结果是什么呢？

```js
fs.writeFile('text.txt', '派大星', 'utf-8', function(err) {
    	console.log(err);// null
	})
```

这个时候我们再打开`text.txt`文件，发现里面的内容变成了`派大星`，也就是说使用`wirteFile()`会覆盖掉文件原来的内容。

此时，我们同样可以根据写入文件回调函数的参数返回的值来判断文件是否写入成功：如果返回的null，代表文件写入成功；否则写入失败。

##  结尾

通过`node.js`的fs模块，我们就可以实现对文件的读取以及写入了，本文章为我学习node.js的学习笔记，有不足之处望大佬们指点。



