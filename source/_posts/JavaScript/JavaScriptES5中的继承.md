---
title: JavaScript  ES5中的继承
date: [2023-5-18]
tags: [前端]
categories: [JavaScript]
---
# JavaScript  ES5中的继承

1. 对象和函数的原型
2. new 、constructor
3. 原型链的查找顺序
4. 原型链实现的继承
5. 借用构造函数实现继承
6. 寄生组合实现继承

# 1. 对象和函数的原型

- 获取对象的原型方式
    1. obj.__proto__：有兼容性问题
    2. Object.getPrototyprOf(obj)

当我们想获取一个属性的值时，如果在自己的对象中查找，找到直接返回，否则沿着原型链向上查找。

在JavaScript中，每一个对象都有一个特殊内置属性[[prototype]]

## 1.1函数的原型

所有的函数都有一个prototype属性，不是__proto__。

1. 将函数看成是一个普通的对象时，它是具备__proto__（隐式原型）

```jsx
function foo() { }
console.log(foo.__proto__);
```

1. 将函数看成一个函数的时候，它是具备prototype的

```jsx
function foo() { }
console.log(foo.prototype);
```

对象是没有`prototype`原型的，这个是显式原型。

这个原型的作用是用来构建对象时，给对象设置隐式原型。

# 2. new、constructor

## 2.1 new操作符

1. 创建空对象
2. 将这个空对象赋值给this
3. 将函数的显式原型赋值给对象，作为该对象的隐式原型
    1. `obj.__proto__ = Person.prototype`
4. 执行函数体代码
5. 将这个对象默认返回

**原型的作用：**

- 多个对象拥有共同的值，我们可以将它放到构造函数对象的原型里面，有构造函数创建出来的对象，都可以共享这些属性。

## 2.2 constructor属性

默认情况下，原型对象上有一个constructor属性，这个constructor指向当前函数对象。

## 2.3 重写原型对象

当我们需要向原型对象添加过多的属性，我们可以选择重写原型。

```jsx
function Perso(){
	
}

Person.prototype = {
	message:"11",
	info:{name:"11",age:30},
	running: function (){},
	eating: function(){},
	constructor:Person, //添加constructor属性
}
```

但是上面这种方式添加constructor属性，会导致constructor可以被遍历。

更好的方式通过Object.defineProperty():`Object.defineProperty(Person.prototype,”constructor”,{value:Person})`

# 3. 面向对象的特征-继承

在ES5中实现继承。

使用原型链。

## 3.1 什么是原型链

我们从一个对象获取属性时，如果在当前对象中没有查找到，就会顺着原型向上查找，一层一层向上查找，这就原型链。

原型链的顶层为Object。

在Object中的原型中，值为null，该对象上有很多默认的属性和方法。

## 3.2 通过原型链实现方法继承

- 方式一： 父类的原型直接赋值给子类的原型（错误）

这样会导致子类修改原型对象的时候，将父类的原型对象也更改了。

- 方法二：创建一个父类的实例对象（new Person()），用这个实例作为子类的原型独享。

```jsx
// 定一个构造函数
function Person (name,age){
    this.name = name;
    this.age = age;
}

// 定义学生类
function Student(name,age,id,score){
    this.name = name;
    this.age = age;
    this.id = id;
    this.score = score;
}

var p = new Person("hmbb",12);
Student.prototype = p;
Student.prototype.studying = function (){
    console.log("learning");
}

var stu1 = new Student("hmbb",12,111,100);
stu1.studying();
```

**原型链存在的弊端：某些属性保存在p对象上**

- 直接打印对象，看不到我们想要的属性
- 会被多个对象共享
- 不能给Person传递参数

## 3.3借用构造函数实现属性继承

在子类的函数内部调用父类的函数。

```jsx
// 定一个构造函数
function Person(name, age) {
    this.name = name;
    this.age = age;
}

// 定义学生类
function Student(name, age, id, score) {
    Person.call(this, [name, age]);
    // this.name = name;
    // this.age = age;
    this.id = id;
    this.score = score;
}
```

## 3.4 组合继承

组合继承是JavaScript最常用的继承方式之一。 

## 3.5创造原型对象的方法

**满足的条件**

1. 必须创建出一个对象
2. 这个对象的隐式原型必须指向父类的显示原型
3. 将这个对象赋值给予子类的显式原型

方案一：(不好)

```jsx
var p = new Person();
 Student.prototype = p;
```

方案二：

```jsx
var obj = {};
Object.setPrototype(obj,Person.prototype);
Student.prototype = obj;
```

方案三：

```jsx
function F(){};
F.prototype = Person.prototype;
Student.prototype = new F();
```

方案四：

```jsx
var obj = Object.create(Person.prototype);
```

使用Object.create()方法创建对象，并且将这个对象的原型指向传入的那个原型对象。

**封装实践**