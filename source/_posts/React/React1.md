---
 title: React1
data: 2023-8-3
tags: [前端]
categories: [React]
---

# React1

[TOC]

## 1.三个API

- React.createElement(): 创建React元素

  > React.createElement(type,[props],[...children])
  >
  > > 第一个参数：元素的名称（小写：div）
  > >
  > > 第二个参数：元素的属性（{id:'btn'}）,使用对象形式
  > >
  > > ​	在设置事件的时候，属性名需要修改为驼峰命名法，并且在后面不能写方法，只能写函数名，React帮你调方法。
  > >
  > > ​	class需求使用className来代替。
  > >
  > > 第三个参数：元素内容
  >
  > 用来创建React元素
  > React元素无法更改，使用新创建的元素进行替换。

- ReactDOM.createRoot()：获取React根元素

	> createRoot(container[,options])
	> 用来 创建React根容器，放置React元素。

- root.render()

	> root.render(element)
	> 当首次调用时，容器节点的所有DOM元素都会被替换，后续调用就会进行比较，只修改有变化的DOM。
	> 不会修改容器节点，只会修改容器子节点。可以在不覆盖现有子节点情况下，将组件插入已有的DOM节点。
	>
	> 当后续调用render渲染页面的时候，React会比较两次渲染的元素，只有在真是DOM发生变化的才会更新，没有变化的保持不变。

例子：

```html
<div id="root"><div>
```

```js
// 创建React 元素
const btn = React.createElement("button",{},"点击")；

// 获取根元素
const root = ReactDOM.createRoot(document.getElementById("root"));

// 将新建的React元素放到根元素里面去
root.render(btn);
```

## 2. JSX

> 在React中可以通过JSX来创建React元素。

JSX需要被React翻译为JS，才能被React执行。（使用Babel来翻译）

JSX其实就是React.createElement()的语法糖。

### 2.1 JSX注意事项

1. 不要加引号，错误：“<div>12</div>>”

2. 有且只有一个根标签，必须有一个根标签。

3. html标签小写开头，React组件标签大写开。

4. 可以使用`{}`插入js表达式。

5. 属性正常写（class --> className;style --> {};事件名大驼峰）。

6. 标签必须闭合。

7. {}中的布尔类型，null，undefined会被忽略。

### 2.2 在JSX中嵌入变量

- 情况一：当变量都是Number、String、Array类型的时候，可以直接显示
- 情况二：当变量是null、undefined、Boolean的时候，内容为空
    - 如果希望可以展示null、undefined、Boolean，需要转换为字符串
    - 转换的方式：toString()、和空字符串拼接、String(变量)
- 情况三：对象类型不能作为子元素

### 2.3 在JSX中嵌入表达式

1. 嵌入运算表达式

```jsx
<h1>{a + b}</h1>
```

2. 嵌入三元表达式

```jsx
<h1>{isLogin?"登陆":"未登录"}</h1>
```

3. 嵌入函数调用

```jsx
<h1>{this.getName()}</h1>
```

### 2.4 JSX中绑定属性

- 绑定普通属性

我们使用大括号的形式给JSX标签绑定属性：

```jsx
<h1 name={变量名}></h1>
```

- 绑定样式

方式一：使用className

当class类名固定不变时，可以这样操作

```jsx
<h1 className="title"></h1>
```

当class类名根据情况hi做出调整的时候，这样操作：

根据调整的变量：

```javascript
let flag = false;
```

绑定class:

```jsx
<h1 className={"固定不变的class" + {flag ? :"test" : ""}}></h1>
```

方式二：绑定style

```jsx
<h1 style={{color:"red",fontSize:"20px"}}></h1>
```

### 2.5 JSX中列表渲染

> 使用map

```JSX
    const arr = [
        { name: '凯瑟琳·约翰逊: 数学家', key: 1 },
        { name: '马里奥·莫利纳: 化学家', key: 2 },
        { name: '穆罕默德·阿卜杜勒·萨拉姆: 物理学家', key: 3 },
        { name: '珀西·莱温·朱利亚: 化学家', key: 4 },
        { name: '苏布拉马尼扬·钱德拉塞卡: 天体物理学家', key: 5 },
    ];
    
<ol>
  {arr.map((item) => {
    return <li key={item.key}>{item.name}</li>
  })}
</ol>
```

