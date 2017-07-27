---
title: 常见排序算法及Python实现
date: 2017-07-27 13:14:58
tags:
---

<img src="http://otnawj8bh.bkt.clouddn.com/images/Sort/sort.png?imageView2/1/w/800/h/310" />

今天介绍一下常用的排序算法，及其用Python的简单实现。
<!--more-->

---

### 冒泡排序BubbleSore

#### 基本思想

<blockquote>
	1、依次比较相邻的两个数，如果前一个数比后一个数大，则交换顺序，直到最大的数移到最后的位置上
	<br>
	2、重复以上步骤，直到最后一个元素
</blockquote>

#### 算法可视化

{% qnimg Sort/BubbleSort.gif %}

#### 算法实现

```Python
def bubble_sort(array):
    n = len(array)
    for i in range(n):
        for j in range(1, n-i):
            if array[j-1] > array[j]:
                array[j-1], array[j] = array[j], array[j-1]
    return array
```