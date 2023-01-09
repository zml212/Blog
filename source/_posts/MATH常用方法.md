# MATH常用方法

我们知道在js中有很多的内置对象，使用这些对象可以极大的提高我们的工作效率。

这里我们讲讲在实际开发中用的比较多的Math对象。

## Math对象

`math`对象与其他的对象不同，他不是一个构造函数，不需要创建对象，所以我们就不需要使用`new`关键字来创建对象，我们可以直接使用里面的属性和方法。

下面列出一些常用的属性方法：
|     方法      |                    描述                    |     备注     |
| :-----------: | :----------------------------------------: | :----------: |
|    Math,PI    |                   圆周率                   | Math对象属性 |
|  Math.abs()   |                 返回绝对值                 |              |
| Math.random() |          生成0-1之间的随机浮点数           |    [0,1)     |
| Math.floor()  |                  向下取整                  |              |
|  Math.ceil()  |                  向上取整                  |              |
| Math.round()  | 四舍五入（正数：四舍五入；负数：五舍六入） |              |
|  Math.max()   |             返回多个数中最大值             |              |
|  Math.min()   |             返回多个数中最小值             |              |
| Math.pow(x,y) |                返回x的y次幂                |              |
|  Math.sqrt()  |                  开方运算                  |              |

下面我们进行详细介绍：

## Math.PI

    var pi = Math.PI;
    console.log(pi);// 3.141592653589793

这个属性直接返回圆周率，我们可以直接使用。

## Math.abs()

    var num = -10;
    console.log(Math.abs(num));// 10

这个属性传入一个参数，并且返回这个参数的绝对值。

## Math.random()

    var num = Math.random();

这个属性返回一个0-1之间的随机浮点数，不包括1。

*生成[0,x)之间的随机数：*

    Math.round(Math.random()*x);

*生成[x,y)之间的随机数：*

    Math.random()*(y-x)+x;

**重要应用：**

生成指定范围[x,y]内的随机整数：

    function getRandom(min,max){
        return Math.floor(Math.random()*(max-min+1)+min);
    }

    console.log(getRandom(1,10));// 1-10之间的随机整数

## Math.floor & Math.ceil()

    let value1 = Math.floor(x);//向下取整
    let value2 = Math.ceil(x);// 向上取整

当我们需要对某个数值取整的时候，我们可以通过这两个方法来决定是向上还是向下取整。

    let down = Math.floor(3.5);
    let up = Math.ceil(3.5);
    console.log(down);// 3
    console.log(up);// 4

## Math.max() & Math.min()

在实际开发中，我们有时候需要获取多个值之间的最大值或者最小值。

如果没有这个方法，我们需要一个一个去比较，有个这个方法之后，我们直接调用这个方法，传入我们的参数就可以直接返回我们想要的值。

    Math.max(x,y,z...);// 返回最大值
    Math.min(x,y,z...);// 返回最小值

**注意：**

这里传入的参数只能是数值形式，若是其他形式，比如数组之类的，我们要先将其转换。

## pow()

    Math.pow(a,b);

最后返回的结果是`a的b次方`

*栗子：*

    let a = Math.pow(2,3);
    console.log(a);// 8

## sqrt()

    Math.sqrt(a);

返回的结果为a的开二次方。

*栗子：*

    var a = Math.sqrt(4);
    console.log(a);// 2