---
title: HTML基础2
date: [2023-4-3]
tags: [前端]
categories: [HTML]
---

# 列表标签

## 无序列表

使用`<ul></ul>`。

```HTML
<ul>
	<li></li>
	<li></li>
	<li></li>
	<li></li>
</ul>
```

四个无序列表。

## 有序列表

```HTML
<ol>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
</ol>
```

## 表格

与表格相关的标记

|标记|描述|
|:--:|:--:|
|table|表标记|
|tr|行标记|
|th|表头标记|
|td|列里面内容标记|

```HTML
<table>
	<tr>
		<th>
			表头
		</th>
	</tr>
	<tr>
		<td>
			里面的内容
		</td>
	</tr>
<table>
```

属性：

- cellspacing:  设置表格单元格的边距
- cellpadding: 设置表格的内边距
- border : 设置表格的边框厚度

## 合并单元格

单元列合并需要用到`colspan`属性，在要跨列的哪一行上设置`colspan`属性，值为要跨的列数。

单元行合并需要用到`rowspan`属性，在要跨列的哪一列上设置`rowspan`属性，值为要跨的行数。

## 表单标签

使用`form`标签。

其中`form`有两个属性：

- action： 值为一个URL，表示表单内容提交到的位置。
- method： 值为几种提交方式，POST、GET。表示提交的方式。
	- get ： 信息在URL中，安全性不高，并且会导致URL过长。
	- post:  表单里面的信息在地址栏看不见，对URL长度没有影响。

### 常见的输入框类型

- 输入框
- 密码框
- 下拉框
- 单选框
- 复选框
- 提交按钮
- 上传文件
- 多行文本

单行输入框：

```HTML
	<input type="text">
```

密码框：

```HTML
	<input type="password">
```

下拉框：

```HTML
	<select> // 开始标签
		<option></option> // 选项标签
		<option></option>
	</select>
```

单选按钮：

```HTML
	<input type="radio" name="sex">
	<input type="radio" name="sex">
```

注意：name属性值相同的为一组。

提交按钮：

```HTML
	<input type="submit">
```

上传文件：

```HTML
	<input type="file">
```

多选框：

```HTML
	<input type="checkbox">
	<input type="checkbox">
```

多行文本：

```HTML
	<textarea></textarea>
```

重置按钮：

```HTML
	<input type="reset">
```