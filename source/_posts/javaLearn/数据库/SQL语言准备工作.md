1. Oracle体系结构

   一个表空间可以建立多个用户、一个用户下可以建立多个表。

   1. 实例：一个数据库可以有n个实例。

   2. 用户：在实例下建立，是管理表的基本单位。

   3. 表空间：对数据文件的逻辑映射。

   4. 数据文件：.dbf、.ora文件。数据文件是数据库的物理存储单位。

   5. 表：

      ![image-20210325095107813](C:\Users\59221\AppData\Roaming\Typora\typora-user-images\image-20210325095107813.png)

2. Oracle安装

   1. XE版本（11gR2）下载地址：https://www.oracle.com/database/technologies/xe-prior-releases.html
   2. 命令行登录：在命令行中输入sqlplus回车，按提示输入账号和密码。在使用sys用户登录时，密码格式为：密码 as sysdba。例如密码为root，则输入：root as sysdba
   3. 查看实例名称：select instance_name from v$instance;
   4. 查看所有表空间：select tablespace_name from dba_tablespaces; 
   5. 查看当前用户表空间：select default_tablespace from user_users;
   6. 查看系统用户：select username from all_users;
   7. 查看当前用户有多少表：select count(1) from user_tables;
   8. 查看所有表的数量：select count(1) from all_tables;

3. Oracle基本操作

   ~~~sql
   -- 创建表空间
   -- 指定表空间名称，表空间是一个逻辑分区
   -- 数据文件存放路径
   -- 数据文件初始大小
   -- 每次自动扩容10m
   CREATE tablespace stx_24
   datafile 'D:\02_workspace\03_dbData\oracle\stx_24.dbf'
   SIZE 128m  
   autoextend ON
   NEXT 16m;  
   
   -- 删除表空间
   DROP tablespace stx;
   
   -- 创建用户
   -- identified by 指定密码
   -- 指定用户的表空间
   CREATE USER frank
   IDENTIFIED BY frank
   DEFAULT tablespace stx;
   
   -- 给用户授权
   -- oracle 常用角色：
   -- connect:连接角色，基本角色
   -- resource:开发者角色
   -- dba:超级管理员角色
   GRANT dba TO frank;
   
   -- 删除用户
   drop user frank;
   
   -- 创建表
   CREATE TABLE FRANK.STUDENT (
   	ID VARCHAR2(16),
   	USERNAME VARCHAR2(16),
   	GENDER VARCHAR2(3)
   )
   TABLESPACE STX;
   
   
   -- 修改oracle简洁版占用8080端口的问题
   -- 在命令行中输入sqlplus, 以system账号登录
   -- 查询当前端口：
   select dbms_xdb.gethttpport() from dual;
   -- 设置新端口：
   exec dbms_xdb.sethttpport(9999); -- 将新端口设为9999
   -- 设置后重启OracleXETNSListener、OracleServiceXE服务
   ~~~

4. **oracle 数据类型：**以11g为例

   https://docs.oracle.com/cd/E11882_01/server.112/e41084/sql_elements001.htm#SQLRF30020