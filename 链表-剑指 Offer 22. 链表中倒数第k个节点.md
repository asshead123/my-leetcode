
## 题目地址(剑指 Offer 22. 链表中倒数第k个节点)

https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/

## 题目描述

```
输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。

 

示例：

给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.
```

## 时间

- 2021年8月19日

## 难度

- 简单

## 思路

1. 初始化两个指针 fast 和 slow，一开始都指向链表的头节点
2. 前指针 fast 先向前走 k 步
3. 两个指针 fast 和 slow 同时向前移动，直到前指针 fast 指向 NULL
4. 由于 fast 和  slow 之间的距离为 k，所以 fast 指向的节点即是倒数第 k 个节点，最后返回 slow

注意判断当 k 值大于链表长度时返回null。

**代码**

Java Code:

```java

/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode getKthFromEnd(ListNode head, int k) {
        ListNode slow = head, fast = head;
        for (int i = 0; i < k; i++) {
            if (fast == null && i < k) return null;
            fast = fast.next;
        }
        while (fast != null) {
            slow = slow.next;
            fast = fast.next;
        }
        return slow;
    }
}

```


**复杂度分析**
- 时间复杂度：O(N)。N 为链表长度；总体看， fast 走了 N 步， slow 走了 (N−k) 步。
- 空间复杂度  O(1)。双指针 fast , slow 使用常数大小的额外空间。


