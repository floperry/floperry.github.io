---
title: 一天一道算法题-ReversePairs
date: 2017-08-03 15:36:03
tags: [Python, 算法]
---

今天要介绍的算法题名为Reverse Pairs，题目来源[LintCode](http://www.lintcode.com/en/problem/reverse-pairs/)/[LeetCode](https://leetcode.com/problems/reverse-pairs/description/)。
<!--more-->

### Description

For an array A, if i < j, and A[i] > A[j], called (A[i], A[j]) is a reverse pair. Return total of reverse pairs in A.

### Example

Given A = ``[2, 4, 1, 3, 5]``, ``(2, 1), (4, 1), (4, 3)`` are reverse pairs. return ``3``.

### Solution

看完题目之后的第一反应是两次遍历数组，但是时间复杂度为O(n^2)，代码运行超时。后来发现可以用分治法来解决问题，时间复杂度为O(nlogn)。

#### 思路

<blockquote>
	1、把数组分割成子数组
	<br>
	2、统计出子数组内部的逆序对数目
	<br>
	3、统计出两个相邻子数组之间的逆序对数目
	<br>
	4、在统计逆序对的过程中，同时对数组进行归并排序
</blockquote>

#### 代码

``` Python
class SolutionL
    # @param {int[]} A an array
    # @return {int} total of reverse pairs
    def reversePairs(self, A):
        self.tmp = [0] * len(A)
        return self.mergeSort(A, 0, len(A)-1)

    def mergeSort(self, A, l, r):
        if l >= r:
            return 0

        m = (l + r) >> 1
        ans = self.mergeSort(A, l, m) + self.mergeSort(A, m+1, r)
        i, j, k = l, m + 1, l
        while i <= m and j <= r:
            if A[i] > A[j]:
                self.tmp[k] = A[j]
                j += 1
                ans += m - i + 1
            else:
                self.tmp[k] = A[j]
                i += 1
            k += 1

        while i <= m:
            self.tmp[k] = A[i]
            k += 1
            i += 1
        while j <= r:
            self.tmp[k] = A[j]
            k += 1
            j += 1
        for i in range(l, r+1):
            A[i] = self.tmp[i]

        return ans
```
