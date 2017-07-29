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

#### 算法伪码

 ```
do
  swapped = false
  for i = 1 to indexOfLastUnsortedElement-1
    if leftElement > rightElement
      swap(leftElement, rightElement)
      swapped = true
while swapped
 ```

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

#### 算法伪码

```
repeat (numOfElements - 1) times
  set the first unsorted elements
  for each of the unsorted elements
    if element < currentMinimum
      set element as new minimum
  swap minimum with first unsorted position
```

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
	1、初始化序列为已排序和未排序两部分，其中第一个元素为已排序，其余为未排序
	<br>
	2、比较未排序部分的第一个元素和已排序部分的元素，并将其插入到合适的位置，直到最后一个元素
</blockquote>

#### 算法可视化

{% qnimg Sort\InsertionSort.gif %}

#### 算法伪码

```
mark first element as sorted
for each unsorted element X
  'extract' the element X
  for j = lastSortedIndex down to 0
    if current element j > X
      move sorted element to the right by 1
    break loop and insert X here
```

#### 算法实现

``` Python
def insert_sort(array):
    n = len(array)
    for i in range(1, n):
        if array[i] < array[i-1]:
            tmp = array[i]
            idx = i
            for j in range(i-1, -1, -1):
                if array[j] > tmp:
                    array[j+1] = array[j]
                    idx = j
                else:
                    break
            array[idx] = tmp
    return array
```

### 归并排序 MergeSort

#### 算法思想

<blockquote>
	1、归：递归的分解序列，直到每个序列只有一个元素
	<br>
	2、并：合并相邻的两个有序序列
</blockquote>

#### 算法可视化

{% qnimg Sort\MergeSort.gif %}

#### 算法伪码

```
split each element into partitions of size 1
recursively merge adjancent partitions
  for i = leftPartStartIndex to rightPartLastIndex inclusive
    if leftPartHeadValue <= rightPartHeadValue
      copy leftPartHeadValue
    else: copy rightPartHeadValue
copy elements back to original array
```

#### 算法实现

``` Python
def merge_sort(array):
    if len(array) <= 1:
        return array

    def merge(left, right):
        l, r = 0, 0
        result = []
        while l < len(left) and r < len(right):
            if left[l] < right[r]:
                result.append(left[l])
                l += 1
            else:
                result.append(right[r])
                r += 1
        result += left[l:]
        result += right[r:]
        return result

    num = int(len(array)/2)
    left = merge_sort(array[:num])
    right = merge_sort(array[num:])
    return merge(left, right)
```

### 快速排序 QuickSort

#### 算法思想

<blockquote>
	1、从序列中选出一个基准元素，重排序列，将所有比基准小的元素放在基准前面，比基准大的元素放在基准后面
	<br>
	2、递归地把两个子序列进行重新排序
</blockquote>

#### 算法可视化

{% qnimg Sort\QuickSort.gif %}

#### 算法伪码

```
for each unsorted partition
set first element as pivot
  storeIndex = pivotIndex + 1
  for i = pivotIndex + 1 to rightmostIndex
    if element[i] < element[pivot]
      swap(i, storeIndex); storeIndex++
  swap(pivot, storeIndex - 1)
```

#### 算法实现

``` Python
def quick_sort(array):
    
    def qsort(array, left, right):
        if left >= right:
            return array
        pivot = array[left]
        lp, rp = left, right
        while lp < rp:
            while array[rp] >= pivot and lp < rp:
                rp -= 1
            while array[lp] <= pivot and lp < rp:
                lp += 1
            array[lp], array[rp] = array[rp], array[lp]
        array[left], array[lp], array[lp], array[left]
        qsort(array, left, lp-1)
        qsort(array, rp+1, right)
        return array

    return qsort(array, 0, len(array)-1)
```

wiki版：

``` Python
def quicksort(array):
    if len(array) <= 1:
        return array
    l = [x for x in array[1:] if x <= array[0]]
    r = [x for x in array[1:] if x > array[0]]
    return quicksort(l) + [array[0]] + quicksort(r)
```

终极版：

``` Python 
qs = lambda xs : ((len(xs) <= 1 and [xs]) or [qs([x for x in xs[1:] if x < xs[0]]) + [xs[0]] + qs([x for x in xs[1:]])])[0]
```

### 堆排序 HeapSort

#### 算法思想

<blockquote>
	1、最大堆调整（Max-Heapify）：调整堆的末端子节点，使子节点永远小于父节点
	<br>
	2、创建最大堆（Build-Max-Heap）：重排堆的所有元素，使其成为最大堆
	<br>
	3、堆排序（Heap-Sort）：移除在第一个元素的根节点，并作最大堆调整的递归运算
</blockquote>

#### 算法可视化

{% qnimg Sort\HeapSort.gif %}

#### 算法伪码

```
HEAPSORT(A)
	BUILD-MAX-HEAP(A)
	for i = length(A) downto 2 
	  do exchange A[1] with A[i]
	    heapSize(A) = heapSize(A) - 1
	    MAX-HEAPIFY(A, 1)

BUILD-MAX-HEAP(A)
	heapSize(A) = length(A)
	for i = length(A)/2 downto 1
	  do MAX-HEAPIFY(A, i)

MAX-HEAPIFY(A, i)
	l = LEFT(i)
	r = RIGHT(i)
	if l <= heapSize(A) and A[l] > A[i]
	  then largest = l
	else largest = i
	if r <= heapSize(A) and A[r] > A[largest]
	  then largest = r
	if largest != i
	  then exchange A[i] with A[largest]
	  MAX-HEAPIFY(A, largest)
```

#### 算法实现

``` Python
def heap_sort(array):

    def max_heapify(start, end):
        root = start
        while True:
            child = 2 * root + 1
            if child > end:
                break
            if child + 1 <= end and array[child] < array[child+1]:
                child += 1
            if array[root] < array[child]:
                array[root], array[child] = array[child], array[root]
                root = child
            else:
                break

    for start in range((len(array)-2) // 2, -, -1):
        max_heapify(start, len(array)-1)

    for end in range(len(array)-1, 0, -1):
        array[0], array[end] = array[end], array[0]
        max_heapify(0, end-1)

    return array  
```