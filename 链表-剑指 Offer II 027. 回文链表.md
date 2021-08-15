
## 题目地址(剑指 Offer II 027. 回文链表)

https://leetcode-cn.com/problems/aMhZSa/

## 题目描述

给定一个链表的 头节点 head ，请判断其是否为回文链表。

如果一个链表是回文，那么链表节点序列从前往后看和从后往前看是相同的。

示例 1：

![](https://pic.leetcode-cn.com/1626421737-LjXceN-image.png)
```
输入: head = [1,2,3,3,2,1]
输出: true
```
示例 2：

![](https://pic.leetcode-cn.com/1626422231-wgvnWh-image.png)
```
输入: head = [1,2]
输出: fasle


 

提示：

链表 L 的长度范围为 [1, 105]
0 <= node.val <= 9

 

进阶：能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

 

注意：本题与主站 234 题相同：https://leetcode-cn.com/problems/palindrome-linked-list/
```

## 时间

- 2021年8月15日

## 难度

- 简单

## 思路

### 方法一：快慢指针（推荐）
将链表的后半部分反转（修改链表结构），然后将前半部分和后半部分进行比较。比较完成后我们应该将链表恢复原样。虽然不需要恢复也能通过测试用例，但是使用该函数的人通常不希望链表结构被更改。

该方法虽然可以将空间复杂度降到 O(1)，但是在并发环境下，该方法也有缺点。在并发环境下，函数运行时需要锁定其他线程或进程对链表的访问，因为在函数执行过程中链表会被修改。

整个流程可以分为以下五个步骤：

1. 找到前半部分链表的尾节点。
2. 反转后半部分链表。
3. 判断是否回文。
4. 恢复链表。
5. 返回结果。

执行步骤一，我们可以计算链表节点的数量，然后遍历链表找到前半部分的尾节点。

我们也可以使用快慢指针在一次遍历中找到：慢指针一次走一步，快指针一次走两步，快慢指针同时出发。当快指针移动到链表的末尾时，慢指针恰好到链表的中间。通过慢指针将链表分为两部分。

若链表有奇数个节点，则中间的节点应该看作是前半部分。

步骤二可以使用「206. 反转链表」问题中的解决方法来反转链表的后半部分。

步骤三比较两个部分的值，当**后半部分**（画图思考为什么）到达末尾则比较完成，可以忽略计数情况中的中间节点。

步骤四与步骤二使用的函数相同，再反转一次恢复链表本身。


**代码**

Java Code:

```java

/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) return true;
        if (head.next.next == null) return head.val == head.next.val;
        ListNode slow = head, fast = head;
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        ListNode rightHead = reverse(slow.next);
        ListNode leftHead = head;
        while (rightHead != null) {
            if (leftHead.val != rightHead.val) 
                return false;
            leftHead = leftHead.next;
            rightHead = rightHead.next;
        }
        // 恢复原链表
        slow.next = reverse(rightHead);
        return true;
    }

    public ListNode reverse(ListNode head) {
        if (head == null || head.next == null) return head;
        ListNode next = reverse(head.next);
        head.next.next = head;
        head.next = null;
        return next;
    }
}

```

**复杂度分析**

- 时间复杂度：O(n)，其中 n 是链表的长度。

- 空间复杂度：O(1)。我们只会修改原本链表中节点的指向，而在堆栈上的堆栈帧不超过 O(1)。

### 方法二：将值复制到数组中后用双指针法

整个流程可以分为以下两个步骤：

1. 复制链表值到数组列表中。
2. 使用双指针法判断是否为回文。


**代码**

Java Code:

```java

class Solution {
    public boolean isPalindrome(ListNode head) {
        List<Integer> vals = new ArrayList<Integer>();

        // 将链表的值复制到数组中
        ListNode currentNode = head;
        while (currentNode != null) {
            vals.add(currentNode.val);
            currentNode = currentNode.next;
        }

        // 使用双指针判断是否回文
        int front = 0;
        int back = vals.size() - 1;
        while (front < back) {
            if (!vals.get(front).equals(vals.get(back))) {
                return false;
            }
            front++;
            back--;
        }
        return true;
    }
}
```

**复杂度分析**

- 时间复杂度：O(n)，其中 n 是链表的长度。

- 空间复杂度：O(n)，其中 n 是链表的长度，我们使用了一个数组列表存放链表的元素值。

### 方法三：递归

currentNode 指针是先到尾节点，由于递归的特性再从后往前进行比较。frontPointer 是递归函数外的指针。若 currentNode.val != frontPointer.val 则返回 false。反之，frontPointer 向前移动并返回 true。

算法的正确性在于递归处理节点的顺序是相反的（回顾上面打印的算法），而我们在函数外又记录了一个变量，因此从本质上，我们同时在正向和逆向迭代匹配。


**代码**

Java Code:

```java

class Solution {
    private ListNode frontPointer;

    private boolean recursivelyCheck(ListNode currentNode) {
        if (currentNode != null) {
            if (!recursivelyCheck(currentNode.next)) {
                return false;
            }
            if (currentNode.val != frontPointer.val) {
                return false;
            }
            frontPointer = frontPointer.next;
        }
        return true;
    }

    public boolean isPalindrome(ListNode head) {
        frontPointer = head;
        return recursivelyCheck(head);
    }
}
```

**复杂度分析**

- 时间复杂度：O(n)，其中 n 是链表的长度。

- 空间复杂度：O(n)，其中 n 是链表的长度。我们要理解计算机如何运行递归函数，在一个函数中调用一个函数时，计算机需要在进入被调用函数之前跟踪它在当前函数中的位置（以及任何局部变量的值），通过运行时存放在堆栈中来实现（堆栈帧）。在堆栈中存放好了数据后就可以进入被调用的函数。在完成被调用函数之后，他会弹出堆栈顶部元素，以恢复在进行函数调用之前所在的函数。在进行回文检查之前，递归函数将在堆栈中创建 n 个堆栈帧，计算机会逐个弹出进行处理。所以在使用递归时空间复杂度要考虑堆栈的使用情况。

这种方法不仅使用了 O(n) 的空间，且比第二种方法更差，因为在许多语言中，堆栈帧的开销很大（如 Python），并且最大的运行时堆栈深度为 1000（可以增加，但是有可能导致底层解释程序内存出错）。为每个节点创建堆栈帧极大的限制了算法能够处理的最大链表大小。


