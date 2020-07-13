---
title: MySQL学习笔记
date: 2018-11-25 11:36:01
tags:
- 数据库
- MySQL
categories:
- 数据库
- MySQL
---

**MySQL数据库学习笔记**

<!--more-->

# 入门语句：

1. 登陆服务器：mysql -u(用户名) -p(密码)。例：mysql -uroot -p123

2. 登陆成功后需要选库：

   - 选库语句：use 数据库名
   - 查看有哪些库：show databases;

3. 查看当前库下的所有表：

   ```mysql
   show tables;
   ```

4. 创建一个数据库：create database 数据库名 [字符集]；

   ```mysql
   create database yy1 charset utf8;
   ```

5. 删除一个数据库：drop database 数据库名 ;

   ```mysql
   drop database yy1;
   ```

6. 新建一个简单表：

   ```mysql
   create table stu(
   sid int;
   sname varchar(10)
   )engine myisam charset utf8;
   ```

   注：`engine myisam charset utf8`是选择数据库引擎和字符集编码格式。

7. 删除表：drop table 表名;

   ```mysql
   drop table stu;
   ```

8. 向表中插入数据

   ```mysql
   insert into stu values
       (1,'zhangsan'),
       (2,'lisi'),
       (3,'wangwu');
   ```

9. 清空表数据：truncate 表名；

   注：truncate相当于删除表再重建一张相同结构的表

   ​		delete是从删除表中的行来操作的。

10. 解决cmd窗口中文乱码问题：

    CMD默认字符集是GBK编码，可以通过下面语句改变默认字符集。

    ```mysql
    set names utf8;
    ```

11. 查看表结构：desc 表名;

# 增删改查

## 增加数据 insert

1. 插入元组

   - 若未写明添加哪些列，则默认添加所有列的数据。

     ```mysql
     insert into stu
     values(1,'刘备')；
     ```

     注：所添加的数据必须与表中的列有一一对应。

   - 若在into中出现了数据列，则需要按照into语句中的顺序添加数据。

     ```mysql
     insert
     into stu(id)
     values(2),
     values(3);
     ```

     ```mysql
     insert
     into stu(id,name)
     values(4,'张辽');
     ```

     注：into子句中没有出现的属性列，新元组将在这些列上去空值。但若在创建表时使用了not null属性，则不能取空值，会取默认值。

     **字符串常数要用单引号括起来。**

2. 插入子查询结果

   ```mysql
   insert
   into<表名>[属性列]
   子查询语句;
   ```

   例：对每一个系中，求学生的平均年龄，并把结果存入数据库。

   解：先建一张表，存放系名和学生平均年龄。

   ```mysql
   create table dept_age
   (Sdept char(15)
   Avg_age SMALLINT
   )engine myisam charset utf8;
   ```

   然后对Sutdent表按系分组求平均值，再把数据放到新表中。

   ```mysql
   insert
   into Dept_age(Sdept,Avg_age)
   select Sdept,AVG(Sage)
   from Student
   group by Sdept;
   ```

## 修改数据

update<表名> set <列名>=<表达式> [where<条件>];

1. 修改某一元组的值

   ```mysql
   update stu set name='aa' where id=3;
   ```

2. 修改多个元组的值

   ```mysql
   update stu set id=id+1;
   ```

3. 带子查询的修改语句

##  删除数据

delete from<表名> [where语句];

## 四



# 关系数据库标准语言SQL

SQL特点：

- 综合统一
- 高度非过程化
- 面向集合的操作方式
- 以同一种语法结构提供多种使用方式
- 语法简单、易学易用

## 数据定义

1. 模式的定义与删除

   - 定义：

     ```mysql
     CREATE SCHEMA <模式名> AUTHORIZATION <用户名>;
     ```

     例：为用户WANG定义一个学生-课程模式S-T；

     ```mysql
     CREATE SCHEMA "S-T" AUTHORIZATION WANG;
     ```

     

   - 删除：

     ```mysql
     DROP SCHEMA <模式名><CASCADE|RESTRICT>;
     ```

     CASCADE与RESTRICT二选一，CASCADE（级联）表示再删除是把该模式中所有的数据库对象全部删除；RESTRICT（限制）表示如果模式中已经定义了下属的数据库对象，则拒绝该删除语句执行。

2. 基本表的定义、删除、修改

   - 定义：

     ```mysql
     CREATE TABLE<表名>(<列名><数据类型>约束条件...）
     ```

     例:建立一个学生表Student

     ```mysql
     CREATE TABLE 
     Student(
         Sno CHAR(9) PRIMARY KEY,
       Sname CHAR(20) UNIQUE,
         Sage SMALLINT
     );
     ```

   - 修改：

     ```mysql
     ALTER TABLE<表名>[ADD[COLUMN]<新列名><数据类型>[完整性约束]]...;
     ```

     例：向Student表增加“入学时间”列，数据类型为日期型

     ```mysql
     ALTER TABLE Student ADD S_entrance DATE;
     ```

     

   - 删除：

     ```mysql
     DROP TABLE <表名> [RESTRICT|CASCADE];
     ```

     例：删除Student表

     ```mysql
     DROP TABLE Student CASCADE；
     ```

     

