---
title: MySQL的两种引擎的区别
date: 2019-07-07 15:26:48
tags:
- MySQL
- 面试题
categories:
- MySQL
- 面试题
---

**两种引擎：Innodb和MyISAM**

<!--more-->

**Innodb引擎概述**

Innodb引擎提供了对数据库[ACID事务](http://blog.lemenk.top/2019/09/10/%E4%BA%8B%E5%8A%A1%E5%8F%8A%E4%BA%8B%E5%8A%A1%E7%9A%84%E5%9B%9B%E5%A4%A7%E7%89%B9%E5%BE%81/#more)的支持，并且实现了SQL标准的四种隔离级别。该引擎还提供了行级锁和外键约束，它的设计目标是处理大容量数据库系统，它本身其实就是基于MySQL后台的完整数据库系统，MySQL运行时Innodb会在内存中建立缓冲池，用于缓冲数据和索引。但是该引擎不支持FULLTEXT类型的索引，而且它没有保存表的行数，当SELECT COUNT(*) FROM TABLE时需要扫描全表。当需要使用数据库事务时，该引擎当然是首选。由于锁的粒度更小，写操作不会锁定全表，所以在并发较高时，使用Innodb引擎会提升效率。但是使用行级锁也不是绝对的，如果在执行一个SQL语句时MySQL不能确定要扫描的范围，InnoDB表同样会锁全表。

**MyISAM引擎概述**

MyISAM是MySQL默认的引擎，但是它没有提供对数据库事务的支持，也不支持行级锁和外键，因此当INSERT(插入)或UPDATE(更新)数据时即写操作需要锁定整个表，效率便会低一些。不过和Innodb不同，MyISAM中存储了表的行数，于是SELECT COUNT(*) FROM TABLE时只需要直接读取已经保存好的值而不需要进行全表扫描。如果表的读操作远远多于写操作且不需要数据库事务的支持，那么MyISAM也是很好的选择。

总结：

1. MyISAM是非事务安全的，而InnoDB是事务安全的
2. MyISAM锁的粒度是表级的，而InnoDB支持行级锁
3. MyISAM支持全文类型索引，而InnoDB不支持全文索引
4. MyISAM相对简单，效率上要优于InnoDB，小型应用可以考虑使用MyISAM
5. MyISAM表保存成文件形式，跨平台使用更加方便

MyISAM是mysql默认的插件式存储引擎，适用于主要插入和查询记录；

InnoDB：用于事务处理，包括ACID事务支持（提供行级锁），适用于需要实现并发控制和事务（ACID）的项目。