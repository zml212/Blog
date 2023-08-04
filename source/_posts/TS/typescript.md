---
title: TypeScript笔记
date: 2023-8-2
tags: 前端
categories: [TypeScript]
---

# TypeScript 

[TOC]

## 1.基本数据类型

### 1.1js中基本数据类型

首先TS支持JavaScript中内置的所有基本类型：

> let str:string = "hmbb"; 
>
> let num:number = 12;
>
> let bol:boolean = false;
>
> let ud:undefined = undefined;
>
> let nu:null = null;
>
> let big:bigint = 100n;
>
> let sym:symbol = symbol("1");

在TypeScript中，null和undefined是所有类型的子类型，也就是这两中类型的值可以赋值给任何一个类型，而不报错。

### 1.2any类型

当给一个变量设置为any类型，那这个变量就没有任何限制，任何数据类型都可以赋值给这个变量。

不要滥用any类型，这会导致TypeScript的类型检查功能失效。

### 1.3 unknown类型

unknown类型比any类型更加安全，所有的数据类型都可以赋值给unknown类型的变量。

只是unknown类型不可以调用属性和函数。

## 2.接口类型

### 2.1 接口基本使用

接口就是一个约束作用：

```typescript
interface Person{
    name:string,
    age:number
}

let student:Person = {
    name:"海绵宝宝",
    age:12,
}
```

如果在上面的student对象中加入其他的属性就会报错：

```typescript
let student:Person = {
    name:"海绵宝宝",
    age:12,
    address:"比奇堡",
}
```

