---
title: GET和POST的区别
date: 2019-04-07 15:55:06
tags:
- 面试题
categories:
- HTTP
- 面试题
---

#### 面试常问之GET与POST

<!--more-->

GET和POST都是HTTP协议的请求方式，底层实现都是基于TCP/IP协议。

基础回答：

1. GET请求参数是放在URL里面的，POST是放在请求体中的；
2. GET请求的URL传参有长度限制，而POST请求没有
3. FET请求的参数只能是ASCII码，所以中文需要URL编码，而POST请求传参没有这个限制；

高级回答：(本质区别)

1. GET：向服务器获取指定资源；
2. POST：向服务器提交数据，数据放在请求体中；

总结：GET一般用于查询，而POST用于修改

