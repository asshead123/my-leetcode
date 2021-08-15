
## 题目地址(剑指 Offer 06. 从尾到头打印链表)

https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/

## 题目描述

```
输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

 

示例 1：

输入：head = [1,3,2]
输出：[2,3,1]

 

限制：

0 <= 链表长度 <= 10000
```

## 时间

- 2021年8月15日

## 难度

- 简单


## 关键点

该题目的两种解法充分说明了递归本质上就是一个栈，先进后出。

## 思路

### 方法一：辅助栈

链表都是从头读到尾依次访问每个节点，题目要求我们 从尾到头 打印链表，这种逆序的操作很显然可以考虑使用

具有 **先入后出** 特点的数据结构，那就是 **栈**。

具体操作如下：

- 入栈： 遍历链表，将各节点值 push 入栈。
- 出栈： 将各个节点值 pop 出栈，存储于数组并返回。

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
    public int[] reversePrint(ListNode head) {
        Stack<Integer> stack = new Stack<>();
        ListNode cur = head;
        while (cur != null) {
            stack.push(cur.val);
            cur = cur.next;
        }
        int n = stack.size();
        int[] res = new int[n];
        for (int i = 0; i < n; i++) {
            res[i] = stack.pop();
        }
        return res;
    }
}

```

- 时间：击败了28.55%

- 空间：击败了92.33%

**复杂度分析**

- 时间复杂度：O(n)。正向遍历一遍链表，然后从栈弹出全部节点，等于又反向遍历一遍链表。
- 空间复杂度：O(n)。额外使用一个栈存储链表中的每个节点。

### 方法二：递归

利用递归，先走至链表末端，回溯时依次将节点值加入列表 ，这样就可以实现链表值的倒序输出。

算法流程：

1. 递推阶段： 每次传入 head.next ，以 head == null（即走过链表尾部节点）为递归终止条件，此时直接返回。
2. 回溯阶段： 层层回溯时，将当前节点值加入列表，即tmp.add(head.val)。
3. 最终，将列表 tmp 转化为数组 res ，并返回即可。

**代码**

Java Code:

```java
class Solution {
    ArrayList<Integer> tmp = new ArrayList<Integer>();
    public int[] reversePrint(ListNode head) {
        recur(head);
        int[] res = new int[tmp.size()];
        for(int i = 0; i < res.length; i++)
            res[i] = tmp.get(i);
        return res;
    }
    void recur(ListNode head) {
        if(head == null) return;
        recur(head.next);
        tmp.add(head.val);
    }
}
```
- 时间：击败了73.66%

- 空间：击败了5.03%

**复杂度分析**

- 时间复杂度：O(n)。遍历链表，递归 N 次。
- 空间复杂度：O(n)。系统递归需要使用 O(N) 的栈空间。


