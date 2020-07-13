---
title: LeetCode-125回文字符串
date: 2019-06-17 18:51:47
tags:
- Java
- LeetCode
categories:
- LeetCode
---

**LeetCode-125回文字符串**

<!--more-->

LeetCode125题—回文字符串
    题目描述：[Go there](https://leetcode.com/problems/valid-palindrome/" target="_blank" rel="noopener")

给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

***说明：本题中，我们将空字符串定义为有效的回文串。

示例1：
输入：`“A man, a plan, a canal: Panama”`
输出：`true`
示例2：

输入：`“race a car”`
输出：`false`
**解题语言种类：Java**

**解题思路**

1. 判断是否为空字符串，若为空，则返回true
2. 把字符串拿出来，将大写字母转换成小写字母。`toLowerCase()`
3. 遍历，取出数字和字母组成新的字符数组；
4. 定义两个指针，分别从前后开始遍历，依次比较是否相等;
5. 若遍历完所有字符后，仍相等，则返回true；若不相等，则返回false。

我的写法：

```java
public static boolean isPalindrome(String s) {
	if(s==null&&s.length()==0){
		return true;
		}
	else{
		s = s.toLowerCase();
		char[] ca = s.toCharArray();
		String str = "";
		for(int i=0;i<ca.length;i++){
			if((ca[i]>=48 && ca[i]<=57) || (ca[i]>=97 && ca[i]<=122)){
				str += ca[i];
				}
			}
		char[] a =str.toCharArray();
		int left = 0;
		int right = a.length-1;
		while(left<right){
			if(a[left] == a[right]){
				left++;
				right--;
				}
			else{
				return false;
				}
			}
		return true;
		}
	}
}
```

github大佬的操作：

```java
class Solution {
    private static final char[] charMap = new char[128];
    static {
        for (int i = 0; i < 10; ++i) 
            charMap[i+'0'] = (char)(1 + i);
        for (int i = 0; i < 26; ++i)
            charMap[i+'a'] = charMap[i+'A'] = (char)(50 + i);
    }
    
    public boolean isPalindrome(String s) {
        char[] pChars = s.toCharArray();
        int start = 0, end = pChars.length - 1;
        char cstart, cend;
        while (start < end) {
            cstart = charMap[pChars[start]];
            cend = charMap[pChars[end]];
            if (cstart != 0 && cend != 0) {
                if (cstart != cend) return false;
                start++;
                end--;
            } else {
                if (cstart == 0) start++;
                if (cend == 0) end--;
            }
        }
        return true;
    }
}
```

```java
class Solution {
        public boolean isPalindrome(String s) {
        if (s.isEmpty()) {
        	return true;
        }
        int head = 0, tail = s.length() - 1;
        char cHead, cTail;
        while(head <= tail) {
        	cHead = s.charAt(head);
        	cTail = s.charAt(tail);
        	if (!Character.isLetterOrDigit(cHead)) {
        		head++;
        	} else if(!Character.isLetterOrDigit(cTail)) {
        		tail--;
        	} else {
        		if (Character.toLowerCase(cHead) != Character.toLowerCase(cTail)) {
        			return false;
        		}
        		head++;
        		tail--;
        	}
        }
        return true;
    }
}
```

嗯，数据对比，我的方法仅仅超过了全网8%左右的leetcode用户。是个打击，也是个动力！come on！

主要的还是分析下面两中方法的优点，利己利友。

先说第一种：
  这个方法的核心是定义了一个新数组`charMap[]`和静态代码块。这种方式很是巧妙，避免使用`toLowerCase()`方法，而是将字符数组的下标由int类型转为char类型，同时将大写字母和小写字母，如’A’和’a’设为相同下标，并放在数组中。在`isPalindrome(String s)`方法中使用`charMap[]`数组时

```java
cstart = charMap[pChars[start]];
cend = charMap[pChars[end]];
```

通过转换查找，识别是否是数字或者字母。

第二种方法：
  这种方法则是用到了Character中的`isLetterOrDigit(char ch)`方法，确定指定字符是否为字母或数字。其返回值类型为boolean，简化了判断操作。
注：`isLetterOrDigit(char ch)`方法和`isLetterOrDigit(int codePoint)`源代码

```java
public static boolean isLetterOrDigit(char ch) {
   return isLetterOrDigit((int)ch);
}
public static boolean isLetterOrDigit(int codePoint) {
   return ((((1 << Character.UPPERCASE_LETTER) |
   (1 << Character.LOWERCASE_LETTER) |
   (1 << Character.TITLECASE_LETTER) |
   (1 << Character.MODIFIER_LETTER) |
   (1 << Character.OTHER_LETTER) |
   (1 << Character.DECIMAL_DIGIT_NUMBER)) >> getType(codePoint)) & 1)!= 0;
}
```

End!