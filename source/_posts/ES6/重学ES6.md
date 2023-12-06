# 重学ES6
[TOC]
# 1.作用域和作用域链

## 1.1 作用域
> - 局部作用域
> - 全局作用域
> - 作用域链

作用域就是定义某个变量能够被访问的范围。

###  1.1.1 局部作用域

局部作用域可以分为两类：

1. 函数作用域：

在函数内部声明的变量只能在函数内部进行访问操作（闭包除外），在外部无法直接访问操作。

```javascript
function test(){
    let x = 2;
    x++;
    console.log(x); // 3，可以正常访问并且操作
}
console.log(x); // 报错，x为定义
```

tips:

​	1.函数内部声明的变量，在函数外部不能正常访问。

​	2.函数中的参数也属于函数内部的局部变量

​	3.不同函数内部声明的变量无法互相访问

​	4.函数执行完毕之后，函数内部的变量就会被回收清除

2. 块级作用域:

在JavaScript中使用`{}`包含起来的代码就是一个代码块，一个代码块里面就有一个块级作用域。块级作用域里面的变量，可能在块级作用域外面无法访问。

说可能是因为有这样几个原因：

1. 在ES5之前，只能使用var来定义变量，但是这个声明变量的方式没有作用域这个概念，所以就导致使用var声明的变量，在外部同样也可以进行访问。
2. 闭包：闭包是JavaScript里面的一个特殊的特性。他可以在函数外层访问到函数内部的变量（块级作用域外部文芳块级作用域内部的变量）。

但是在ES6之后，新增了两种声明变量的方式：let和const。

这两种声明变量的方式都可以拥有块级作用域。只不过let声明的是一个变量，而const声明的是一个常量（原始值：不可以改变，引用值：地址不可以改变）。

在ES6之后推荐使用let和const来定义变量。

### 1.1.2 全局作用域

<script>标签和.js文件中的【最外层】就是全局作用域，在这里声明的变量，在整个文件都可以访问到，也就是在其他任何的作用域都可以被访问。

- 在window对象动态添加的属性默认是全局作用域下的变量。
- 在函数中不使用任何关键字定义的变量也是全局作用域下的变量。
- 我们应该尽可能的少使用全局变量，因为这个污染当前的全局变量，造成一些奇奇怪怪的bug。

### 1.1.3 作用域链

作用域链本质就是底层的变量查找机制。

- 在函数被执行的时候，会优先查找当前函数作用域中的变量
- 如果当前作用域没有该变量，那么就逐级向上查找父级作用域，直到全局作用域，如果在全局作用域下还没有找到该变量，编译器就会爆出错误（该变量未定义）。

在作用域链中，自己作用域可以访问父级作用域的变量，但是父级作用域的变量不能访问子级作用域内的变量。

## 1.2 垃圾回收机制

垃圾回收机制（GC）

内存的生命周期：

1. 内存分配：声明一个变量，函数，对象。系统就会自动给他们分配内存。
2. 内存使用：在读写内存的时候（使用变量，函数，对象）的时候
3. 内存回收：当这个变量、函数、对象，没有被使用的时候，系统的垃圾回收机制就会自动将不再使用的内存回收掉。

**注意：**

- 全局变量的内存一般不会被回收，除非页面被关闭的时候，这个时候内存就会被回收掉。
- 一般情况下，局部变量的值不使用之后，这个内存就会被回收。

对于局部变量有一个特殊情况：闭包。

正常情况下，局部变量不使用之后，内存就会被回收，但是在闭包中，该变量不会被回收，如果闭包产生的内存过多，就会发生内存泄露，所以要慎用闭包。

内存泄露解释：

内存泄露就是当分配的内存由于某种元婴不能释放或者未释放。

### 1.2.1 垃圾回收算法

- 引用计数法
- 标记清除法

**引用计数法：**
> 判断一个内存是否还在被使用，就看这个内存有没有指向它的引用，如果没有指向这个内存的引用，那么这个内存就会被回收。

首先跟踪记录这个内存被引用的次数，如果被引用，那么记录就加一，如果减少一个引用，那么记录就减一，如果当记录的引用为0的时候，这个内存就会被回收。

但是这样的回收机制有一个弊端，就是当存在相互引用的时候，这两个变量可能永远不会被回收。

**标记清除法：**

