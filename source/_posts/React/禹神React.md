# React

[TOC]

# 第一章：React入门

## 1. 创建虚拟DOM的两种方式

- 使用JSX创建

```react
cosnt VDOM = <h1>hello React</h1>;
ReactDOM.render(VDOM,document.getElementById('test'));
```

- 使用js创建虚拟DOM(一般不用)

```react
const VDOM = React.createElement('h1',{id:'content'},'hello React')；
ReactDOM.render(VDOM,document.getElementById('test'));
```

使用纯js创建虚拟DOM的时候，会有一个问题，如果在创建的虚拟DOM时，内部有嵌套的标签，直接在标签内容部分写上标签，最后会发现，这个标签没有被当做HTML解析，而是被当做了一个普通的字符串解析。

## 2. 虚拟DOM和真实DOM

虚拟DOM：

1. 虚拟DOM本质是一个对象（Object）
2. 虚拟DOM身上的属性比较少，真实DOM身上的属性比较多。这是因为虚拟DOM是在React中使用，所以不需要很多属性。
3. 虚拟DOM最终会被React转换为真实DOM呈现在视图上。

## 3. JSX语法规则

JSX全称为JavaScript XML，也是就是JavaScript的一种扩展语法。 

1. 在定义虚拟DOM时，不要写引号。
2. 标签中混入JS表达式的时候，我们使用花括号`{}`括起来。
3. 在给标签设置类名的时候，我们不使用class,而是使用className。
4. 在设置标签内联样式的时候，我们需要使用双花括号`{{}}`，并且变量名为小驼峰，值为字符串。
5. 在JSX中，只能有一个根标签。
6. 标签必须闭合，如果是双标签，必须写双标签，如果是单标签，必须自闭合。
7. 标签首字母
    - 若是小写字母，就将这个标签转换为HTML中同名元素；如果在HTML没有同名元素，就报错。
    - 若是大写字母，就将这个标签作为一个React组件渲染；如果组件没有定义，就报错。

```css
.title{
    width:200px;
    background:#0ff;
}
```

```JSX
const id = "React";
const content = "Hello React!";

const VDOM = (
    <h1 className="title" id={id.toLowerCase()} style={{color:"white"}}>{content.toUpperCase()}</h1>
)
```

js语句与js表达式的区别：

- 表达式：一个表达式会产生一个值，可以放在任何一个需要值的地方。(有返回值)
- 语句/代码：比如if判断语句，没有返回值的代码。

## 4. 模块与组件、模块化与组件化的理解

### 4.1 模块

- 模块就是向外提供特定功能的JS程序，一般就是一个JS文件
- 模块的出现是为了解决随着业务逻辑的增加，代码越来越复杂的问题
- 通过模块来复用JS，简化JS的编写，提高JS的运行效率。

### 4.2 组件

- 组件就是用来实现局部功能的代码和资源的集合（包括HTML，CSS，JS，静态资源）。
- 同样也是为了简化代码，提高代码复用率。

### 4.3 模块化

当一个项目都是用JS模块来写的，这个项目就是一个模块化应用。

### 4.4 组件化

当一个项目都是以组件的方式来写的，这个项目就是一个组件化的项目。

# 第二章：React面向组件编程

## 1. 函数式组件（适用于简单组件的定义）

```jsx
function Demo(){
    console.log(this); // 此时的this 为 undefined 
    return <h1>这是函数式组件 </h1>
}

ReactDOM.render(<Demo/>,document.getElementById('test'));
// 上面这一行代码执行步骤为：
// 1. React解析组件标签，找到名为Demo的组件
//  2. 发现组件是使用函数定义的，随后调用该函数，将返回的虚拟DOM转为真实DOM，随后展示在视图上。
```

在JSX中开启了严格模式，也就是在函数式组件中，this是不会指向windows的。

## 2. 类式组件(适用于复杂组件的定义)

> 类的基本知识

```javascript
class Person{
    // 构造方法
    constructor(name){
        // 构造器中的this为类的实例对象
        this.name = name;
    }
    // 一般方法（除构造器之外的方法）
    sayHi(){
        console.log(this.name);
    }
}

const p1 = new Person('hmbb');

console.log(p1); 
p1.sayHi();
```

