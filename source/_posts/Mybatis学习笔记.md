---
title: Mybatis学习笔记
date: 2020-02-25 12:58:48
tags:
- Spring
categories:
- SSM框架
---

**SSM框架之Mybatis学习笔记**

<!--more-->

## 一、 Mybatis基础

### 1.Mybatis概述

Mybatis本是apache的一个开源项目iBatis，后来改名为Mybais，并迁移到Github。是一个基于Java的**持久层框架**。

它的内部封装了JDBC，使开发者只需关注sql语句本身，而不需要花费更多时间去处理加载驱动、创建连接等过程。

mybatis使用xml或者注解的方式将要执行的各种statement配置起来，并通过java对象和statement中sql 的动态参数进行映射生成最终执行的 sql 语句，最后由 mybatis 框架执行 sql 并将结果映射为 java 对象并 返回。

### 2.jdbc编程的问题分析

1. 数据库链接创建、释放频繁造成系统资源浪费从而**影响系统性能**，如果使用数据库链接池可解决此问题。
2. Sql 语句在代码中硬编码，造成**代码不易维护**，实际应用 sql 变化的可能较大，sql 变动需要改变 java代码。
3. 使用preparedStatement向占有位符号传参数存在硬编码，因为sql语句的where 条件不一定，可能多也可能少，修改 sql 还要修改代码，系统不易维护。
4. 对结果集解析存在硬编码（查询列名），sql 变化导致解析代码变化，**系统不易维护**，如果能将数据库记录封装成 pojo 对象解析比较方便。

## 二、 Mybatis框架入门

### 1.环境搭建

1. 创建maven工程并导入相关坐标

   ```xml
   <dependency>
       <groupId>org.mybatis</groupId>
       <artifactId>mybatis</artifactId>
       <version>x.x.x</version>
   </dependency>
   ```

2. 创建实体类和dao的接口

3. 创建Mybatis的**主配置文件**

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <!--配置环境-->
       <environments default="mysql">
           <!--配置mysql的环境-->
           <environment id="mysql">
               <!--配置事务的类型-->
               <transactionManager type="JDBC"></transactionManager>
               <!--配置数据源(连接池)-->
               <dataSource type="POOLED">
                   <!--配置连接数据库的四个基本信息-->
                   <property name="driver" value="com.mysql.cj.jdbc.Driver"></property>
                   <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useUnicode=true&amp;characterEncoding=utf8&amp;serverTimezone=UTC"></property>
                   <property name="username" value="root"></property>
                   <property name="password" value="123456"></property>
               </dataSource>
           </environment>
       </environments>
   
       <!--指定映射配置文件的位置
               映射配置文件：每个dao独立的配置文件
           -->
       <mappers>
           <mapper resource="top/lemenk/dao/***.xml"></mapper>
       </mappers>
   </configuration>
   ```

4. 创建映射配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <!--namespace值为Dao接口的全限定类名-->
   <mapper namespace="top.lemenk.dao.IUserDao">
       <!--配置查询所有，注意resultType用于指定封装的实体类-->
       <select id="findAll" resultType="top.lemenk.domain.User">
           //sql语句
           select * from user
       </select>
   </mapper>
   ```

### 2.环境搭建的注意事项：

1. mybatis的映射配置文件位置必须和dao接口的包结构相同
2. 映射配置文件的mapper标签namespace属性的取值必须是dao接口的全限定类名
3. 映射配置文件的操作配置（select），id属性的取值必须是dao接口的方法名

### 3.测试入门案例

~~~java
@Test
public void Test() throws Exception {
    //1.读取配置文件(从类路径加载配置文件)
    InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
    //2.创建SqlSessionFactory工厂
    SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
    SqlSessionFactory factory = builder.build(in);
    //3.使用工厂生产SqlSession对象
    SqlSession session = factory.openSession();
    //4.使用SqlSession创建Dao接口的代理对象
    IUserDao userDao = session.getMapper(IUserDao.class);
    //5.使用代理对象执行方法
    List<User> users = userDao.findAll();
    for(User user : users){
        System.out.println(user);
    }
    //6.释放资源
    session.close();
    in.close();
}
~~~

为什么要使用Dao接口的代理对象来实现功能呢？

因为在开发中，如果自己写Dao接口的实现类，就需要自己写相关的实现方法。

UserImpl：

~~~java
SqlSessionFactory factory;
//创建有参数的构造函数，用于传入参数
public UserDaoImpl(SqlSessionFactory factory){
    this.factory = factory;
}

public List<User> findAll(){
    //使用工厂创建SqlSession对象
    SqlSession session = factory.openSession();
    /*
    *使用session执行查询所有方法
    *	其中selectList方法为SqlSession中的方法，用于查询所有
    *	参数为改sql语句的位置。
    */
    List<User> users = session.selectList("top.lemenk.dao.IUserDao.findAll");
    session.close();
    return users;
}
~~~

使用代理工厂可以减少代码量。

## 三、Mybatis 连接池与事务深入

### 1.Mybatis 的连接池技术

在 Mybatis的 SqlMapConfig.xml配置文件中，通过<dataSource type="pooled">来实现Mybatis中连接池的配置。

#### 1.1 Mybatis 连接池的分类

UNPOOLED    不使用连接池的数据源

POOLED         使用连接池的数据源

JNDI                使用JNDI实现的数据源

相应地，MyBatis 内部分别定义了实现了 java.sql.DataSource 接口的 UnpooledDataSource， PooledDataSource 类来表示UNPOOLED、POOLED 类型的数据源。

#### 1.2 Mybatis 中数据源的配置

在 SqlMapConfig.xml 文件中:

```xml
<dataSource type="POOLED">
    <property name="driver" value="${jdbc.driver}"></property>
    <property name="url" value="${jdbc.url}"></property>
    <property name="username" value="${jdbc.username}"></property>
    <property name="password" value="${jdbc.password}"></property>
</dataSource>
```

type=”POOLED”：MyBatis 会创建 PooledDataSource 实例 type=”UNPOOLED” ： MyBatis 会创建 UnpooledDataSource 实例 type=”JNDI”：MyBatis会从JNDI服务上查找DataSource 实例，然后返回使用

### 2.Mybatis 的事务控制

#### 2.1 JDBC事务提交方式

在 JDBC中我们可以通过手动方式将事务的提交改为手动方式，通过 setAutoCommit()方法就可以调整。

#### 2.2 Mybatis 中事务提交方式

Mybatis 中事务的提交方式，本质上就是调用 JDBC 的setAutoCommit()来实现事务控制。