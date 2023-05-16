---
title: JavaScript函数的增强知识
date: 20223-5-16
tags: [前端]
categories: [JavaScript]
---
# JavaScript函数的增强知识

1. 函数属性和arguments
2. 纯函数的理解和应用
3. 柯里化的理解和应用
4. 组合函数理解和应用
5. with、eval的使用
6. 严格模式的使用

# 1.函数对象的属性

在JavaScript中，函数也是属于一种对象。

## 1.1 函数的属性

函数也可以像对象一个添加自定义属性：foo.address = “asd”;

```jsx
function foo(num,count,name){
	
}
```

默认的函数对象中有一些自己的属性：

- name属性，函数的名称

```jsx
foo.name; // foo
```

- length属性：参数(形参)的个数

```jsx
foo.length;// 3
```

- 额外补充：剩余参数

```jsx
function foo(x,y,z...arg){
	
}
```

这个时候length还是为3。

## 1.2 函数中的arguments

在使用函数的时候，传入的参数个数多于定义函数时形式参数个数时，这些参数还是会存在于函数中，只是他们被存放在arguments中。

```jsx
function foo(a,b){
	console.log(arguments);
}

foo(1,2,3,4,5); // 1,2,3,4,5
```

arguments是一个传递给函数的参数的类数组对象。

类数组对象就是有数组的一些属性，length等等；但是没有数组的一些方法。

arguments可以通过索引获取到对应的值。

```jsx
arguments[0] // 1
```

- arguments转为数组的方法
    - 遍历arguments，将其每一个元素添加进一个新数组。
    
    ```jsx
    function foo1() {
        let arr = [];
        for (const item of arguments) {
            arr.push(item);
        }
        console.log(typeof arr);
        console.log(arr);
    }
    ```
    
    - ES6中的方式1：
    
    ```jsx
    let newArr = Array.from(arguments);
    ```
    
    - ES6中的方式2：解构赋值
    
    ```jsx
    let newArr = [...arguments];
    ```
    
    - 调用slice(不常用)
    
    ```jsx
    let newArr = [].slice.call(arguments);
    
    ```
    

**注意：**

箭头函数不绑定arguments，也就是箭头函数没有arguments。

```jsx
let bar = () => {
	console.log(arguments); // 报错 arguments没有被定义
}

function foo(){
	let bar = () => {
		consolel.og(arguments); // 上层作用域的arguments
	}
}
```

## 1.3函数的剩余（rest）参数

如果最后一个参数是以`…`为前缀，那么他会将剩余的参数方法到该参数中，并且作为一个数组。

```jsx
function foo(x,y,...arg){
	
};

foo(1,2,3,4,5,6);
```

默认一个函数只有一个剩余参数。

**注意：**

- 剩余参数只能写到其他参数最后，否则会报错。

剩余参数与arguments的区别：

1. 剩余参数只包含没有对应形参的实参，arguments对象包含了所有传递给函数的参数。
2. arguments不是一个真正的数组，而rest剩余参数是一个真正的数组。
3. arguments是早期的技术，rest剩余参数是ES6为了取代arguments出现的。

# 2.函数式编程

## 2.1 理解JavaScript纯函数

纯函数输入相同的值，每一次输出的结构应该都相同。

并且函数与I/O设备的输入无关。

并且不能有语义上可观察的副作用功能。

**总结：**

1. 确定的输入，结果为确定的输出，结果不会改变。
2. 函数在执行的过程中，不能产生副作用。

**什么是副作用：**

就是在执行一个函数的时候，出来返回函数值之外，还对其他东西产生了影响，比如修改全局变量，修改了对象的某个属性。

## 2.2纯函数的作用域优势

- 不需要关心外部作用域对函数的影响。
- 调用函数时，确定的输入，一定产生确定的输出。

## 2.3 柯里化概念理解

把接受多个参数的函数，变成接受一个单一参数的哈数，并且返回接受余下的参数，返回结果的新函数的技术。

比如:

```jsx
function foo(x,y,z){
	
}

// 下面就是经过柯里化的函数

function foo1(x){
	return function (y){
		return function (z){
			
		}
	}
}
```

**总结**

- 只传递给函数一部分参数来调用它，让它返回一个函数去处理剩余的参数
- 柯里化不会调用函数，只是对函数进行一个转换

```jsx
function foo(x){
	return function (y){
		return function (z){
			return x+y+z;		
		}
	}
}

foo(10)(20)(30); // 60
```

**柯里化的另一种写法：**

箭头函数的写法：

```jsx
function foo(x){
	return y => {
		return z =>{
			return x+y+z;
		}
	}
}
```

还可以这样写：

```jsx
let foo = x => y => z => x+y+z;
```

**柯里化高级-自动柯里化**

封装一个函数，传入一个函数对象，返回一个柯里化完成的函数。

## 2.3 组合函数

在开发中你，需要将上一个函数的结果作为参数传入另一个函数。当我们需要多次执行这样的函数，我们可以使用组合函数来解决。

```jsx
function double(num){
	return num*2;
}

function pow(num){
	return num**2;
}

function composeFn(num){
	return pow(double(num));
}

let result = composeFn(2); // 16
```

## 2.4 eval函数

- eval是一个特殊的函数，它可以将传入的字符串当做

JavaScript代码运行

- eval会将最后一直执行语句的结果作为返回值

```jsx

eval(`console.log("123")`)
```

这个字符串是可以访问全局变量的。

# 3. 严格模式

在支持严格模式的浏览器中国，检测到有严格模式的时候，会以更加严格的方式对代码进行检测和执行。

严格模式对JavaScript语义进行了一些限制：

- 通过抛出错误消除一些原有的静默错误
- 严格模式让JS引擎在执行代码时可以进行更多的优化（不用对一些特殊语法进行处理）
- 严格模式禁用了可能在未来版本可能会定义的一些语法。

## 3.1 开启严格模式

- 开启全局严格模式：在代码开始写上下面这段代码

```jsx
"use strict"
```

- 给某个函数开启严格模式：在函数体内部开始写上这样一段代码.

```jsx
function foo(){
	"use strict"
}
```

没有指令是程序返回默认模式。

在现代JavaScript中，class与module自动启用`use strict`