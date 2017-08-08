---
title: 一天一道算法题-Sqrt(x)
date: 2017-08-08 12:24:38
tags: [Python, 算法]
---

今天介绍的算法题为Sqrt(x)，题目来源[LintCode](http://www.lintcode.com/en/problem/sqrtx/)。
<!--more-->

### Description

Implement int ``sqrt(int x)``.
Compute and return the square root of x.

### Example

sqrt(3) = 1  
sqrt(4) = 2  
sqrt(5) = 2  
sqrt(10) = 3  

### Solution

#### 二分法
二分法是比较容易想到的一种方法，其基本思路是：在一个区间中，每次拿中间数的平方来测试，如果大了，就继续测试左区间的数；如果小了，就继续测试右区间的数。具体代码如下：

``` Python
class Solution:
    # @param x: An integer
    # @return: The sqrt of x
    def sqrt(self, x):
        start, end = 1, x
        while start + 1 < end:
            mid = (start + end) / 2
            if mid * mid == x:
                return mid
            elif mid * mid < x:
                start = mid
            else:
                end = mid
        if end * end <= x:
            return end
        return start
```

#### 牛顿法
牛顿法实际上是用线性取代非线性的求解方法。取$f(x)$泰勒展开得线性部分，并令其等于0，即
$$f(x_0)+f'(x_0)(x-x_0)=0$$
以此来作为$f(x)=0$的近似解。整理之后，就能推出
$$x_1 = x_0-\frac{f(x_0)}{f'(x_0)}$$

所以，牛顿迭代法的公式可以表示为
$$x_{n+1}=x_n-\frac{f(x_n)}{f'(x_n)}$$

具体代码如下：
``` Python
class Solution:
    # @param x: An integer
    # @return: The sqrt of x
    def sqrt(self, x):
        if x == 0 or x == 1:
            return x
        guess = x / 2
        fx = guess * guess - x
        count = 0
        while abs(fx) > 1 and count < 100:
            guess = guess - fx / (2.0 * guess)
            fx = guess * guess - x
            count += 1
        return int(guess)
```