标记清除法就是从根对象出发，寻找可以到达的内存，对于那些不能到达的内存，就会被标记为未使用，隔一段时间之后，这个内存就会被回收。 

这个方法就可以会比引用计数法中的重复引用问题。

## 1.3 闭包（Closure）

一个函数对周围的状态的引用捆绑在一起，内层函数中访问到其外层函数的作用域。

闭包 = 内层函数+外层函数的变量

闭包的作用，封闭变量，为外部提供内部变量的操作方式。

```javascript
funtion outer(){
    let i = 1;
    function inner(){
        i++;
        console.log(i);
    }
    return inner;
}

let add = outer();

add(); // 2
```

在这段代码中，只有通过outer返回的函数才可以操作i变量，其他方式都不可以，这样就将变量给封闭起来了，并且提供了对应的操作方法。



但是闭包中的变量可能会引起内存泄露，因为通过add方法可以找到i这个变量，但是一般函数结束之后，内部的内存就会被自动回收，但是闭包不会，因为里面的变量时可达的，所以就不会回收，就有可能引起内存泄露。  

## 1.4 变量提升

变量提升就是在某个变量声明之前，就可以访问这个变量（var声明的变量）

```javascript
console.log(num); // undefined(没有报错)
var num = 10;
```

此时变量声明在输出语句之后，但是它并没有报变量未定义的错误，说明变量提升了。

但是它只提升了声明，并没有提升赋值，也就是说这段代码其实是这样的（将声明语句提升到当前作用域头部）：

```javascript
var num;
console.log(num);
num = 10;
```

**注意：**

- let和const不存在变量提升
- 变量提升出现在相同的作用域中

# 2. 函数进阶

> - 函数提升
> - 函数参数
> - 箭头函数

## 2.1 函数提升

在当前作用域，就函数的声明提升到顶部。

```javascript
fn();
function fn(){
    console.log("函数提升");
}
```

但是函数表达式不会提升，它只会提升变量，而不会提升函数声明：

```javascript
fn(); // 报错
var fn = function(){
    console.log("函数表达式");
}
```

## 2.2 函数参数

> 形参，实参
>
> 默认参数
>
> 动态参数
>
> 剩余参数

- 形参、实参：

定义函数时传入的参数叫做形参，调用函数时传入的参数叫做实参。

- 默认参数：

当使用函数时，没有给形参传入数据的时候，就使用形参的默认参数。

```javascript
function fn(x = 1,y = 2){
    return x+y;
}
```

这个函数的默认参数：x为1，y为2。

- 动态参数

arguments是函数内部内置的伪数组变量，这里面包含了调用函数时传入的所有实参。

往函数里面传入任意个参数，在函数内部通过这个arguments可以对这些参数进行处理。

注意arguments是一个伪数组，没有数组的一些特定方法，所以我们可以通过for循环来获取到传递过来的参数。

- 剩余参数

剩余参数允许我们将一个不定量的参数表示为一个数组。

语法：

`...`是剩余参数的语法符号，它被防止在函数形参的末尾，用于获取多与的实参

使用剩余参数获取的这个变量，是一个真数组，也就是可以使用数组的一些特殊方法。

```javascript
function fn(a,b,...arg){
    console.log(arg);
}
```

上面代码中的arg就是剩余参数。

**拓展：**展开运算符

我们可以使用`...`将一个数组展开。

比如：

```javascript
let arr = [1,2,3,4,5];
console.log(...arr); // 1,2,3,4,5
```

这个语法不会修改原数组。

## 2.3 箭头函数

1. 基本语法：

```javascript
const fn = () => {
    console.log("箭头函数");
}
```

2. 简写形式：

- 当只有一个参数的时候，可以省略小括号：

```javascript
const fn = x => {
    console.log("箭头函数");
}
```

- 当只有一行代码的时候，可以省略大括号：

```javascript
const fn = (x,y) => console.log( x+y);
```

- 当只有一行代码的时候，可以省略return，直接返回表达式：

```javascript
const fn = (x,y) => x+y;
```

- 箭头函数可以直接返回一个对象：

```javascript
const fn = (name) => ({name})
```

3. 箭头函数的参数：

普通函数有arguments参数，用于收集传递过来的参数，但是在箭头函数里面没有arguments参数，取而代之的是**剩余参数**。

4. 箭头函数的this:

在箭头函数内部没有this，他会沿着自己的作用域链，往上查找，知道找到this，调用该this。

# 3.解构赋值

> - 数组解构
> - 对象解构

## 3.1 数组解构

数组解构就是将数组的单元制快速批量的复制给一系列变量的简洁语法。

**基本语法：**

```javascript
const arr = [1,2,3];
const [x,y,z] = arr;
console.log(x,y,z); // 1,2,3
```

使用数组解构赋值的时候，`=`左侧的`[]`内用于声明变量，右侧的数组将被一一赋值给左边的变量。

变量的值，将会按照其位置从数组依次赋值。

下面几种特殊情况：

- 变量多，数组内单元值少

```javascript
const [a,b,c,d] = [1,2,3];
console.log(a,b,c,d); // 1,2,3,undefined
```

- 变量少，数组内单元值多

```javascript
const [a,b,c] = [1,2,3,4];
console.log(a,b,c); // 1,2,3
```

- 变量少，数组内单元值多，使用剩余参数来接受多与单元值

```javascript
const [a,b,...arg] = [1,2,3,4,5];
console.log(a,b,arg); // 1,2,[3,4,5](这是一个真数组)
```

- 防止undefined传递

```javascript
const [a = 0,b = 0] = [1];
console.log(a,b); // 1,0
```

- 按需导入赋值

```javascript
const [a,b,,d] = [1,2,3,4];
console.log(a,b,d); // 1,2,4
```

- 支持多维数组解构

```javascript
const [a,b,[c,d]] = [1,2,[3,4]];
console.log(a,b,c,d); // 1,2,3,4
```

## 3.2 对象解构

对象解构就是将对象属性和方法快速批量的赋值给一系列变量的方法。

**1.基本语法：**

使用对象解构赋值的时候，`=`左侧的`{}`内用于声明变量，右侧对象的属性值将被一一赋值给左边的变量。

对象属性的值江北赋值给与属性名相同的变量

注意解构的变量名不要和外面的变量名冲突，否则会报错

对象中找不到与变量名一致的属性时，变量值为undefined

```javascript
const obj = {
    name:"hmbb",
    age:12,
}
const {name,age} = obj;
console.log(name,age); // hmbb 12
```

下面几种特殊情况：

- 给新的变量名赋值

可以从一个对象中提取遍历并同时修改新的变量名

```javascript
const {name:myName,age:myAge} = {name:"hmbb",age:12};
console.log(myName,myAge); // "hmbb"  12
```

- 数组对象解构

```javascript
const pig = [
    {
        name:"p",
        age:12,
    }
]

const [{name,age}] = pig;
console.log(name,age); // p 12
```

- 多级对象解构

```javascript
const obj = {
    name:'hmbb',
    firends:{
        first:'pdx',
        sencend:'sd',
    },
    age:12,
}
const {name,{first,sencend},age} = obj;
```

**补充：**使用forEach遍历数组

语法：

```javascript
array.forEach((item,index,array) => {
    // 函数体
})
```

其中：

- item:表示当前遍历的数组元素

- index:当前遍历的元素在数组的索引
- array:当前遍历的数组

# 4.深入对象

> 1. 创建对象的三种方式
> 2. 构造函数
> 3. 实例成员&静态成员
> 4. 内置的构造函数

## 4.1 创建对象的三种方式

1. 使用字面量创建对象

```javascript
const obj = {
    name:'hmbb',
}
```

2. 使用new Object创建对象

```javascript
const obj = new Object();
```

3. 使用 构造函数创建对象

请看下面第二点，构造函数。

## 4.2 构造函数

构造函数是一种特殊的函数，主要用来初始化对象。

可以将多个对象中相同的属性，抽离出来，封装到一个函数里面。

```javascript
const Obj（name,age,address）{
    this.name = name;
    this.age = age;
    this.address = address;
}

const hmbb = new Obj('hmbb',12,'比奇堡');
```

**注意：**

构造函数有以下两个约定：

1. 构造函数命名，通常以大写字母开始。
2. 他们只能有`new`操作符来执行。

使用new关键字的行为，就叫做实例化

实例化构造函数的时候，没有参数时可以省略

构造函数内部不需要写return语句，它的返回值就是新创建的对象

并且在构造函数内部写上return，它的返回值也是无效的

### 4.2.1new实例化过程

