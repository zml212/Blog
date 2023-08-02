---
title: TypeScript笔记
date: 2023-8-2
tags: 前端
categories: [TypeScript]
---

# TypeScript 

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
    export interface Person {
        name: string,
        readonly age: number,
    }

    export interface Student extends Person {
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

