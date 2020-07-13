---
title: Python学习笔记
date: 2019-11-01 16:36:46
tags:
- Python
categories:
- Python
---

**Python学习笔记**

<!--more-->

## 一、基础知识

### 1.了解python

- Python的创始人为吉多·范罗苏姆。
- Python是完全面向对象的语言。函数、模块、数字、字符串都是对象。并且完全支持继承、重载、派生、多重继承，有益于增强源代码的复用性。Python支持重载运算符，因此Python也支持泛型设计。相对于Lisp这种传统的函数式编程语言，Python对函数式编程]只提供了有限的支持。有两个标准库（functools, itertools）提供了与Haskell和Standard ML中类似的函数式程序设计工具。
- Python是可扩展的。提供了丰富的API和工具，以便程序员能够轻松地使用C、C++、Cython来编写扩展模块。Python编译器本身也可以被集成到其它需要脚本语言的程序内。
- 应用范围：
  - 网络应用程序
  - GUI开发
  - 数据挖掘、人工智能
  - 网络爬虫
  - 自动化脚本
- 两种版本：2.x和3.x

### 2.注释

- 单行注释

  ```python
  # 说明文字
  ```

- 多行注释

  ```python
  """
  说明文字
  说明文字
  """  
  
  '''
  说明文字
  说明文字
  '''
  ```

### 3.基本数据类型

Python中有六个标准的数据类型

- Number（数字）

  - int（整数）
  - float（浮点数）：1.23，3.1415
  - bool（布尔类型）：True和False
  - complex（复数）：如1+2j

- String（字符串）

  - Python中字符串用单引号`'`或者双引号`"`括起来

  - 字符串的下标及截取

    ```python
    # 下标
    str = 'Python'
    a = str[2]
    b = str[-3]
    # 字符串有两种索引方式，分别为从前和从后索引
    # 从前索引：索引值从0开始递增
    # 从后索引：从-1开始，-1，-2……递减
    ```

    ```python
    # 截取
    str = 'China'
    
    print(str)  # 输出字符串
    print(str[0:3])  #输出第一个到第四个之间所有的字符
    print(str[1:-1])  #输出第二个到倒数第一个之间的所有字符
    ```

- List（列表）：其使用最为频繁，类似于数组，但可以存放不同数据结构的元素

  - 截取方式与字符串相同

  - 输出

    ```python
    list = ['abcd', 123, 2.34]
    print(list)
    ```

    

- Tuple（元组）:

  元组（tuple）与列表类似，不同之处在于元组的元素不能修改。元组写在小括号 `()` 里，元素之间用逗号隔开。

  ```python
  tuple = ( 'abcd', 786 , 2.23, 'runoob', 70.2  )
  tinytuple = (123, 'runoob')
   
  print (tuple)             # 输出完整元组
  print (tuple[0])          # 输出元组的第一个元素
  print (tuple[1:3])        # 输出从第二个元素开始到第三个元素
  print (tuple[2:])         # 输出从第三个元素开始的所有元素
  print (tinytuple * 2)     # 输出两次元组
  print (tuple + tinytuple) # 连接元组
  ```

  

- Set（集合）:

  - 定义：集合（set）是由一个或数个形态各异的大小整体组成的，构成集合的事物或对象称作元素或是成员。

    基本功能是进行成员关系测试和删除重复元素。

    可以使用大括号 `{ }` 或者 `set()` 函数创建集合，注意：创建一个空集合必须用 `set()` 而不是 `{ }`，因为 `{ }` 是用来创建一个空字典。

  - 格式：

    ```python
    parame = {value01,value02,...}
    或者
    set(value)
    ```

    ```python
    student = {'Tom', 'Jim', 'Mary', 'Tom', 'Jack', 'Rose'}
     
    print(student)   # 输出集合，重复的元素被自动去掉
     
    # 成员测试
    if 'Rose' in student :
        print('Rose 在集合中')
    else :
        print('Rose 不在集合中')
     
     
    # set可以进行集合运算
    a = set('abracadabra')
    b = set('alacazam')
     
    print(a)
     
    print(a - b)     # a 和 b 的差集
     
    print(a | b)     # a 和 b 的并集
     
    print(a & b)     # a 和 b 的交集
     
    print(a ^ b)     # a 和 b 中不同时存在的元素
    ```

    以上实例输出结果：

    ```python
    {'Mary', 'Jim', 'Rose', 'Jack', 'Tom'}
    Rose 在集合中
    {'b', 'a', 'c', 'r', 'd'}
    {'b', 'd', 'r'}
    {'l', 'r', 'a', 'c', 'z', 'm', 'b', 'd'}
    {'a', 'c'}
    {'l', 'r', 'z', 'm', 'b', 'd'}
    ```

- Dictionary（字典）：

  - 定义：字典是一种映射类型，字典用 `{ }` 标识，它是一个无序的 `键(key) : 值(value)` 的集合。

    键(key)必须使用不可变类型。

    在同一个字典中，键(key)必须是唯一的。

    ```python
    dict = {}
    dict['one'] = "1 - 菜鸟教程"
    dict[2]     = "2 - 菜鸟工具"
     
    tinydict = {'name': 'runoob','code':1, 'site': 'www.runoob.com'}
     
     
    print (dict['one'])       # 输出键为 'one' 的值
    print (dict[2])           # 输出键为 2 的值
    print (tinydict)          # 输出完整的字典
    print (tinydict.keys())   # 输出所有键
    print (tinydict.values()) # 输出所有值
    
    '''输出：
    1 - 菜鸟教程
    2 - 菜鸟工具
    {'name': 'runoob', 'code': 1, 'site': 'www.runoob.com'}
    dict_keys(['name', 'code', 'site'])
    dict_values(['runoob', 1, 'www.runoob.com'])
    '''
    ```


### 4.数字(Number)及其常用函数

