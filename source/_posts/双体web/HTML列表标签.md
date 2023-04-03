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