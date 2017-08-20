---
title: 一天一道算法题-独立的小易
date: 2017-08-20 22:30:47
tags: [Python, 算法]
---

今天介绍的算法题为独立的小易，题目来源[网易2018年校招内推编程题集合](https://www.nowcoder.com/question/next?pid=6291726&qid=112726&tid=10078527)。
<!--more-->

### 题目描述

小易为了向他的父母表现他已经长大独立了，他决定搬出去自己居住一段时间。一个人生活增加了许多花费：小易每天必须吃一个水果并且需要每天支付x元的房屋租金。当前小易手中已经有f个水果和d元钱，小易也能去商店购买一些水果，商店每个水果售卖p元。小易为了表现他独立生活的能力，希望能独立生活的时间越长越好，小易希望你来帮他计算一下他最多能独立生活多少天。

#### 输入描述
<blockquote>
	输入包括一行，四个整数x, f, d, p (1 <= x, f, d, p <= 2 * 10^9)，以空格分隔
</blockquote>

#### 输出描述
<blockquote>
	输出一个整数，表示小易最多能独立生活多少天
</blockquote>

#### 输入例子
<blockquote>
	3 5 100 10
</blockquote>

#### 输出例子
<blockquote>
	11
</blockquote>

### 解题思路

直接根据数学列公式即可，需要考虑两种情况：一是水果的个数大于生活的天数时，多余的水果可以出售换钱，二是当水果的个数小于生活的天数时，需要额外花钱购买水果。具体的代码如下：
``` Python
def indep_day(x, f, d, p):
    if d // x > f:
        day = (d + f * p) // (x + p)
    else:
        day = d // x
    return day

def main():
    x, f, d, p = [int(x) for x in input().strip().split()]
    result = indep_day(x, f, d, p)
    print(result)

if __name__ == '__main__':
    main()
```