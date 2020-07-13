---
title: IO相关知识
date: 2019-06-27 13:48:25
tags:
- JAVA_IO
categories:
- JAVA_IO
---

**IO相关知识**

<!--more-->

**JAVA——IO相关知识**

<!--more-->

java.io.File类：只能新建、删除、重命名，不能查看文件内容的类

```java
import java.io.File;
/*略*/
File f = new File("D:/a.txt")
```

输入流：相对于程序，将数据写入到程序中

输出流：相对于程序，通过程序将数据输出到指定目录

文件流：基于文件

FileInputStream/FileOutputStream/FileReader/FileWriter

缓冲流：基于内存的操作

BufferedInputStream/BufferedoutputStream/BufferedReader/BufferedWriter

转换流：文件流与缓冲流的转换

InputStreamReader/OutputStreamWriter

对象流：把对象转化为数据流进行读写

ObjectInputStream/ObjectOutputStream

随机存取文件流：可以随机的在文件中的某一行读取/插入数据

RandomAccessFile

### 字节流：传输数据为字节的流

#### 一、字节输入流

常用的字节输入流主要有：

- **InputStream** 
- **FileInputStream**
- **BufferedInputStream**（是FilterInputStream的子类）

**InputStream** 为字节输入流的基类，以下为几个常用方法：

- read(byte[] b)：从流中读取b的长度个字节的数据存储到b中，返回结果是读取的字节个数（当再次读时，如果返回-1说明到了结尾，没有了数据）
- read(byte[] b, int off, int len)：从流中从off的位置开始读取len个字节的数据存储到b中，返回结果是实际读取到的字节个数（当再次读时，如果返回-1说明到了结尾，没有了数据）
- close()：关闭流，释放资源。

**FileInputStream**主要用来操作文件输入流，实现了父类的无参数的read()方法。

- int read()：返回值为int，字符的ASCll码。

