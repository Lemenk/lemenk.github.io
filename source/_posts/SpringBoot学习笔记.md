---
title: SpringBoot学习笔记
date: 2020-02-26 08:53:10
tags:
- 学习笔记
categories:
- SpringBoot
- 学习笔记
---

### SpringBoot学习笔记

<!--more-->

# 一、Spring Boot基础

## 1、Spring Boot 简介

简化Spring应用开发的一个框架；

整个Spring技术栈的一个大整合；

J2EE开发的一站式解决方案；

## 2、入门案例HelloWorld

实现功能：浏览器发送hello请求，服务器接受请求并处理，响应Hello World字符串；

### 1、步骤

1. 创建maven普通jar工程，导入spring boot依赖

   ~~~xml
   <parent>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-parent</artifactId>
           <version>1.5.9.RELEASE</version>
       </parent>
       <dependencies>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
       </dependencies>
   ~~~

2. 编写主程序，用来启动spring boot

   ~~~java
   /**
    * @SpringBootApplication :该注解标注一个主程序类，说明这是一个spring boot应用
    */
   @SpringBootApplication
   public class HelloWorldMainApplication {
   
       public static void main(String[] args) {
   
           //将Spring boot程序启动起来
           SpringApplication.run(HelloWorldMainApplication.class,args);
       }
   }
   ~~~

3. 编写相关controller、service相关类

   ~~~java
   @Controller
   public class HelloController {
   
       @ResponseBody
       @RequestMapping("/hello")
       public String hello(){
           return "Hello World";
       }
   }
   ~~~

4. 运行及部署

   - 运行：直接执行主程序

   - 部署：使用maven插件，将应用打包成可执行的jar包，直接使用**java -jar**命令执行。

     ~~~xml
     <build>
         <plugins>
             <plugin>
                 <groupId>org.springframework.boot</groupId><artifactId>spring-boot-maven-plugin</artifactId>
             </plugin>
         </plugins>
     </build>
     ~~~

### 2、使用Spring Initializer快速创建Spring Boot项目

使用Spring Initializer会联网创建一个Spring Boot项目。

- 主程序自动生成，只用编写业务逻辑
- resources文件夹中目录结构
  - static：保存所有的静态资源； js css  images；
  - templates：保存所有的模板页面；（Spring Boot默认jar包使用嵌入式的Tomcat，默认不支持JSP页面）；可以使用模板引擎（freemarker、thymeleaf）；
  - application.properties：Spring Boot应用的配置文件；可以修改一些默认设置；

~~~java
/**
 * @RestController 注解是@ResponseBody和@Controller结合
 */
@RestController
public class HelloController {

    @RequestMapping("/hello")
    public String hello(){
        return "hello world quick";
    }
}
~~~



### 3、配置文件

SpringBoot使用一个全局的配置文件，配置文件名是固定的；

- application.properties
- application.yml

配置文件的**作用**：修改SpringBoot自动配置的默认值；

#### 1、YAML

YAML（YAML Ain't Markup Language）

*YAML*是"YAML Ain't a Markup Language"（YAML不是一种标记语言的递归缩写。在开发的这种语言时，*YAML* 的意思其实是："Yet Another Markup Language"（仍是一种标记语言），但为了强调这种语言**以数据做为中心**，而不是以标记语言为重点，而用**反向缩略语重命名**。

与XML语言对比：

~~~YAml
#YAML方式
server:
  port: 8090
~~~

~~~xml
<!--XML方式-->
<server>
	<port>8090</port>
</server>
~~~

#### 2、YAML语法

##### 1、基本语法

k:(空格)v：表示一对键值对（空格必须有）；

以**空格**的缩进来控制层级关系；只要是左对齐的一列数据，都是同一个层级的

~~~yaml
server:
    port: 8090
    path: /hello
~~~

属性和值也是大小写敏感；

##### 2、值的写法

2.1、**普通值**（数字，字符串，布尔）

k: v：字面直接来写；

​		字符串默认不用加上单引号或者双引号；

​		""：双引号；不会转义字符串里面的特殊字符；特殊字符会作为本身想表示的意思

​				例子：name:   "zhangsan \n lisi"：输出；zhangsan 换行  lisi

