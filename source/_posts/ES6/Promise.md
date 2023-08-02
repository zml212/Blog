# Promise

## Promise解决回调地狱的问题

有三种状态：

1.pending:未处理，等待中
2.reslove:成功处理
3.reject：失败处理

当new Promise之后，这个Promise的状态为pending，还未处理，当在promise中调用reslove方法之后，promise的状态就变为reslove；当在promise中调用reject方法之后，promise的状态就变为reject。

并且一个Promise的状态一经改变，就不可以二次改变了：

>pending --> pending
> pending --> reslove
> pending --> reject

创建一个Promise:

```javascript
	const promise = new Promise((reslove,reject) => {
		if(判断条件){
			reslove();// reslove状态
		}else{
			reject(); // reject状态
		}
	})
```

## Promise的实例方法

当Promise状态发生改变之后，我们使用Promise的实例方法进行处理：

1. then(resFunction,rejFunction);
2. catch(rejFunction);

then方法里面有两个参数，第一个参数是当Promise状态为reslove时进行调用，第二个参数是

