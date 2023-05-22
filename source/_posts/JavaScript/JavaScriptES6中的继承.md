---
title: JavaScript ES6中的继承
date: [2023-5-19]
tags: [前端]
categories: [JavaScript]
---
# JavaScript ES6中的继承

1. 原型继承关系图
2. class方式定义类
3. extends实现继承
4. Babel的ES6转ES5 
5. 面向对象多态理解
6. ES6对象增强

# 1. 原型继承关系图

![image.png]([https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/90fb1bd1e89e4f7da9ea53d3d8ba4095~tplv-k3u1fbpfcp-watermark.image?)](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/90fb1bd1e89e4f7da9ea53d3d8ba4095~tplv-k3u1fbpfcp-watermark.image?))

对象只有隐式原型，但是函数既有显式原型，又有隐式原型。

函数的隐式原型看这个函数是被谁创造出来的。

中间最上面的Function Foo()是被function Function 创建出来的，所以隐式原型__proto__指向Function的显示原型。

# 2.class方式定义类

在ES5中定义类使用function，在ES6中定义类使用class.

第一种方法：

```jsx
class Person{
	// 类中的构造函数
	// 当我们通过new创建一个Person类时，默认调用class类中的constructor方法
	constructor(name,age){
		 this.name = name;
			this.age = age;
	}

	// 类中的实例函数
	 runing(){
		console.log(this.name+"正在跑步");
	}
}
```

构造函数矛实例函数之间没有逗号间隔。 

创建实例对象：

```jsx
var p1 = new Person("admin",12);
console.log(p1.name,p1.age); // admin 12
p1.runing(); // admin正在跑步
console.log(p1.__proto__ === Person.prototype); // true
```

第二种方法：

```jsx
var Student = class{
	
}
// 创建实例对象
var p2 = new Student();
```

在class不支持函数的重载，也就是一个class里面只能有一个constructor函数。

**class与function的不同点** 

- class定义的类，不能作为一个一个普通的函数去调用

## 2.1 class中定义访问器方法

```jsx
class Person {
    // 类中的构造函数
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    // setter
    set name(age) {
        this.age = age;
    }

    // getter
    get name() {
        return age;
    }
   
};
```

## 2.2静态方法

静态方法通常用于定义直接使用类来执行的方法，不需要有类的实例，使用static关键字来定义。

```jsx
class Person {
    constructor(name) {
        this.name = name;
    }
    // 实例方法
    running() {
        console.log(this.name + "正在奔跑");
    }

    // 静态方法
    static eating() {
        console.log("吃饭");
    }
}

var p1 = new Person("海绵宝宝");
p1.running();
Person.eating();
```

实例方法需要通过new关键字创建出的实例来调用，而静态方法可以直接调用。

静态方法同样也有this，谁调用这个方法，this就指向谁。

# 3.extends实现继承

```jsx
// 父类
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    running() {
        console.log(this.name + "正在奔跑");
    }
}

class Student extends Person {
    constructor(name, age, score) {
        super(name, age);
        this.score = score;
    }

    studying() {
        console.log(this.name + "正在学习");
    }
}

class Teacher extends Person {
    constructor(name, age, money) {
        super(name, age);
        this.money = money;
    }
    working() {
        console.log(this.name + "正在工作");
    }
}

var stu1 = new Student("海绵宝宝", 12, 100);
console.log(stu1.name, stu1.age, stu1.score);
var tea1 = new Teacher("泡芙", 22, 1000);
console.log(tea1.name, tea1.age, tea1.money);
```

首先定义一个父类，子类中需要用到父类的属性时，直接使用super关键字。

将共同的方法可以抽取到父类之中。

我们在继承的时候：`子类 extends 父类`

## 3.1 super关键字

class为我们的方法中提供了一个方法

- super.menthd()：在子类调用父类的一个方法
- super():用来调用父类的constructor方法

注意

- 在子类中的构造方法中使用super必须在this之前，也就是说必须先通过super调用父类的构造函数。
- super的使用位置有三个：
    - 子类的构造方法
    - 实例方法
    - 静态方法

```jsx
class Animal {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    running() {
        console.log("running");
    }
    eating() {
        console.log("eating");
    }

    // 静态方法
    static sleep() {
        console.log("正在睡觉");
    }
}

class Dog extends Animal {
    constructor(name, age) {
        super(name, age);
    }
    // 如果父类继承的方法达不到效果，我们可以重写方法
    running() {
        console.log(this.name + "正在奔跑");
        // 如果要保留父类的方法，可以使用super调用父类的方法
        super.running();
    }
    static sleep() {
        console.log(this.name + "趴着睡觉");
        super.sleep();
    }
}

var dog = new Dog("李长林", 23);
dog.running();
dog.eating();
Dog.sleep();
```

