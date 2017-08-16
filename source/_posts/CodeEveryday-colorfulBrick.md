---
title: 一天一道算法题-彩色的砖块
date: 2017-08-16 10:41:02
tags: [Python, 算法]
---

今天介绍的算法题为彩色的砖块，题目来源[网易2018校招内推编程题集合](https://www.nowcoder.com/question/next?pid=6291726&qid=112726&tid=9918487)。
<!--more-->

### 题目描述

小易有一些彩色的砖块，每种颜色由一个大写字母表示，各个颜色砖块看起来都完全一样。现在有一个给定的字符串s，s中每个字符代表小易的某个砖块的颜色。小易想把他所有的砖块排成一行。如果最多存在一对不同颜色的相邻砖块，那么这行砖块就很漂亮。请你帮助小易计算有多少种方式将他所有砖块排成漂亮的一行。（如果两种方式对应的砖块颜色序列是相同的，那么认为这两种方式是一样的。）  
例如：s = "ABAB"，那么小易有六种排列的结果：  
"AABB"，"ABAB"，"ABBA"，"BAAB"，"BABA"，"BBAA"  
其中只有"AABB"和"BBAA"满足最多只有一对不同颜色的相邻砖块。

#### 输入描述
<blockquote>
	输入包括一个字符串s，字符串s的长度length(1 <= length <= 50)，s中的每一个字符都为一个大写字母（A到Z）。
</blockquote>

#### 输出描述
<blockquote>
	输出一个整数，表示小易可以有多少种方式。
</blockquote>

#### 输入例子
<blockquote>
	ABAB
</blockquote>

#### 输出例子
<blockquote>
	2
</blockquote>

### 解题思路

不要被题目描述迷惑，这道题的本质就是判断字符串s中不同字符的个数。当不同字符的个数为1时，满足要求的排列方式只有1种；当不同字符的个数为2时，满足要求的排列方式只有2种；当不同字符的个数超过2个时，满足要求的排列方式不存在。具体代码如下：

``` Python
def color_brick(str):
    if len(set(str)) == 1:
        return 1
    elif len(set(str)) == 2:
        return 2
    else:
        return 0

def main():
    str = input()
    result = color_brick(str)
    print(result)

if __name__ == '__main__':
    main()
```