1. 创建新的对象（在内存中开辟一块空间）
2. 将新建的对象的[[prototype]]属性指向构造函数的原型
3. 构造函数的this指向刚刚创建的这个对象，执行构造函数代码，添加新的属性
4. 返回新的对象

## 4.3 实例成员&静态成员

- 实例成员：

通过构造函数创建的对象成为实例对象，实例对象中的属性和方法称为实例成员（实例属性和实例方法）

在使用构造函数创建对象的时候，传入不同的参数，就可以创建出结构相同，但是值不同的不同对象。

使用相同构造函数创建出的实例对象互不影响。

- 静态成员

构造函数中的属性和方法，就叫做静态成员（静态属性，静态方法）。

静态成员只能由构造函数进行访问：

```javascript
let max = Math.max(1,2,3,4);
```

静态方式中的this指向构造函数。

## 4.4 内置的构造函数

> - Object
> - Array
> - RegExp
> - Date
> - String
> - Number
> - Boolean

当我们使用字面量声明一些变量的时候，JS的底层帮我们实例化了。

```javascript
// 我们自己写的
const str = 'hmbb';
// js底层帮助我们实例化
const str = new String('hmbb');
```

### 4.4.1 Object

几个常用的**静态方法**：

- 获取对象中的键：`Object.keys(obj)`

```javascript
const a = Object.keys({name:'hmbb',age:12,})
console.log(a) // ['name','age']
```

