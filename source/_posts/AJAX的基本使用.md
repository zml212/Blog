---
title: AJAX的基本使用
date: 2022-10-22
tags: [前端]
categories: [AJAX]
---
# AJAX的基本使用

## 1. 什么是AJAX

AJAX是Asynchronous JavaScript and XML的缩写，意思是异步的JavaScript和XML。

在实际开发当中，我们经常会更新网页中的数据，但是又不想更新一部分数据，将整个页面进行更新，这个时候就需要用到我们今天讲得ajax技术了。

AJAX 是一种用于创建快速动态网页的技术。

通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

## 2. AJAX的基本原理

AJAX的基本原理是通过XMLHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用javascript来操作DOM而更新页面。

### 使用AJAX的步骤

- 创建一个XMLHttpRequest实例化对象
- 初始化
- 发送请求
- 更新网页

首先，第一步，我们需要创建一个XMLHttpRequest对象，这个对象是AJAX的核心，通过这个对象，我们可以向服务器发送请求，获取服务器返回的数据。

```javascript
var xhr = new XMLHttpRequest();
```

通过上面的代码，我们成功创建了一个`XMLHttpRequest`对象，接下来我们需要初始化这个对象，初始化的时候，我们需要指定请求的类型，请求的URL，以及是否异步发送请求。

第二步，初始化`XMLHttpRequest`对象

```javascript
xhr.open("get", "http://localhost:8080/ajax", true);
```

在初始化这一步，我们使用`XMLHttpRequest`对象的`open()`方法，这个方法接收三个参数，第一个参数是请求的类型，第二个参数是请求的URL，第三个参数是是否异步发送请求。 
第一个参数，请求的类型，一般有两种，一种是`get`，一种是`post`，`get`是从服务器上获取数据，`post`是向服务器发送数据。  
当第三个参数为`true`的时候，表示异步发送请求，当第三个参数为`false`的时候，表示同步发送请求。

第三步，发送请求

```javascript
xhr.send();
```

在发送请求的时候，我们使用`XMLHttpRequest`对象的`send()`方法，这个方法接收一个参数，这个参数就是要发送到服务器的数据。  
如果是`get`请求，这个参数可以省略。但是如果是`post`请求，这个参数不可以省略。

第四步，更新网页

在这一步，我们需要判断请求是否请求成功，然后我们将服务器返回的数据拿到，然后通过javascript来操作DOM，来更新页面。

```javascript
xhr.onreadystatechange = function () {
    if (xhr.readyState == 4 && xhr.status == 200) {
        var data = xhr.responseText;
        console.log(data);
    }
}
```

在这一步，我们使用`XMLHttpRequest`对象的`onreadystatechange`事件，这个事件会在`XMLHttpRequest`对象的`readyState`属性发生改变的时候触发。

那么什么是`readyState`呢？`readyState`属性表示`XMLHttpRequest`对象的状态，它有五种状态，分别是：

- 0：请求未初始化
- 1：服务器连接已建立
- 2：请求已接收
- 3：请求处理中
- 4：请求已完成，且响应已就绪

那么整个请求过程中，`readyState`属性的变化有四个阶段：

- 0 -> 1
- 1 -> 2
- 2 -> 3
- 3 -> 4

也就是说，在完整的请求过程中，会触发四次`onreadystatechange`事件。

上面代码中`xhr.readyState == 4 && xhr.status == 200`表示。只有当`readyState`属性为4的时候，表示请求已经完成，且响应已经就绪，这个时候我们才能拿到服务器返回的数据。  
然后进行更新页面。