通过类new出来的实例对象本身并没有类的方法，而是在原型对象上。

> 类的继承

```javascript
class Student extends Person{
    constructor(name,age){
        super(name);
        this.age = age;
    }
}

const s1 = new Student('pdx',12);
console.log(s1);
s1.sayHi();
```

在子类中，构造函数需要在最开始的位置写上`super`关键字，然后将父类需要的数据传递过去，自己再将剩余的参数自行赋值；对于父类的方法，子类可以直接进行调用，并且在子类可以将父类的方法进行重写，同样也可以增加自己的方法。

> React中的类式组件

```react
// 创建类式组件
class Demo extends React.Component{
    // render放在原型对象上，供实例使用。
    render(){
        // render中的this为该类的实例对象 
        return (
            <h1>hello React!</h1>
        )
    }
}

ReactDOM.render(<Demo/>,document.getElementById('test'));
// 上面这一行代码执行步骤为：
// 1. React解析组件标签，找到名为Demo的组件
//  2. 发现组件是使用类定义的，随后new该类的实例，并通过该实例调用原型上的render函数，将返回的虚拟DOM转为真实DOM，随后展示在视图上。
```

## 3. 组件三大核心属性

### 3.1 状态 State

复杂组件于简单组件的区别： 

如果一个组件中，含有状态（state），那么这个组件就是一个复杂组件。

**什么是状态：**

1. state是组件对象最重要的属性，值必须是一个对象，可以包含多个key-value的组合。

2. 组件被称为“状态机”，通过更新组件的state来更新对应的页面显示（重新渲染组件）。

```react
// 创建类式组件
class Demo extends React.Component{
    constructor(props){
        super(props);
        // 初始化状态
        this.state= {isHot:false};
    }
    // render放在原型对象上，供实例使用。
    render(){
        // render中的this为该类的实例对象  
        return (
            <h1>今天天气很{ this.state.isHot?'炎热':'凉爽'}!</h1>
        )
    }
}

ReactDOM.render(<Demo/>,document.getElementById('test'));
```

在原生的JavaScript中，绑定事件的三种方式：

1. 通过获取到相应DOM元素，然后使用`.onclick`给相应DOM元素添加事件。
2. 通过获取到相应DOM元素，然后使用`addEventListener`给相应DOM元素添加事件。
3. 直接在DOM元素上使用相应的事件标识符，直接绑定事件。

在React中绑定事件的方式：

其实在React中三种绑定事件的方式都可以使用，但是在React中，更推荐使用直接在组件上直接绑定事件。

```react
// 创建类式组件
class Demo extends React.Component{
    constructor(props){
        super(props);
        // 初始化状态
        this.state= {isHot:false};
        this.change = this.change.bind(this)
    }
    // render放在原型对象上，供实例使用。
    render(){
        // render中的this为该类的实例对象  
        return (
            <h1 onClick={this.change}>今天天气很{ this.state.isHot?'炎热':'凉爽'}!</h1>
        ) 
    }
    change(){
        // change方法放在在Demo的原型对象上，供实例使用
   		// 通过Demo实例调用change方法时，change中的this就是Demo实例，在这里我们可以拿到state。
        // 类中的方法默认开启了严格模式，所以此时方法中的this为undefined
     	// this.state.isHot = !this.state.isHot;
        
        // 获取原来的isHot
        // this.state.isHot = !this.state.isHot;
        // 其实这个方法是错误的(状态state不可以直接更改)
        const isHot = this.state.isHot;
        // 这一行代码直接将state中的数据全部替换了。
        this.setState({isHot:!isHot});
	}
}

ReactDOM.render(<Demo/>,document.getElementById('test'));

```

 下面我们需要使用setState去更新state里面的值。

这一行代码：

```javascript
this.state.isHot = !this.state.isHot;
```

他虽然将状态里面的数据进行改变了，但是React没有监测到数据的更新，所以就导致了数据发生了改变，但是视图没有更新。

此时我们就需要setState了。

```javascript
this.setState({isHot:!isHot});
```

下面是代码的(state)简写形式：