3. 索引的建立、修改和删除

   - 建立：

     ```mysql
     CREATE [UNIQUE] [CLUSTER] INDEX <索引名> ON <表名>(<列名> [<次序>] [,<列名> [<次序>]]...);
     ```

     次序可以选则ASC（升序）或者DESC（降序），默认为ASC；

     例：为学生-课程数据库中Student、SC两个表建立索引，Student按照学号升序，SC表按照学号降序和课程号降序建唯一索引。

     ```mysql
     CREATE UNIQUE INDEX Stusno ON Student(Sno);
     CREATE UNIQUE INDEX SCno ON SC (Sno ASC,Cno DESC);
     ```

     

   - 修改索引：

     ```mysql
     ALTER INDEX <旧索引名> RENAME TO <新索引名>;
     ```

     

   - 删除索引：

     ```mysql
     DROP INDEX <索引名>;
     ```

     

## 数据查询

一般格式：

```mysql
SELECT [ALL|DISTINCT] <目标列表达式> [,<目标表达式>] ... 

FROM <表名或视图名> [,<表名或视图名>...] | (<SELECT语句>) [AS] <别名> 
[WHERE<条件表达式>] 

[ GROUP BY <列名1> [HAVING <条件表达式>]] 

[ ORDER BY <列名2> [ASC|DESC]];
```

含义：根据WHERE子句的条件表达式从FROM自居指定的基本表、视图或者派生表中找到满足条件的元组，再按照SELECT子句中的目标列表达式选出元组中的属性值形成的结果表。**如果有GROUP BY 子句**，则将结果按<列名1>的值进行分组，该属性列值相等的元组为一个组。通常会在每个组中作用聚集函数。**如果GROUP BY 子句带HAVING短语**，则只有满足指定条件的组才予以输出。**如果有OEDER BY子句**，则结果表还要按<列名2>的值的升序或者降序排序。

运算符：

- 简单条件运算符：

- 

  大于：>  

  小于：<    

  不等于：<>  

  大于等于：>=  

  小于等于：<=

- 逻辑运算符：

  and ：与

  or ：或

  not：非

- 模糊查询：

  like：一般与通配符一起使用

  ```mysql
  SELECT 
  	* 
  FROM 
  	employees 
  WHERE 
  	last_name LIKE '%a%';#   %通配符
  ```

  ```mysql
  SELECT 
  	last_name 
  	salary 
  FROM 
  	employees 
  WHERE 
  	last_name LIKE '%_n_1%';#   _通配符
  ```

  between and ：并包含临界值

  ```mysql
  SELECT
  	*
  FROM
  	employees
  WHERE 
  	employee_id BETWEEN 100 AND 120;
  
  ```

  in：用于判断某字段的值是否属于in列表中的某一项

  ```mysql
  SELECT
  	last_name,job_id
  FROM
  	employees
  WHERE 
  	job_id IN('IT_PRO','AD_VP','AD_PRES');
  ```

  is null：用于判断是否时null值

通配符：

%:任意长度字符

_:单个字符

1. 单表查询

   - 选择表中的若干列

     例：查询全体学生的详细记录

     ```mysql
      SELECT * FROM Student;
     ```

   - 选择表中的若干元组

     - 消除取值重复的行(关键词为DISTINCT,默认为ALL)

       ```mysql
       SELECT DISTINCT Sno FROM SC;
       ```

     - 查询满足条件的元组

       ```mysql
       SELECT DISTINCT Sno FROM SC WHERE Sname=Like;
       ```

       

     - 对查询结果降序排序

       ```mysql
       SELECT * FROM employees ORDER BY salary DESC;
       ```

       

## DML语言

1. 插入语句

   insert into 表名 (表名，……) values (值1，……);

   ```mysql
   INSERT INTO beauty (id,NAME,sex,borndate,phone) VALUES(13,'唐艺夕'，'女'，'1990-4-23','18162662320')； 
   ```

2. 修改语句

   - 修改单表

     update 表名 set 列=新值，列=新值，…… where 条件；

     ```mysql
     UPDATE beauty 
     SET phone = '13899951633' 
     WHERE NAME LIKE '刘%';
     ```

     

   - 修改多表

     update 表1 别名，表2 别名 set 列=值 where 连接条件 and 筛选条件

     ```mysql
     UPDATE boys bo 
     INNER JOIN beauty b ON bo.'id'=b.'boyfriend_id'
     WHERE bo.'boyName'='张无忌'；
     ```

3. 删除语句

   - delete：删除某一元组

     delete from 表名 where 筛选条件

   - truncate：清空表数据

     truncate table 表名