- 支持三种不同的数值类型：

  - 整型(Int)：无大小限制的正数或者负数
  - 浮点型(float)：由整数部分和小数部分组成，也可以用科学计数法表示
  - 复数(complex)：可以用`a+bj`或者`complex(a,b)`来表示，并且复数的实部和虚部都是浮点型

- 数字类型转换：

  - **int(x)** 将x转换为一个整数。
  - **float(x)** 将x转换到一个浮点数。
  - **complex(x)** 将x转换到一个复数，实数部分为 x，虚数部分为 0。
  - **complex(x, y)** 将 x 和 y 转换到一个复数，实数部分为 x，虚数部分为 y。x 和 y 是数字表达式。

- 数学函数：

  - `abs(x)`：返回数字的绝对值，如abs(-10) 返回 10
  - `ceil(x)`：返回数字的上入整数，如math.ceil(4.1) 返回 5
  - `exp(x)`：返回e的x次幂(ex),如math.exp(1) 返回2.718281828459045
  - `fabs(x)`：返回数字的绝对值，如math.fabs(-10) 返回10.0
  - `max(x1, x2,...)`：返回给定参数的最大值，参数可以为序列。
  - `log10(x)`：返回以10为基数的x的对数，如math.log10(100)返回 2.0
  - `pow(x, y)`：x**y 运算后的值。
  - `round(x [,n])`：返回浮点数x的四舍五入值，如给出n值，则代表舍入到小数点后的位数。
  - `sqrt(x)`：返回数字x的平方根。

- 随机数函数

  | 函数                               | 描述                                                         |
  | :--------------------------------- | :----------------------------------------------------------- |
  | choice(seq)                        | 从序列的元素中随机挑选一个元素，比如random.choice(range(10))，从0到9中随机挑选一个整数。 |
  | randrange ([start,\] stop [,step]) | 从指定范围内，按指定基数递增的集合中获取一个随机数，基数默认值为 1 |
  | random()                           | 随机生成下一个实数，它在[0,1)范围内。                        |
  | seed([x\])                         | 改变随机数生成器的种子seed。                                 |
  | shuffle(lst)                       | 将序列的所有元素随机排序                                     |
  | uniform(x, y)                      | 随机生成下一个实数，它在[x,y]范围内。                        |

- 数学常量

  - pi：圆周率，一般用Π来表示
  - e，数学常量e，即自然常数

### 5.字符串常用方法

- `capitalize()`：将字符串的第一个字符转换为大写
- `center(width, fillchar)`：返回一个指定的宽度 width 居中的字符串，fillchar 为填充的字符，默认为空格。
- `count(str, beg= 0,end=len(string))`：返回 str 在 string 里面出现的次数，如果 beg 或者 end 指定则返回指定范围内 str 出现的次数
- `find(str, beg=0, end=len(string))`：检测 str 是否包含在字符串中，如果指定范围 beg 和 end ，则检查是否包含在指定范围内，如果包含返回开始的索引值，否则返回-1
- `index(str, beg=0, end=len(string))`：跟find()方法一样，只不过如果str不在字符串中会报一个异常.
- `lower()`：转换字符串中所有大写字符为小写.
- `lstrip()`：截掉字符串左边的空格或指定字符。
- `replace(old, new [, max])`：把 将字符串中的 str1 替换成 str2,如果 max 指定，则替换不超过 max 次。
- `rstrip()`：删除字符串字符串末尾的空格.
- `split(str="", num=string.count(str))`：num=string.count(str)) 以 str 为分隔符截取字符串，如果 num 有指定值，则仅截取 num+1 个子字符串
- `upper()`：转换字符串中的小写字母为大写

### 6.列表函数

| 方法                                | 方法描述                                                     |
| :---------------------------------- | :----------------------------------------------------------- |
| list.append(obj                     | 在列表末尾添加新的对象                                       |
| list.count(obj)                     | 统计某个元素在列表中出现的次数                               |
| list.extend(seq)                    | 在列表末尾一次性追加另一个序列中的多个值（用新列表扩展原来的列表） |
| list.index(obj)                     | 从列表中找出某个值第一个匹配项的索引位置                     |
| list.insert(index, obj)             | 将对象插入列表                                               |
| list.pop(index=-1\])                | 移除列表中的一个元素（默认最后一个元素），并且返回该元素的值 |
| list.remove(obj)                    | 移除列表中某个值的第一个匹配项                               |
| list.reverse()                      | 反向列表中元素                                               |
| list.sort( key=None, reverse=False) | 对原列表进行排序                                             |
| list.clear()                        | 清空列表                                                     |
| list.copy()                         | 复制列表                                                     |

### 7.元组方法

- `len(tuple)`：计算元组元素个数。
- `max(tuple)`：返回元组中元素最大值。
- `min(tuple)`：返回元组中元素最小值。
- `tuple(seq)`：将列表转换为元组。

### 8.变量定义

- python是弱类型语言，并不需要事先定义变量类型

  ```python
  a = 10  # int类型
  name = '小明'  # str类型
  height = 1.80  # float类型
  flag = True  # bool类型
  ```

- 查看变量类型

  ```python
  print(type(name))
  print(type(a))
  print(type(10.0))
  ```

### 9.命名规范

- 常量使用以下划线分隔的大写命名

  ```python
  MAX_OVERFLOW = 100
  ```

- 变量名尽量小写, 如有多个单词，用下划线连接

  ```python
  school_name = '北京大学'
  ```

- 函数名一律小写，如有多个单词，用下划线隔开,私有函数在函数名前加一个下划线

- 类名使用大驼峰风格

- 模块名使用小驼峰，避免使用下划线

### 10.导入模块

- 使用import关键字

  ```python
  import keyword
  ```

- 利用keyword模块查看关键字

  ```python
  print(keyword.kwlist)
  ```

- 关键字

  ```python
  ['False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
  ```

  

### 11.输出

