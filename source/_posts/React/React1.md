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

### 2.2 JSX中列表渲染

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

## 3.虚拟DOM

在React中操作的元素叫做React元素，并不是真正的原生DOM。

React通过虚拟DOM将React元素与真实DOM进行映射。

好处：

> 1. 降低API复杂度
> 2. 解决兼容问题
> 3. 提升性能（减少不必要的DOM操作）

React使用diffing算法，将新旧DOM进行比较，替换更改的DOM，保留不变的DOM。

比较时，React会先比较父元素，如果父元素发生了改变，就直接替换该元素，不再比较子元素。如果父元素一致，则往下比较，直到发现不一样的节点。

## 4. 组件

### 4.1 函数式组件

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

### 4.2 类式组件

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

### 4.3 补充

render函数可以：

- 返回React元素：

> 通过JSX创建 

- 数组或者fragments、

> 使得render返回多个元素

- Portals：

> 渲染子节点到不同的DOM字树中 

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

####  6.1.2 字传父

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









 

