---
title: 常见排序算法及Python实现
date: 2017-07-27 13:14:58
tags:
---

<img src="http://otnawj8bh.bkt.clouddn.com/images/Sort/sort.png?imageView2/1/w/800/h/310" />

今天介绍一下常用的排序算法，及其用Python的简单实现。
<!--more-->

---

### 冒泡排序 BubbleSore

#### 算法思想

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

### 选择排序 SelectionSort

#### 算法思想

<blockquote>
	1、在未排序序列中找到最小的元素，放到排序序列的起始位置
	<br>
	2、从剩余未排序序列中找到最小元素，放到已排序列的末尾，直到最后一个元素
</blockquote>

#### 算法可视化

{% qnimg Sort\SelectionSort.gif %}

#### 算法实现

``` Python
def select_sort(array):
    n = len(array)
    for i in range(n):
        min = i
        for j in range(i+1,n):
            if array[j] < array[min]:
                min = j
        array[min], array[i] = array[i], array[min]
    return array
```

### 插入排序 InsertionSort

#### 算法思想

<blockquote>
	1、初始化数组为已排序和未排序两部分，其中第一个元素为已排序，其余为未排序
	<br>
	2、比较未排序部分的第一个元素和已排序部分的元素，并将其插入到合适的位置，直到最后一个元素
</blockquote>

#### 算法可视化

{% qnimg Sort\InsertionSort.gif %}

#### 算法实现

``` Python
def insert_sort(array):

    return array
```