- 常用格式符号

  ```python
  % s : 字符串(采用str()的显示)
  % r : 字符串(采用repr()的显示)
  % c : 单个字符
  % b : 二进制整数
  % d : 十进制整数
  % i : 十进制整数
  % o : 八进制整数
  % x : 十六进制整数
  % e : 指数(基底写为e)
  % E : 指数(基底写为E)
  % f : 浮点数
  % F : 浮点数，与上相同
  % g : 指数(e)或浮点数(根据显示长度)
  % G : 指数(E)或浮点数(根据显示长度)
  % % : 字符"%"
  ```

- 普通输出

  ```python
  age = 10
  print('今年我%d岁了' % age)
  ```

- 多个变量输出

  ```python
  name = '小明'
  age = 18
  print('姓名：%s，年龄：%d' % (name, age))
  ```

- 换行输出

  ```python
  print('您好\n世界')
  ```

### 12.输入

- 输入 input()函数

  ```python
  tel = input('请输入手机号：')
  print(tel)
  ```

### 13.算术运算符

- 常见运算符

  +、-、*、/

- 其他运算符

  //：整除运算符，例如9//2的结果为4

  **：指数运算符，例如

  ```python
  2**3
  ```

  结果为8

- 运算优先级：`**`高于 `*` `/` `%` `//`高于`+` `-`，但避免歧义，应使用`()`来处理优先级 

### 14.运算符

- 比较运算符

  ```python
  # 定义两个变量
  a = 10
  b = 10
  
  # ==	检查两个操作数的值是否相等，如果是则条件变为真。
  # ret = (a == b)
  # print(ret)
  
  # !=	检查两个操作数的值是否相等，如果值不相等，则条件变为真。
  # if a != b:
  #     print('a不等于b')
  
  # >	检查左操作数的值是否大于右操作数的值，如果是，则条件成立。
  # if a > b:
  #     print("a大于b")
  
  # <=	检查左操作数的值是否小于或等于右操作数的值，如果是，则条件成立。
  # if a <= b:
  #     print("a小于等于b")
  ```

- 逻辑运算符

  - and：和运算
  - or：或运算
  - not非运算

- 成员运算符

  | 运算符 | 描述                                                    |
  | ------ | ------------------------------------------------------- |
  | in     | 如果在指定的序列中找到值返回 True，否则返回 False。     |
  | not in | 如果在指定的序列中没有找到值返回 True，否则返回 False。 |

  ```python
  a = 10
  b = 20
  list = [1, 2, 3, 4, 5 ];
   
  if ( a in list ):
     print ("1 - 变量 a 在给定的列表中 list 中")
  else:
     print ("1 - 变量 a 不在给定的列表中 list 中")
   
  if ( b not in list ):
     print ("2 - 变量 b 不在给定的列表中 list 中")
  else:
     print ("2 - 变量 b 在给定的列表中 list 中")
   
  # 修改变量 a 的值
  a = 2
  if ( a in list ):
     print ("3 - 变量 a 在给定的列表中 list 中")
  else:
     print ("3 - 变量 a 不在给定的列表中 list 中")
     
  '''
  1 - 变量 a 不在给定的列表中 list 中
  2 - 变量 b 不在给定的列表中 list 中
  3 - 变量 a 在给定的列表中 list 中
  '''
  ```

- 身份运算符

  - is：描述两个标识符是否引自同一对象	
  - is not：判断两个标识符是不是引自不同对象
  - **is和==的区别：is 用于判断两个变量引用对象是否为同一个， == 用于判断引用变量的值是否相等。**

- 位运算符

  ```python
  a = 0011 1100  # 60的二进制
  
  b = 0000 1101  # 13的二进制
  
  -----------------
  
  a&b = 0000 1100  # 按位与：对应位值相同为1，否则为0
  
  a|b = 0011 1101  # 按位或：对应位若有一个为1，则结果的该位为1
  
  a^b = 0011 0001  # 按位异或：对应位不同时结果为1
  
  ~a  = 1100 0011  # 按位取反：对每一位取反
  
  # << ： 左移运算符：各位左移若干位，高位丢弃，低位补零 
  # >> ： 右移运算符：向右移动若干位
  ```

  

### 15.选择结构语句

#### 15.1 if语句基本格式

- 基本格式

  ```python
  if 要判断的条件:
  	条件成立时，需要做的事情
  ```

- 例子

  ```python
  num = 13
  if num > 10:
  	print('该数字大于10')
  ```

#### 15.2 if-else

- 如果一个条件成立, 做一个事情, 如果条件不成立, 做另一个事情

- 格式：

  ```python
  if 判断条件:
    如果成立, 执行的代码
  else:
    如果不成立, 执行的代码
  ```

#### 15.3 if-elif-else

格式：

```python
if 判断条件1:
  	pass
elif 判断条件2:
  	pass
elif 判断条件3:
  	pass
else:
 		以上都不满足才可以执行
```

#### 15.4 三目运算符

格式：

```python
变量 = 如果条件成立 if 判断条件 else 如果条件不成立
```

例子：

```python
max = a if a > b else b
```

#### 15.5 while循环

- 格式：

  ```python
  定义一个计数器
  定义while循环
  书写循环体代码
  修改计数器
  ```

- 例：打印4次hello word

  ```python
  i = 0
  while i <= 3
  	print('hello word')
  	i +=1
  ```

#### 15.6 for循环

- 格式：

  ```python
  for <variable> in <sequence>:
      <statements>
else:
      <statements>
  ```
  
- 实例：

  ```python
  languages = ["C", "C++", "Perl", "Python"] 
  for x in languages:
      print (x)
  
  '''输出
  C
  C++
  Perl
  Python
  '''
  ```

#### 15.7 break和continue语句

- break 语句可以跳出 for 和 while 的循环体。如果你从 for 或 while 循环中终止，任何对应的循环 else 块将不执行。

- continue语句被用来告诉 Python 跳过当前循环块中的剩余语句，然后继续进行下一轮循环。

