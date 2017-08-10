---
title: 一天一道算法题-数字翻转
date: 2017-08-10 21:03:23
tags: [Python, 算法]
---

今天介绍的算法题为数字翻转，题目来源[网易2017年秋招笔试](https://www.nowcoder.com/question/next?pid=2811407&qid=46572&tid=9738957)。
<!--more-->

### 题目描述

对于一个整数X，定义操作rev(X)为将X按数位翻转过来，并且去除掉前导0。例如：
如果X = 123，则rev(X) = 321；
如果X = 100，则rev(X) = 1。
现在给出整数x和y，要求rev(rev(x) + rev(y))为多少？

#### 输入描述
<blockquote>
	输入为一行，x, y(1 <= x、y <= 1000)，以空格隔开
</blockquote>

#### 输出描述
<blockquote>
	输出rev(rev(x) + rev(y))的值
</blockquote>

#### 输入例子
<blockquote>
	123 100
</blockquote>

#### 输出例子
<blockquote>
	223
</blockquote>

### 解题思路

题目比较简单，重点是如何对输入的整数进行翻转，即将数字的最高位与最低位互换位置。通常的操作是，对数字进行取余来获得最低位，对数字进行除10来去掉最低位。具体的代码如下：

``` Python
def rev(num):
    new_num = 0
    while num > 0:
        new_num = new_num * 10 + num % 10
        num = num // 10
    return new_num

def main():
    num = [int(i) for i in input().strip().split()]
    result = rev(rev(num[0]) + rev(num[1]))
    print(result)

if __name__ == '__main__':
    main()
```
