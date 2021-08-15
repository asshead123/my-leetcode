
## 题目地址(剑指 Offer 35. 复杂链表的复制)

https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/

## 题目描述


请实现 copyRandomList 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 next 指针指向下一个节点，还有一个 random 指针指向链表中的任意节点或者 null。

 

示例 1：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e1.png)

```
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]
```

示例 2：

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e2.png)
```
输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]
```

示例 3：
![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/01/09/e3.png)
```
输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]


示例 4：

输入：head = []
输出：[]
解释：给定的链表为空（空指针），因此返回 null。


 

提示：

-10000 <= Node.val <= 10000
Node.random 为空（null）或指向链表中的节点。
节点数目不超过 1000 。

 

注意：本题与主站 138 题相同：https://leetcode-cn.com/problems/copy-list-with-random-pointer/

 
```

## 时间

- 2021年8月15日

## 难度

- 中等

## 思路

### 方法一：回溯 + 哈希表
如果没有 random 指针的话，那就是普通的链表，只需要遍历链表，然后每轮创建新节点，同时赋值 val 和调整前驱指针指向当前节点就行。

这题出现了 random 指针，由于它可以指向 null 、前面的节点或者后面的节点， 无法做到在一次遍历的过程中就确定下来，因为如果是指向后面的节点，但后面的节点还没有创建生成，无法确定。

所以，我们需要在一开始把所有的节点都创建出来，避免 random 找不到指向，每个节点都对应着一个新的节点，原链表中的节点有 val、next、random 三个属性，而新节点只有 val 有值，并且和原节点一一对应，这种一一对应的关系，符合哈希表的特征。

此时的哈希表以原链表的节点作为键，新创建的节点作为值。

原链表（Key）中的每个节点都有 next 和 random 指针，而新链表（Value） 没有 next 和 random 指针。

需要通过第二次的遍历过程进行指针指向的调整。

在第二次遍历过程中，以原链表中的节点作为键，查找当前原节点的指针指向，然后调整新节点的指针指向。

**代码**

Java Code:

```java

/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/
class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) return null;

        Map<Node, Node> map = new HashMap<>();
        Node cur = head;

        while (cur != null) {
            map.put(cur, new Node(cur.val));
            cur = cur.next;
        }
        cur = head;
        while (cur != null) {
            // 在字典中找到一个 cur 为 key 对应的那个 value 值
            Node valueCur = map.get(cur);

            Node keyNextNode = cur.next;
            Node valueNextNode = map.get(keyNextNode);
            valueCur.next = valueNextNode;

            Node keyRandomNode = cur.random;
            Node valueRandomNode = map.get(keyRandomNode);
            valueCur.random = valueRandomNode;

            cur = cur.next;
        }
        return map.get(head);
    }
}

```


### 方法二：回溯 + 哈希表


本题要求我们对一个特殊的链表进行深拷贝。如果是普通链表，我们可以直接按照遍历的顺序创建链表节点。而本题中因为随机指针的存在，当我们拷贝节点时，「当前节点的随机指针指向的节点」可能还没创建，因此我们需要变换思路。一个可行方案是，我们利用回溯的方式，让每个节点的拷贝操作相互独立。对于当前节点，我们首先要进行拷贝，然后我们进行「当前节点的后继节点」和「当前节点的随机指针指向的节点」拷贝，拷贝完成后将创建的新节点的指针返回，即可完成当前节点的两指针的赋值。

具体地，我们用哈希表记录每一个节点对应新节点的创建情况。遍历该链表的过程中，我们检查「当前节点的后继节点」和「当前节点的随机指针指向的节点」的创建情况。如果这两个节点中的任何一个节点的新节点没有被创建，我们都立刻递归地进行创建。当我们拷贝完成，回溯到当前层时，我们即可完成当前节点的指针赋值。注意一个节点可能被多个其他节点指向，因此我们可能递归地多次尝试拷贝某个节点，为了防止重复拷贝，我们需要首先检查当前节点是否被拷贝过，如果已经拷贝过，我们可以直接从哈希表中取出拷贝后的节点的指针并返回即可。

在实际代码中，我们需要特别判断给定节点为空节点的情况。

**代码**

Java Code:

```java

class Solution {
    Map<Node, Node> cachedNode = new HashMap<Node, Node>();

    public Node copyRandomList(Node head) {
        if (head == null) {
            return null;
        }
        if (!cachedNode.containsKey(head)) {
            Node headNew = new Node(head.val);
            cachedNode.put(head, headNew);
            headNew.next = copyRandomList(head.next);
            headNew.random = copyRandomList(head.random);
        }
        return cachedNode.get(head);
    }
}

```

**复杂度分析**

- 时间复杂度：O(n)，其中 n 是链表的长度。对于每个节点，我们至多访问其「后继节点」和「随机指针指向的节点」各一次，均摊每个点至多被访问两次。
- 空间复杂度：O(n)， 其中 n 是链表的长度。为哈希表的空间开销。


