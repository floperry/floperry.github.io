---
title: 一天一道算法题-最大的奇约数
date: 2017-08-12 21:53:23
tags: [Python, 算法]
---

今天介绍的算法题为最大的奇约数，题目来源[网易2017年秋招笔试](https://www.nowcoder.com/question/next?pid=2811407&qid=46572&tid=9838879)。
<!--more-->

### 题目描述

小易是一个数论爱好者，并且对于一个数的奇数十分感兴趣。一天小易遇到这样一个问题：定义函数f(x)为x最大的奇数约数，x为正整数。例如：f(44) = 11。
现在给出一个N，需要求出f(1) + f(2) + f(3) ... f(N)  
例如：N = 7  
f(1) + f(2) + f(3) + f(4) + f(5) + f(6) + f(7) = 1 + 1 + 3 + 1 + 5 + 3 + 7 = 21  
小易计算这个问题遇到了困难，需要你来设计一个算法帮助他。

#### 输入描述
<blockquote>
	输入一个整数N (1 <= N <= 1000000000)
</blockquote>

#### 输出描述
<blockquote>
	输出一个整数，即为f(1) + f(2) + f(3) ... f(N)
</blockquote>

#### 输入例子
<blockquote>
	7
</blockquote>

#### 输出例子
<blockquote>
	21
</blockquote>

### 解题思路

一开始想的比较简单，对于每个数，先求其最大奇约数，然后直接进行求和，代码如下：

``` Python
def max_odd_divisor(num):
    while (num % 2) == 0:
        num = num // 2
    return num

def main():
    n = int(input())
    result = 0
    for i in range(1, n+1):
        result += max_odd_divisor(i)
    print(result)

if __name__ == '__main__':
    main()
```

提交后发现代码运行超时，分析一下，算法的时间复杂度为O(n)，但是输入数据的大小达到了10^10，显然不能满足要求，需要进一步对算法进行优化。