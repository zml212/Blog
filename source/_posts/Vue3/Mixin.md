---
title: Vue3中的Mixinyu extends
data: [2023-3-21]
tags: [前端]
categroies: [Vue3]
---

# Vue3中的Mixin与extends

组件之间有时候会存在一些相同的代码逻辑，我们此时就需要对相同的代码逻辑进行抽取。

这个时候我们就可以使用到Mixin这种方式。

- Mixin提供了一种非常灵活的方式，用来分发Vue组件中可复用的功能。
- 一个Mixin对象可以包含任何组件选项
- 当组件使用Mixin对象时，所有Mixin对象选项将被混入该组件本身的选项当中。

## Mixin的基本使用

[见代码](https://github.com/zml212/vue3_learn/tree/master/vue_vite_learn/src/17_Mixin%E4%B8%8Eextends%E7%9A%84%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)

### Mixin的合并规则

如果Mixin对象中的选项和组件对象中的选项发生了冲突，Vue会这样处理：

- 如果data函数的返回值对象与Mixin发生了冲突
	- 返回值对象默认情况下会进行合并
	- 如果data的返回值与Mixin发生了冲突，那么会保留组件自身的数据

- 如果与生命周期函数发生冲突
	- 生命周期钩子函数会被合并到一个数组，并且都会执行

- 值为对象的一个选项，比如methods、components、directives，将会被合并为同一个对象
	- 比如都有methods选项，并且都定义了方法，那么他们都会生效
	- 但是如果对象的key相同，那么组件内部的会取代Mixin的

### 全局混入Mixin

如果我们组件当中，某些选项是所有组件都需要的，这个时候我们可以使用全局Mixin

- 全局Mixin可以使用应用app的方法Mixin来完成注册
- 一旦注册，那么全局混入的选项将会影响每一个组件

## Vue3中的extends

另外一个类似于Mixin的方式就是通过extends属性

- 允许声明扩展另一个组件，类似于Mixin