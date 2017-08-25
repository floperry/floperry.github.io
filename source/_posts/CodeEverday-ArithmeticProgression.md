---
title: 一天一道算法题-等差数列
date: 2017-08-18 16:00:54
tags: [Python, 算法]
---

今天介绍的算法题为等差数列，题目来源[网易2018校招内推编程题集合](https://www.nowcoder.com/question/next?pid=6291726&qid=112723&tid=10013199)。
<!--more-->

### 题目描述

日过一个数列S满足对于所有的合法的i，都有S[i + 1] = S[i] + d，这里的d可以是负数和零，我们就称数列S为等差数列。  
小易现在有一个长度为n的数列x，小易想把x变成一个等差数列。小易允许在数列上做交换任意两个位置的数值的操作，并且交换操作允许多次。但是有些数列通过交换还是不能变成等差数列，小易需要判别一个数列是否能通过交换操作变成等差数列。

#### 输入描述
<blockquote>
	输入包括两行，第一行包含整数n (2 <= n <= 50)，即数列的长度。  
	第二行n个元素x[i] (0 <= x[i] <= 1000)，即数列中的每个整数。
</blockquote>

#### 输出描述
<blockquote>
	如果可以变成等差数列输出"Possible"，否则输出"Impossible"。
</blockquote>

#### 输入例子
<blockquote>
	3  
	3 1 2
</blockquote>

#### 输出例子
<blockquote>
	Possible
</blockquote>

### 解题思路

先对数列进行排序，然后依次比较前后两个元素的差值是否相等即可。具体代码如下：

``` Python
def arith_pro(n, list):
    list.sort()
    d = list[1] - list[0]
    for i in range(2, n):
        if list[i] - list[i-1] != d:
            return 'Impossible'
        else:
            continue
    return 'Possible'

def main():
    n = int(input().strip())
    list = [int(x) for x in input().strip().split()]
    result = arith_pro(n, list)
    print(result)

if __name__ == '__main__':
    main()
```