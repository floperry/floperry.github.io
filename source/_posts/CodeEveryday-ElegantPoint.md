---
title: 一天一道算法题-优雅的点
date: 2017-08-15 00:05:00
tags: [Python, 算法]
---

今天介绍的算法题为优雅的点，题目来源[网易2017年秋招笔试](https://www.nowcoder.com/question/next?pid=2811407&qid=46572&tid=9900704)。
<!--more-->

### 题目描述

小易有一个圆心在坐标原点的圆，小易知道圆的半径的平方。小易认为在圆上的点而且横纵坐标都是整数的点是优雅的，小易现在想寻找一个算法计算出优雅的点的个数，请你来帮帮他。  
例如：半径的平方如果为25  
优雅的点就有：(+/-3, +/-4)，(+/-4, +/-3)，(0, +/-5)，(+/-5, 0)，一共12个点。

#### 输入描述
<blockquote>
	输入为一个整数，即为圆半径的平方，范围在32位int范围内。
</blockquote>

#### 输出描述
<blockquote>
	输出为一个整数，即为优雅的点的个数
</blockquote>

#### 输入例子
<blockquote>
	25
</blockquote>

#### 输出例子
<blockquote>
	12
</blockquote>

### 解题思路

具体代码如下：

``` Python
from math import sqrt

def elegant_point(num):
    count = 0
    sqrt_num = int(sqrt(num))
    for i in range(1, sqrt_num+1):
        if i**2 + (int(sqrt(num - i**2)))**2 == num:
            count += 4
    return count

def main():
    num = int(input())
    count = elegant_point(num)
    print(count)

if __name__ == '__main__':
    main()
```