​		''：单引号；会转义特殊字符，特殊字符最终只是一个普通的字符串数据

​				例子：name:   ‘zhangsan \n lisi’：输出；zhangsan \n  lisi

2.2、**对象、Map**（属性和值）（键值对）：

k: v：在下一行来写对象的属性和值的关系；注意缩进

​		对象还是k: v的方式

~~~yaml
friends:
		lastName: zhangsan
		age: 20
~~~

行内写法：

~~~yaml
friends: {lastName: zhangsan,age: 18}
~~~

2.3、**数组**（List、Set）：

用- 值表示数组中的一个元素

```yaml
pets:
 - cat
 - dog
 - pig
```

行内写法:

```yaml
pets: [cat,dog,pig]
```

#### 3、配置文件注入

3.1、编写配置文件：application.yml

~~~yaml
person:
  lastName: zhangsan
  age: 18
  boss: false
  birthday: 2012/05/23
  map: {k1: v1, k2: 2}
  list:
    - lisi
    - zhaoliu

  dog:
    name: 狗子
    age: 2
~~~

3.2、在javaBean中使用注解注入：

~~~java
/**
 * 将配置文件中配置的属性值映射到该组件中
 * @ConfigurationProperties ：该注解将本类中所有属性与配置文件中属性绑定
 * prefix = "person"：与配置文件中的进行绑定
 * @Component ：把普通pojo实例化到spring容器中
 */
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    //省略
}
~~~

3.3、添加pom坐标，配置文件处理器

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
~~~

3.4、测试类

~~~java
@RunWith(SpringRunner.class)
@SpringBootTest
public class ApplicationTests {

    @Autowired
    Person person = new Person();

    @Test
    public void contextLoads() {
        System.out.println(person);
    }

}
~~~

#### 4、相关注解

1. **@ConfigurationProperties**：该注解将本类中所有属性与配置文件中属性绑定。

   ~~~java
   @ConfigurationProperties(prefix = "person")
   ~~~

   `prefix = "person"`：与配置文件中的进行绑定

2. @**PropertySource**：加载指定的配置文件。

   `@PropertySource(value = {"classpath:person.properties"})`

   person.properties为指定的文件

3. @**ImportResource**：导入Spring的配置文件，让配置文件里面的内容生效；

   Spring Boot里面没有Spring的配置文件，我们自己编写的配置文件，也不能自动识别。@**ImportResource**需要标注在一个配置类上。

   ```xml
   @ImportResource(locations = {"classpath:beans.xml"})
   导入Spring的配置文件让其生效
   ```

   xml配置文件：
   
   ~~~xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <bean id="helloService" class="top.lemenk.springboot.service.HelloService">
   
       </bean>
   </beans>
   ~~~
   
   SpringBoot推荐给容器中添加组件的方式；推荐使用全注解的方式
   
   ~~~java
   @Configuration//指明当前类是配置类
   public class MyApplicationConfig {
   
       //将方法的返回值添加到容器中，容器中这些组件的默认的id就是方法名
       @Bean//给容器中添加组件
       public HelloService helloService(){
           System.out.println("配置类@Bean给容器中添加组件");
           return new HelloService();
       }
   }
   ~~~
   

#### 5、profile

我们在主配置文件编写的时候，文件名可以是   application-{profile}.properties/yml

默认使用application.properties的配置；

1. yml文件方式

   ~~~yaml
   server:
     port: 8081
   spring:
     profiles:
       active: prod
   
   ---
   server:
     port: 8083
   spring:
     profiles: dev
   
   
   ---
   
   server:
     port: 8084
   spring:
     profiles: prod  #指定属于哪个环境
   ~~~

2. 其他方式

   在配置文件中指定  spring.profiles.active={profile}



# 二、日志

## 1、常见日志

常见日志种类：JUL、JCL、Jboss-logging、logback、log4j、log4j2、slf4j....

**SpringBoot选用 SLF4j和logback；**

## 2、SLF4j使用

# 三、Web开发

|      | 普通CRUD（uri来区分操作） | RestfulCRUD       |
| ---- | ------------------------- | ----------------- |
| 查询 | getEmp                    | emp---GET         |
| 添加 | addEmp?xxx                | emp---POST        |
| 修改 | updateEmp?id=xxx&xxx=xx   | emp/{id}---PUT    |
| 删除 | deleteEmp?id=1            | emp/{id}---DELETE |

