---
title: 一天一道算法题-疯狂队列
date: 2017-08-19 19:34:05
tags: [Python, 算法]
---

今天介绍的算法题为疯狂队列，题目来源[网易2018校招内推编程题集合](https://www.nowcoder.com/question/next?pid=6291726&qid=112723&tid=10050450)。
<!--more-->

### 题目描述

小易老师是非常严厉的，他会要求所有学生在进入教室前都排成一列，并且他要求学生按照身高不递减的顺序排列。有一次，n个学生在列队的时候，小易老师正好去卫生间了。学生们终于有机会反击了，于是学生们决定来一次疯狂的队列，他们定义一个队列的疯狂值为每对相邻排列学生身高差的绝对值总和。。由于按照身高顺序排列的队列的疯狂值是最小的，他们当然按照疯狂值最大的顺序来进行列队。现在给出n个学生的身高，请计算出这些学生列队的最大可能的疯狂值。

#### 输入描述
<blockquote>
	输入包括两行，第一行一个整数n (1 <= n <= 50)，表示学生的人数  
	第二行为n个整数h[i] (1 <= h[i] <= 1000)，表示每个学生的身高
</blockquote>

#### 输出描述
<blockquote>
	输出一个整数，表示n个学生列队可以获得的最大的疯狂值  
	<br>
	如样例所示：  
	当队列排列顺序是：25-10-40-5-25，身高差的绝对值的综合为15+30+35+20=100，这是最大的疯狂值了。
</blockquote>

#### 输入例子
<blockquote>
	5  
	5 10 25 40 25
</blockquote>

#### 输出例子
<blockquote>
	100
</blockquote>

### 解题思路

题目稍微有些复杂，对于数字类的题目，最重要的是找到满足要求时数字的排列规律。具体到本题，当疯狂值最大时，数字的排列规律是：数列的中间是最大的数，然后右边和左边依次是最小和次小的数，再往外的右边和左边是次大和第三大的数...以此类推。找到数字排列的规律，剩下的就是实现的问题。为方便实现，这里采用队列的数据结构。具体代码如下：

``` Python
from collections import deque
import copy

def compute_queue(queue):
    return sum(abs(queue[i+1]-queue[i]) for i in range(len(queue)-1))

def crazy_queue(n, list):
    list.sort()
    queue = deque(list)
    new_queue = deque()
    count = 0
    new_queue.append(queue.pop())
    while len(queue) > 0:
        if count % 2 == 0:
            if len(queue) != 0:
                new_queue.append(queue.popleft())
            if len(queue) != 0:
                new_queue.appendleft(queue.popleft())
            count += 1
        else:
            if len(queue) != 0:
                new_queue.append(queue.pop())
            if len(queue) != 0:
                new_queue.appendleft(queue.pop())
            count += 1
    if n % 2 == 1:
        queue_tmp = copy.copy(new_queue)
        queue_tmp.append(queue_tmp.popleft())
        return max(compute_queue(new_queue), compute_queue(queue_tmp))
    return compute_queue(new_queue)

def main():
    n = int(input().strip())
    list = [int(x) for x in input().strip().split()]
    result = crazy_queue(n, list)
    print(result)

if __name__ == '__main__':
    main()
```