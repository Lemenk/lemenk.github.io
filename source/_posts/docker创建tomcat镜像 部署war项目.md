---
title: Docker中安装tomcat，并部署war项目
date: 2018-09-07 09:25:00
author: Lemenk
img: https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/docker.jpg
top: true
cover: false
coverImg: https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/docker.jpg
password: 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
toc: true
mathjax: false
summary: 阿里云服务器安装docker并启动Tomcat容器，部署war项目
categories: Docker
tags:
  - 教程
  - 网站运维
---

**Docker中安装tomcat，并部署war项目**

1. 新建war项目，并打成war包。
2. 安装docker
3. 拉取tomcat镜像
4. 启动tomcat容器
5. 上传war项目
6. 访问项目



教程开始。

# 一、新建war项目，并打成war包。

1、创建web项目。

<img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/docker_tomcat_war_01.png" alt="image-20200509000324552" style="zoom:50%;" />

<img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/docker_tomcat_war_02.png" alt="image-20200509000614459" style="zoom: 80%;" />

**简单实验项目就不做复杂功能，只做一个index页面**

<img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/docker_tomcat_war_03.png" alt="image-20200509000948707" style="zoom:50%;" />

<img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/docker_tomcat_war_04.png" alt="image-20200509001159607" style="zoom: 80%;" />

2、打包成war包。

<img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/docker_tomcat_war_05.png" alt="image-20200509001728107" style="zoom:50%;" />![image-20200509001939683](upload%5Cimage-20200509001939683.png)

<img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/docker_tomcat_war_05.png" alt="image-20200509001728107" style="zoom:50%;" />

![image-20200509001939683](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/docker_tomcat_war_06.png)

点击加号，选择项目的目录。

<img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/docker_tomcat_war_07.png" alt="image-20200509002148921" style="zoom:50%;" />![image-20200509002321736](upload%5Cimage-20200509002321736.png)

<img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/docker_tomcat_war_07.png" alt="image-20200509002148921" style="zoom:50%;" />

右键单击项目名称，选择`Put into Output Root`

![image-20200509002321736](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/docker_tomcat_war_08.png)

最终的结果。然后apply就好。

<img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/docker_tomcat_war_09.png" alt="image-20200509002654806" style="zoom:50%;" />

3、bulidwar包

点击bulid，选择Build Artifacts...

<img src="https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/docker_tomcat_war_10.png" alt="image-20200509002823186" style="zoom:50%;" />

选择这个war包，点击build或者Rebuild。

![image-20200509002958253](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/docker_tomcat_war_11.png)

这样就可以在图7中的目录里找到war包。

![image-20200509003250974](https://lemenk-img-aliyun.oss-cn-beijing.aliyuncs.com/img/image-20200509003250974.png)

# 二、安装docker，并拉取tomcat镜像。

我这里使用的是centOS7。

这里可以看我另一篇docker博客，有详细的命令介绍。

拉取tomcat镜像

~~~shell
$ docker  search tomcat  #查找tomcat镜像
$ docker pull tomcat  #拉取最后一个版本的tomcat镜像
$ docker images       #查看当前所有镜像
~~~

在某一个位置创建容器挂载的目录。我在`/usr/local/mytomcat/webapps`目录下

~~~shell
$ 创建容器 并挂在在这个目录下。
docker run -d --name mytomcat -p 80:8080  -v /usr/local/webapps:/usr/local/tomcat/webapps  -v /etc/localtime:/etc/localtime 927899a31456
# 参数说明：
	#-d：让其在后台运行；--name：把当前容器修改名称为mytomcat；
	#-p 80:8080：80为外部访问端口，简单来说就是浏览器中输入的端口，需要开启。8080为tomcat的端口。
	#-v：挂在目录。冒号之前为宿主机的目录，后者为docker的tomcat中的目录。
	#最后的数字川为docker images查询得到的这个镜像的id
~~~

容器创建成功，可根据`docker ps -a`查询正在运行的容器

~~~shell
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                  NAMES
f3239a4bc8b8        927899a31456        "catalina.sh run"   About an hour ago   Up About an hour    0.0.0.0:80->8080/tcp   mytomcat
~~~

进入容器内文件：

~~~shell
$ docker exec -it f3239a4bc8b8 /bin/bash    #f3239a4bc8b8为容器id
#ls 此时可以看到多个目录
BUILDING.txt  CONTRIBUTING.md  LICENSE	NOTICE	README.md  RELEASE-NOTES  RUNNING.txt  bin  conf  include  lib	logs  native-jni-lib  temp  webapps  webapps.dist  work
~~~

此时如果使用`公网ip地址:80`会报404错误，这是因为tomcat的自带项目文件在`webapps.dist`目录下，而webapps目录为空。可通过以下操作访问tomcat首页。

~~~shell
$ rm -rf webapps           #删除空文件夹
$ mv webapps.dist webapps  #重命名为webapps
~~~

此时可以通过公网ip地址访问tomcat首页。（80端口号可以省略）

# 三、上传war项目，访问项目。

将步骤一中制作好的Test.war包通过ftp上传到此前设置好的`/usr/local/mytomcat/webapps`目录下。

再次进入容器内，可以看到war包和已经呗tomcat服务器解压的Test文件夹

~~~shell
$ docker exec -it f3239a4bc8b8 /bin/bash
$ cd webapps
$ ls
Test Test.war
~~~

重启容器

~~~shell
$ docker restart f3239a4bc8b8
~~~

静待几分钟，等服务器启动刷新便可通过`公网ip地址:/Test`访问到该项目。

若需要只通过公网ip地址访问，可以修改Test.war包为ROOT.war。重启之后便可。

或者可以通过修改server.xml的方式。

至此结束。



# 四、总结

此次在服务器的docker部署Wab项目看似不难，但对于运维新手来说也并不是很快就能完成的。对于我这个菜鸟来说，花费了至少三个小时才完全搞明白，然后又花费了近四十分钟完成这篇博客。

其中易错点是：

1. 容器内的文件不能直接修改，必须得通过宿主机中的文件移动到webapps文件夹中，或者就使用我的方法进行目录挂载。
2. 在其中一次尝试中看教程修改conf/server.xml文件，但是依旧不能访问。后来果断放弃。到现在也不明白如何正确修改server.xml文件。
3. 就在最后一步，重启容器后需要等待一段时间。我有好几次倒在这一步，每次都是页面刷新转圈不能显示。然后我就心急寻找解决方案。最后一次是重启容器之后，恰好女朋友给我发消息，回复完后刷新页面竟然就访问成功了。啊哈哈哈，在这里得感谢女朋友。

2020年5月9日，凌晨1点15分。