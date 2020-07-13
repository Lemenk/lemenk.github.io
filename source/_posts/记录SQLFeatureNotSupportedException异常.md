记录`SQLFeatureNotSupportedException`异常

数据库中`create_time`字段的类型是`datetime`

使用`mybatis-plus`逆向工程生成实体类后，`create_time`的类型为`LocalDateTime`

```java
private LocalDateTime createTime;
```

这是由于`mybatis-plus`在3.0以上默认时间为`LocalDateTime`类型。

至于为什么是`LocalDateTime`，可以

因此就会报错：

~~~java
org.springframework.dao.InvalidDataAccessApiUsageException: Error attempting to get column 'create_time' from result set.  Cause: java.sql.SQLFeatureNotSupportedException; null;nested exception is java.sql.SQLFeatureNotSupportedException
~~~

解决办法：

- **mybatis-plus版本降至3.1.0或以下即可**
- **也可以参考下面网友提供的其它解决方法**
- **[官方解决方案](https://mp.baomidou.com/guide/faq.html#error-attempting-to-get-column-create-time-from-result-set-cause-java-sql-sqlfeaturenotsupportedexception)： 1. 升级druid到1.1.21解决这个问题；2.保持mp版本3.1.0；3.紧跟mp版本，换掉druid数据源**