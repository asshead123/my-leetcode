
## 题目地址(剑指 Offer 24. 反转链表)

https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/

## 题目描述

```
定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

 

示例:

输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL

 

限制：

0 <= 节点个数 <= 5000

 

注意：本题与主站 206 题相同：https://leetcode-cn.com/problems/reverse-linked-list/
```

## 时间

- 2021年8月12日

## 难度

- 简单 

## 思路

### 方法一：递归

假设链表的其余部分已经被反转，现在应该如何反转它前面的部分？

假设链表为：n1 →n2→…→nk

若从节点 n2 到 nk 已经被反转，即n1→n2←...←nk

接下来要做的是 n1.next.next = n1; n1.next = null;

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
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode cur = reverseList(head.next);
        head.next.next = head;
        head.next = null;
        return cur;
    }
}

```


**复杂度分析**

- 时间复杂度：`O(n)`，其中 n 是链表的长度。需要对链表的每个节点进行反转操作。
- 空间复杂度：`O(n)`，其中 n 是链表的长度。空间复杂度主要取决于递归调用的栈空间，最多为 n 层。

### 方法二：迭代

在遍历链表时，将当前节点的 next 指针改为指向前一个节点。由于节点没有引用其前一个节点，因此必须事先存储其前一个节点。在更改引用之前，还需要存储后一个节点。最后返回新的头引用。

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
    public ListNode reverseList(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode prev = null, cur = head;
        while (cur != null) {
            ListNode next = cur.next;
            cur.next = prev;
            prev = cur;
            cur = next;
        }
        return prev;
    }
}

```


**复杂度分析**

- 时间复杂度：`O(n)`，其中 n 是链表的长度。需要遍历链表一次。
- 空间复杂度：`O(1)`。