- 实例：

  ```python
  # while中的break语句
  n = 5
  while n > 0:
      n -= 1
      if n == 2:
          break
      print(n)
  print('循环结束。')
  ```

  输出结果：

  ```python
  4
  3
  循环结束。
  ```

  while中的continue语句：

  ```python
  n = 5
  while n > 0:
      n -= 1
      if n == 2:
          continue
      print(n)
  print('循环结束。')
  ```

  输出结果：

  ```python
  4
  3
  1
  0
  循环结束。
  ```

#### 15.8 迭代器与生成器

- 两个方法：`iter()`和`next()`

- 字符串，列表或元组对象都可用于创建迭代器

  ```python
  list=[1,2,3,4]
  it = iter(list)    # 创建迭代器对象
  print (next(it))   # 输出迭代器的下一个元素
  ```

  - 通过for循环遍历

    ```python
    list=[1,2,3,4]
    it = iter(list)    # 创建迭代器对象
    for x in it:
        print (x, end=" ")
    ```

  - 使用next()函数

    ```python
    import sys         # 引入 sys 模块
     
    list=[1,2,3,4]
    it = iter(list)    # 创建迭代器对象
     
    while True:
        try:
            print (next(it))
        except StopIteration:
            sys.exit()
    ```



### 16.python中的正则表达式

Python自1.5版本起增加了re模块，用于支持正则表达式。

- `re.match`函数：从字符串的起始位置匹配一个模式，**若匹配成功返回match对象，若匹配失败则返回None**。

  ```python
  re.match(pattern, string, [flags])
  ```

  - 参数说明：

    - `pattern`：匹配的正则表达式
    - `string`：要匹配的字符串。
    - `flags`：标志位，用于控制正则表达式的匹配方式

  - flags可用值：

    | 修饰符 | 描述                                                         |
    | :----- | :----------------------------------------------------------- |
    | re.I   | 使匹配对大小写不敏感                                         |
    | re.L   | 做本地化识别（locale-aware）匹配                             |
    | re.M   | 多行匹配，影响 ^ 和 $                                        |
    | re.S   | 使 . 匹配包括换行在内的所有字符                              |
    | re.U   | 根据Unicode字符集解析字符。这个标志影响 \w, \W, \b, \B.      |
    | re.X   | 该标志通过给予你更灵活的格式以便你将正则表达式写得更易于理解。 |

  - 可以使用group(num) 或 groups() 匹配对象函数来获取匹配表达式。

    - `group(num=0)`：匹配的整个表达式的字符串，group() 可以一次输入多个组号，在这种情况下它将返回一个包含那些组所对应值的元组。

    - `groups()`：返回一个包含所有小组字符串的元组，从 1 到 所含的小组号。

    - 实例：

      ```python
      #!/usr/bin/python3
      import re
       
      line = "Cats are smarter than dogs"
      # .* 表示任意匹配除换行符（\n、\r）之外的任何单个或多个字符
      matchObj = re.match( r'(.*) are (.*?) .*', line, re.M|re.I)
       
      if matchObj:
         print ("matchObj.group() : ", matchObj.group())
         print ("matchObj.group(1) : ", matchObj.group(1))
         print ("matchObj.group(2) : ", matchObj.group(2))
      else:
         print ("No match!!")
      ```

      运行结果：

      ```python
      matchObj.group() :  Cats are smarter than dogs
      matchObj.group(1) :  Cats
      matchObj.group(2) :  smarter
      ```

  - 其中match对象包含了匹配值的位置和匹配数据，可使用方法得到。

    ```python
    匹配值的起始位置： match1.start()
    匹配值的结束位置：match1.end()
    匹配位置的元组： match1.span()
    被匹配的字符串：match1.string
    匹配数据： match1.group()
    ```

  - 例子：

    ```python
    import re
    
    
    pattern = r'mr_\w+'
    string = 'MR_SHOP mr_shop'
    match1 = re.match(pattern, string, re.I)
    print(match1)
    string = 'new MR_SHOP mr_shop'
    match2 = re.match(pattern, string, re.I)
    print(match2)
    
    print('匹配值的起始位置：', match1.start())
    print('匹配值的结束位置：', match1.end())
    print('匹配位置的元组：', match1.span())
    print('被匹配的字符串：', match1.string)
    print('匹配数据：', match1.group())
    ```

    运行结果：

    ```python
    <re.Match object; span=(0, 7), match='MR_SHOP'>
    None
    匹配值的起始位置： 0
    匹配值的结束位置： 7
    匹配位置的元组： (0, 7)
    被匹配的字符串： MR_SHOP mr_shop
    匹配数据： MR_SHOP
    ```

- `re.search`方法：扫描整个字符串并返回第一个成功的匹配。**匹配成功re.search方法返回一个匹配的对象，否则返回None。**

  - 函数语法：

    ```python
    re.search(pattern, string, [flags])
    ```

    参数说明：

    - `pattern`：匹配的正则表达式
    - `string`：要匹配的字符串。
    - `flags`：标志位，用于控制正则表达式的匹配方式

  - 示例：

    ```python
    import re
    
    pattern = r'mr_\w+'
    string = 'MR_SHOP mr_shop'
    search = re.search(pattern, string, re.I)
    print(search)
    ```

  - **re.match与re.search的区别**：

    re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。