![image-20230802163633256](https://raw.githubusercontent.com/zml212/FigureBed/main/202308021636307.png)

### 2.2 可选属性

如果我们想要设置某个属性是可以有无的，我们可以设置一个可选属性：`?:`。

我们将上面Person接口重写一下：

```typescript
interface Person{
    name:string,
    age:number,
    address?:string,
}
```

此时我们student就不会出现报错了。

### 2.3 只读属性

如果要确保某个变量不能被修改，只能读取，我们可以使用只读修饰：`readonly`。

我们将Person中的`age`改为只读属性（年龄不可以随意修改）。

```typescript
interface Person{
    name:string,
    age:number,
    address?:string,
}

let student: Person = {
    name: "海绵宝宝",
    age: 12,
    address: "比奇堡",
}

student.age = 13; // 在这里修改age只读变量
```

报错：

![image-20230802164924942](https://raw.githubusercontent.com/zml212/FigureBed/main/202308021649977.png)

党我们把属性设置为只读的时候，就不可以进行二次修改了，但是可以正常读取。

### 2.4 继承

当我们想要把两个接口结合在一起的时候，我们可以使用`extends`继承。

```typescript
export interface Person {
    name: string,
    readonly age: number,
    address?: string,
}

export interface Student extends Person {
    score: number,
}

let student: Student = {
    name: "海绵宝宝",
    age: 12,
    address: "比奇堡",
    score: 12,
}
```

如果我们将score的值设置为一个字符串，就会报错：

![image-20230802165553055](https://raw.githubusercontent.com/zml212/FigureBed/main/202308021655094.png)

### 2.5 补充

当我们在同一个文件夹下，不同的文件中，声明了同一个接口（或者其他类型），使用的变量名相同时，TypeScript也会报错：

![image-20230802170341207](https://raw.githubusercontent.com/zml212/FigureBed/main/202308021703242.png)

主要有两种方法解决：

1. 使用使用`export`和`import`关键字：在每个文件中，将你想要在其他文件中使用的内容通过`export`关键字进行导出，然后在需要使用这些内容的文件中使用`import`关键字引入。这样，每个文件都成为一个独立的模块，它们的作用域不会互相干扰。

> 导出接口(interface.ts)

```typescript
export interface Student{}
```

> 在另一个文件导入接口	

```typescript
import {Student} from "./interface.ts"
```

2. 使用命名空间（namespace）：命名空间是一种在全局环境中组织代码的方式，可以避免不同文件中的变量名冲突。通过在不同文件中使用相同的命名空间，可以确保它们的内容不会相互干扰。

> 定义命名空间

```typescript
// 这里将空间命名为P
namespace P {
    interface Person {
        name: string,
        readonly age: number,
    }

    interface Student extends Person {
        score: number,
    }

    let student: Student = {
        name: "海绵宝宝",
        age: 12,
        score: 12,
    }

    // student.age = 13;

}
```

> 在另一个文件使用该命名空间的接口

```typescript
const student: P.Student = {
    score: 0,
    name: '',
    age: 0
}
```

推荐使用第一种方法。

## 3. 对象类型

在TypeScript中，对象类型并不是单单指object.

### 3.1 原始数据类型中的对象

在原始值类型中，有下面几种对象类型：

| 原始数据类型 | 对应的对象 |
| :----------: | :--------: |
|    number    |   Number   |
|   boolean    |  Boolean   |
|     null     |    Null    |
|    string    |   String   |
|  undefined   | Undefined  |

原始数据类型与其对象有下面几个区别：

1. 存储位置不同：原始数据类型直接存放在栈内存当中，但是对象存放在堆内存中。

2. 传值方式不同：原始数据类型按值传递，对象是按引用传递。

所以定义变量类型的时候，使用原始数据类型或者对象类型都是可以的，只是有上面的两点区别。

### 3.2 Object类型

Object类型是所有Object类的实例，如果使用Object来限制变量类型，那么值类型（原始数据类型）和引用类型（原始数据的对象类型）都会指向Object。

```typescript
let a:Object = 12;
let b:Object = "hmbb";
let c:Object = [1,2];
let d:Object = {name:"hmbb",age:12};
let e:Object = () => {};
```

### 3.3 object类型

object 代表所有非值类型(非原始类型)的类型，例如 数组 对象 函数等，常用于泛型约束。

它只支持引用数据类型，对于原始数据类型不支持。

```typescript
let a:object = 12; // 报错
let o:object = {name:'hmbb'}; // 合法，不报错
```

![image-20230803084523895](https://raw.githubusercontent.com/zml212/FigureBed/main/202308030845954.png)

对于引用数据类型就不会报错。

## 4. 数组类型

### 4.1 普通声明方式

使用`数据类型[]`方式来声明。

```typescript
let arr1: number[] = [1, 2, 3, 4];
```

使用这种方法定义时，前面的数据类型是什么，后面数组里面的数据类型也只能是这种或者是这种的子类。

否则就会报错：

```typescript
let arr1: number[] = [1, 2, 3, "q",4]; // 不能将类型“string”分配给类型“number”。
```

### 4.2 使用泛型来声明

> Array<类型名>

```typescript
let arr2:Array<boolean> = [false,true,false];
```

同样数组里面只能存放定义时的数据类型或者它的子类型。

### 4.3 类数组 -- arguments

> 类数组就是所有参数的集合

在TS中，有一个内置的类型：IArguments

```typescript
function getArr(a: number, b: number, ...arg: any[]): void {
    let arr: IArguments = arguments;
    console.log(arr);
}

getArr(1, 2, [33, 4, 5, 6, 7]);  // [Arguments] { '0': 1, '1': 2, '2': [ 33, 4, 5, 6, 7 ] }
```

IArguments类型其实就是：

```typescript
interface IArguments{
	[index:number]:any,
    length:number,
    callee:Function,
}
```

### 4.4 使用接口来声明

> 使用接口来定义

```typescript
interface Person {
		name: string;
  		age: number;
 		 email: string;
}

const people:Person[] = [
    {
        name:"zzz",
        age:12,
        email:"1111@qq.com"
    }
]
```

## 5.函数

### 5.1 普通方式定义

> TS可以限制函数的参数类型，以及函数的返回值类型

比如:

```typescript
function fn(name:string,age:number):string{
	return name+age;
}
```

上面的函数第一个参数只能传入一个字符串类型，第二个参数只能传入number类型，并且返回值只能是字符串类型。

如果调用函数时，传入的参数或者返回的数据类型与定义类型不符，就会报错。

![image-20230803094025380](https://raw.githubusercontent.com/zml212/FigureBed/main/202308030940435.png)

### 5.2 对象形式定义

> 使用接口或者type 的方式

```typescript
interface User{
	name:string,
	age:12,
	address?: string,
}

function fn1(user:User) :string{
    return user.name+user.age;
}

console.log(fn1({ name: "hmbb", age: 12 })); //hmbb12
```

### 5.3 函数重载

> 函数重载就是：方法名相同，参数类型不同，返回值类型也可以不相同。
>
> 执行重载函数的时候，会从上往下执行，所以需要把最精确的类型放在最前面。

```typescript
function fn(params:number):void//第一套规则
function fn(params:string,params2:number):void//第二套规则
function fn(params:any,params2?:any):void{
    console.log(params)
    console.log(params2)
}
```

执行器从上往下执行，一旦匹配到合适的函数规则，就不再往下继续匹配，直接开始执行函数体里面的内容。

## 6.联合类型|交叉类型|类型断言

### 6.1 联合类型

> 当我们一个变量可能会存放两种及其以上的数据类型，这个时候我们可以使用联合类型。

```typescript
let a:number|string = 12;
a = "hmbb";
```

同理这个方法在函数的参数一样可以使用。

### 6.2 交叉类型

> 前面的联合类型是对基础的单个数据类型进行了扩充，交叉数据类型就是将多种数据类型的集合(比如接口)进行扩充。

```typescript
interface Per1{
	name:string,
	age:number,
}
interface Stu1{
	score:number,
}

const p: Per1 & Stu1 = {
        name: "hmbb",
        age: 12,
        score: 90,
    }
```

### 6.3 类型断言

> 类型断言可以使用: 值 as 类型 或者 <类型>值
>
> 推荐前面那一种写法，因为在TSX中，<>可能会引起误会。

- 类型断言的用途

（1）将一个联合类型推断为其中一个类型

（2）将一个父类断言为更加具体的子类

（3）将任何一个类型断言为 any

（4）将 any 断言为一个具体的类型