```java
//字节输入流FileInputStream
public static void test1(){
    try {
        String path = "D:/a.java";
        FileInputStream fis = new FileInputStream(path);
        byte[] b = new byte[5];//创建一个字节长度为5的字符数组，用于临时存放读出的字符。
        int len = 0;
        while ((len = fis.read(b)) != -1){
            System.out.print(new String(b,0,len));//参数：字符数组；起始位置；字节数；
        }
        fis.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

**BufferedInputStream**：先将数据封装到内存中，然后从内存中读，所以它的效率要要非缓冲的要高。它是一种封装别的流以提高效率的流，所以它的初始化需要一个的InputStream流对象。

```java
//字节输入流BufferedInputStream
public static void test2(){
    try {
        String path = "D:/a.java";
        FileInputStream fis = new FileInputStream(path);
        BufferedInputStream bis = new BufferedInputStream(fis);//参数为其他流
        byte[] b = new byte[5];
        int len = 0;
        while ((len = bis.read(b)) != -1){
            System.out.print(new String(b,0,len));
        }
        bis.close();
        fis.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

#### 二、字节输出流

常用的字节输入流主要有：

- OutputStream
- FileOutputStream
- BufferedOutputStream（是FilterOutputStream的子类）

OutputStream是字节输出流的基类：

- write(byte[] b):将b的长度个字节数据写到输出流中。
- write(byte[] b,int off,int len):从b的off位置开始，获取len个字节数据，写到输出流中。
- flush():刷新输出流，把数据马上写到输出流中。
- close():关闭流，释放系统资源。

FileOutputStream是用于写文件的输出流：

- write(int b)：将b转成一个字节数据，写到输出流中。

FileOutputStream包含两个参数：1，所要写入的文件路径，若不存在则新建；2，是否使用追加写入默认，为true时代表在原有文件内容后面追加写入数据，默认为false。

```java
//字节输出流FileOutputStream
public static void test3() {
    try {
        String path = "D:/b.java";
        FileOutputStream fos = new FileOutputStream(path,true);//路径+是否追加
        String str = "I Like China!";
        fos.write(str.getBytes());
        fos.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

BufferedOutputStream

- write(int b):write(int b)：将b转成一个字节数据，写到输出流中。

```java
//字节输出流BufferedOutputStream
public static void test4() {
    try {
        String path = "D:/b.java";
        FileOutputStream fos = new FileOutputStream(path,true);//路径+是否追加
        BufferedOutputStream bos = new BufferedOutputStream(fos);//参数为其他流对象
        String str = "I Like China!";
        bos.write(str.getBytes());
        bos.close();
        fos.close();
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

### 字符流：传输数据的最基本单位是字符的流。

#### 一、字符输入流

字符流的类通常以reader和writer结尾

**Reader**：字符输入流的抽象父类

- read() ：读取单个字符，返回结果是一个int，需要转成char;到达流的末尾时，返回-1
- read(char[] cbuf):读取cbuf的长度个字符到cbuf这种，返回结果是读取的字符数，到达流的末尾时，返回-1
- close()  ：关闭流，释放占用的系统资源。

**InputStreamReader**：把InputStream中的字节数据流根据字符编码方式转成字符数据流

- read(char[] cbuf, int offset, int length) ：从offset位置开始，读取length个字符到cbuf中，返回结果是实际读取的字符数，到达流的末尾时，返回-1

- 需要一个字节输入流对象作为实例化参数。还可以指定第二个参数，第二个参数是字符编码方式，可以是编码方式的字符串形式，也可以是一个字符集对象

  ```java
  //字节转换输入流InputStreamReader 字节流--->字符流
  private static void test1(){
      try {
          //参数1：字节输入流对象作为实例化参数。参数2（可选）：是字符编码方式，可以是编码方式的字符串形式，也可以是一个字符集对象
          InputStreamReader isr = new InputStreamReader(new FileInputStream("D:/a.java"));
          int ch;
          while ((ch = isr.read())!=-1){
              System.out.print((char) ch);
          }
          isr.close();
      } catch (Exception e) {
          e.printStackTrace();
      }
  }
  ```

  

**FileReader**：把FileInputStream中的字节数据转成根据字符编码方式转成字符数据流

```java
//字符输入流FileReader
    private static void test2() {
        try {
            FileReader fr = new FileReader(filepath);
            char[] cbuf = new char[1024];
            int len;
            while ((len = fr.read(cbuf))!=-1){
                System.out.println(new String(cbuf,0,len));
            }
            fr.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

**BufferedReader**：把字符输入流进行封装，将数据进行缓冲，提高读取效率

- read(char[] cbuf, int offset, int length) ：从offset位置开始，读取length个字符到cbuf中，返回结果是实际读取的字符数，到达流的末尾时，返回-1

- readLine() ：读取一个文本行，以行结束符作为末尾，返回结果是读取的字符串。如果已到达流末尾，则返回 null

  ```java
  //字符缓冲输入流
      private static void test3(){
          try {
              FileReader fr = new FileReader(filepathin);
              BufferedReader bf = new BufferedReader(fr);//将字符文件输入流作为对象
              String line;
              while ((line=bf.readLine())!=null){
                  System.out.println(line);
              }
              bf.close();
              fr.close();
          } catch (Exception e) {
              e.printStackTrace();
          }
      }
  ```

  

#### 二、字符输出流

**Writer**：抽象父类

- write(char[] cbuf) :往输出流写入一个字符数组。
- write(int c) ：往输出流写入一个字符。
- write(String str) ：往输出流写入一串字符串。
- write(String str, int off, int len) :往输出流写入字符串的一部分。
- flush()：刷新输出流，把数据马上写到输出流中。
- close() ：关闭流，释放资源。 关闭之前回刷新内部缓冲区的数据，将其刷到目的位置。与flush的区别是flush刷新后，流可以继续使用，close刷新后将会将流关闭。

**OutputStreamWriter**：直接往流中写字符串数据，根据字符编码方式把字符数据转成字节数据再写给输出流。需要一个字节文件输出流对象作为参数

```java
//字符转换输出流OutputStreamWriter，字符流--->字节流
private static void test4(){    
    try {        
        //两个参数，路径+是否追加        
        FileOutputStream out = new FileOutputStream(filepathout,true);//创建一个字节文件输出流        
        OutputStreamWriter osw = new OutputStreamWriter(out);
        osw.write("abcdefg");        
        osw.close();    
    } catch (Exception e) {        
        e.printStackTrace();    }}
```

**FileWriter**：与OutputStreamWriter类似。

```java
//字符输出流
    private static void test5(){
        try {
            FileWriter fw = new FileWriter(filepathout,true);
            fw.write("ABCDEFG");
            fw.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

**BufferedWriter**：利用缓冲区来提高写入的效率。

- newLine():写入一行行分隔符。

  ```java
  //字符缓冲输出流
      private static void test6(){
          try {
              FileWriter fw = new FileWriter(filepathout,true);
              BufferedWriter bw = new BufferedWriter(fw);
              bw.write("我爱中国");
              bw.close();
              fw.close();
          } catch (IOException e) {
              e.printStackTrace();
          }
      }
  ```

  