```react
class demo extends React.Component{
    state = {isHot:false}
    render(){
        return <h1 onClick={this.change}>今天天气很{isHot?'炎热':'凉爽'}！</h1>
    }
    // 自定义方法：用赋值语句的形式+箭头函数
    change = () => {
        const isHot = this.state.isHot;
        this.setState( {isHot:!isHot});
    }
}
```

**注意**：

- 组件中的render方法中的this为组件实例对象。
- 组件自定义方法中的this指向为undefined，如何解决？
    - 强制绑定this，通过构造器强制将this绑定为组件实例对象
    - 使用箭头函数（推荐）
- 状态数据，不可以直接更改或者更新没需要借助setState。

### 3.2 props

 简单使用props:

```react
class Person extends React.Component{
    render(){
        const {name,age,sex} = this.props;
        return (
            <ul>
                <li>姓名：{name}</li>
                <li>年龄：{age}</li>
                <li>性别：{sex}</li>
            </ul>
        )
    }
}

ReactDOM.render(<Person name="张三" age="18" sex="男"/>,document.getElementById("test"));
```

批量传递props:

```react
class Person extends React.Component{
    render(){
        const {name,age,sex} = this.props;
        return (
            <ul>
                <li>姓名：{name}</li>
                <li>年龄：{age}</li>
                <li>性别：{sex}</li>
            </ul>
        )
    }
}

const p = {name:"老刘",age:12,sex:"男"}
ReactDOM.render(<Person {...p}"/>,document.getElementById("test"));
```

上面两种传递props有一点区别：

第一种方式，此时传递过去的数据，其实是一个字符串，如果你想传递一个数据类型过去，那么就需要换一种写法：

```react
ReactDOM.render(<Person name="张三" age={18} sex="男"/>,document.getElementById("test"));
```

将引号换成花括号的形式。

为了防止这样的错误出现，我们就需要对于props的属性进行限制：

```react
// 对属性值的数据类型以及是否必填限制
Person.porpTypes = {
    // 这种写法在React15版本之后就被弃用了
   //  name:React.PropTypes.string;
    
    // 在16版本之后，我们需要单独引入文件prop-types
    name:PropTypes.string.isRequired, // 限制name必传，且为字符串
    sex:PropTypes.string, // 限制sex为字符串
    age:PropTypes.number, // 限制age为数字
    speak:PropTypes.func, // 限制speak为函数
}

// 对属性值的默认值进行设置
Person.defaultProps = {
    age:18, // 限制年龄默认值为18
}
```

**注意**：

props是只读的，不允许修改数据。

props的简写方式：

```react
class Person extends React.Component{
    static porpTypes = {
   		name:PropTypes.string.isRequired, // 限制name必传，且为字符串
    	sex:PropTypes.string, // 限制sex为字符串
   		age:PropTypes.number, // 限制age为数字
    	speak:PropTypes.func, // 限制speak为函数
	}
    
    static defaultProps = {
    	age:18, // 限制年龄默认值为18
	}
    
    render(){
        const {name,age,sex} = this.props;
        return (
            <ul>
                <li>姓名：{name}</li>
                <li>年龄：{age}</li>
                <li>性别：{sex}</li>
            </ul>
        )
    }
}

const p = {name:"老刘",age:12,sex:"男"}
ReactDOM.render(<Person {...p}"/>,document.getElementById("test"));
```

在类的构造器中，是否接受props，以及是否在super中传递，取决于是否希望在构造器中通过this访问props。如果没有这个需求，那么就可以不接props。

**在函数组件中使用props：**

```react
function Person(props){
    const {name,age,sex} = props;
    return (
        <ul>
            <li>姓名：{name}</li>
            <li>年龄：{age}</li>
            <li>性别：{sex}</li>
        </ul>
    )
}

// 对属性值的数据类型以及是否必填限制
Person.porpTypes = {
    // 这种写法在React15版本之后就被弃用了
   //  name:React.PropTypes.string;
    
    // 在16版本之后，我们需要单独引入文件prop-types
    name:PropTypes.string.isRequired, // 限制name必传，且为字符串
    sex:PropTypes.string, // 限制sex为字符串
    age:PropTypes.number, // 限制age为数字
    speak:PropTypes.func, // 限制speak为函数
}

// 对属性值的默认值进行设置
Person.defaultProps = {
    age:18, // 限制年龄默认值为18
}

const p = {name:"老刘",age:12,sex:"男"}

ReactDOM.render(<Person {...p}"/>,document.getElementById("test"));
```

