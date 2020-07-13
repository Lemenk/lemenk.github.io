---
title: String、StringBuffer和StringBuilder区别
date: 2019-05-20 17:33:45
tags:
- MySQL
- 分享
categories:
- MySQL
- 教程
---



### 前言

在MySQL的学习中，由于MySQL版本的不同，其中有者许多差异，作为学习者，最好同时拥有多个版本的MySQL来学习。在查看网上众多教程之后，编写了这篇博客来记录我本次的学习历程，下面教程开始。

<!--more-->

### 教程开始

环境：Windows10

在此之前，我的电脑上已经安装好了MySQL8.0.15版本。其端口号默认为3306。

![image-20200318104307745](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200318104307745.png)

#### 1、下载MySQL安装包

#### 2、首先去MySQL官网下载安装包，选择需要的版本。**最好是ZIP压缩包**。[传送门](https://dev.mysql.com/downloads/mysql/)

#### 3、解压

将文件解压到需要的位置

![image-20200318104831286](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200318104831286.png)

#### 4、新建my.ini文件，并注意保存为ANSI格式。

```ini
[mysql]
# 设置mysql客户端默认字符集
 character-set-server = utf8
[mysqld]
#设置端口
port = 3305
# 设置mysql的安装目录
basedir=C:\Program Files\MySQL\mysql-5.6.25
# 设置mysql数据库的数据的存放目录
datadir=C:\Program Files\MySQL\mysql-5.6.25\data
# 允许最大连接数
max_connections=200
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
log_error=error.log
```

注：

1.修改mysql的安装目录

2.一般可以设置端口号为`3307`，由于我的阿里云服务器映射端口号为`3307`，为了区分，就设置为`3305`.

#### 5、在服务中关闭之前安装好的mysql服务。

#### 6、使用管理员打开cmd，执行以下命令。注意修改端口号和文件位置

```
mysqld -install mysql3305 --defaults-file="C:\Program Files\MySQL\mysql-5.6.25\my.ini"
```

出现Service successfully installed 说明成功了。

#### 7、修改注册表

![修改注册表](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200318105836816.png)

在HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\services下修改Imagepath。
（不一定是ControlSet001，有可能在ControlSet002、ControlSet003…中）

```shell
"C:\Program Files\MySQL\mysql-5.6.25\bin\mysqld" --defaults-file="C:\Program Files\MySQL\mysql-5.6.25\my.ini" mysql3305
```

#### 8、打开cmd管理员启动MySQL服务。

```shell
net start mysql3305
```

此时会正常启动。

#### 9、修改登陆密码。

默认为空密码，可使用以下命令进入

~~~shell
mysql -u root -p -P3305
~~~

输入密码是直接回车即可。

若要修改密码，可以使用以下方法。

~~~shell
#mysql> set password for 用户名@localhost = password('新密码'); 例子：
mysql> set password for root@localhost = password('root'); 
#上面例子将用户root的密码更改为root　；
~~~

#### 10、此时两个版本的MySQL就安装完成了。

![3305端口Mysql](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/20200318111736.png)

![3306端口Mysql](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/fafafa.png)

完结撒花！