- `findall()`方法：用于在整个字符串中搜索所有符合正则表达式的字符串，并以列表返回。若匹配成功，则返回包含匹配结构的列表，若匹配失败，则返回空列表。

  - 语法格式：

    ```python
    re.findall(pattern, string, [flags])
    ```

  - 参数说明：

    - pattern：模式字符串，正则表达式
    - string：要匹配的字符串
    - flags：标志位，可选

  - 示例：

    ```python
    import re
    
    pattern = r'mr_\w+'
    string = 'MR_SHOP mr_shop'
    find = re.findall(pattern, string, re.I)
    print(find)
    print(type(find))
    ```

    运行结果：

    ```python
    ['MR_SHOP', 'mr_shop']
    <class 'list'>
    ```

  - 运用：分割IP地址
  
    ```python
    import re
    
    
    pattern = r'([1-9]{1,3}(\.[0-9]{1,3}){3})'
    str = '127.0.0.1 192.168.1.66 187.0.1.9'
    match = re.findall(pattern, str)
    for item in match:
        print(item[0])
    ```
  
    运行结果：
  
    ```python
    127.0.0.1
    192.168.1.66
    187.0.1.9
    ```
  
    注：`item[0]`需要索引`0`的必要性：match对象的结果是一个列表，其中包含了三个元组，每一个元组包含了两个字符串。
  
    直接输出item的结果是：
  
    ```python
    ('127.0.0.1', '.1')
    ('192.168.1.66', '.66')
    ('187.0.1.9', '.9')
    ```
  
    第一个字符串是所要输出的字符串，因此需要索引号。
  
- `re.sub()`方法：用于实现字符串的替换。返回值为字符串。

  - 语法格式：

    ```python
    re.sub(pattern, repl, string, [count], [flags])
    ```

    - pattern：表示模式字符串，正则表达式
    - repl：表示要替换的字符串
    - string：表示要被查找替换的原始字符串
    - count：可选，表示模式匹配后替换的最大次数，默认值为0，表示替换所有
    - flags：可选，标志位，用于控制匹配方式

  - 示例：

    ```python
    import re
    
    pattern = r'(黑客)|(抓包)|(监听)|(Trojan)'
    about = '我是一名黑客，喜欢抓包，监听网络，想研究Trojan。'
    sub = re.sub(pattern, '**', about)
    print(sub)
    ```

    输出结果：

    ```python
    我是一名**，喜欢**，**网络，想研究**。
    ```

- `re.split()`：分割字符串。返回值为列表。

  - 格式：

    ```python
    re.split(pattern, string, [maxsplit], [flags])
    ```

    - maxsplit：可选，表示最大可拆分的次数
    - flags：标志位

  - 示例：

    ```python
    import re
    
    pattern = r'[?|&]'
    url = 'http://www.baidu.com/login.jsp?username="liming"&pwd="123"'
    result = re.split(pattern, url)
    print(result)
    ```

    输出结果：

    ```python
    ['http://www.baidu.com/login.jsp', 'username="liming"', 'pwd="123"']
    ```

### 17.函数

1. 一般语法：使用def关键字

   ```python
   def 函数名（参数列表）:
       函数体
   ```

   实例：

   ```python
   # 在控制台输出hello world!
   
   def hello():
       print("Hello World!")
   ```

   示例二：

   ```python
   # 计算长方形面积函数
   def area(width, height):
       return width * height
   
   
   def print_welcome(name):
       print("Welcome to", name)
   
   
   print_welcome("python")
   w = 4
   h = 5
   print("width =", w, "height =", h, "area =", area(w, h))
   ```

   输出结果：

   ```python
   Welcome to python
   width = 4 height = 5 area = 20
   ```

   示例三：

   ```python
   # 可写函数说明
   def changeme( mylist ):
      "修改传入的列表"
      mylist.append([1,2,3,4])
      print ("函数内取值: ", mylist)
      return
    
   # 调用changeme函数
   mylist = [10,20,30]
   changeme( mylist )
   print ("函数外取值: ", mylist)
   ```

   输出结果：

   ```
   函数内取值:  [10, 20, 30, [1, 2, 3, 4]]
   函数外取值:  [10, 20, 30, [1, 2, 3, 4]]
   ```

2. return语句：用于退出函数，选择性地向调用方返回一个表达式。不带参数值的return语句返回None。

### 18.面向对象编程

#### 18.1 类的定义：

```python
class 类名:
	'''类的帮助信息'''
	statement
```

示例一：

```python
class Geese:
    """大雁类"""
    print("我是大雁")


wildGoose = Geese()
print(wildGoose)
```

输出结果：

```
我是大雁
<__main__.Geese object at 0x000001AF79E3FFA0>
```

`_init_()`方法：类似于JAVA的构造方法，在创建类的实例时，会自动调用该方法。`_init_()`必须包含一个self参数，并且是第一个参数。self参数是指向实力本身的引用，用于访问类的属性和方法，在方法被调用时主动传递实际参数self。

```python
class Geese:
    """大雁类"""

    def __init__(self, beak, wing, claw):
        print("我是大雁类，包含以下特征：")
        print(beak)
        print(wing)
        print(claw)


beak_1 = "喙的基部比较高，长度和头部的长度几乎相同"
wing_1 = "翅膀长而尖"
claw_1 = "爪子是蹼状的"
wildGoose = Geese(beak_1, wing_1, claw_1)

```

运行结果：

```python
我是大雁类，包含以下特征：
喙的基部比较高，长度和头部的长度几乎相同
翅膀长而尖
爪子是蹼状的
```

**注：`__init__()`方法前后时两个下划线。**

#### 18.2 类的实例方法：

```python
def functionName(self, paramenterlist):
    block
```

例：

```python
class Geese:
    """大雁类"""

    def __init__(self, beak, wing, claw):
        print("我是大雁类，包含以下特征：")
        print(beak)
        print(wing)
        print(claw)

    def fly(self, state):       # 定义实例方法
        print(state)


beak_1 = "喙的基部比较高，长度和头部的长度几乎相同"
wing_1 = "翅膀长而尖"
claw_1 = "爪子是蹼状的"
wildGoose = Geese(beak_1, wing_1, claw_1)
wildGoose.fly("我飞行的时候，一会儿排成人字，一会儿排成一个一字")
```

运行结果：

```
我是大雁类，包含以下特征：
喙的基部比较高，长度和头部的长度几乎相同
翅膀长而尖
爪子是蹼状的
我飞行的时候，一会儿排成人字，一会儿排成一个一字
```

