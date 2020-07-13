---
title: String、StringBuffer和StringBuilder区别
date: 2019-05-20 17:33:45
tags:
- 面试
categories:
- 面试
- JAVA基础
---

#### 面试常问之String、StringBuffer 和 StringBuilder

<!--more-->

Q:String、StringBuffer 和 StringBuilder 的区别是什么？String 为什么是不可变的?

查看源码：

String：

```java
public final class String implements java.io.Serializable, Comparable<String>, CharSequence {
    private final char value[];
    /*省略*/
}
```

StringBuffer：

```java
public final class StringBuffer extends AbstractStringBuilder implements java.io.Serializable, CharSequence
{
    private transient char[] toStringCache;
    /*省略*/
}
```

```java
abstract class AbstractStringBuilder implements Appendable, CharSequence {
    char[] value;
    /*省略*/
}
```

StringBuilder:

```java
public final class StringBuilder extends AbstractStringBuilder implements java.io.Serializable, CharSequence
{ /*省略*/
}
```

通过查看源码可知，String类通过final修饰，且通过final修饰的字符数组保存字符串，所以String是不可变的。而StringBuffer和StringBuilder都继承自`AbstractStringBuilder`方法，而`AbstractStringBuilder`中字符串使用未被final修饰的字符数组修饰，所以是可变的。

以下通过两个方面来区分String、StringBuffer和StringBuilder：

**线程安全性**

String 中的对象是不可变的，也就可以理解为常量，线程安全。AbstractStringBuilder 是 StringBuilder 与 StringBuffer 的公共父类，定义了一些字符串的基本操作，如 expandCapacity、append、insert、indexOf 等公共方法。StringBuffer 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。StringBuilder 并没有对方法进行加同步锁，所以是非线程安全的。

**性能**

每次对 String 类型进行改变的时候，都会生成一个新的 String 对象，然后将指针指向新的 String 对象。StringBuffer 每次都会对 StringBuffer 对象本身进行操作，而不是生成新的对象并改变对象引用。相同情况下使用 StringBuilder 相比使用 StringBuffer 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。

**总结：**

- String用于操作少量的数据
- StringBuilder：单线程操作字符串缓冲区下操作大量数据
- StringBuffer：单线程操作字符串缓冲区下操作大量数据
- 三者都是不可被继承的