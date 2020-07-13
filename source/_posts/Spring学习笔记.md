---
title: Spring学习笔记
date: 2019-11-21 21:38:03
tags:
- Spring
categories:
- SSM框架
---

**SSM框架之Spring学习笔记**

<!--more-->

## 一 、Spring基础

### 1.Spring概述

​		spring是分层的Java SE/EE应用**全栈式**轻量级开源框架，以**IOC**(Inverse Of Control，控制反转)和**AOP**(Aspect Oriented Progranming，面向切面编程)为内核，提供了展现层SpringMVC和持久层Spring JDBC以及业务层事务管理等众多的企业级应用技术，还能整合开源世界众多著名的第三方框架和类库，逐渐成为使用最多的JavaEE企业应用开源框架。

### 2. Spring的体系结构

![Spring的体系结构](http://ww1.sinaimg.cn/large/006zQofnly1gbh0mtz179j30k00f00u1.jpg)

# 二、IoC的概念和作用

### 1.程序的耦合

​        耦合性(Coupling)，也叫耦合度，是对模块间关联程度的度量。耦合的强弱取决于模块间接口的复杂性、调用模块的方式以及通过界面传送数据的多少。模块间的耦合度是指模块之间的依赖关系，包括控制关系、调用关 系、数据传递关系。模块间联系越多，其耦合性越强，同时表明其独立性越差( 降低耦合性，可以提高其独立性)。

​        耦合是影响软件复杂程度和设计质量的一个重要因素，在设计上我们应采用以下原则：如果模块间必须 存在耦合，就尽量使用数据耦合，少用控制耦合，限制公共耦合的范围，尽量避免使用内容耦合。

```java
public class JdbcDemo1{
    public static void main(String[] args) {
        try {
            //1.注册驱动
            //DriverManager.registerDriver(new com.mysql.cj.jdbc.Driver());
            Class.forName("com.mysql.cj.jdbc.Driver");
            //2.获取链接
            Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/spring?useUnicode=true&characterEncoding=utf-8&useSSL=false&serverTimezone=UTC","root", "123456");
            //3.获取操作数据库的预处理
            PreparedStatement ps = con.prepareStatement("select * from account");
            //4.执行SQL，得到结果集
            ResultSet rs = ps.executeQuery();
            //5.遍历结果集
            while (rs.next()){
                System.out.println(rs.getString("name"));
            }
            //6.释放资源
            rs.close();
            ps.close();
            con.close();
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

在JDBC操作中使用`Class.forName`的方式比使用`DriverManager.registerDriver`好是因为DriverManager 的 register 方法需要依赖数据库的MuSQL驱动类，当更换了数据库类型时就需要修改源码。使用Class.forName降低耦合度。

`Class.forName("com.mysql.cj.jdbc.Driver");`中是使用反射来注册驱动的。

### 2. 解耦

#### 2.1 工厂方式解耦

在实际开发中我们可以把三层的对象都使用配置文件配置起来，当启动服务器应用加载的时候，让一个类中的方法通过读取配置文件，把这些对象创建出来并存起来。在接下来的使用的时候，直接拿过来用就好了。 

那么，这个读取配置文件，创建和获取三层对象的类就是工厂。

#### 2.2 控制反转(Inversion Of Control)解耦

控制反转把创建对象的权力交给框架，是框架的主要特征，并非面向对象的专用术语。它包括依赖注入（Dependency Injection，简称DI）和依赖查找（Dependency Lookup）

# 三、使用Spring的IOC解决程序的耦合