- 获取对象中的值：``Object.values(obj`

```javascript
const a = Object.values({name:'hmbb',age:12,})
console.log(a) // ['hmbb',12]
```

- 获取对象中的键值对：`Object.entries(obj)`

```javascript
const a = Object.entries({name:'hmbb',age:12,})
console.log(a); 
/*
[
    [
        "name",
        "hmbb"
    ],
    [
        "age",
        12
    ]
]
*/
```

- 拷贝一个对象到另一个对象：`Object.assign(obj1,obj2,...)`(将后面对象拷贝到obj1对象)

如果后面对象的属性与目标对象属性相同，则后面对象的属性会覆盖前面对象的属性。

```javascript
const obj1 = {
    name:'hmbb',
    age:12,
};
const obj2 = {
    name:'pdx',
    address:'比奇堡',
};
console.log(Object.assign(obj1,obj2));
/*
{
    "name": "pdx",
    "age": 12,
    "address": "比奇堡"
}
*/
```

### 4.4.2 Array

 几个常用的数组**实例方法**：

- forEach:遍历数组，不返回数组，用于遍历查找数组元素
- filter:过滤数组，返回新的数组，返回的是复合筛选条件的数组元素
- map:迭代数组，返回新的数组，返回经过处理之后的数组元素
- reduce:累计器，返回累计处理的结果，常用于求和

reduce基本语法：

```javascript
arr.reduce((上一次值，当前值){},初始值);
```

```javascript
let arr = [1,2,3,4,5,6,7,8,9];
console.log(arr.reduce((prev,current) => {
    return prev+current;
},0)); // 45
```

当reduce函数没有初始值的时候，那么上一次值就会将数组的第一个值作为初始值，每一次循环就会把返回值作为下一次的循环的上一次值；如果有初始值，那么起始的第一次循环上一次值就是初始值。

**其他的一些常见方法：**

- 实例方法：`join`将数组元素按照指定间隔符拼接为字符串，返回的是拼接之后的字符串。

```javascript
let arr = [12,2323,'asd'];
let str1 = arr.join();
let str2 = arr.join('-');
console.log(str1,str2); // 12,2323,asd 12-2323-asd
```

默认使用`,`拼接。

- 实例方法：`find`查找元素，返回符合测试条件的第一个数组元素，如果没有复合条件的返回undefined
- 实例方法：`every`检测数组所有元素是否都符合测试条件，如果都符合返回`true`,否则返回`false`
- 实例方法：`some`检测数组中是否有符合测试条件的元素，有就返回`true`，都没有就返回`false`
- 实例方法：`concat`合并两个数组，生成新的数组
- 实例方法：`sort`对原数组单元值进行排序
- 实例方法：`splice`删除或者替换原数组单元

```javascript
let arr = [1,2,3];
arr.splice(1,2,7,8);
console.log(arr);  // [1,7,8]
```

第一个参数：开始操作的数组索引（包含该元素）

第二个参数：从操作的元素位置开始要删除的元素个数

第三个参数（可以是多个）：要加入的元素

- 实例方法：`reverse`反转数组
- 实例方法：`findIndex`查找符合测试条件元素的索引值
- 实例方法：`fill`使用指定元素填充数组

```javascript
let arr = [1,1,1,1,1,1];
arr.fill("a",2,4);
console.log(arr); //[1, 1, 'a', 'a', 1, 1]
```

第一个参数：要填充的元素

第二个参数：表示填充开始的位置（包含）

第三个参数：表示填充结束的位置（不包含）

### 4.4.3 String

实例属性：`length`用来获取字符串的长度

**常见实例方法：**

- 实例方法：`split('分隔符')`用来将祖父穿分解为数组

```javascript
let str = 'qw-erty-ui';
let arr = str.split();
let arr2 = str.split('-');
console.log(arr,arr2); // ['qw-erty-ui']    ['qw', 'erty', 'ui']
```

- 实例方法：`substring(需要截取的第一个字符出现的索引[,结束的索引])`，用于字符串截取

```javascript
let str = 'asdasdzxcqwe';
let str1 = str.substring(2,5);
console.log(str1); // das
```

截取范围左闭右开。

- 实例方法：`startsWith(检测字符串[,检测位置开始索引])`检测某字符是否以某字符开头

```javascript
let str = 'qwerasf';
console.log(str.startsWith('q')); // true
console.log(str.startsWith('w',1)); // true
```

- 实例方法：`includes（搜索的字符串[,检测位置开始索引]）`判断一个字符串是否包含在另一个字符串那种，根据情况返回true或者false
- 实例方法：`toUpperCase`将字符串的中字母全部转换为大写
- 实例方法：`toLowerCase`将字符串中的字母全部转换为小写
- 实例方法：`indexOf`检测是否包含某字符
- 实例方法：`endsWith`检测是否以某字符结尾
- 实例方法：`replace`用于蹄花字符串，支持正则匹配
- 实例方法：`match`用于查找字符串，支持正则匹配

# 5.深入面向对象

> 1. 编程思想
> 2. 构造函数
> 3. 原型

## 5.1编程思想

> - 面向过程编程
> - 面向对象编程

### 5.1.1 面向过程编程

面向过程就是分析出解决问题所需要的步骤，然后使用函数将这些步骤一个一个实现，使用的时候一个一个调用。

### 5.1.2面向对象编程

面向对象就是把事情分解为一个一个的对象，然后由对象之间分工合作解决问题。

**面向对象的特性：**

- 封装性
- 继承性
- 多态性

## 5.2 构造函数

封装对象思想在面向对象编程中是比较重要的，JavaScript中封装对象可以使用构造函数来实现。

同样的将变量和函数组合到了一起，并且通过this实现数据的共享，不同的是借助构造函数创建出来的实例对象之间是互不影响的。

**构造函数缺陷：**

当我们多次使用构造函数实例出多个方法的时候，这些方法都是不相等的，也就是说这些方法都是各自开辟了一块内存空间，所以就会造成内存浪费。

## 5.3原型

使用原型就可以解决构造函数的缺陷。

> - 原型
> - constructor属性
> - 对象原型
> - 原型继承
> - 原型链

### 5.3.1 原型

- 构造函数通过原型分配的函数是所有实例对象都可以共享的。
- 在JavaScript中，每一个构造函数都有一个prototype属性，这个属性指向另一个对象，这个被指向的对象称为原型对象。
- 这个原型对象可以挂载含糊，对象实例化不会多次创建原型上的函数，这样可以节约内存。
- 我们可以将不变的，且需要多次使用的方法，直接定义在构造函数的prototype对象上，这样通过该构造函数实例化的对象都可以共享该方法。
- 构造函数和原型对象中的this，都指向实例对象。

### 5.3.2 constructor属性

在每个原型对象中，都有一个constructor属性，该属性指向原型对象的构造函数。

![proto](https://raw.githubusercontent.com/zml212/FigureBed/main/202309082312495.jpeg)

**constructor作用：**

我们可以使用prototype属性在原型上挂载共有的方法，但是只能一个一个挂载：

```javascript
function Obj (){
    
};
Obj.proptotype.dance(){};
Obj.prototype.sing(){};
```

上面的代码只能一个一个添加属性，如果使用对象形式一起添加，就会覆盖之前的原型，这个时候就需要用到constructor了。

```javascript
Obj.proptotype = {
    construoctor:Obj,
    dance:() => {},
    sing:() => {},
}
```

### 5.3.3`__proto__`

在构造函数中，有一个prototype属性指向该构造函数的原型对象（P1），同时这个构造函数又可以实例化一个一个对象(O1)，但是在实例化对象上如何拿到原型对象上的属性或者方法呢?

这个时候，就可以用到`__proto__`属性啦，注意它的前后都是双下划线。

在每个实例化对象身上，都有一个`__proto__`属性指向构造函数的prototype原型对象。

```javascript
function Obj (){};
const o1 = new Obj();
console.log(o1.__proto__ === Obj.prototype);  // true
```

上面返回的结果为true,说明实例化对象身上的`__proto__`与构造函数`prototype`所指向的原型对象是同一个对象。

### 5.3.4原型继承

```javascript
const Obj={
    age:12,
    name:'hmbb',
};
function Per(){};
Per.prototype = Obj;
Per.prototype.constructor = Per;
const p1 = new Per();
console.log(p1);

```

结果：

![image-20230908235240505](https://raw.githubusercontent.com/zml212/FigureBed/main/202309082352554.png) 

### 5.3.5 原型链

基于原型对象的继承使得不同构造函数的原型对象关在在一起，并且这个关联关系是一种链状的结构，我们将这种链状结构称为原型链。

举个例子：

```javascript
// 定义一个构造函数
function Obj (){};
// 实例化一个对象
const o1 = new Obj();

```

现在我们这个o1对象的`__proto__`属性就指向`Obj.prototype.__proto__`,同时`Obj.prototype.__proto__`又指向`Object.prototype.__proto__`,这样就形成了原型链。

原型规则：

1. 只要是对象，里面就有`__proto__`属性，指向该构造函数的原型对象。
2. 只要是原型对象，里面就有constructor，指向该构造函数。

原型链其实就是一个查找规则：

- 当访问一个对象的属性（或者方法）时，首先在该对象身上查找，有没有该定义。
- 如果没有定义（在该对象上没有找到），就查找该对象的原型对象（``该对象.__proto__`，也就是对应构造函数的原型对象）
- 如果还是没有找到，就继续沿着原型链查找，也就是查找原型对象的原型（Object的原型对象）
- 以此类推一直找到Object为止（`Object.prototype.__proto__ = null`）
- `__proto__`对象原型的意义就是为对象成员的查找机制提供一个方法，或者说一条路线
- 我们可以使用`instanceof`运算符用于检测构造函数的prototype属性是否出现在某个实例对象的原型链上。

# 6.高阶技巧

> 1. 深浅拷贝
>
> 2. 异常处理
>
> 3. 处理this
> 4. 性能优化

## 6.1 深浅拷贝

深浅拷贝只针对引用类型。

### 6.1.1 浅拷贝

浅拷贝：拷贝的是地址

**常见方法：**

1.拷贝对象：Object.assgin()/展开运算符{...obj}

2.拷贝数组:Arrar.prototype.concat()或者[...arr]

```javascript
const obj = {
    name:'hmbb',
    age:12,
}
let obj1 = {...obj}
console.log(obj,obj1) 
/*
	{name: 'hmbb', age: 12} {name: 'hmbb', age: 12}
*/
```

```javascript
const obj = {
    name:'hmbb',
    age:12,
}
let obj1 =Object.assign(obj1,obj)
console.log(obj,obj1)  
/*
{name: 'hmbb', age: 12} {name: 'hmbb', age: 12}
*/
```

浅拷贝对于这种简单的对象还可以正常使用，如果对于一些复杂对象就无能为力了。

```javascript
// 复杂对象
const obj = {
    name:'hmbb',
    age:12,
    friends:{
        one:'pdx',
        two:'zyg',
    }
}
```

浅拷贝值拷贝最外面的一层，对于内部的对象，仍然是拷贝的地址，所以对于复杂的对象就不适用了。

### 6.1.2 深拷贝

**常见方法：**

1. 通过递归实现

2. lodash/cloneDeep

3. 通过JSON.stringify()实现

- 使用递归实现

前置知识：函数递归

如果一个函数在其函数体内调用自己，这个函数就是一个递归函数。

递归函数的作用于循环类似，但是递归容易发生堆栈溢出，所以需要给递归函数加上退出条件。

```javascript
function  fn(index){
    index++;
    console.log(index);
    if(index < 10){
        fn(index);
    }else{
     	return;   
    }
}
fn(0);
```

这就是一个递归函数，执行10次之后，就会停止执行。

递归小案例：使用setTimeout每隔一秒输出时间：

```javascript
function logTime(){
    let nowTime = new Date().toLocaleString();
    console.log(nowTime);
    setTimeout(logTime(),10000)
}
logTime();
```

使用递归函数实现深拷贝：

```javascript
let obj = {
    name:'hmbb',
    age:12,
    friends:{
        one:'pdx',
        two:'zyg',
    }
}
// 拷贝函数
function deepCopy(newObj,oldObj){
    for(let i in oldObj){
        if(oldObj[i] instanceof Object){
            newObj[i] = {};
            deepCopy(newObj[i],oldObj[i]);
        }else{
            newObj[i] = oldObj[i];
        }
    }
    return newObj;
}
let obj1 = {};
obj1 = deepCopy(obj1,obj);
console.log(obj1,obj);
```

- lodash/cloneDeep

lodash是一个库，可以实现各种的处理。

- JSON.stringify()

先使用JSON.stringify()将对象转换为一个字符串，然后使用JSON.parse()将这个字符串重新转换为对象。并且这两个对象是独立的。

```javascript
const obj = {
    name:'hmbb',
    age:12,
    friends:{
        one:'pdx',
        two:'zyg',
    },
    hub:[1,2,3,'asd','adasd']
}
const obj1 = JSON.parse(JSON.stringify(obj));
console.log(obj,obj1);
```

## 6.2 异常处理

> 1. throw抛出异常
> 2. try/catch 捕获异常
> 3. debugger 调试代码

### 6.2.1 抛出异常

使用Error对象抛出异常

```javascript
// 创建并抛出一个异常
throw new Error("这是一个异常");
```

![image-20230911143757016](https://raw.githubusercontent.com/zml212/FigureBed/main/202309111437188.png)

throw可以抛出异常信息，程序也会终止执行；throw后面跟着的是错误的提示信息；通过与Error配合使用，可以设置更加详细的错误信息。

### 6.2.2 捕获异常

我们可以使用try/catch/finally来捕获处理信息以及处理错误信息。

- try:执行可能存在错误的代码
- catch:捕获发生的错误（没有发生错误就跳过），并且可以在这里执行一些代码逻辑处理异常
- finally:无论如何都会执行的代码

```javascript
try{
    throw new Error('这是异常');
}catch(err){
    console.log(err.message);
    throw new Error('继续往外抛出异常');
}finally{
    console.log('这是最后的代码');
}
```

### 6.2.3 debugger调试代码

想在代码哪个位置打断点，就在哪里写上debugger.

```javascript
function logTime(){
    let nowTime = new Date().toLocaleString();
    console.log(nowTime);
    setTimeout(logTime(),10000)
}
debugger; // 调试断点
logTime();
```

![image-20230911145348794](https://raw.githubusercontent.com/zml212/FigureBed/main/202309111453843.png)

第一个图标：继续执行代码，跳过该断点

第二个图标：跳过下一个函数调用

第三个图标：进入下一个函数调用

第四个图标：跳出当前函数调用

第五个图标：单步调试，一步一步的执行代码

第六个图标：停用调试

## 6.3 处理this

> 1. this指向
> 2. 改变this指向

### 6.3.1 this指向

- 普通函数this指向

**默认绑定：**

普通函数中的this，谁调用这个函数，这个函数中的this就指向谁。

如果在调用的时候，没有明确说明调用者，那么在非严格模式下this指向window，在严格模式下为undefined。

**隐式绑定：**

如果一个函数在某个上下文中被调用，那么this就绑定在该上下文中。

```javascript
let a = 1;
const obj = {
    a:2,
    fn:function (){
        console.log(this.a);
    }
}
obj.fn(); // 2
```

**显示绑定：**

call、apply、bind，见下文

**new绑定（构造函数）：**

1. 创建一个新对象；
2. 构造函数的 prototype 被赋值给这个新对象的 **proto**；
3. 将新对象赋给当前的 this；
4. 执行构造函数；

**特殊的this指向：**

（1）箭头函数

在箭头函数里面不存在this。

在箭头函数内部会默认绑定外层this，所以在箭头函数内部的this指向与将箭头函数包含的函数this指向是一样的。

箭头函数中的this引用就是最近作用域中的this。

向外层作用域中，一层一层的查找this，直到有this的定义。

（2）数组方法

```javascript
array.forEach(function(currentValue, index, arr), thisValue)
```

数组forEach方法中，前面参数是回调函数，后面的参数就是forEach中this的值。

如果不传入this指向，默认为undefined，所以输出为全局对象。

（3）立即执行函数

立即执行函数是一个匿名函数，通常是直接调用，所以它的this是确定的，默认为全局对象。

（4）setTimeoutyusetInterval

这两个函数都是在全局作用域下实现的，所以无论在哪里执行，他们内部的this都是指向全局作用域，在浏览器内也就是window。

### 6.3.2 改变this指向

- call()
- apply()
- bind()

**call:**

使用call方法调用函数，同时指定被调用函数中的this的值。

语法：

```javascript
fun.call(this的值，fun参数1,fun参数2.....);
```

call的返回值就是函数返回值，因为它就是调用该函数。

**apply:**

apply方法调用函数，同时指定被调用函数中的this的值。

语法：

```javascript
fun.apply(this的值[,fun函数参数]);
```

apply函数的返回值就是被调用函数的返回值。

与call区别：call传入的函数参数是一个一个传入的，但是apply传入函数的参数是以数组形式传入的。

**bind:**

bind方法不会调用函数，但是能够改变函数内部的this指向。

```javascript
fun.bind(this的值，fun参数1,fun参数2.....);
```

返回由指定的this值和初始化参数改造的原函数的拷贝（返回一个新函数）

当我们想要改变一个函数的this指向，但是又不想调用这个函数的时候，就可以使用bind方法。

## 6.4 性能优化

> 1. 防抖
> 2. 节流

### 6.4.1 防抖

防抖：单位时间内，频繁触发事件，只执行最后一次

![image-20230911161422841](https://raw.githubusercontent.com/zml212/FigureBed/main/202309111614919.png)

**使用场景：**

- 搜索框输入，当用户最后一次输入之后，才发送请求
- 手机号，邮箱输入验证

防抖的核心，就是使用定时器。

1. 声明一个定时器变量
2. 当事件触发的时候，判断是否有定时器，如果有定时器就清除之前的定时器
3. 如果没有定时器，就开启一个定时器，并且将其存在一个变量里面
4. 在定时器内部调用触发事件要调用的函数

```javascript
let index = 0;
function clickEvent(){
    index++;
}
function debounce(fn,time){
    // 1.声明定时器变量
    let timer ;
    return function(){
        // 2.判断是否有定时器
        if(timer){
            clearTimeout(timer);
        }else{
            // 3.没有定时器就开启一个定时器，并将其存到一个变量里面
             timer =  setTimeout(() => {
                 // 4.在定时器内部调用触发事件要调用的函数
                 fn();
             },time);
        }
    }
}
box.addEventListener('click',debounce(clickEvent,500))
```

### 6.4.2 节流 throttle

节流就是在单位时间内，频繁触发事件，但是只执行一次。

![image-20230912111445751](https://raw.githubusercontent.com/zml212/FigureBed/main/202309121114259.png)

节流使用场景：

- 高频事件：鼠标移动,页面尺寸缩放，滚动条滚动。

节流的核心就是利用定时器来实现:

1. 声明一个定时器变量
2. 当事件发生时，都判断是否有定时器，如果有定时器就不开启新的定时器。
3. 如果没有定时器，就开启一个定时器，存到变量里面去。

```javascript
let index = 0;
function clickEvent(){
    index++;
}
function debounce(fn,time){
    // 1.声明定时器变量
    let timer ;
    return function(){
        // 2.判断是否有定时器
        if(!timer){
             // 3.没有定时器就开启一个定时器，并将其存到一个变量里面
             timer =  setTimeout(() => {
                 // 4.在定时器内部调用触发事件要调用的函数
                 fn();
                 timer = null;
             },time);
            
        }else{
            
        }
    }
}
box.addEventListener('click',debounce(clickEvent,500))
```

