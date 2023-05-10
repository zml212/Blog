---
title: Vue中的插槽
data: [2023-2-28]
tags: [前端]
categories: [JavaScript]
---

# 函数中的this

- 函数调用时，JavaScript或默认给this绑定一个值
- this的绑定与定义的位置没有关系
- this的绑定与调用方式以及调用的位置有关系
- this是在运行时被绑定的

# 1.this的绑定规则

1. 默认绑定
2. 隐式绑定
3. 显示绑定
4. new绑定

## 1.1默认绑定

1. 普通函数被独立调用

```jsx
function foo() {
    console.log(this);
}

foo(); // windows
```

1. 函数定义在对象中，但是被独立调用

```jsx
var obj = {
	bar: function(){
		console.log(this);
	}
}

let baz = obj.bar;
baz(); // window
```

1. 严格模式下，独立调用的函数this指向undefined

## 1.2 隐式绑定

通过某个对象进行调用

也就是函数调用位置是某个对象发起的函数调用。

```jsx
function foo() {
    console.log(this);
};

let obj = {
    baz: foo,
};

obj.baz(); // obj
```

**隐式绑定有一个前提条件**

- 必须在调用对象内部有一个对函数的引用（比如一个属性）
- 如果没有这样的引用，在调用的时候，就会报找不到该函数的错误
- 通过这个引用，间接将this绑定到了这个对象上

## 1.3 通过new绑定

new会经过以下几个阶段:

1. 创建新的空对象
2. 将this指向这个空对象
3. 执行函数体重的代码
4. 没有显示返回非空对象时，默认返回这个对象

## 1.4 显示绑定

如果我们不希望在对象内部包含这个函数的引用，同时又希望在该函数上进行强制调用该函数。

我们可以使用apply与call方法 

- 这两个方法第一个参数都是为一个对象
    - 这个对象就是给this准备IDE
    - 在调用你这个函数时，会将this绑定到传入的这个对象身上
- 后面的参数：apply为数组，call为参数列表

比如：

```jsx
foo.aply(obj,["as",12,4]);
foo.call(obj,"as",12,4);
```

如果想要将一个函数总是显示绑定到一个对象上，我们可以使用bind方法。

- bind方法，bind()方法创建一个新的绑定函数

```jsx
foo.bind(obj);
```

bind函数传入参数的方式与call是一样的。

## 1.5 内置函数的this绑定

1. 定时器
    
    定时器的this绑定为window。
    
2. 事件监听
3. forEach
    
    forEach默认绑定为window，你也可以在forEach第二个参数中传入要绑定的this对象。
    

```jsx
var names = ["avx","asd","fgd"];
names.forEach(function(){
	console.log(this);
},obj)
```

## 1.6 几种绑定规则的优先级

- 默认规则的优先级最低
- 显示绑定的优先级高于隐式绑定
- new绑定高于隐式绑定
- new不可以appLy/call一起使用
- new的优先级高于bind
- bind的优先级高于apply/call

new > bind > apply/call > 隐式绑定 > 默认绑定

# 2.this规则之外-忽略显示绑定

- 如果在显示绑定中，传入一个null或者undefined,那么该这个显示绑定失效，使用默认规则
- 创建一个函数的间接引用，这种情况使用默认绑定规则。

# 3.箭头函数

箭头函数是ES6之后新增的一种编写函数的方法。

- 箭头函数不会绑定this。arguments属性
- 箭头函数不能作为构造函数䣂使用（不能和new一起使用，报错）

## 3.1 箭头函数的写法

```jsx
(参数列表) => {
	函数体
}
```

## 3.2 箭头函数的编写优化（简写）

- 如果箭头函数只有一个参数，那么小括号可以省略

```jsx
item => {
	console.log(item);
}
```

- 如果函数体只有一行代码，可以省略大括号
    
    并且这行代码的返回值会作为整个函数的返回值
    
    ```jsx
    item => consloe.log(item);
    ```
    

**注意**

这一行代码中不能包含return关键字

如果要返回值，那么这行代码的运算结果就会作为整个函数的返回值返回。

```jsx
item => item*2;
```

- 如果默认返回值是一个对象，那么这个对象必须加()

```jsx
foo = () => ({name:"hi"});
```

# 3.3箭头函数里面的this

在普通函数里面是有this标识符的，但是在箭头函数里面没有this。

通过apply来调用还是没有this指向。

查找this的规则与查找变量一样：该作用域没有this，就往上一层作用域查找，直到查找到this。

# 4.面试题

## 4.1 面试题1

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/31e5753ac9ea42aabcc556d153159d6f~tplv-k3u1fbpfcp-watermark.image?)


第一个`sss()`调用时默认调用，所以this为默认绑定，this指向window，所以最后的值为window。

第二个函数调用为隐式绑定，所以this指向person,所以最后结果为person。

第三个同样也是隐式绑定，所以结果一样。

第四个是间接函数引用，返回结果为一个独立的函数，this指向window，结果为window。

## 4.2 面试题2

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1d54305f0ee74ea09f8d4f7bfce063f4~tplv-k3u1fbpfcp-watermark.image?)

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/de309ac813814f4f874fbe6ec45cbd7c~tplv-k3u1fbpfcp-watermark.image?)

- 22行： 隐式绑定，this指向person1,结果person1
- 23行：显式绑定，this指向person2，结果为person2
- 25行：箭头函数没有this，去上层作用域查找，所以this绑定为window，最后结果为window
- 26行：箭头函数没有this，去上层作用域查找，所以this绑定为window，最后结果为window
- 28行：首先执行person1.foo3()，这个函数结果为一个函数，后面接着执行返回的这个函数，所以为函数单独调用，为默认绑定，this指向window，所以最后结果为window。
- 29行：虽然foo3()的this绑定为person2，但是后面返回的一个函数，还是相当于是函数独立调用，所以最后this还是指向window，结果为window。
- 30行：最后调用的函数为显式绑定，this指向person2，所以最后结果为person2。
- 32行：因为foo4()函数返回的结果函数没有this指向，所以会往上层作用域查找，最后this指向person1，结果为person1
- 33行：foo4()的this被显式绑定为person2，所以返回结果箭头函数this也指向person2，结果为person2.
- 34行：因为箭头函数没有this，所以显式绑定不起作用，所以最后结果为person1。

## 4.3 面试题3

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c4640f4437b848c2a21724c96b0d821c~tplv-k3u1fbpfcp-watermark.image?)

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fbcb6b6392ca4271a3d35e246ddd6342~tplv-k3u1fbpfcp-watermark.image?)

new执行步骤：

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1d28c5e3c5534e7fbdfb72cc1147fcc7~tplv-k3u1fbpfcp-watermark.image?)

new操作符运行示意图：

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ee986bc7381140c7ab9549d8219154bb~tplv-k3u1fbpfcp-watermark.image?)

- 22行： 隐式绑定，结果为person1。
- 23行：显式绑定到person2，结果为person2。
- 25行：上层作用域查找，查找到person1
- 26行：箭头函数没有this，显式绑定失效，所以还是person1
- 28行：独立函数调用，默认绑定，window
- 29行：还是独立函数调用，默认绑定，结果为window
- 30行：显式绑定为person2，结果为person2
- 32行：箭头函数没有this，往上层查找，所以为foo4()的this，foo4()this指向person1，所以结果为person1
- 33行：foo4()this显式绑定为person2，所以最后结果为person2
- 34行：箭头函数没有this，显式绑定失效，所以结果还是为person1