#### 18.3 访问权限

- `_fun`：以单下划线开头表示`protected`（保护）类型的成员，只允许类的本身和子类进行访问，不能用`from module import *`语句导入。

  ```python
  class Swan:
      """天鹅类"""
      _neck_swan = '天鹅的脖子很长'
  
      def __init__(self):
          print("__init__()", Swan._neck_swan)  # 在实例方法中访问
  swan = Swan()
  print("直接访问：", swan._neck_swan)  #通过实例访问
  ```

  运行结果：

  ```
  __init__() 天鹅的脖子很长
  直接访问： 天鹅的脖子很长
  ```

- `__fun`：双下划线表示private（私有）类型的成员，只允许定义该方法的类本身进行访问，而且也不能通过类的实例进行访问，但是可以通过“类的实例名.类名_XXX”方式进行访问。

  ```python
  # private
  class Swan:
      """天鹅类"""
      __neck_swan = '天鹅的脖子很长'
  
      def __init__(self):
          print("__init__()", Swan.__neck_swan)
  
  
  swan = Swan()
  print("加入类名访问：", swan._Swan__neck_swan)
  # print("直接访问：", swan.__neck_swan)
  ```

  运行结果：

  ```python
  __init__() 天鹅的脖子很长
  加入类名访问： 天鹅的脖子很长
  ```

- `__fun__()`定义特殊方法，如`__init__()`。

#### 18.4 属性

可以通过`@property`（装饰器）将一个方法转化为属性，从而实现用于计算的属性。将方法转化为属性后，可以通过方法名来访问方法，而不需要再添加小括号。

- 语法格式：

  ```python
  @property
  def methodname(self):
      block
  ```

- 例子：

  ```python
  class Rect:
      def __init__(self, width, height):
          self.width = width  # 实例属性
          self.height = height  # 实例属性
  
      @property
      def area(self):
          return self.width * self.height
  
  
  rect = Rect(800, 600)
  print("面积为：", rect.area)
  ```

  运行结果：

  ```
  面积为： 480000
  ```

  **注：通过@property转换后的属性不能重新赋值。**

#### 18.5 继承

- 基本语法：

  ```python
  class ClassName(baseclasslist):
      '''类的帮助信息'''
      Statement
  ```

  baseclasslist：用于指定要继承的父类，可以有多个，用“,”隔开。

- 示例：

  ```python
  class Fruit:
      color = "绿色"
  
      def harvest(self, color):
          print("水果是：" + color + "的")
          print("水果已经收获")
          print("水果原来是：" + Fruit.color + "的");
  
  
  class Apple(Fruit):
      color = "红色"
  
      def __init__(self):
          print("我是苹果")
  
  
  class Orange(Fruit):
      color = "橙色"
  
      def __init__(self):
          print("我是橘子")
  
  
  apple = Apple()
  apple.harvest(apple.color)
  orange = Orange()
  orange.harvest(orange.color)
  ```

  运行结果：

  ```python
  我是苹果
  水果是：红色的
  水果已经收获
  水果原来是：绿色的
  我是橘子
  水果是：橙色的
  水果已经收获
  水果原来是：绿色的
  ```

- 方法重写：

  在Orange类中重写`harves()`方法。

  ```python
  class Orange(Fruit):
      color = "橙色"
  
      def __init__(self):
          print("我是橘子")
  
      def harvest(self, color):
          print("水果是：" + color + "的")
          print("橘子已经收获")
          print("水果原来是：" + Fruit.color + "的");
  ```

  运行结果：

  ```
  我是橘子
  水果是：橙色的
  橘子已经收获
  水果原来是：绿色的
  ```

- 调用基类的`__init__()`方法：

  ```python
  class Fruit:
  
      def __init__(self, color="绿色"):
          Fruit.color = color
  
      def harvest(self):
          print("水果原来是：" + Fruit.color + "的")
  
  
  class Apple(Fruit):
      def __init__(self):
          print("我是苹果")
          super().__init__()
  
  
  apple = Apple()
  apple.harvest()
  ```

  运行结果：

  ```
  我是苹果
  水果原来是：绿色的
  ```

### 19.模块

- 定义：一个拓展名为.py的文件就是一个模块，包含了一个完整的功能

- 使用import语句导入模块

  ```python
  import modulename [as alias]
  ```

  - modulename：要导入的模块名
  - [as alias]：为模块起的别名

- 实例：创建两个模块，分别包含矩形和圆形的计算周长面积函数

  ```python
  #!/usr/bin/env python
  # encoding: utf-8
  """
  @author: lemenk
  @Blog: blog.lemenk.top
  @software: PyCharm
  @file: rectangle.py
  @time: 2019/11/7 21:34
  @Desc: 计算矩形周长和面积
  """
  
  
  def girth(width, height):
      """功能：计算矩形周长
         参数 ：width（宽度），height（高度）
      """
      return (width + height) * 2
  
  
  def area(width, height):
      """功能：计算矩形面积
             参数 ：width（宽度），height（高度）
          """
      return width * height
  
  
  if __name__ == '__main__':
      print(area(10, 20))
  
  ```

  ```python
  #!/usr/bin/env python
  # encoding: utf-8
  """
  @author: lemenk
  @Blog: blog.lemenk.top
  @software: PyCharm
  @file: circular.py
  @time: 2019/11/7 21:34
  @Desc: 计算圆形周长和面积
  """
  import math
  
  PI = math.pi
  
  
  def girth(r):
      """功能：计算圆形周长
         参数 ：r（半径）
      """
      return round(2 * PI * r, 2)
  
  
  def area(r):
      """功能：计算圆形面积
             参数 ：r（半径）
          """
      return round(PI * r * r, 2)
  
  
  if __name__ == '__main__':
      print(area(10))
  
  ```

  测试代码：

  ```python
  #!/usr/bin/env python
  # encoding: utf-8
  """
  @author: lemenk
  @Blog: blog.lemenk.top
  @software: PyCharm
  @file: computer.py
  @time: 2019/11/7 21:46
  @Desc: 调用rectangle.py和circular.py
  """
  
  import rectangle as r
  import circular as c
  
  if __name__ == '__main__':
      print("圆形的周长为：", c.girth(10))
      print("矩形周长为：", r.girth(10, 20))
  
  ```

  运行结果：

  ```
  圆形的周长为： 62.83
  矩形周长为： 60
  ```

  **注：`if __name__ == '__main__'`代码块的必要性：**

  类似于Java的main方法，`__name__`是python的内置变量，用于指代当前模块。

  当模块A导入到模块B中时，若需要模块A中的某些代码不需要在B中执行时，可放在`if __name__ == '__main__':`内部。