**关于props的总结：**

每个组件对象都会有props这个属性对象

组件标签的所有属性都保存在props。

作用：

通过标签属性从组件外向组件内传递变化的数据。

组件内部不能改变props传递的数据。

### 3.3 refs

组件内的标签可以定义ref属性来标识自己。

我们在标签上定义ref之后，我们可以在组件实例对象上的refs属性上获取到设置了ref的DOM。

然后就可以在JavaScript中操作DOM，获取到DOM的一些相关信息：比如input标签的值。

- 字符串形式的ref(官方已经不推荐使用)

```react
class Demo extends React.Component{
    render(){
        return <h1 ref="h1"></h1>
    }
}
```

此时的ref就是字符串形式的，在组件实例对象上查找的refs中的key值就是"h1"。

字符串的ref存在一些效率问题，使用过多的字符串ref，将会导致效率变低，这种写法后续可能会被移除。

- 回调形式的ref

```react
class Demo extends React.Component{
    render(){
        return <h1 ref={(currentNode) => {this.input1 = currentNode;}}></h1>
    }
}
```

使用回调函数，传递进去的一个参数，此时这个参数就是这个ref所处的DOM节点，然后在这个回调函数执行一件事情，将这个DOM节点赋值到组件实例对象身上。

关于这个回调形式的ref，执行次数：

1. 在组件创建的时候,这个函数执行一次。

2. 在更新的时候执行两次：

    - 第一次传入的参数为NULL。

    - 第二次传入的参数才是DOM元素。

    - 这是因为每次渲染组件（组件更新）时，React都会重新创建一个新的函数，所以就会先把旧的ref函数清除，然后再创建新的函数。 

    - 如果想要解决这个问题，可以将Ref的回调函数定义为class的绑定函数的方式避免上述问题。

```react
class Demo extends React.Component{
    saveRef = (node) => {
        this.h1 = node;
    }
    render(){
        return <h1 ref={this.saveRef}></h1>
    }
}
```

- 使用createRef创建ref

React.createRef调用后可以返回一个容器，该容器可以存储被ref所标识的节点。

```react
class Demo extends React.Componetn{
    myRef = React.createRef();
    
    render(){
        return <h1 ref={this.myRef}></h1>
    }
}
```

但是这个容器只能存储一个DOM节点，如果我们需要使用多个Ref，我们需要创建多个React.createRef。

不要过度使用ref。

### 3.4 事件处理

1. 通过`onXxx`的属性 指定事件处理函数，一般是on小写，后面的单词首字母大写。
    - React使用的是自定义（合成）事件，而不是元素的DOM事件。（这是为了更好的兼容性）
    - React中的事件是通过事件委托的方式处理的（委托给组件最外层的元素）。（为了高效）
2. 通过`event.target`可以获取都爱发生事件的DOM元素。

## 4. 表单（受控组件与非受控组件）

- 受控组件：就是表单中可以输入值的DOM元素（输入框，选择框等等），在你输入数据的时候，经过一些事件的绑定，你一输入数据，就会执行这个事件，然后拿到这个组件此时的值，这样的DOM元素就是受组件。

- 非受控组件：表单中可以输入值的DOM元素，在你输入数据的时候，并不会立即将数据收集起来，需要手动执行一些事件（比如提交按钮），这些组件的数据才会被收集，这样的DOM元素，就叫做非受控组件。

## 5. 组件的生命周期

生命周期其实就是一些回调函数，也就是生命周期钩子函数，也可以叫做生命周期函数，或者是生命周期钩子。

- 组件从创建到死亡（卸载）会经历一些特定的阶段。

- React组件中包含一系列的钩子函数（生命周期回调函数），会在特定的时刻掉用。
- 我们在定义组件时，会在特定的生命周期回调函数中做特定的事情。

### 5.1 旧版本生命周期