# 四、SpringBoot与数据访问

## 1、JDBC数据访问

1、pom文件依赖

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
~~~

2、配置文件：application.yml

~~~yaml
spring:
  datasource:
    url: jdbc:mysql://39.96.42.61:3307/springboot
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: 123456
~~~

3、测试：

~~~java
@Autowired
DataSource dataSource;

@Test
void contextLoads() throws SQLException {
    System.out.println(dataSource.getClass());
    Connection connection = dataSource.getConnection();
    System.out.println(connection);
    connection.close();
}
~~~

4、结果：

~~~java
Spring Boot2之后默认数据源为：
    class com.zaxxer.hikari.HikariDataSource
	可以在配置文件中配置
HikariProxyConnection@1195909598 wrapping com.mysql.cj.jdbc.ConnectionImpl@3d64c581
~~~

5、运行建表语句：

~~~yaml
schema-*.sql、data-*.sql
默认规则，将sql文件命名为：schema.sql，schema-all.sql；
默认在resources目录下寻找

也可以在配置文件中指定位置：
schema:
      - classpath:department.sql
      
***：在运行完建表语句后需要将改文件删除，否则会在下次重启服务器是覆盖掉该表
~~~

6、自动配置了JdbcTemplate

~~~java
@Controller
public class JdbcTemplateController {

    @Autowired
    JdbcTemplate jdbcTemplate;

    @ResponseBody
    @GetMapping("/query")
    public Map<String,Object> map(){
        List<Map<String, Object>> list = jdbcTemplate.queryForList("select * from department");
        return list.get(0);
    }
}
~~~

## 2、整合Mybatis

1、pom文件：

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<!--mybatis-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.1</version>
</dependency>

<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
<!--导入druid-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.10</version>
</dependency>
~~~

2、配置druid