- 标准模块：

  - random模块：用于生成随机函数

    ```python
    import random
    print(random.randint(0, 10))
    ```

    生成验证码：

    ```python
    #!/usr/bin/env python
    # encoding: utf-8
    """
    @author: lemenk
    @Blog: blog.lemenk.top
    @software: PyCharm
    @file: checkcode.py
    @time: 2019/11/7 22:33
    @Desc: 随机生成由数字和字母组成的4位验证码
    """
    
    import random
    if __name__ == '__main__':
        checkcode = ""
        for i in range(4):
            index = random.randrange(0, 4)
            if index != i and index + 1 != i:
                checkcode += chr(random.randint(97, 122))
            elif index + 1 == i:
                checkcode += str(random.randint(1, 9))
            else:
                checkcode += str(random.randint(1, 9))
        print("验证码：", checkcode)
    
    ```

  - 其他标准模块

### 20.异常

常用语法：

- try...except

  ```python
  try:
      block1
  except [ExceptionName [as alias]]:
      block2
  ```

- try...except...else

  ```python
  try:
      block1
  except [ExceptionName [as alias]]:
      block2
  else:
      block3
  ```

- try...except...finally语句

  ```python
  try:
      block1
  except [ExceptionName [as alias]]:
      block2
  else:
      block3
  finally:
      block4
  ```

- raise语句抛出

  ```python
  def fun:
      num = int(input("请输入不小与3的数字："))
      if num < 3:
          raise ValueError("输入值出错")
  ```

### 21.文件

- 创建和打开文件：

  - 语法格式：

    ```python
    file = open(filename[,mode[,buffering]])
    ```

    参数说明：

    - file：被创建的文件对象

    - filename：要创建或者打开的文件路径

    - mode：可选参数，用于指定文件的打开方式

      | t    | 文本模式 (默认)                                              |
      | ---- | :----------------------------------------------------------- |
      | x    | 写模式，新建一个文件，如果该文件已存在则会报错。             |
      | b    | 二进制模式。                                                 |
      | +    | 打开一个文件进行更新(可读可写)。                             |
      | r    | 以只读方式打开文件。文件的指针将会放在文件的开头。这是默认模式。 |
      | rb   | 以二进制格式打开一个文件用于只读。文件指针将会放在文件的开头。这是**默认模式**。一般用于非文本文件如图片等。 |
      | r+   | 打开一个文件用于读写。文件指针将会放在文件的开头。           |
      | rb+  | 以二进制格式打开一个文件用于读写。文件指针将会放在文件的开头。一般用于非文本文件如图片等。 |
      | w    | 打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。 |
      | wb   | 以二进制格式打开一个文件只用于写入。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。一般用于非文本文件如图片等。 |
      | w+   | 打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。 |
      | wb+  | 以二进制格式打开一个文件用于读写。如果该文件已存在则打开文件，并从开头开始编辑，即原有内容会被删除。如果该文件不存在，创建新文件。一般用于非文本文件如图片等。 |
      | a    | 打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
      | ab   | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。也就是说，新的内容将会被写入到已有内容之后。如果该文件不存在，创建新文件进行写入。 |
      | a+   | 打开一个文件用于读写。如果该文件已存在，文件指针将会放在文件的结尾。文件打开时会是追加模式。如果该文件不存在，创建新文件用于读写。 |
      | ab+  | 以二进制格式打开一个文件用于追加。如果该文件已存在，文件指针将会放在文件的结尾。如果该文件不存在，创建新文件用于读写。 |

      注：以r开头的参数需要文件必须存在，否则会出错。以w开头的参数，若文件不存在，则会创建文件；若存在，则会覆盖该文件。以a开头的参数，会对已存在的文件追加内容。

    - buffering：可选参数，用于指定读写文件的缓冲模式，0表示不缓存；1表示缓存；如果大于1，则表示缓冲区的大小。默认为1。

    - encoding: 一般使用utf8

    - errors: 报错级别

    - newline: 区分换行符

    - closefd: 传入的file参数类型

  - 例子：

    - 1.创建一个文件，并存储一些信息。

      ```python
      """创建一个不存在的文件"""
      print("\n", "="*10, "学习记录", "="*10)   # 正则表达式
      file = open('msg.txt', 'w')   # 在此目录下创建msg.txt
      print("\n 即将显示……\n")
      ```

      运行结果：在此目录下创建了msg.txt文件，并打印以下语句：

      ```
       ========== 学习记录 ==========
      
       即将显示……
      ```

    - 2.以二进制形式打开文件

      ```python
      file = open('p.png', 'rb')
      print(file)
      ```

    - 3.打开文件时指定编码方式

      ```python
      file = open('e.txt', 'r', encoding='utf-8')
      print(file)
      ```

- 关闭文件：

  ```python
  file.close()
  ```

- 打开文件时使用with语句：使用with语句，可以避免在打开文件时出现异常导致文件不能被即时关闭。

  - 语法格式：

    ```python
    with expression as target:
        with-body
    ```

  - 示例：

    ```python
    print("\n", "="*10, "学习记录", "="*10)   # 正则表达式
    with open('msg.txt', 'w') as file:  # 在此目录下创建msg.txt
        pass
    print("\n 即将显示……\n")
    ```

