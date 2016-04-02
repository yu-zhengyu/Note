---
title: 总结(I) Array and String Review --- 数组类和String题目总结
date: 2015-12-29 09:22:30
tags:
- leetcode
- 刷题
categories:
- 总结
---

这几篇我会归类总结一下做过的题目。总体会根据《Cracking the Code interview 189》这本书来进行复习。
<!--more-->

## String
***
### Write a method anagram(s,t) to decide if two strings are anagrams or not.

这题是google一道面试题。题目的意思很简单，就看一个String是不是另一个String重新排序后的结果。方法很多。比较好的做法是开一个int[256], 分别扫一遍String，把相应字符的ASCII码记录。然后相互对比。又或者扫第一个字符串的时候，每一个相应字符下的数量+1，扫第二个的时候就-1，然后看看所有数组是不是都是0，这样的时间复杂度是O(N), 空间的话是O(N). 还有比较SB的方法。把字符排序，然后直接对比，这样时间复杂度是O(nlogn).

lintcode上有个follow up说能不能用O(n) time, O(1) extra space，我的想法是用bit 来做。利用相同的数字异或后结果为0. 然后我们就可以判断了。这样就只是用了O(1) extra space。

####code[java]

```
public class Solution {
    /**
     * @param s: The first string
     * @param b: The second string
     * @return true or false
     */
    public boolean anagram(String s, String t) {
        // write your code here
        int result = 0;
        if(s.length() != t.length())
            return false;
        for(int i = 0; i < s.length(); i++) {
            result ^= s.charAt(i);
            result ^= t.charAt(i);
        }
        if(result == 0)
            return true;
        return false;
    }
};
```

***
### Check if the String is unique.

这题也是比较高频的。其实可以直接用HashTable直接记下每一个字母出现的字数，如果有字母已经出现了，那么就返回false即可。当然，面试的时候follow up可能会要求你不要用extra data structure. 那么HashTable就不可以用了。 我们可以用一个int数组来解决一个问题。还是和上题一样，用一个256位的数组储存每一个字母出现的次数，如果发现改字母的数据已经不是0了，就可以返回false了。

####code[java]

```
public static boolean isUniqueNoExtraDataStructure(String testStr){
    int alphabetInt = 0;
    String str = testStr.toLowerCase();
    for (int i = 0; i < str.length(); i++){
        int val = str.charAt(i) - 'a';
        if ((alphabetInt & (1 << val)) > 0){
            return false;
        }
        alphabetInt |= (1 << val);
    }
    return true;
}
```

我还发现了一个比较贱的做法，就是用String的API，String有一个方法叫indexOf和lastIndexOf。分别是求出一个字符的首次出现的下标和最后一次出现的下标。那这就简单了。当一个字符的第一个下标和最后一个下标都相等，那就说明它只出现了一次。

####code[java]

```
public static boolean compareUnique(String a) {
	boolean result = true;
	for(int i = 0; i < a.length(); i++) {
		if (a.indexOf(a.charAt(i)) != a.lastIndexOf(a.charAt(i))) {
			result = false;
			break;
		}
	}
	return result;
}
```

***
### check the if sentence is palinpermutation

回文，比较经典的一道题目。考法太多。还有专门的算法来求一个长String中求出最长回文的，[Manacher算法：求解最长回文字符串，时间复杂度为O(N)](http://blog.csdn.net/yzl_rex/article/details/7908259)，等有空在回过头来看。

测一个字符串是不是回文还是比较简单。CC里面的题目稍微增加点难度，字符串中可能会有空格。在处理的时候要小心处理空格的部分即可。

for example：“Tact Coa” 就是一个回文字符串。

那么，这个题目可以有几种做法。我比较喜欢的就是用双指针来做，不会使用太多的空间。

####code[java]

```
public class Solution {
    public boolean isPalindrome(String s) {
        int i = 0;
        int j = s.length() - 1;
        if(s.length() == 1)
            return true;
        while(i < j) {
            // 略过不是数字和字母的情况
            while(!Character.isLetterOrDigit(s.charAt(i)) && i < j)
                i++;
            while(!Character.isLetterOrDigit(s.charAt(j)) && i < j)
                j--;
                
            char a = Character.toLowerCase(s.charAt(i));
            char b = Character.toLowerCase(s.charAt(j));
            if(a != b)
                return false;
            // 开始对比下一个字符
            i++;
            j--;
        }
        return i >= j;
    }
}
```

***
### stringcompress

我是没想到有面试会出现这个。还是挺经典的。就是把相同的字母用数字和字母进行概括。比如说：aaabbbccc就替换成3a3b3c.所以直接上代码了。

####code[java]

```
public static String stringcompress(String s) {
	int count = 0;
	StringBuffer result = new StringBuffer();
	for (int i = 0; i < s.length(); i++) {
		count++;
		if ((i + 1) == s.length() || s.charAt(i) != s.charAt(i + 1)) {
			result.append(s.charAt(i));
			result.append(count);
			count = 0;
		}

	}
	if (result.toString().length() < s.length())
		return result.toString();
	return s;
}
```

***
### check if the s2 is the rotate from s1

这题就是技巧问题了。如果一个字符串是另一个字符串的rotation，那么其实将S2拼接上S1的尾部，然后查看S2是否是新字符串的字串即可。

####code[java]

```
public static boolean isrotate (String s1, String s2) {
	if (s1.length() != s2.length())
			return false;
	String newstring = s1 + s1;
	return newstring.contains(s2);
}
```

***
### Reverse a sentenace. such as "code is good", after reverse, it would become "good is code".

翻转一个句子，一个一个单词进行翻转。这题一开始我还是懵了。后来发现也挺简单。用给一个下标来进行遍历。如果遇到一个空格，就重头开始插入。否则就一直顺着原来的顺序进行插入。看了代码就明白了，还是比较巧妙。

####code[java]

```
public static void main(String[] args) {
	String str = "much. very you love I";
	System.out.println(reverse(str));

}

public static String reverse(String s) {
	int pos = 0;
	StringBuilder sb = new StringBuilder();
	for (int i = 0; i < s.length(); i++) {
		char c = s.charAt(i);
		if (c == ' ') {
			pos = 0;
		}
		sb.insert(pos, c);
		if (c != ' ') {
			pos++;
		}
	}
	return sb.toString();
}

```

***

***

## Array

### basci skill

Reverse a array in java.
比较会常用的一种，记一下。当然也可以用库函数：***Collections.reverse(Arrays.asList(nums));***

```

public void reverse(int[] nums, int left, int right){
    while(left<right){
        int temp = nums[left];
        nums[left]=nums[right];
        nums[right]=temp;
        left++;
        right--;
    }
}

```