![image-20231117172855316](https://raw.githubusercontent.com/zml212/FigureBed/main/202311171728398.png)

在组件挂载的时候，生命周期是这样的：

1.  执行class中的构造器
2. 执行componentWillMount中生命周期
3. 执行render函数
4. 执行componentDidMount生命周期
5. 如果组件被卸载，那么就执行componentWillUnmount生命周期

组件更新：

1. 更新State:

    - 在组件中调用来了setState()，就会执行shouldComponentUpdate，这个生命周期就像是一个阀门，如果返回是false，生命周期就不会继续往下面走了，也就不会更新了；如果返回是true，那么就更新。如果没有明确写这个生命周期，那么它的默认值就是true。

    ```React
    shouldComponentUpdate(){
        return true;
    }
    ```

    - 如果上面的shouldComponetUpdate返回值为true，那么继续执行下面的生命周期：componentWillUpdate。
    - 执行render函数
    - 执行componentDidUpdate生命周期

2. 强制更新（forceUpdata()）

    - 普通更新需要改变状态，React才会去更新页面（shouldComponetUpdate默认返回为true）。但是如果开发者想不改变状态也更新，那么就可以使用强制更新，并且强制更新不会走shouldComponentUpdate生命周期。

    ```react
    // 强制更新回调函数
    force = () => {
        this.forceUpdate();
    }
    ```

    - 执行componentWillUpdate
    - 执行render函数
    - 执行componentDidUpdate生命周期

3. 父组件render

生命周期大体分为三个阶段：

1. 初始化阶段：由ReactDOM.render()触发-- 初次渲染
    1. constructor()
    2. componentWillMount()
    3. render()
    4. componentDidMount()  --- 常用，一般在这个钩子里面做一些初始化
2. 更新阶段：由组件内部this.setSatet()或父组件重新渲染render触发
    1. shouldComponentUpdate()
    2. componentWiillUpdate()
    3. render()
    4. componentDidUpdate()
3. 卸载组件：由ReactDOM.unmountComponentAtNode()触发
    1. componentWillUnmount()

### 5.2 新版本生命周期

![image-20231117182634620](https://raw.githubusercontent.com/zml212/FigureBed/main/202311171826690.png)

在新版本中可以使用旧版本的声明周期钩子，但是会报警告，也就是不被推荐使用。

在新版本中的生命周期中，以下三个生命周期钩子需要加上`UNSAFE_`：

- componentWillMount
- componentWillReceiveProps
- componetWillUpdate

并且这三个生命周期可能会在后续的版本中被删除。

componentWillMount生命周期被替换成了getDrivedStateFromProps。

新版本的生命周期中，在render与componentDidUpdate出现了一个新的生命周期：getSnapshotBeforeUpdate。

在getSnapshotBeforeUpdate生命周期中，可以传入两个参数，分别是更新之前的props和更新之前的state。 

也就是在这个生命周期中，可以从更新之前的DOM结构中拿到一些数据，然后返回这些数据，并且在componentDidUpdate中可以通过参数获取到这个生命周期所返回的快照值。

## 6. React的Diff算法

虚拟DOM中key值的作用：

- key值就是虚拟DOM的对象标识，在更新显示时key有重要作用。
- 当状态中的数据发生变化的时候，react会根据新数据生成新的虚拟DOM，然后React根据新旧DOM进行diff对比：
    - 在旧的DOM中找到了与新DOM相同的key:
        - 如果这两个相同key节点的内容没有发生变化，直接使用之前的**真实DOM**；
        - 如果虚拟DOM中的内容发生了变化，那么生成新的真实DOM，随后替换掉之前的真实DOM；
    - 旧虚拟DOM中没有找到与新虚拟DOM相同的key:
        - 根据数据创建新的真实DOM，并且挂载到页面；
- 使用index作为key值，可能会引发的问题：
    - 如果对数据进行：逆序添加，逆序删除等破坏顺序的操作：会产生没有必要的真实DOM更新，虽然页面效果没有问题，但是会导致性能问题；
    - 如果结构中存在输入类DOM：会产生错误的更新；
- 开发中选择key值：
    - 最好使用每条数据唯一的标识：id、手机号、身份证号...
    - 如果确定只是简单的数据展示（不更新数据），也可以使用index作为key值。

# 第三章：React脚手架

## 1. 使用create-react-app创建react项目