- 写入文件内容

  - 语法格式：

    ```python
    file.write(string)
    ```

  - 例子：

    ```python
    print("\n", "="*10, "学习记录", "="*10)   # 正则表达式
    string = "今天学习了python的文件操作"
    with open('msg.txt', 'a', encoding='utf-8') as file: 
        file.write(string + "\n")
    print("\n 在文件中写入了内容……\n")
    file.close()
    ```

- 读取文件

  - 语法格式：

    ```python
    file.read([size])
    ```

    size：可选参数，用于指定要读取的字符个数，省略则一次性读取所有内容

  - 例：

    ```python
    with open('msg.txt', 'r', encoding='utf-8') as file:
        string = file.read(7)
        print(string)
    ```

    注：read(size)方法读取文件时时从文件头开始读的。

  - 使用seek()方法移动文件指针

    ```python
    with open('msg.txt', 'r', encoding='utf-8') as file:
        file.seek(3)
        string = file.read(3)
        print(string)
    ```

    **注：`seek()`方法中文占两个字符，数字和英文占一个字符。和`read()`不同**

- 其他方法：

  - file.flush()：刷新文件内部缓冲，直接把内部缓冲区的数据立刻写入文件, 而不是被动的等待输出缓冲区写入。
  - file.readline([size])：读取整行，包括 "\n" 字符。
  - file.readlines([sizeint])：读取所有行并返回列表，若给定sizeint>0，返回总和大约为sizeint字节的行, 实际读取值可能比 sizeint 较大, 因为需要填充缓冲区。

### 22.数据库

- 使用mysql-connector 驱动连接mysql数据库

  - 安装**mysql-connector**驱动

    ```
    pip install mysql-connector
    ```

  - 创建数据库连接：

    ```python
    import mysql.connector
     
    mydb = mysql.connector.connect(  # 创建连接对象mydb
      host="localhost",       # 数据库主机地址
      user="yourusername",    # 数据库用户名
      passwd="yourpassword"   # 数据库密码
    )
     
    print(mydb)
    ```

    连接对象有如下几个方法：
  
    - close()：关闭数据库连接
    - commit()：提交事务
    - rollback()：回滚事务
    - cursor()：获取游标对象，操作数据库
  
  - 游标对象：代表数据库中的游标，用于指示抓取数据操作的上下文。主要提供执行SQL语句、调用存储过程、获取查询结果等方法。
  
    - callproc(name[,params])：使用指定的参数调用指定的数据库过程；
    - close()：关闭当前游标，关闭后，这个游标就不能使用了；
    - execute(oper[,params])：执行一个SQL操作；
    - executemany(oper,pseq)：执行SQL操作多次，每次用序列中的一组参数；
    - fetchone()：以序列的方式取回查询结果中的下一行，如果没有更多的行，就返回None；
    - fetchmany([size])：取回查询时结果中的多行，其中参数size的值默认为arraysize；
    - fetchall()：已序列的方式取回余下的行；
    - nextset()：跳到下一个结果集，这个方法是可选的；
    - setinputsizes(sizes)：用于为参数预定义内存区域；
    - setoutputsize(size[,col])：围裙会大量数据而设置缓冲区长度。
  
    创建游标对象：
  
    ```python
    mycursor = mydb.cursor()
    ```
  
    
  
  - 其他语句：
  
    - 查看数据库列表：
  
      ```python
      mycursor.execute("SHOW DATABASES")
      for x in mycursor:
          print(x)
      ```
  
    - 创建数据库：
  
      ```python
      mycursor.execute("CREATE DATABASE DBName")
      ```
  
    - 创建数据表sites：
  
      ```python
      mycursor.execute("CREATE TABLE sites (name VARCHAR(255), url VARCHAR(255))")
      ```
  
    - 给已经存在的sites表添加主键：
  
      ```python
      mycursor.execute("ALTER TABLE sites ADD COLUMN id INT AUTO_INCREMENT PRIMARY KEY")
      ```
  
    - 向表中添加数据：
  
      ```python
      sql = "INSERT INTO sites (name, url) VALUES (%s, %s)"
      val = ("百度", "https://www.baidu.com")
      mycursor.execute(sql, val)
      
      mydb.commit()  # 数据表内容有更新，必须使用到该语句
      ```
  
    - 向表中批量添加数据，第二个参数为元组列表：
  
      ```python
      sql = "INSERT INTO sites (name, url) VALUES (%s, %s)"
      val = [
          ('Google', 'https://www.google.com'),
          ('Github', 'https://www.github.com'),
          ('Taobao', 'https://www.taobao.com'),
          ('stackoverflow', 'https://www.stackoverflow.com/')
      ]
      
      mycursor.executemany(sql, val)   #批量操作
      
      mydb.commit()
      ```
  
    - 查询数据：
  
      - 查询下一条记录：`fetchone()`
  
        ```python
        mycursor.execute("SELECT * FROM sites")
        
        my_result = mycursor.fetchone()  # fetchone() 获取结果集中下一条记录
        print(x)
        ```
  
      - 查询表中所有记录：`fetchall()`
  
        ```python
        mycursor.execute("SELECT * FROM sites")
        
        my_result = mycursor.fetchall()  # fetchall() 获取所有记录
        
        for x in my_result:
            print(x)
        ```
  
      - 查询指定数量的记录：`fetchmany()`
  
        ```python
        mycursor.execute("SELECT * FROM sites")
        
        my_result = mycursor.fetchmany(3)  # fetchmany() 获取指定数量的记录
        
        for x in my_result:
            print(x)
        ```
  
- PyMySQL 驱动连接数据库：

  - PyMySQL 安装：

    ```python
    $ pip3 install PyMySQL
    ```

    