![image-20230803143441470](https://raw.githubusercontent.com/zml212/FigureBed/main/202308031434526.png)

### 2.6 React JSX中绑定事件

- 普通绑定

```jsx
<h1 onClick={this.handleClick}></h1>
```

如果在事件处理函数中，需要拿到类实例上的state，此时我们需要将这个事件处理函数写成箭头函数，这样此时箭头函数中的this就会指向类的实力，这样就可以拿到state了。

还有一种不太方便的方案，那就是在标签绑定事件的时候，使用bind手动绑定this：
```jsx
<h1 onClick={this.handleClick.bind(this)}></h1>
```

- 事件绑定之后传递参数

```jsx
<h1 onClikk={(e) => {this.handleClick(参数一,参数二,e)}}></h1>
```

### 2.7 React中的条件渲染

在某些情况下，会根据不同的情况，判断某个组件显示不同的内容，或者决定是否渲染某部分内容。

在React中，所有的判断都和JavaScript都是一样的。

- 方式一：条件判断语句：适合逻辑判断较多的代码

```jsx
let isLogin = false;
let title = null;
if(isLogin){
    title = <h2>欢迎回来</h2>
}else{
    title = <h2>请先登录</h2>
}
```

```jsx
<div>
    {title}
    <button>{this.state.isLogin?"登录":"退出"}</button>
</div>
```

上面的这种写法其实类似于Vue中的v-if，如果要实现vue中的v-show，我们可以通过动态设置display的值为none来实现：

```javascript
let flag = false;
let isShow = flag?"block":"none";
```

```jsx
<h1 style={{display:isShow}}>React实现v-show</h1>
```

### 2.8 JSX的本质

```jsx
let message = <h1 name="test">这是一个JSX</h1>;
```

其实上面这段代码是React的一个语法糖，原本的写法其实是这样的：

```jsx
let message = React.createElement("h1",{name:"test"},"这是一个JSX");
```

所有的JSX最终都会被转换为React.createElement的函数调用。

createElement这个函数需要传入三个参数：

- 参数一：type
    - 当前ReactElement的类型
    - 如果是标签元素，那么就使用字符串表示："div"、"span"...
    - 如果是组件元素，那么就直接使用组件的名称：App、Test...
- 参数二：config
    - 所有JSX中的属性，都在config中以对象的属性和值的形式存储
- 参数三：children
    - 存在放标签中的内容，以children数组的方式存储
    - 如果是多个元素，React内部有相应处理

## 3.虚拟DOM

在React中操作的元素叫做React元素，并不是真正的原生DOM。

也就是通过React.createElement创建出来的元素，其实是一个ReactElement对象。

这个ReactElement对象组成了一个JavaScript对象树，这就是虚拟DOM。

React通过虚拟DOM将React元素与真实DOM进行映射。

好处：

> 1. 降低API复杂度
> 2. 解决兼容问题
> 3. 提升性能（减少不必要的DOM操作）

React使用diffing算法，将新旧DOM进行比较，替换更改的DOM，保留不变的DOM。

比较时，React会先比较父元素，如果父元素发生了改变，就直接替换该元素，不再比较子元素。如果父元素一致，则往下比较，直到发现不一样的节点。

## 4. 组件化开发

在React中，根据不同的标准，可以分为一下几类：

- 根据组件定义方式：函数式组件和类式组件
- 根据组件内部是否有状态维护：有状态组件和无状态组件
- 根据组件不同的职责：展示型组件和容器型组件

其中函数式组件、无状态组件、展示型组件主要关注UI界面展示；类式组件、有状态组件、容器性组件主要关注数据逻辑。

在React还有一些其他组件，比如异步组件、高阶组件。

### 4.1 React脚手架 

- yarn与npm命令对照表

![image-20231130151249204](https://raw.githubusercontent.com/zml212/FigureBed/main/202311301512364.png)

-  创建React项目

安装create-react-app这个脚手架，使用npm进行安装。

然后在项目位置的终端中打开，输入命令：

`create-react-app 项目名称`

这个项目名称中，不能包含大写字母。 

### 4.2 函数式组件

> 适用于简单组件

```jsx
export default function Hello() {
    return (
        <>
            <div>Hello React!</div>
        </>
    )
}
```

在父组件引入挂载。

> 函数式组件，在没有hooks之前：
>
> 1. 没有生命周期，但是会被更新并挂载
> 2. 没有this(组件实例)
> 3. 没有内部状态

### 4.3 类式组件

实现类式组件有以下要求：

1. 组件的名称必须要大写
2. 类组件需要继承React.Component
3. 类组件必须实现render函数

使用class定义组件的时候：

- constructor是可选的，我们通常在constructor中初始化一些数据，比如父组件传递过来的props，还有组件内部的state
- this.state中维护的就是组件内部的数据
- render()方法是class组件中，唯一必须实现的方法

```jsx
import React from 'react'

export default class ClassCom extends React.Component {
    render() {
        return (<>
            <div>类组件</div>
        </>)
    }
}
```

其中render函数位于类实例上。

当这个类组件被root.render解析的时候,发现这个组件是一个类组件，所以就帮你创建了一个当前组件的实例，随后调用该组件上的render方法。

最后将root.render返回的虚拟DOM转为真实DOM，展现到网页上。

###  补充

render函数可以：

- 返回React元素：

> 通过JSX创建 

- 数组或者fragments、

> 使得render返回多个元素

- Portals：

> 渲染子节点到不同的DOM子树中 

- 字符串或者数值类型：

> 在DOM中被渲染为文本节点

- 布尔类型或者null：

> 什么都不渲染

## 5. React中的生命周期

![image-20230803165508423](https://raw.githubusercontent.com/zml212/FigureBed/main/202308031655513.png)

### 5.1 生命周期解析

![image-20230803165803615](https://raw.githubusercontent.com/zml212/FigureBed/main/202308031658674.png)

- Mounting：挂载阶段
- Updating：更新阶段
- Unmounting：卸载阶段

```jsx
import React from 'react'

export default class ClassCom extends React.Component {
    constructor() {
        super();
        console.log("执行组件constructor");
    }
    render() {
        console.log("执行组件的render函数");
        return (<>
            <div>类组件</div>
        </>)
    }
    componentDidMount() {
        console.log("执行组件的componentDidMount");
    }
}
```

![image-20230803170826951](https://raw.githubusercontent.com/zml212/FigureBed/main/202308031708984.png)

在组件更新的时候，会调用componentDidUpdate生命周期函数；当组件卸载的时候，会调用componentWillUnmount方法。 

1. Constructor：

    ​	通过this.state赋值对象来初始化内部的state

    ​	为事件绑定this。

2. componentDidMount：

    ​	依赖DOM的操作在这里执行

    ​	在此处发送网络请求

    ​	添加订阅

3. componentDidUpdate:

    ​	当组件更新后，在此处对DOM进行操作

    ​	如果更新前后props进行了比较，可以在此处网络请求

4. componentWillUnmount：

    ​	在此方法中执行必要清理操作

    ​	比如：timer,网络请求，创建的订阅

## 6. 组件间的通信

### 6.1 父子组件之间的通信

#### 6.1.1 父传子

> 使用props

- 类式组件

父组件：

```jsx
import React from 'react'
import ClassSon from './ClassSon';

export default class ClassCom extends React.Component {
    constructor() {
        super();
        this.state = {
            age: 12.
        }
    }
    render() {
        return (<>
            <div>类组件</div>
            <ClassSon age={this.state.age}></ClassSon>
        </>)
    }
}
```

子组件：

```jsx
import React from "react";

export default class ClassSon extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
        const { age } = this.props
        return (<>
            <div>我是类式组件的子组件</div>
            <div>{age}</div>
        </>)
    }
}
```

结果：

![image-20230804090700448](https://raw.githubusercontent.com/zml212/FigureBed/main/202308040907506.png)

- 函数式组件

父组件

```jsx
import FunSon from "./FunSon"

export default function FunF() {
    return (<>
        <div>函数式组件</div>
        <FunSon name={"hmbb"}></FunSon>
    </>)
}
```

子组件

```jsx
export default function FunSon(props) {
    const { name } = props;
    return (<>
        <div>子组件{name}</div>
    </>)
}
```

结果：

![image-20230804093205321](https://raw.githubusercontent.com/zml212/FigureBed/main/202308040932368.png)

- 对传递过来的属性进行验证：

当传过来的数据类型与期望类型不一致的时候，就会报错，这个时候就需要对传递的数据类型进行验证。

> 使用PropTypes进行验证。

> 从React15.5开始，这个功能被移入了prop-types库。(使用时需要单独引入)

```jsx
import PropTypes from "prop-types"

export default function FunSon(props) {
    const { name } = props;
    return (<>
        <div>子组件{name}</div>
    </>)
}

FunSon.PropTypes = {
    name: PropTypes.string.isRequired, // 表示必传且数据类型为字符串
}
```

我们还可以设置属性的默认值，当父组件没有传递任何值的时候，子组件就会使用默认值进行渲染。

```jsx
FunSon.defaultProps = {
	name:'pdx'
}
```

上面的属性验证写法，类组件同样使用，使用组件名`.`方法。

在类组件中还有另外一种写法：

```jsx
class Class extends React.Component{
	static propTypes = {
	
	}
    static defaultProps = {
        
    }
}
```

####  6.1.2 子传父

> 在React中通过Props传递消息，父组件给子组件传递一个回调函数，子组件调用。

父组件：

```jsx
import React from "react";
import ClassSon from "./ClassSon";

export default class ClassF extends React.Component {
    constructor(props) {
        super(props);

        this.state = {
            count: 0
        }
    }

    render() {
        return (<>
            <div>当前计数{this.state.count}</div>
            <ClassSon click={this.add.bind(this)}></ClassSon>
        </>)
    }

    add() {
        console.log(this.state);
        this.setState({
            count: this.state.count + 1
        })
    }
}
```

子组件

```jsx
import React from "react";

export default class ClassSon extends React.Component {
    constructor(props) {
        super(props);
    }

    render() {
        const { click } = this.props
        return (<>
            <button onClick={click}>+1</button>
        </>)
    }
}
```

父组件定义方法，并且通过props的方法将事件传递给子组件，子组件再进行调用。

### 6.2 如何在React中实现Vue中的slot

- 方式一

使用props.children给子组件传递组件数据。

父组件：

```jsx
export default class Slot extends React.Component {
    constructor(props) {
        super(props);
    }
    render() {
        return (<>
            <NavBar>
                <div>1212</div>
                <div>center</div>
                <div>right</div>
            </NavBar>
        </>)
    }
}
```

使用类式组件，将子组件引入，并且在子组件内部写上要传递的数据，这里有三个部分，到时候props.children中也会有三个数据。

子组件：

```jsx
export default class NavBar extends React.Component {
    constructor(props) {
        super(props)
        console.log(this.props);
    }
    render() {
        return (<>
            <div className="nav-bar">
                <div className="nav-left">
                    {this.props.children[0]}
                </div>
                <div className="nav-center">
                    {this.props.children[1]}
                </div>
                <div className="nav-right">
                    {this.props.children[2]}
                </div>
            </div>
        </>)
    }
}
```

控制台输出子组件中的props为：

![image-20230807104525285](https://raw.githubusercontent.com/zml212/FigureBed/main/202308071045373.png)

可以看到确实children中有我们组件的数据。

- 方式二

将父组件中子组件内部的数据通过props传递：

![image-20230807105633832](https://raw.githubusercontent.com/zml212/FigureBed/main/202308071056895.png)

在子组件中使用对象解构，将对应的插槽给解构出来：

![image-20230807105739216](https://raw.githubusercontent.com/zml212/FigureBed/main/202308071057319.png)

实现的效果是一样的。

### 6.3 跨组件通信

#### 6.3.1 使用props进行祖先组件通信

```jsx
import React from 'react';
// 父组件
export default class TxProps extends React.Component {
    constructor(props) {
        super(props)
        this.state = {
            name: "海绵宝宝",
            age: 12,
        }
    }
    render() {
        return (<>
            <Son1 name={this.state.name} age={this.state.age}></Son1>
        </>)
    }
}
// 子组件
class Son1 extends React.Component {
    constructor(props) {
        super(props);
        console.log("son1:", this.props);
    }
    render() {
        const { name, age } = this.props;
        return (<>
            <Son2 name={name} age={age}></Son2>
        </>)
    }
}
// 孙组件
class Son2 extends React.Component {
    constructor(props) {
        super(props);
        console.log("son2:", this.props);
    }
    render() {
        const { name, age } = this.props
        return (<>
            <div>name:{name}</div>
            <div>age:{age}</div>
        </>)
    }
}
```

使用props逐层传递。

 这种方法对于中间层（Son1）它根本不需要这些数据，这样就会造成性能的浪费以及后期维护成本的增加。

所以第二种方法：

#### 6.3.2 Context实现非父子组件数据通信

> Context相关API

- 创建Context对象

React.createContext()

​	创建一个需要共享的Context对象，如果某个组件订阅了Context，那么该组件就会寻找离自身最近的那个Provider并且读取到当前context的值。

​	defaultValue是组件在顶层查找中没有找到对应的Provider，那么就使用默认值。

```jsx
const MyContext = React.createContext(defaultValue);
```

- Context.Provider（组件）

​	每个Context对象都会返回Provider 组件，这个组件允许消费组件（使用该数据的组件）订阅Context组件的变化。

​	Provider接受一个value属性，并且将这个值传递给消费组件

​	一个Provider组件允许被多个消费组件使用

​	Provider组件可以被嵌套使用，里层的数据会覆盖外层的数据

​	当Provider中value发生变化，所以依赖该Provider组件的消费组件都会被重新渲染（更新）。

```jsx
<MyContext.Provider value={"value"}/>
```

- Class.contextType

​	挂载在class上的contextType属性会被重新赋值为一个由React.createContext()创建的Context对象。

​	在该类组件可以使用this.context来消费最近的Context那个值

​	可以在任何声明周期内方法，包括render函数

```jsx
MyClass.contextType = MyContext;
```

- Context.Consumer

​	React订阅组件的变更

​	将函数作为子元素

​	该函数接受当前context的值，返回react节点

```jsx
import React from 'react';

// 创建Context对象,并且设置默认值
const UserContext = React.createContext({
    name: "defaultName",
    age: "undefaultAge"
})

export default class ContextProps extends React.Component {
    constructor(props) {
        super(props)
        this.state = {
            name: "海绵宝宝",
            age: 12,
        }
    }
    render() {
        return (<>
            {/* 使用Context.Provider */}
            <UserContext.Provider value={this.state}>
                <Son1></Son1>
            </UserContext.Provider>
        </>)
    }
}

class Son1 extends React.Component {
    constructor(props) {
        super(props);
    }
    render() {
        return (<>
            <Son2></Son2>
        </>)
    }
}


class Son2 extends React.Component {
    constructor(props) {
        super(props);
    }
    render() {
        console.log(this.context);
        return (<UserContext.Consumer>
            {
                value => {
                    return (<>
                        <div>name:{value.name}</div>
                        <div>age:{value.age}</div>
                    </>)
                }
            }
        </UserContext.Consumer>)
    }
}

// 一定要放在组件之后
Son2.contextType = UserContext;
```

## 7. setState

在下面的代码中，我们使用`this.state={}`的方式在类式组件里面定义了数据，如果我们直接修改里面的数据：

```jsx
this.state = {
    name:"12",
}

this.state.name = "海绵宝宝";
```

这样的操作确实把数据给更改了，但是页面不会更新，因为React察觉不到数据是否发生了改变，也就是在开发中，我们并不能通过直接更改state的值来让页面发生更新。这就是我们为什么需要使用setState。

### 7.1 setState异步更新

也就是我们通过setState将数据更改之后，数据并不会立即发生改变，而是等到所有的数据都更新之后，才会将数据传递给页面，页面重新渲染，如果我们在setState之后立即输出该数据，我们会发现数据是没有变化的。

```jsx
import React from 'react';

export default class SetState extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            name: "121212",
        }
    }

    change() {
        this.setState({
            name: "海绵宝宝"
        })
        console.log(this.state.name);
    }

    render() {
        return (<>
            <div>当前数据：{this.state.name}</div>
            <button onClick={e => this.change()}>更改数据</button>
        </>)
    }
}
```

点击之后，页面更新：

![image-20230807150454145](https://raw.githubusercontent.com/zml212/FigureBed/main/202308071504220.png)

但是控制台输出的结果确实更新之前的数据：
![image-20230807150522490](https://raw.githubusercontent.com/zml212/FigureBed/main/202308071505524.png)

React为什么要把setState设计为异步呢？

首先设计为异步可以提升性能：

​	如果每次调用setState都进行一次更新，那么就意味着每次数据改变都会调用render函数，页面重新渲染。这样就会导致性能低下，最好的办法就是将多个更新收集到一起，然后再下次更新的时候进行批量更新。

还有一个问题，如果setState是同步的话，可能会导致state与props不同步：

​	如果更新了state，但是还没有来得及执行完毕render函数，就可能会导致上面的state与props不同步问题。

**下面有两种方式获取更新之后的state**

> 方式一：在setState回调函数中获取

这种方式有点类似于Vue的nextTick。

在setState方法里面有两个参数，`setState(更新的state,回调函数)`

```jsx
 change() {
        this.setState({
            name: "海绵宝宝"
        },() => {
            console.log(this.state.name)
        })
    }
```

此时拿到的数据就是更新之后的数据：

![image-20230807151434215](https://raw.githubusercontent.com/zml212/FigureBed/main/202308071514257.png)

> 方式二：通过生命周期获取componentDidUpdate

在页面重新挂载之后，获取到相应的数据。

```jsx
componentDidUpdate() {
        console.log(this.state.name);
    }
```

同样可以拿到更新之后的数据。

相较于方法一，方法二比方法一更早执行，也就是先执行生命周期，后面再执行setState的回调函数。

**注意：**

setState在以下两种情况时同步的：

1. setTimeout事件中
2. 原生的DOM事件中

### 7.2 数据的合并

```jsx
import React from 'react';

export default class SetState extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            count: 0,
        }
    }

    componentDidUpdate() {
        console.log(this.state);
    }

    change() {
        this.setState({
            name: "海绵宝宝"
        })
    }


    render() {
        return (<>
            <div>当前数据：{this.state.count}</div>
            <button onClick={e => this.change(e)}>给state新增数据</button>
        </>)
    }
}
```

上面代码的操作：

我们使用setState对state进行操作：

1. 如果state里面有这个属性，那么就将这个属性的值给替换掉；
2. 如果state里面没有这个属性，那么就为state新增这个属性；

这个在React源码中，其实是使用的类似Object.assign这个方法：
`Object.assign({},this.state,{setState里面的操作})`

### 7.3 setState本身的合并

```jsx
import React from 'react';

export default class SetState extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            count: 0,
        }
    }

    componentDidUpdate() {
        console.log(this.state);
    }

    change() {
        this.setState({
            count: this.state.count + 1,
        });
        this.setState({
            count: this.state.count + 1,
        });
        this.setState({
            count: this.state.count - 1,
        });
    }


    render() {
        return (<>
            <div>当前数据：{this.state.count}</div>
            <button onClick={e => this.change(e)}>多次加一</button>
        </>)
    }
}
```

上面的代码看似做了三次操作，最后在源代码里面会被合并为只执行最后一次，但是最后页面更新的时候，最终数据只更新了一次（数据减一）。

这就是setState本身合并的机制。

如果我们想要将操作进行累加，那我们就可以在setState传入一个函数：

```jsx
import React from 'react';

export default class SetState extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            count: 0,
        }
    }

    componentDidUpdate() {
        console.log(this.state);
    }

    add = () => {
        this.setState((preState, nextProps) => {
            return { count: preState.count + 1 }
        });
        this.setState((preState, nextProps) => {
            return { count: preState.count + 1 }
        });
        this.setState((preState, nextProps) => {
            return { count: preState.count + 1 }
        });
    }

    render() {
        return (<>
            <div>当前数据：{this.state.count}</div>
            <div>当前名字：{this.state.name + ""}</div>
            <button onClick={e => this.add(e)}>累加操作</button>
        </>)
    }
}


```



## 8.React的更新机制

- React 的渲染流程: JSX → 虚拟 DOM → 真实 DOM
- React 的更新流程: props/state 改变 → render 函数重新执行→产生新的 DOM 树→新旧 DOM 树进行 diff→ 计算出差异进行更新→更新到真实的 DOM 

### 8.1 情况一：

当对比的节点是不同的元素时，比如原本的div节点变为了一个span节点，那么React就会将原来的树给删除掉，并且重新新建一个子树：
```jsx
// 原来的代码
<div>
    <p>
        <span>这是测试文本</span>
    </p>
</div>

// 更改之后的代码
<div>
    <div>
        <span>这是测试文本</span>
    </div>
</div>
```

上面的代码在React中，会将原来的p元素以及子节点的虚拟树卸载掉，重新建立新的树。

### 8.2情况二：

当对比两个相同的类型的React的元素时，React会保留DOM节点，只会对更改的属性进行更新：

```jsx
// 原来的代码
<div className={style}>
	这是测试文本
</div>

// 更改之后的代码
<div className={test}>
	这是测试文本
</div>
```

上面的代码，并不会将整个div元素给替换掉，而只是将发生改变的属性进行更新。

如果是同类型的组件元素：

组件会保持不变，React会更新该组件的props，并且调用componentWillReceiveProps()和componentWillUpdate()方法；然后调用render方法，diff算法将在之前的结果以及新的结果中进行递归。

### 8.3 情况三：

 对子节点进行递归，默认情况下，当递归DOM节点的子元素时，React会同时遍历两个子元素；当产生差异的时候，就会生成一个mutation。产生一个mutation就会触发相应的节点更新。

这个对于列表的倒序操作，就会产生很大的性能问题，所以为什么对于列表循环，我们需要添加上一个唯一的key值原因（使用index作为key对性能没有任何优化）。

## 9. 受控组件与非受控组件

### 9.1 ref的使用

在React中，不推荐直接操作DOM，但是有些情况下，又不可避免的会操作DOM。

所以就诞生了ref，通过ref我们可以获取到DOM对象，下面是几种React中通过Ref获取到DOM对象的方式：

- 方式一：字符串形式

```jsx
<h2 ref="titleRef">测试文本</h2>
```

在标签上面设置了ref属性之后，我们就可以在componentDidMount生命周期中拿到对应的DOM，并进行相关操作了：

```jsx
componentDidMount(){
    this.refs.titleRef.innerHTML = "这是字符串形式";
}
```

- 方式二：对象形式

使用这种方式的时候，我们需要在构造器中，提前创建一个变量：

```jsx
const titleRef = createRef();
```

然后我们就可以在标签上面使用这个变量了：

```jsx
<h1 ref={this.titleRef}>测试文本</h1>
```

然后我们就可以在componentDidMount生命周期中拿到对应的DOM，并进行相关操作了：

```jsx
componentDidMount(){
    this.titleRef.innerHTML = "这是字符串形式";
}
```

官网比较推荐使用第二种方法。并且第一种方法可能会在后面被废除。

- 方式三：函数形式

在构造器中利用一个方法定义相关变量。

```jsx
const titleRef = null;
```

```jsx
<h1 ref={(arg) => {this.titleRef = arg}}></h1>
```

在componentDidMount生命周期中拿到对应的DOM，并进行相关操作:

```jsx
componentDidMount(){
    this.titleRef.innerHTML = "这是字符串形式";
}
```

但是不能在函数式组件上使用ref，因为函数式组件没有实例。

### 9.2 受控组件

在React中，HTML表单的处理方式和普通的DOM元素不太一样，表单元素通常会保存在一些内部State中。

```jsx
import { PureComponent } from "react";

export default class App extends PureComponent {
    constructor(props) {
        super(props);
        this.state = {
            userName: "",
            fruit: "",
        };
    }

    componentDidMount() {
        console.log(this.state.userName);
    }

    handleChange(e) {
        this.setState({
            [e.target.name]: e.target.value
        });
    }

    handleSubmit(e) {
        // 取消默认行为
        e.preventDefault();
        console.log(this.state);
    }
    render() {
        return (
            <div>
                <form onSubmit={e => this.handleSubmit(e)}>
                    <label htmlFor="userName">用户：
                        <input type="text" id="userName" name="userName"
                            value={this.state.userName}
                            onChange={e => this.handleChange(e)} />
                    </label>
                    <select name="fruit" onChange={e => this.handleChange(e)} value={this.state.fruit}>
                        <option value="undefined" disabled selected>请选择</option>
                        <option value="apple">苹果</option>
                        <option value="banana">香蕉</option>
                        <option value="orange">橘子</option>
                    </select>
                    <button>提交</button>
                </form>
            </div>
        )
    }
}
```

### 9.3 非受控组件

在React中，大多数都推荐使用受控组件来处理表单数据，如果我们需要使用非受控组件中的数据，我们可以使用ref来从DOM中获取到表单的数据。

```jsx
import React, { PureComponent } from "react";

export default class App extends PureComponent {
    constructor(props) {
        super(props);
        this.userNameRef = React.createRef();
    }

    handleSubmit(e) {
        // 取消默认行为
        e.preventDefault();
        // 直接操纵DOM，不建议
        // const userName = document.getElementById("userName").value;
        console.log(this.userNameRef.current.value);
    }
    render() {
        return (
            <div>
                <form onSubmit={e => this.handleSubmit(e)}>
                    <label htmlFor="userName">用户：
                        <input type="text" id="userName" name="userName" ref={this.userNameRef} />
                    </label>
                    <button>提交</button>
                </form>
            </div>
        )
    }
}
```

从这里我们就可以看到，受控组件会将里面的值交给State进行管理，但是在非受控组件中，这个值我们是从DOM元素中直接获取的，并没有交给State进行集中管理。 

## 10.高阶函数和高阶组件

### 10.1 高阶函数

满足以下条件之一的都可以被称作为是一个高阶函数：

- 接受一个或者多个函数作为参数输入
- 输出一个函数

在JavaScript一些常见的函数：map,renduce等都是高阶函数。

### 10.2 高阶组件

高阶组件定义：高阶组件的参数是组件，返回值是新的组件的函数。

高阶组件并不是一个组件，而是一个函数，这个函数的参数是一个组件，返回值也是一个组件。

- 高阶组件的基本使用

```jsx
import { PureComponent } from 'react';

class App extends PureComponent {
    constructor(props) {
        super(props);
    }

    render() {
        return (
            <div>
                adsad:{this.props.name}
            </div>
        )
    }
}

function enhanceComponent(WrapperComponent) {
    return class NewApp extends PureComponent {
        constructor(props) {
            super(props);
        }
        render() {
            return (
                <WrapperComponent {...this.props} />
            )
        }
    }
}

const anhancedApp = enhanceComponent(App);

export default anhancedApp;
```

在高阶组建中，不仅仅可以返回类式组件，同样可以返回函数式组件。

### 10.3 高阶组件应用

- 可以劫持props，对props进行批量处理
- 可以劫持组件：可以根据不同情况返回不同的组件，比如可以做登陆鉴权。
- 可以劫持生命周期

## 11.React中的CSS

### 11.1 内联样式

内联样式是官方比较推荐的一种写法：

style接受一个采用小驼峰命名属性的JavaScript对象，而不是CSS字符串

并且可以引用State中的装填来设置相关的样式

```jsx
import { PureComponent } from "react";

export default class App extends PureComponent {
    constructor(props) {
        super(props);
        this.state = {
            color: "red",
        }
    }
    render() {
        const hStyle = {
            color: "red",
            fontSize: "50px",
            animation: "logo-spin 10s linear infinite",
        }
        return (
            <div>
                <h1 style={{ color: "red", fontSize: this.state.color }}>我是标题</h1>
                <h1 style={hStyle}>我是标题</h1>
            </div>
        )
    }
}
```

内联样式，样式之间不会有冲突。

并且可以动态根据State中的数据进行设置样式。

**缺点：**

- 写法上需要使用小驼峰命名
- 某些样式没有提示
- 在style中写上大量样式，代码显得非常混乱，不优雅
- 某些样式无法书写：伪类\伪元素

### 11.2 普通CSS写法

```jsx
import { PureComponent } from "react";
import "./app.css"

export default class App extends PureComponent {
    constructor(props) {
        super(props);
    }

    render() {
        return (
            <div>
                <h2 className="title">测试文本</h2>
            </div>
        )
    }
}
```

```css
.title {
    color: red;
    font-size: 50px;
}
```

但是这样写css会出现一个问题，就是可能会出现样式冲突，它没有Vue中的scoped。

### 11.3 CSS modules

```jsx
import { PureComponent } from "react";

import appStyle from './app.module.css'
import Test from "./test/test"

export default class App extends PureComponent {
    constructor(props) {
        super(props);
    }

    render() {
        return (
            <div>
                <h2 className={appStyle.title}>测试文本</h2>
                <Test></Test>
            </div>
        )
    }
}
```

```css
.title {
    font-size: 50px;
    color: skyblue;
}
```

并且在React中，已经内置了css module的配置，比如scss,less等样式文件都被修改为了`.module.scss`、`.module.less`

CSS Modules解决了局部作用域的问题。

**缺点：**

- 引用的类名不能使用连接符（.home-title），因为这在JavaScript不会被识别
- 所有的className都需要通过`style.className`的方式来进行编写，不是很方便
- 不方便动态的改变某些样式，所以还需要使用内联样式来动态设置样式

### 11.4 CSS in JS

CSS in JS是一种模式，其中CSS由JavaScript生成而不是在外部定义。

÷CSS in JS由第三方库提供。


目前比较流行的CSS in JS的库有：

- styled-components
- emption
- glamorous

通过style-components这个库来实现。

## 12.Ant-Design组件库

 Ant-Design是蚂蚁金服开源的一套React组件库。

在我们使用这个组件库之前，我们需要进行安装：

`npm install antd`注意这里不是ant-design，而是antd。

并且这个组件库的图标是没有继承在Ant-Design这个组件库里面的，所以如果想要使用图标的话，我们需要另外再下载一个库：`ant-design/icon`，使用命令进行安装：`npm insatll @ant-design/icon`。

## 13.在React中使用axios

### 13.1 axios的基本使用

- axios(config) 
- axios.request(config)
- axios.get(url[,config])
- axios.post(url[,config])
- axios.put(url[,data[,config]])
- axios.delete(url[,config])
- axios.head(url[,config])
- Axios.patch(url[,data[,config]])

上面的这些请求，其实本质上都是调用的`axios.request(config)`。

例子：

```javascript
import axios from "axios";

axios({
  url:"请求的地址",
  // 如果是get请求，传递的参数
  parms:{
		data:"test",
  }
  method:"get",// 默认为get请求
}).then(res => {
  // 成功结果处理
}).catch(err => {
  // 失败处理 
})
```

如果想要将这种异步代码，写成同步的形式，我们可以使用async和await的方式来实现。

```javascript
import axios from "axios";

request = async function (){
  let result = "";
  try{
    result = await axios({
      url:"请求地址",
      // 请求方法为post，传递的数据
      data:{
        id:12,
      },
      method:"post",
    })
  }catch(err){
    // 错误处理
  }
  console.log(result);
}
```

### 13.2 axios的配置信息

1. 请求配置信息

 在请求配置信息中，只有url是必须要传的。其他的都是按需配置。

常见的一些配置项：

```javascript
url:"请求地址", // 配置请求地址
method:"请求方法",// 配置请求方法
baseUrl:"", //baseURL会自动加载url的前面，是一个绝对的url
transformRequest:[function(data,headers) {
  return data;
}] // 该配置项主要应用与post，put,patch这几个方法，因为只有这几个方法有data，这个配置项用于在发送请求之前对请求参数做修改
transformResponse:[function (res){
  return data;
}]// 这个方法主要用作对响应的数据进行修改操作
headers:{}, //主要用于设置请求头相关数据
params:{}, // get请求的一些参数
data:{}, //post请求的一些参数
timeout:1000,// 设置请求超时的时间
```

### 13.3 响应结构信息

```javascript
{
  data:{}, //响应的结果
  status:200,// 来自服务器的http代码
  statusText:"OK",// 来自服务器的http状态信息描述
  headers:{},// 服务器响应的请求头
  config:{}, // 请求的配置信息
  request:{}, //请求对象
}
```

### 13.4 全局默认配置

在实际的开发中，可能多个请求中，有一些配置是相同的，比如baseUrl,headers。这个时候我们就可以在全局配置中，将这些配置给配置好。

```javascript
import axios from "axios";

axios.defaults.baseUrl = "https://httpbin.org"; // 全局配置baseUrl
axios.defaults.timeout = 10000; // 全局配置请求超时时间
axios.defaults.headers.common["token"] = "token"; // 配置所有请求携带token
axios.defaults.headers.post["token"] = "token"; // 配置post请求携带token
```

### 13.5 创建axios新的实例

我们可以使用`axios.create()`来创建多个实例。

并且我们可以在创建实例的时候，进行一些配置初始化。

```javascript
let request = axios.create({
  baseUrl:"XXX",
  timeout:10000,
  headers:{
    XX:XX,
  }
});
```

在上面我们在很多地方都进行了配置，那么他们的优先级是怎么样的呢？

- 一级： 请求中的config配置
- 二级：实例中的defaults配置 
- 三级：创建实例时的配置

### 13.6 axios拦截器

在axios中，我们可以对网络请求进行拦截：发送请求前可以拦截，得到响应体之后可以拦截。

- 请求拦截

```javascript
import axios from "axios";

// 传入两个回调函数 第一个回调函数，传入目前发送网络请求的配置信息；第二个回调函数，发送网络请求失败的函数
axios.interceptors.request.use(config => {
  // 进行操作（可以对配置信息进行操作），操作完毕之后，记得将最后的config返回
  return config;
},err => {
  // 这里可以做一些网络请求发送失败的操作(很少)
})
```

- 响应拦截

```javascript
import axios from "axios";

// 这个函数一般传入两个回调函数，第一个回调函数，传入网络请求的结果，第二个回调函数，当网络请求发生错误的时候，或者服务器返回错误码
axios.interceptors.response.use(res => {
  // 可以在这里对响应体进行一些操作
  return res.data;
},err => {
  if(err && err.response){
    switch(err.response.status){
      case 404:
        console.log("Not Found!");
        break;
      case 401:
        console.log("未授权！");
        break;
      default:
        console.log("请求错误");
        break;
    } 
  }
  return err;
})
```

### 13.7 二次封装

在真实的开发中，一般都会对这种网络请求的库，进行二次封装。

在一个单独的文件(request.js)中，进行封装：

```javascript
import axios from "axios";

let instance = axios.create({
  baseUrl:"https://httpbin.org";
  timeout:10000;
});

// 请求拦截
instance.interceptors.request.use(config => {
  // 进行操作（可以对配置信息进行操作），操作完毕之后，记得将最后的config返回
  return config;
},err => {
  // 这里可以做一些网络请求发送失败的操作(很少)
});

// 响应拦截
instance.interceptors.response.use(res => {
  // 可以在这里对响应体进行一些操作
  return res.data;
},err => {
  if(err && err.response){
    switch(err.response.status){
      case 404:
        console.log("Not Found!");
        break;
      case 401:
        console.log("未授权！");
        break;
      default:
        console.log("请求错误");
        break;
    } 
  }
  return err;
})；

// 向外导出这个axios实例
export default instance;
```

 ## 14. React-transition-group

在我们开发中，想要给某个组件添加显示和消失的过渡动画，可以增加更好的用户体验。

在React中，为我们提供了一个库来实现这些动画：React-Transition-Group

这个库可以帮助我们方便的实现组件的入场和离场的动画，使用时需要进行额外的安装。

```shell
npm install react-transition-group -s
```

### 14.1 React-transition-group主要的四个组件

- Transition（不经常使用）

这个组件是一个与平台无关的组件，它不一定要结合CSS

- CSSTransition

我们通常使用这个组件来实现过渡的动画效果

- SwitchTransition

当两个组件显示与隐藏切换的时候，使用这个组件

- TransitionGroup

将多个组件包裹在这里面，一般用于列表中的元素动画

### 14.2 CSSTransition基本使用

CssTransition在执行的过程中，有三种状态：appear(初次加载)、enter（进入/显示动画）、exit（退出/隐藏动画）

这三种状态再不同时期，有不同的样式;

- 开始时期：-appear、-enter、-exit
- 执行动画时期：-appear-active、-enter-active、-exit-active
- 执行结束：-appear-done、-enter-done、-exit-done

使用：

```jsx
import {PureComponet} from "react";
import {CSSTransition} from "react-transition-group";

export default class CssTransitionDemo extends PureComponent{
  constructor(props){
    super(props);
    
    this.state = {
      isShow:true, 
    }
  }
  render(){
    const {isShow} = this.state;
    return (
    	<div>
        <button onClick={e => this.setState({isShow:!isShow})}>切换</button>
      	<CSSTransition in={isShow} classNames="demo" timeout={300} unmountOnExit={true} appear
          onEnter={el => console.log("开始进入状态") }
          onEntering={el => console.log("正在进入状态") }
          onEntered={el => console.log("进入完成状态") }
          onExit={el => console.log("开始退出状态") }
          onExiting={el => console.log("正在退出状态") }
          onExited={el => console.log("退出完成状态") }
          >
  				<h2>这里放的是需要显示与隐藏的组件</h2>
				</CSSTransition>
      </div>
    )
  }
}
```

CSSTransition中有几个常见属性：

- `in`我们这里通过设置一个布尔值，来控制被包裹组件的显示与隐藏（true:显示，flase:隐藏）。

- `classNames`通过这个属性我们可以为这个组件设置一个别名，方便我们在后续的CSS中，书写有关于和这个组件的一些样式

```css
// 进入之前
.demo-enter,demo-appear{
  opacity:0;
}

// 正在进入
.demo-enter-active,demo-appear{
  opacity:1;
  transition:opacity 300ms;
}

// 进入完成之后
.demo-enter-done,demo-appear-done{
  
}

// 退出动画之前
.demo-exit{
  opacity:1;
}

// 退出动画执行
.demo-exit-active{
  opacity:0;
  transition:opacity 300ms;
}

// 退出动画完成之后
.demo-exit-done{
  
}

// 最后将这个文件在全局文件中引入即可
```

- `timeout`：该属性设置class的添加删除时间

- `unmountOnExit`：这个属性的值是一个布尔值，用来控制动画退出完成之后是否将组件卸载掉（true:卸载,false:不卸载），默认情况为false，也就是不卸载。

- `appear`：设置组件初次加载的动画

CSSTransition中有几个常见声明周期钩子：

这几个生命周期钩子都会传过来一个元素，这个元素就是被CSSTransition包裹的元素。

- `onEnter`：此时钩子在进入动画快要开始执行时执行。
- `onEntering`：此时钩子在进入动画在开始执行的那一瞬间开始执行
- `onEntered`：此时钩子在进入动画结束之后执行
- `onExit`：此时钩子在退出动画快要开始执行时执行。
- `onExiting`：此时钩子在退出动画在开始执行的那一瞬间开始执行
- `onExited`：此时钩子在退出动画结束之后执行

### 14.3 SwitchTransition基本使用

这个组件用于实现两个组件之间的动画切换效果。

SwitchTransition中有一个重要的属性：mode

- 值1：`in-out`：表示组件先进入，旧组件再移除
- 值2：`out-in`：表示旧组件先移除，新组件再进入(默认值)

使用：

```jsx
import {PureComponet} from "react";
import {CSSTransition,SwitchTransition} from "react-transition-group";

export default class SwitchTransitionDemo extends PureComponent{
  constructor(props){
    super(props);
    
    this.state = {
      isOn:true,
    }
  }
  
  render(){
    const {isOn} = this.state;
    return(
    	<div>
      	<SwitchTransition mode="in-out">
        	<CSSTransition key={isOn?"on":"off"} classNames="switch" >
          	<button onClick={e => this.setState({isOn:!isOn,})} timeout={300}>{isOn?"on":"off"}</button>
          </CSSTransition>
        </SwitchTransition>
      </div>
    )
  }
}
```

此时我们在CSSTransition中添加的就不是`in`了，而是一个`key`，我们根据不同情况将其设置为不同的值。

书写CSS样式：

```css
.switch-enter{
	opacity:0;
}

.switch-enter-active{
	opacity:1;
  transition:opacity 300ms;
}

.switch-exit{
	opacity:1;
}

.switch-exit-active{
  opacity:0;
  transition:opacity 300ms;
} 
```

### 14.4 TransitionGroup基本使用

使用：

```jsx
import {PureComponent} from "react";
import {CSSTransition,TransitionGroup} from "react-transition-group";

export default class TransitionGroupDemo extends PureComponet{
  constructor(props){
    super(props);
    
    this.state = {
      names:["hmbb","pdx","xlb"]
    }
  }
  
  render(){
    return (
    	<TransitionGroup>
      	{this.state.names.map((item,index) = {
          return (
          	<CSSTransition  key={index} timeout={500} classNames="item">
            	<div>{item }</div>
            </CSSTransition>
          )
        })}
        <button onClick={e => this.handleClick(e)}>添加数据</button>
      </TransitionGroup>
    )
  }
  
  handleClick = () => {
    this.setState = ({
      names:[...this.state.names,"qwe"],
    })
	}
}
```

设置CSS样式：

```css
.item-enter{
  opacity:0;
}

.item-enter-active{
  opacity:1;
  transition:opacity 500ms;
}
```

## 15. Redux

### 15.1 JavaScript纯函数

只要是函数式编程（JavaScript），就会有这样一个概念：纯函数。

一个函数只要符合下面的情况，那他就是纯函数：

- 对于确定的输入，输出的值是固定的。（比如输入1，输出的值只有一个固定的值2，不会有其他值输出）
- 在这个函数的执行过程中，不会产生影响外部的副作用，比如改变外部变量的值，或者触发外部什么事件。

在React中，我们不论是使用函数还是class来声明组件的时候，都必须要像纯函数一样，保证它们的props不被改变。

### 15.2 为什么需要Redux

在React中，它帮我们解决了DOM填充的问题，也就是页面渲染，但是在开发中的state还是交给开发者自己来维护的。

而Redux就是一个帮助我们管理state的容器，提供了一种可预测的状态管理。

并且Redux不仅可以在React中使用，在其他的页面框架中也可以使用，比如Vue或者jQuery。

### 15.3 Redux的核心理念

1. Store:

对于Redux中存放的数据，我们一般放在Store中

2. action

Redux要求我们在更新数据的时候，需要通过action来更新数据

所有的数据操作，都必须通过派发(dispatch)action来进行更新

action只是一个普通的JavaScript对象，用来描述更新的类型(type)和内容(content)
