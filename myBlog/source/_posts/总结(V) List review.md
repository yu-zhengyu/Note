---
title: 总结(V) List review --- List 题目总结
date: 2016-01-06 09:22:30
tags:
- leetcode
- 刷题
categories:
- 总结
---

List的题目可以说是我做的最不好的。并不是说不会方法，而是有时候边界条件的检查让我们可能很难在15分钟之内做到bug free。所以我这里举出一些常用的手法，到时候大家可以直接背下来，直接用。当然你也可以现场画图，但是我觉得那样时间就慢了。
<!--more-->
***
### 1. Reverse a list in place

不用多余的空间，翻转一个链表，这个其实一开始我是觉得挺有难度的，因为每次我都记不住，所以我这次就直接记下了，免得到时候用着又来查。

####code[java]

```

public ListNode reverseList(ListNode head) {
    if(head==null) return head;
    ListNode temp1= head.next;
    ListNode temp2= null;
    head.next=null;
    while(temp1!=null){
        temp2=temp1.next;
        temp1.next=head;
        head=temp1;
        temp1=temp2;
    }
    return head;
}              
```

***
### 2. find the middle of linked list

高频率用到，主要的方法就是用一个runner指针。这个技巧也会用在寻找一个链表中是否有回路。

####code[java]

```

public static ListNode getMiddleOfList(ListNode l) {
	ListNode fast = l;
	ListNode slow = l;
	ListNode prev = null;
	while(fast != null && fast.next != null) {
		prev = slow;
		slow = slow.next;
		fast = fast.next.next;
	}
	
	return slow;
}

```

上面代码基本包含了奇偶的情况，有这种题就大胆往上写，绝对不出错。

***

### 3. reverse a list from m to n

这题我做的真的不好，后来还是看答案来写了。一开始的思路不太好。总认为是要先把 m 到 n 的链表先翻转了，然后再将这个翻转后的链表连回原来的头和尾。这样导致了我总是没办法满足一些边界条件。

其实这题不需要我想的那么麻烦，其实就记得一个头，然后每次遍历以后把当前的node插入到头的下一个就好了。这样的话还不容易出错。还有就是dummy的技巧用的不是很熟，以后打算每天都写一遍这个题，加强手部反应。

####code[java]
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode reverseBetween(ListNode head, int m, int n) {
        if(head == null)
            return null;
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode pre = dummy;
        for(int i = 0; i < m - 1; i++)
            pre = pre.next;
        ListNode current = pre.next;
        ListNode nextn = current.next;
        
        for(int i = 0; i < n - m; i++) {
            current.next = nextn.next;
            nextn.next = pre.next;
            pre.next = nextn;
            nextn = current.next;
        }
        
        return dummy.next;
        
    }
}
```





