
## 题目地址(剑指 Offer II 069. 山峰数组的顶部)

[https://leetcode-cn.com/problems/B1IidL/](https://leetcode-cn.com/problems/B1IidL/)

## 题目描述


符合下列属性的数组 arr 称为 山峰数组（山脉数组） ：

- arr.length >= 3
- 存在 i（0 < i < arr.length - 1）使得：
    - arr[0] < arr[1] < ... arr[i-1] < arr[i]
    - arr[i] > arr[i+1] > ... > arr[arr.length - 1]

给定由整数组成的山峰数组 arr ，返回任何满足 arr[0] < arr[1] < ... arr[i - 1] < arr[i] > arr[i + 1] > ... > arr[arr.length - 1] 的下标 i ，即山峰顶部。

 

示例 1：
```
输入：arr = [0,1,0]
输出：1
```

示例 2：
```
输入：arr = [1,3,5,4,2]
输出：2
```

示例 3：
```
输入：arr = [0,10,5,2]
输出：1
```

示例 4：
```
输入：arr = [3,4,5,1]
输出：2
```

示例 5：
```
输入：arr = [24,69,100,99,79,78,67,36,26,19]
输出：2
```

 

提示：

- 3 <= arr.length <= 10^4
- 0 <= arr[i] <= 10^6
- 题目数据保证 arr 是一个山脉数组

 

进阶：很容易想到时间复杂度 O(n) 的解决方案，你可以设计一个 O(log(n)) 的解决方案吗？

 

注意：本题与主站 852 题相同：[https://leetcode-cn.com/problems/peak-index-in-a-mountain-array/](https://leetcode-cn.com/problems/peak-index-in-a-mountain-array/)

## 时间

- 2021年10月14日（每日一题）

## 难度

- 简单
- 还是有很多细节问题需要注意！

## 思路：二分法



简单字符串模拟，从后往前处理，避免对首个分区的分情况讨论和取余操作。

**Java Code:**


```java

class Solution {
    public int peakIndexInMountainArray(int[] arr) {
        // 峰值只可能在 [1..n-2]区间
        int left = 1, right = arr.length - 1; // 相当于左闭右开区间
        while (left < right) {
            int mid = left + right + 1 >> 1; // 中间偏右的位置
            if (arr[mid - 1] < arr[mid]) left = mid;
            else right = mid - 1; // 如果写right = mid 会陷入死循环，如[0,1,5,3]
        }
        return left;
    }
}

```


**复杂度分析**

- 时间复杂度：O(log n)
- 空间复杂度：O(1)