可以看到静态方法也可以使用super。

## 3.2 继承内置类

我们还可以继承JavaScript内部类，比如array，我们可以在Array添加一些新方法：

```jsx
// 给数组添加一个方法，获取最后一个元素
class ZMLArray extends Array {
    lastItem() {
        return this[this.length - 1];
    }
}

var arr = new ZMLArray(1, 2, 3, 4,2,1,2);
console.log(arr.lastItem()); // 2
```

## 3.3 类的混入mixin(了解)

JavaScript继承只支持单继承。

# 4.JavaScript中的多态

不同的数据类型进行同一个操作，表现出不同的行为，这就是多态。

- 继承是多态的前提（实现接口）。
- 必须有父类的引用指向子类对象。

```jsx
// 计算面积
class shape {
    getArea() {

    }
}

// 矩形
class Rectangle extends shape {
    constructor(width, height) {
        super();
        this.width = width;
        this.height = height;
    }
    getArea() {
        console.log(this.width * this.height);
    }
}

class Circle extends shape {
    constructor(radius) {
        super();
        this.radius = radius;
    }
    getArea() {
        console.log(Math.PI * this.radius * this.radius);
    }
}

var rect1 = new Rectangle(100, 200);
var rect2 = new Rectangle(10, 20);

var c1 = new Circle(10);
var ac2 = new Circle(20);

function getShapeArea(shape) {
    console.log(shape.getArea());
}

getShapeArea(rect1);
getShapeArea(c1);
```

# 5. 对象字面量增强

- 属性的简写
- 方法的简写
- 计算属性名
- 解构Destrucuring:
    - 数组解构
    - 对象解构

## 5.1 属性的简写

当我们在对象里面写一个外部已经存在的变量时，我们可以直接使用简写。例如：

```jsx
// 属性的简写
var name = "hmbb";
var obj = {
    // name: name;
    name,
}
console.log(obj.name); // hmbb
```

## 5.2 方法的间隙

方法的简写：这种写法绑定this。

```jsx
// 方法的简写
running(){
    // 方法体
}
```

## 5.3 计算属性名：

```jsx
// 属性的简写
var key = "address"
var obj = {
   
    // 计算属性的简写
    [key]: "广州"
}
console.log(obj.address); // 广州
```

## 5.4 解构：

最简单的数组以及对象解构：

```jsx
var name = ["asda", "fwe", "zxcz"];
var obj = { name: "hmbb", age: 13, height: 1.99 };

// 数组的解构
var [name1, name2, name3] = name;
console.log(name1, name2, name3);

// 对象的解构
var { name, age, height } = obj;
console.log(name, age, height);
```

**关于数组的解构**

- 顺序问题：

在数组里面进行解构的时候有严格的顺序问题。如果我们想要跳过一个元素：

```jsx
var [name1,,name3] = name;
```

也就是在跳过的元素要用逗号留出位置。

- 解构出数组

我们解构之后剩余的元素全部放到一个新数组里面。

```jsx
 var [name1,..newArr] = name;
```

这样的结果就是`name1:asda`,`newArr:[”fwe”,”zxcz”]`。

- 解构的默认值

当解构失败的时候，我们可以为这个值设置一个自定义的值。

```jsx
var [name1,name2,name3 = "undefined"] = name;
```

如果name3解构失败，那么它的值就是undefined。

**关于对象的解构**

- 对象解构的顺序问题

对象的解构时没有顺序的，是根据key来进行解构的。

```jsx

var obj = {mame:"ada",age:12,height:1.99};
var {height,age,name} = obj;
```

对象解构是按照对象的属性名来进行一一匹配，然后解构。

- 对象解构重命名

将结构出来的结果，名字进行更改。

```jsx
var {height:gaodu,age:nianling,name:mingzi} = obj;
```

进行重命名之后，调用的时候，就使用我们重命名之后的变量名进行调用

- 对象解构默认值

与数组的默认值一样，当解构失败的时候，可以设置一个默认值。

```jsx
var {height:gaodu,age:nianling,name:mingzi,address:dizhi = undefined} = obj;
```

- 剩余

这个与数组解构是一样的，将没有变量存放的元素，放到一个新的对象里面。

```jsx
var {height,..newObject} = obj;
```

## 5.5解构的应用场景

- 拿到变量自动进行解构之后使用
- 对函数的参数进行解构、