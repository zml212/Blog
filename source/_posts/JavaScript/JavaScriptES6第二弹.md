---
title: JavaScript中ES6第二弹
date: [2023-5-22]
tags: [前端]
categories: [JavaScript]
---
# JavaScript中ES6第二弹

- 模板字符串
- ES6函数增强用法
- 展开运算符使用
- Symbol类型
- 数据结构-Set
- 数据结构-Map

# 1.模板字符串

## 1.1 模板字符串基本使用

我们使用模板字符串可以将一些动态的数据与字符串组合在一起。

```jsx
const info = `this is my name:${name}`;
```

使用反引号来编写字符串，使用`${}`编写动态数据

# 2. ES6的函数增强

## 2.1 函数的默认参数

```jsx
function foo(arg1 = "默认值1",arg2 = "默认值2"){
	// 函数体
}
```

当调用函数的时候，没有给对应参数传递参数的时候，JS就会使用默认值。

**注意事项：**

- 有默认参数的形参尽量写在后面
- 有默认参数的形参，不会计算在length之内的（并且后面的所有参数都不会计算在length之内）。比如一个函数全有默认参数，那么参数长度就为0。
- 如果参数还有剩余参数，那么剩余参数放在最后面。（默认参数的位置放在剩余参数前面）

默认参数也可以和解构一起使用：

```jsx
const obj = {
	name:"as",
	age:12,
}

function foo({name,age} = obj){
	// 方法体
}

// 如果传入的是一个空对象
function foo1({name = "asd",age = 12} = {}){
	// 方法体
}
```

## 2.2 箭头函数

箭头函数没有显式原型，不可以将箭头函数作为构造函数，不可以使用new来构造新的对象。

```jsx
var foo = () => {
	// 函数体
}

console.log(foo.prototype); // undefined
```

# 3. 展开语法

- 可以函数调用/数组构造时，将数组表达式或者String在语法层面展开
- 在构造字面量的时候，将对象表达式以key-value的方式展开

```jsx
const name = ["as","qw","sf"]
console.log(...name); // as qw sf

const str = "zmlzmlz";
console.log(...str); // z m l z m l z

// ES9之后还可以展开对象(在构建字面量时)
const obj = {
	name:"qwe",
	age:18,
}
const info = {
	...obj,
	height:1.99,
}
console.log(info);

// 结果
//{name: 'qwe', age: 18, height: 1.99}
//age
//: 
//18
//height
//: 
//1.99
//name
//: 
//"qwe"
//[[Prototype]]
//: 
//Object
```

## 3.2 引用类型拷贝

赋值引用类型：

```jsx
const obj = {
	name:"qwe",
	age:18,
}

let info = obj;
info.name = "asd";
console.log(obj.name); // "asd"
```

引用赋值存在的问题，多个变量操作的都是同一个对象，一个改编，全部改变。

**浅拷贝：**

```jsx
const obj = {
	name:"qwe",
	age:18,
	firend:{
		name:"zmlzml",
	}
}

let info = {
	...obj,
}

info.name = "hmbb";
info.firend.name = "pai",
console.log(info.name); // hmbb
console.log(obj.name); // qwe

console.log(info.firend.name); // pai
console.log(obj.firend.name);  // pai
```

之前的操作同一对象的问题有所改善，但是还没有完全解决。

**深拷贝：**

方法一：第三方库

方法二：自己实现

方式三：利用现有的js机制，实现深拷贝JSON

```jsx
const obj = {
	name:"qwe",
	age:18,
	firend:{
		name:"zmlzml",
	}
}

const info = JSON.parse(JSON.stringify(obj));

info.firend.name = "hmbb";
console.log(info.firend.name); // "hmbb"
console.log(obj.firend.name); // "zmlzml"
```

# 3. 数值表示

- 二进制：0b
- 八进制：0o
- 十六进制：0x

ES2021新增特性：当数字过长时，可以使用`_`作为连接符。

# 4.Symbol基本使用

解决了对象属性名的命名冲突。

Symbol用于生成一个独一无二的值。

```jsx
const s1 = Symbol();

let obj = {
	[s1]:"asda",
}

console.log(obj);
```

在Symbol创建的时候，可以传入一个描述。ES2019新增。

```jsx
const s1 = Symbol();
const s2 = Symbol()';

// 加入对象中
const obj = {
	[s1]:"asd",
}

Object.defineProperty(obj,si,{
	value :"aaa",
})

// 或者Symbol对应的key
console.log(Object.keys(obj)); // 获取不到Symbol
console.log(Object.getOwnPropetySymbols(obj));

// Symbol的描述(创建的时候传入)
const s3 = Symbol("描述");
console.log(s3.description);
```

