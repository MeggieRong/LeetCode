# 61 - 旋转链表

## 题目描述
![problem](images/61.png)

>审题：就是把单链表想象成一个循环链表，向右转k个位置。

## 双指针
**特殊情况：**
1. 链表长度为0或1，直接返回；
2. k是链表长度的整数倍，直接返回

**思路：**
1. fast指针比slow先走k步；
2. 两指针同时向后遍历直到fast指针指向最后一个结点，此时slow指向倒数第k个结点；
3. fast指向头结点，slow指向None。
```python
# Definition for singly-linked list.
class ListNode:
    def __init__(self, x):
        self.val = x
        self.next = None

class Solution:
    def rotateRight(self, head, k):
        """
        :type head: ListNode
        :type k: int
        :rtype: ListNode
        """
        # 链表长度为0或1，直接返回
        if head is None or head.next is None:
        	return head

        # 计算链表长度
        n = 1
        p = head
        while p.next is not None:
        	p = p.next
        	n += 1

        # k是链表长度的整数倍，直接返回
        if k % n == 0:
        	return head

        k %= n
        p = head
        slow = p
        fast = p
        while k > 0 and fast.next is not None:
        	fast = fast.next
        	k -= 1

        while fast.next is not None:
        	fast = fast.next
        	slow = slow.next
        newHead = slow.next
        fast.next = head
        slow.next = None
        return newHead
```

## 错误
判断特殊情况时不能先计算链表长度再合并三种情况：
```python
if n == 0 or n == 1 or k % n == 0:
	return head
```
否则会出现以下错误：
![error](images/error.png)

链表长度为0的情况必须单独判断（1的就随意了）
```python
if head is None or head.next is None:
    return head
```