~~~yaml
spring:
  #配置jdbc数据连接
  datasource:
    #基本配置
    url: jdbc:mysql://数据库地址
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: 123456
    initialization-mode: always
    type: com.alibaba.druid.pool.DruidDataSource
    #其他配置
    druid:
      initial-size: 5
      max-active: 100
      min-idle: 5
      max-wait: 60000
      pool-prepared-statements: true
      max-pool-prepared-statement-per-connection-size: 20
      validation-query: SELECT 1 FROM DUAL
      validation-query-timeout: 60000
      test-on-borrow: false
      test-on-return: false
      test-while-idle: true
      time-between-eviction-runs-millis: 60000
      min-evictable-idle-time-millis: 100000

      #监控配置#
      # WebStatFilter配置，说明请参考Druid Wiki，配置_配置WebStatFilter
      web-stat-filter.enabled: true
      web-stat-filter.url-pattern: /*
      web-stat-filter.exclusions: [.js,.gif,.jpg,.png,.css,.sql]
      # StatViewServlet配置，说明请参考Druid Wiki，配置_StatViewServlet配置
      stat-view-servlet.enabled: true
      #配置url打开，此时为http://localhost:8080/druid
      stat-view-servlet.url-pattern: /druid/*
      stat-view-servlet.reset-enable: false
      #配置登陆用户名和密码
      stat-view-servlet.login-username: admin
      stat-view-servlet.login-password: admin

      # 配置StatFilter
      filter:
        stat:
          db-type: mysql
          log-slow-sql: true
          slow-sql-millis: 5000
        # 配置WallFilter
        wall:
          enabled: true
          db-type: mysql
          #学习阶段需要开启
          config:
            delete-allow: true
            drop-table-allow: true
~~~

3、创建javaBean

4、mybatis对数据的CURD操作

1. 注解：

   - controller

     ~~~java
     @RestController
     public class DeptController {
     
         @Autowired
         DepartmentMapper departmentMapper;
     
         @GetMapping("/dept/{id}")
         public Department getDepartment(@PathVariable("id") Integer id){
             return departmentMapper.getDeptById(id);
         }
     
         @GetMapping("/dept")
         public Department insertDept(Department department){
             departmentMapper.insertDept(department);
             return department;
         }
     
     }
     ~~~

   - Mapper

     ~~~java
     @Mapper
     public interface DepartmentMapper {
     
         @Select("select * from department where id=#{id}")
         public Department getDeptById(Integer id);
     
         @Delete("delete from department where id=#{id}")
         public int deleteDeptById(Integer id);
     
         //自动生成递增主键，并指明是id属性
         @Options(useGeneratedKeys = true,keyProperty = "id")
         @Insert("insert into department(departmentName) value(#{departmentName})")
         public int insertDept(Department department);
     
         @Update("update department set departmentName=#{departmentName} where id=#{id}")
         public int updateDept(Department department);
     }
     ~~~

     

2. 配置文件：

   - 全局配置文件：mybatis-config.xml

     ~~~xml
     <?xml version="1.0" encoding="UTF-8" ?>
     <!DOCTYPE configuration
             PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
             "http://mybatis.org/dtd/mybatis-3-config.dtd">
     <configuration>
         <!--配置驼峰式-->
         <settings>
             <setting name="mapUnderscoreToCamelCase" value="true"/>
         </settings>
     </configuration>
     ~~~

   - Mapper

     ~~~java
     @Mapper
     public interface EmployeeMapper {
     
         public Employee getEmpById(Integer id);
     
         public void insertEmp(Employee employee);
     }
     ~~~

     

   - 具体类配置文件

     ~~~xml
     <mapper namespace="top.lemenk.data_mybatis.mapper.EmployeeMapper">
         <select id="getEmpById" resultType="top.lemenk.data_mybatis.bean.Employee">
             SELECT * FROM employee WHERE id=#{id}
         </select>
     
         <insert id="insertEmp">
             INSERT INTO employee(lastName,email,gender,d_id) VALUES (#{lastName},#{email},#{gender},#{dId})
         </insert>
     </mapper>
     ~~~

   - 主配置文件需要配置mybatis配置文件路径：

     ~~~yml
     mybatis:
       config-location: classpath:mybatis/mybatis-config.xml
       mapper-locations: classpath:mybatis/mapper/*.xml
     ~~~

   - web类

     ~~~java
     @RestController
     public class EmpController {
     
         @Autowired
         EmployeeMapper employeeMapper;
     
         @GetMapping("/emp/{id}")
         public Employee getEmp(@PathVariable Integer id){
             return employeeMapper.getEmpById(id);
         }
     }
     ~~~

## 3、整合SpringData JPA

1、导入pom依赖文件

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
~~~

2、编写实体类bean，并用注解和数据表进行映射，配置好映射关系

~~~java
//使用JPA注解配置映射关系
@Entity//告诉JPA这是一个实体类，与数据表映射
@Table(name = "tbl_user")//注定与之对应的数据表名，默认表名为类名小写
public class User {

    @Id//标识主键
    @GeneratedValue(strategy = GenerationType.IDENTITY)//自增主键
    private Integer id;

    @Column(name = "last_name",length = 50)//与数据表一个列对应
    private String lastName;

    @Column//若省略则默认数据表类名为属性名
    private String email;
}
~~~

3、编写Dao接口来操作实体类对应的数据表（Respository）

~~~java
/继承JpaRepository来完成对数据库的操作
public interface UserRepository extends JpaRepository<User,Integer>{}
~~~

4、在配置文件中进行基本配置

~~~yaml
spring:
  #JAP配置
  jpa:
    hibernate:
      #更新或者创建数据表结构
      ddl-auto: update
    #在控制台显示SQL
    show-sql: true
~~~

5、Controller

~~~java
@RestController
public class UserController {

    @Autowired
    UserRepository userRepository;

    @GetMapping("/user/{id}")
    public User getUser(@PathVariable Integer id){
        User user = userRepository.findById(id).orElse(null);
        return user;
    }

    @GetMapping("/user")
    public User insertUser(User user){
        User save = userRepository.save(user);
        return save;
    }
}
~~~

## 4、整合Redis

1. 安装Redis

   在linux的docker中加入Redis容器，并启动

2. 在pom文件加入依赖

   ~~~xml
   <!--集成redis-->
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-data-redis</artifactId>
   </dependency>
   ~~~

3. 在主配置文件中配置redis属性

   ~~~yaml
   #配置redis
     redis:
       host: 39.96.42.61
   ~~~

4. 测试

   ~~~java
   //导入Redis模板
   @Autowired
   StringRedisTemplate stringRedisTemplate;
   
   @Autowired
   RedisTemplate redisTemplate;
   
       /**
        * 测试redis的数据类型
        */
       @Test
       public void test01(){
           //给key为msg的值追加hello。
                 stringRedisTemplate.opsForValue().append("msg","hello");
       }
   ~~~

   ~~~shell
   127.0.0.1:6379>get msg
   "hello"
   ~~~

   



