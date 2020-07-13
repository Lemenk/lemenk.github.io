---
title: JAVAWEB学习笔记
date: 2019-3-25 8:14:50
tags:
- JAVAWEB
categories:
- JAVAWEB
---

**JAVAWEB学习笔记**

<!--more-->

# 一、JSP相关笔记

### JSP简介

1. JSP全称**Java Server Pages**，是一种**动态网页开发技术**。它使用JSP标签在HTML网页中插入Java代码。标签通常以<%开头以%>结束。
2. JSP是一种Java servlet，主要用于实现Java web应用程序的用户界面部分。网页开发者们通过结合HTML代码、XHTML代码、XML元素以及嵌入JSP操作和命令来编写JSP。
3. JSP通过网页表单获取用户输入数据、访问数据库及其他数据源，然后动态地创建网页。
4. JSP标签有多种功能，比如访问数据库、记录用户选择信息、访问JavaBeans组件等，还可以在不同的网页中传递控制信息和共享信息。

### JSP环境搭建

1. JDK环境配置
2. Tomcat环境配置
3. 在IDEA或Eclipse中配置Tomcat

### JSP基础语法

#### 三种语法

1. ```jsp
   <% 代码片段 %>
   ```

2. ```jsp
   <%! int i = 0; %>
   <%! string a = "abc"; %>
   ```

3. ```jsp
   <%= 表达式>
   ```

#### 注释

```jsp
<%-- jsp代码注释，不会在网页中显示 --%>
```

注：java注释可以写在java代码行中

### JSP九大对象

1. request：请求对象；存储客户端向服务端发送的请求信息。

   常见方法

   - String getParameter(String name)：根据请求的字段名key，返回字段值value。
   - String[] getParameterValue(String name)：根据请求的字段名key，返回多个字段值value。常见如多选按钮中。
   - void setCharacterEncoding("utf-8")：设置请求编码
   - getRequestDispatcher(b.jsp).forward(request,response)：请求转发的方式跳转页面
   - getServerContext()：获取页面的ServletContext对象

   例子：用户注册及展示：

   ```jsp
   <%-- register.jsp --%>
   <%@ page language="java" contentType="text/html; charset=UTF-8"
       pageEncoding="UTF-8"%>
   <!DOCTYPE html>
   <html>
   <head>
   <meta charset="UTF-8">
   <title>用户注册页面</title>
   </head>
   <body>
   	<form action="show.jsp">
   		用户名：<input type="text" name = "uname"/><br>
   		密&nbsp;&nbsp;&nbsp;码：<input type="password" name = "upwd"/><br>
   		年&nbsp;&nbsp;&nbsp;龄：<input type="text" name = "uage"/><br>
   		爱&nbsp;&nbsp;&nbsp;好：<br>
   			<input type="checkbox" name = "uhobbies" value = "足球"/>足球
   			<input type="checkbox" name = "uhobbies" value = "篮球"/>篮球
   			<input type="checkbox" name = "uhobbies" value = "乒乓球"/>乒乓球
   		<input type="submit" value = "注册">
   	</form>
   </body>
   </html>
   ```

   ```jsp
   <%--show.jsp--%>
   <%@ page language="java" contentType="text/html; charset=UTF-8"
       pageEncoding="UTF-8"%>
   <!DOCTYPE html>
   <html>
   <head>
   <meta charset="UTF-8">
   <title>用户信息展示页面</title>
   </head>
   <body>
   	<% 
   		//设置编码
   		request.setCharacterEncoding("utf-8");
   		//通过uname获取name值
   		String name = request.getParameter("uname");
   		//通过upwd获取pwd值
   		String pwd = request.getParameter("upwd");
   		//通过uage获取age值
   		int age = Integer.parseInt(request.getParameter("uage"));
   		//通过uhobbies值获取hobbies数组
   		String[] hobbies = request.getParameterValues("uhobbies");
   	%>
   	注册成功，信息如下：<br>
   	姓名：<%=name %><br>
   	<%
   		if(age<=0||age>100){
   			age = 0;
   		}
   	%>
   	年龄：<%=age %><br>
   	密码：<%=pwd %><br>
   	爱好：
   	<%
   	if(pwd != null){
   		for(String hobby : hobbies){
   			out.print(hobby + "&nbsp;");
   			}
   		}
   	
   		
   	%>
   </body>
   </html>
   ```

   ![项目图解](http://ww1.sinaimg.cn/large/006zQofnly1gab0y6zouej30b706oq2y.jpg)

2. response：响应对象

   常见方法：

   - void addCookie(Cookie cookie)：服务端向客户端增减cookie对象
   - void sendRedirect(String location) throws IOException：页面跳转的一种方式（重定向）
   - void setContentType(String type)：设置服务端响应编码

   例子：用户登陆

   ```jsp
   <%@ page language="java" contentType="text/html; charset=UTF-8"
       pageEncoding="UTF-8"%>
   <!DOCTYPE html>
   <html>
   <head>
   <meta charset="UTF-8">
   <title>用户登陆页面</title>
   </head>
   <body>
   	<form action="check.jsp" method = "post">
   		用户名：<input type="text" name = "uname"/><br>
   		密&nbsp;&nbsp;&nbsp;码：<input type="password" name = "upwd"/><br>
   		年&nbsp;&nbsp;&nbsp;龄：<input type="text" name = "uage"/><br>
   		<input type="submit" value = "登陆">
   	</form>
   </body>
   </html>
   ```

   ```jsp
   <%@ page language="java" contentType="text/html; charset=UTF-8"
       pageEncoding="UTF-8"%>
   <!DOCTYPE html>
   <html>
   <head>
   <meta charset="UTF-8">
   <title>登陆校验页面</title>
   </head>
   <body>
   <%
   request.setCharacterEncoding("utf-8");
   String name = request.getParameter("uname");
   String pwd = request.getParameter("upwd");
   if(name.equals("zs") && pwd.equals("abc")){
   	//response.sendRedirect("success.jsp");//重定向方式出错
   	//使用requset的请求转发
   	request.getRequestDispatcher("success.jsp").forward(request, response);
   }else{
   	//登录失败
   	out.print("用户名或者密码错误");
   }
   
   %>
   </body>
   </html>
   ```

   ```jsp
   <%@ page language="java" contentType="text/html; charset=UTF-8"
       pageEncoding="UTF-8"%>
   <!DOCTYPE html>
   <html>
   <head>
   <meta charset="UTF-8">
   <title>欢迎界面</title>
   </head>
   <body>
   登陆成功！<br>
   欢迎您：
   <%
   String name = request.getParameter("uname");
   out.print(name);
   %>
   </body>
   </html>
   ```

   注：在使用response的重定向方式时会出现`欢迎您：null`的错误。而使用request的请求转发方式时会正确显示。

   [转：请求转发与重定向的区别->](https://blog.csdn.net/u010452388/article/details/80398929)

3. out：输出对象，向客户端输出内容

4. session

5. application 

6. config

7. pageContext

8. page

9. Exception

