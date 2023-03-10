---
title: 包和final与权限修饰符
date: 2023-1-7
tags: [后端]
categories: [Java]
---
# 包和final与权限修饰符
[TOC]

## 包

包就是文件夹，用来管理各种不同的`java`类，方便后期代码维护。

- **<font color=red>包名的规则：</font>**公司域名反写+包的作用，需要全部英文小写，见名知意。
- **<font color = red>使用其他类的规则：</font>**
	- 使用同一个包中的类时，不需要导包
	- 使用`java.lang`包中的类时，不需要导包
	- 其他情况都需要导包
	- 如果同时使用两个包中同名类，需要用全类名

## final

 最终的，不可以被改变的

final可以修饰方法，类，变量。

- 修饰方法的时候：表明该方法是最终方法，不能被重写（不能被继承）
- 修饰类的时候：表明该类是最终类，不能被继承
- 修饰变量的时候：这时候这个变量叫做常量，只能被赋值一次

在实际开发当中，常量一般作为系统的配置信息，方便维护，提高可读性。

常量的命名规范：

- 单个单词：全部大写
- 多个单词：全部大写，单词之间用下划线隔开

细节：

- final修饰变量是基本数据类型的时候，里面存储的数据值不能发生改变。
- final修饰变量是引用类型的时候，变量存储的地址值是不能发生改变的，但是对象内部的数据可以发生改变。

## 权限修饰符

- 权限修饰符：使用来控制一个成员能够被访问范围的。

- 可以修饰成员变量、方法、构造方法、内部类

**<font color=red>权限修饰符分类：</font>**

权限修饰符有四种，范围由小到大：<font color = red>private < 空着不写 < protected < public</font>

修饰符作用范围：
|  修饰符   | 同一个类中 | 同一个包中其他类 | 不同包下的子类 | 不同包下的无关类 |
| :-------: | :--------: | :--------------: | :------------: | :--------------: |
|  private  |     √      |                  |                |                  |
| 空着不写  |     √      |        √         |                |                  |
| protected |     √      |        √         |       √        |                  |
|  public   |     √      |        √         |       √        |        √         |