# 五、SpringBoot整合Swagger

## 1、Swagger简介

[swagger官网](https://swagger.io/)

- 号称最流行的API框架
- RestFul API文档在线自动生成，**同步更新**
- 直接运行，可在线测试
- 支持多种语言

## 2、在SpringBoot中集成Swagger

1、创建spring-web工程

2、导入swagger相关依赖

~~~xml
<!--导入swagger2依赖-->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>

<!--swagger-ui依赖-->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
~~~

3、编写一个hello工程

4、集成Swagger

~~~java
@Configuration  //声明配置类
@EnableSwagger2 //开启Swagger2
public class SwaggerConfig {
    
}
~~~

5、访问测试：http://localhost:8080/swagger-ui.html

## 3、配置Swagger

### 1、示例：修改APIinfo信息

~~~java
@Bean
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo());
    }
//配置Swagger信息：apiInfo
    private ApiInfo apiInfo(){

        //作者信息
        Contact contact = new Contact("Lemenk", "https://www.lemenk.top", "Lemenk@163.com");

        return new ApiInfo(
                "Lemenk的SwaggerAPI文档",
                "这是第一个swaggerAPI示例",
                "v1.0",
                "https://www.lemenk.top",
                contact,
                "Apache 2.0",
                "http://www.apache.org/licenses/LICENSE-2.0",
                new ArrayList<>()
        );
    }
~~~



### 2、配置扫描接口

~~~java
    //配置Swagger的Docket的bean实例
    @Bean
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                /**
                 * RequestHandlerSelectors:配置要扫描接口的方式
                 *      basePackage：指定要扫描的包
                 *      any()：扫描全部
                 *      none()：全部不扫描
                 *      withClassAnnotation：扫描类上的注解
                 *      withMethodAnnotation：扫描方法上的注解
                 */
                .apis(RequestHandlerSelectors.basePackage("top.lemenk.swagger.controller"))
                //.apis(RequestHandlerSelectors.withClassAnnotation(RestController.class))
                //.apis(RequestHandlerSelectors.withMethodAnnotation(GetMapping.class))
                /**
                 * 过滤路径
                 */
                .paths(PathSelectors.ant("/abc/**"))
                .build();
    }
~~~

### 3、配置是否启动Swagger

~~~java
    @Bean
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                //是否启动Swagger，ture为启动
                .enable(true)
    }
~~~

**问：如何根据当前开发环境判断是否开启Swagger？**

~~~java
//配置Swagger的Docket的bean实例
    @Bean
    public Docket docket(Environment environment){

        //设置要显示的Swagger环境
        Profiles profiles = Profiles.of("dev","prod");
        //通过environment.acceptsProfiles判断是否处在自己设定的环境当中
        boolean flag = environment.acceptsProfiles(profiles);

        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                //传入flage参数，使其根据环境判断是否开启
                .enable(flag)
~~~

需要在配置文件中配置当前开发环境，注：测试时需要同步修改端口

### 4、配置API文档分组

创建多个分组，只需要创建多个Docket即可。

~~~java
//配置多个分组，只需要创建多个Docket即可
    @Bean
    public Docket docket1(){
        return new Docket(DocumentationType.SWAGGER_2).groupName("AAA");
    }
    @Bean
    public Docket docket2(){
        return new Docket(DocumentationType.SWAGGER_2).groupName("BBB");
    }
    @Bean
    public Docket docket3(){
        return new Docket(DocumentationType.SWAGGER_2).groupName("CCC");
    }

//
.groupName("Lemenk")
~~~

### 5、注解