即使创建的时候，传入的描述是一样的，但是创建出来的Symbol还是不一样。

生成相同的Symbol：

- 使用Symbol.for方法创建相同的Symbol
    - 如果有相同的key(传入的描述)，通过Symbol.for可以生成相同的Symbol值
- 使用Symbol.keyFor方法获取对象的key

```jsx
const s1 = Symbol("aaa");
const s2 = Symbol.for(s1.description);
const s3 = Symbol.for(s1.description);

console.log(s3 === s2); // true
```

# 5. Set与WeakSet的基本使用

**SET**

set里面存放的数据是不会重复的，所以我们可以利用这个特性给数组去重。

创建一个空set:

```jsx
const arr = new set();
```

给set添加元素：

```jsx
const arr = new Set();
let obj = {
	name:"asd",
	age:18,
}

arr.add("str");
arr.add(12);
arr.add(obj);

console.log(arr);
```

用处：给数组去重

```jsx
let arr = [1,2,2,3,4,4,5,5,6,7,8,9,9];
let set = new Set();
for (const i of arr) {
    set.add(i);
}
arr = Array.from(set);
console.log(arr);
```

set常见的属性以及方法：

- size：获取set里面的元素个数
- add()：添加元素
- delete(value):从set删除指定与元素
- has(value)：是否包含某个元素
- clean():清空整个Set
- for  each遍历
- for  of遍历

**WEAKSET**

与Set类似，也是不能存放重复的数据。

与Set的区别：

- WeakSet只能存放对象类型，不可以存放基本数据类型
- WeakSet对元素的引用是弱引用，若没有其他引用对其某个元素的一弄，那么GC（垃圾回收机制）就会对该对象进行回收。

```jsx
// 默认情况下，都为强引用
let obj1 = {
    name: "qw",
}
let obj2 = {
    name: "asd",
}
let obj3 = {
    name: "er",
}
let arr = [obj1, obj2, obj3];
obj1 = null;
obj2 = null;
obj3 = null;
// 对象赋值为空之后，arr依然对其有引用，所以不会被GC回收
```

只能存放对象：

```jsx
let obj1 = {
    name: "qw",
}
let obj2 = {
    name: "asd",
}
let obj3 = {
    name: "er",
}

// 区别一：WeakSet只能存放对象
const ws = new WeakSet();
ws.add(obj1);
ws.add(obj2);
// ws.add(12);  报错
console.log(ws);
```

常见的方法：

- add():添加元素
- delete（value）：删除指定元素
- has（value）：是否包含某个元素

**注意：**

WeakSet不能被遍历，因为里面的元素可能下一秒就被GC回收了，所以存放在WeakSet的元素是没有办法获取的。

# 6. Map与WeakMap的基本使用

**Map**

Map与对象有点类似，都是以键值对存储。用于存储映射关系。

但是对象Key只能用字符串或者Symbol，而Map可以用任何数据形式作为Key。 

- 对象使用对象作为Key，会导致Key值为Object，最后一个为Object的键值对会覆盖之前的数据。
    
    ```jsx
    let obj = {
    	name:"q",
    }
    let obj2 = {
    	name:"s",
    }
    
    let test = {
    	[obj]:"12",
    	[obj2]:"zml",
    }
    
    console.log(test); // [object Object]: "zml"
    ```
    

Map常见的属性及其方法：

- size：获取Map中元素的个数
- set(key,value)：添加元素，返回整个Map对象
- get(key)：根据key获取Map中的value
- has（key）:根据key判断该元素是否在Map中
- delete（key）:根据key删除指定元素
- clear():清空所有元素
- forEach（callback,[,thisArg]）：遍历元素

**WeakMap:**

WeakMap的key只能使用对象，不接受其他的类型作为key。

WeakMap对对象的引用是弱引用，如果没有其他引用指向该对象，该对象就会被GC回收。

WeakMap常见方法：

- set(key,value)：添加元素，返回整个WeakMap对象
- get(key)：根据key获取WeakMap中的value
- has（key）:根据key判断该元素是否在Weak Map中
- delete（key）:根据key删除指定元素

WeakMap不支持遍历。

# 7. ES7~ES13新特性

## 7.1 字符串填充方法

```jsx
const str1 = "5";
const str2 = "5";

str1.padStart(2, "0");
str2.padEnd(2, "0");
console.log(`${str1}, ${str2}`);
```

通常用于时间的格式化。