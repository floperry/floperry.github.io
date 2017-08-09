---
title: 一天一道算法题-PalinSequence
date: 2017-08-09 11:52:07
tags: [Python, 算法]
---

今天介绍的算法题为回文序列，题目来源[网易2017年秋招笔试](https://www.nowcoder.com/test/question/0147cbd790724bc9ae0b779aaf7c5b50?pid=2811407&tid=9698147)。
<!--more-->

### 题目描述

如果一个数字序列逆置之后跟原序列是一样的就称这样的数字序列为回文序列。例如：
{1,2,1}, {15,78,78,15}, {112}是回文序列。
{1,2,2}, {15,78,87,51}, {112,2,11}不是回文序列。
现在给出一个数字序列，允许使用一种转换操作：
选择任意两个相邻的数，然后从序列移除这两个数，并用这两个数字的和插入到这两个数之前的位置（只插入一个和）。
现在对于所给序列要求出最少需要多少次操作可以将其变成回文序列。

#### 输入描述
<blockquote>
	输入为两行，第一行为序列长度n (1 <= n <=50) 
	<br> 
	第二行为序列中的n个整数item[i] (1 <= item[i] <= 1000)，以空格分隔
</blockquote>

#### 输出描述
<blockquote>
	输出一个数，表示最少需要的转换次数
</blockquote>

#### 输入例子
<blockquote>
	4
	<br>
	1 1 1 3
</blockquote>

#### 输出例子
<blockquote>
	2
</blockquote>

### 解题思路

思路比较简单，直接进行首尾指针跟踪，两个数不相等就进行转换操作：小的数加上相邻的数。具体代码如下：

``` Python
def palin(item, start, end):
    left = item[start]
    right = item[end]
    count = 0
    while start < end:
        if left < right:
            start += 1
            left += item[start]
            count += 1
        elif left > right:
            end -= 1
            right += item[end]
            count += 1
        else:
            start += 1
            end -= 1
            left = item[start]
            right = item[end]
    return count

def main():
    n = int(input().strip())
    item = [int(x) for x in input().strip().split()]
    count = palin(item, 0, n-1)
    print(count)

if __name__ == '__main__':
    main()
```


