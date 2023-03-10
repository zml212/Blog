---
title: AJAX常见一些问题的处理
date: 2022-10-23
tags: [前端]
categories: [AJAX]
---
# AJAX常见一些问题的处理

在用户体验中，可能会存在这样的问题，当服务器半天没有返回数据的时候，用户可能会多次进行请求。这就会导致浏览器增加许多的请求，这样就会导致服务器的压力增大，而且用户体验也会变差。

## 1. 取消请求

当用户多次点击按钮的时候，会导致浏览器发送多次请求，这样就会导致服务器压力增大，而且用户体验也会变差。  
为了解决这个问题，就需要将用户多与的请求给取消掉，只保留第一次请求。

这样就可以极大的缓解服务器的压力，而且用户体验也会变好。

### 1.1 代码实现

```js
btn.onclick = function(){
    // 1. 创建XMLHttpRequest对象
    var xhr = new XMLHttpRequest();
    // 2.取消请求
    xhr.abort();
}
```

上面的代码中，我们只需要在发送请求之前，调用`abort()`方法就可以了。

也就是说，我们如果要取消用户的请求，只需要使用`abort()`方法,这个方法是`XMLHttpRequest`对象自带的方法。

## 2. 超时处理

当然实际的情况往往是很复杂的，也就会出现这样一种情况，当用户点击按钮发送请求之后，因为服务器的问题，很久都没有返回数据。但是我们不能让用户一直干等着。  
这个时候，我们就需要给用户一个提示，告诉用户，服务器没有返回数据，你可以等待，也可以取消。

这个时候我们使用`timeout`属性就可以了。

### 2.1 代码实现

```js
    const xhr = new XMLHttpRequest;
    // 设置超时时间 超过时间不响应  取消请求
    xhr.timeout = 2000;
    // 超时处理
    xhr.ontimeout = function(){
        console.log('请求超时');
    };
    // 网络异常回调
    xhr.onerror = function(){
        console.log('网络异常');
    };
```

代码解释：

上面的代码中我们通过`timeout`属性设置了超时时间，当超过这个时间的时候，就会触发`ontimeout`事件。当用户因为网络问题导致不能正常请求的时候，就会触发`onerror`事件。

`timeout`属性是用来设置超时时间的，单位是毫秒。  
`ontimeout`属性是用来设置超时处理的。在这个事件中，我们可以做一些处理，比如提示用户，服务器没有返回数据。  
`onerror`属性是用来设置网络异常处理的。在这个事件中，我们可以做一些处理，比如提示用户，网络异常。

## 结尾

本文主要讲解了`XMLHttpRequest`对象的取消请求和超时处理。

好了今天的分享就到这里了。如果你觉得本文对你有帮助，欢迎点赞和分享。