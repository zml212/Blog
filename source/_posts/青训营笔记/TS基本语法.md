---
title: TS基本语法
date: 2023-1-29
tags: [前端]
categories: [青训营]
---

# TS基本语法

## 基础数据类型

给一个变量定义类型的时候，我们经常在变量名的后面加上一个":",再加上类型的一个名称。

例如：

```ts
let str:string = 'string';
let num:number = 12;
let isTrue:boolean = true;
let n:null = null;
let un:undefined = undefined;
```

## 对象数据类型

在对象数据类型里面，ts可以定义一个接口，然后定义一个对象来实现这个接口。在接口里面，我们可以定义对象的一些属性，以及属性的类型。并且可以对一些属性进行限制，比如只能读，或者只能写。

```ts
interface test{
    // 只读属性
    readonly name:string;
    // 一般属性
    age:number;
    sex:'man'|'woman';
    // 可选属性
    hobby?:string;
    // 任意属性
    [key:string]:any;
}
```

在上面的代码中，我们写了一个接口，里面我们设置了一些属性：

- name是一个只读属性，只能在创建的时候赋值，之后不能修改。（readonly修饰）
- age,sex是一般属性，可以在创建的时候赋值，也可以在之后修改。
  - 其中sex我们在接口时就规定了它的值只有那几个，如果赋值为其他的，就会报错。
- hobby:是一个可选属性，可以在创建的时候赋值，也可以在之后修改，但是可以不赋值。（可选属性就是在冒号前面加上一个?）
- 任意属性：我们可以在接口中定义一个任意属性，这个属性的值可以是任意类型，但是我们必须要定义一个任意属性，这样才能保证我们的接口可以有任意的属性。
  - 其中key代表对象的属性名，string代表属性名的类型，any代表属性值的类型（这里的any代表任意类型）。

**注意：**

当我们定义了任意属性的时候，那么其余属性的值的类型必须是任意属性类型的子类型。否则就会报错。

```ts
interface test{
    // 一般属性
    age:number;
    // 任意属性
    [key:string]:string;
}

const user: test={
    age:18,
    address:'重庆',
}
// Property 'age' of type 'number' is not assignable to 'string' index type 'string'.

```
这里因为我们定义了任意属性的值的类型是string，而age的值是number，所以就会报错。

## 函数数据类型

在ts里面我们给函数定义类型的时候，我们可以有两种方式：

1. 在函数定义的时候就定义类型
2. 在函数的参数里面定义类型

就像这样：

这是一个普通的函数：

```js
function add(x,y){
    return x+y;
}
```
这是我们js中定一个函数，如果我们想要给函数定义类型，我们可以这样：

- 第一种方法，在函数定义的时候，就定义类型

```ts
interface Iadd{
    (x:number,y:number):number;
}
const add:Iadd = (x,y) => x=y;
```

- 第二种方法，在函数的参数里面定义类型

```ts
const add = (x:number,y:number):number => x=y;
```