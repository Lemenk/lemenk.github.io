---
title: 牛客网-剑指offer
date: 2019-07-16 14:49:44
tags:
- 算法
categories:
- 算法
- 剑指offer
---

**牛客网-剑指offer学习笔记**

<!--more-->

##### 一 、二维数组中的查找

1、题目描述：[链接](https://www.nowcoder.com/practice/abc3fe2ce8e146608e868a70efebf62e?tpId=13&tqId=11154&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

​        在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

时间限制：1秒 空间限制：32768K

2、思路分析：

题中所述，每行从左到右递增的顺序排序，每列从上到下递增的顺序排序，因此可以得到二维数组中每行的最后一个数字必是该行最大数字。若要判断该元素是否存在在二位数组中，只要判断该元素是否小于某一行的最后一个元素。即：对于此二维数组来说，只用判断target是否小于每一行的最后一个数字即可判断target是否存在于该二维数组中。

<img src="http://img.cdn.lemenk.top/Q1p1.png" />

<img src="http://img.cdn.lemenk.top/Q1p2.png" />

若要查找target=22，则判断target<=行内最大元素即可。



3、代码：

```java
public class Solution {
    public boolean Find(int target, int [][] array) {
        int y = array.length;
        int x = array[0].length;
        if(x ==0 || y ==0){
            return false;
        }else{
            for(int i = 0;i<y;i++){
                if(array[i][x-1] >= target){
                    for(int j = 0;j<x;j++){
                        if(array[i][j] == target){
                            return true;
                        }
                    }
                }
            }
        }
        return false;
    }
}
```

提交结果：

<img src="http://img.cdn.lemenk.top/Q1p3.png" />

##### 二、替换空格

1. 题目描述：[链接](nowcoder.com/practice/4060ac7e3e404ad1a894ef3e17650423?tpId=13&tqId=11155&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)

   请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。

   时间限制：1秒 空间限制：32768K

2. 思路分析：

   