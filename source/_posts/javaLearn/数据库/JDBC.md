---
title: JDBC
date: [2023-3-27]
tags: [java]
categroies: [后端]
---

# JDBC基本使用

- DQL　数据的查询语句
- DML   数据的增删改查语句

## JDBC的基础

java数据库连接：

JDBC是java的一组API。（由java JDK给我们提供的）

步骤有以下几步：

1. 连接数据库
2. 执行数据库语句
3. 获取并处理返回的数据

JDBC是一种规范，不同数据库的厂商，根据这个规范制作了不同的驱动（Driver）。

JDBC驱动是连接数据库的具体实现。

**JDBC API包**
- java.sql
- javax.sql

## 操作步骤

1. 加载驱动

```java
Class.forName("oracle.jdbc.driver.OracleDriver");  // 加载驱动
```

这句代码我们需要异常捕获，因为这个驱动可能找不到，所以我们需要错误捕获。

2. 创建连接

```java
con = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:xe", "frank_24", "frank_24");
```

使用`DriverManager`中`getConnection()`方法，传入三个参数：

- url（JDBC进程的位置）
- 用户名
- 密码

3. 创建数据库语句对象

```java
Statement st = con.createStatement();
```

4. 执行语句（DML：不需要处理结果集，DQL：处理结果集）

```java
String sql = "delect * from T_STUDENT";
ResultSet rs = st.executeQuery(sql);
```

5. 释放资源

