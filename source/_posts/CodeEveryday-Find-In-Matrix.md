---
title: 一天一道算法题-二维数组中的查找
date: 2017-08-25 21:28:40
tags: [Python, C/C++, 算法]
---

今天介绍的算法题为二维数组中的查找，题目来源剑指offer，在线编程可上[牛客OJ](https://www.nowcoder.com/practice/abc3fe2ce8e146608e868a70efebf62e?tpId=13&tqId=11154&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)。
<!--more-->

### 题目描述

在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

### 解题思路
比较容易想到的就是直接对二维数组进行遍历，看是否存在需要查找的数，暴力但不美观。是不是有更好的解决方案呢？答案是肯定的。注意一下给定二维数组的规律：**每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序**，即二维数组的左上角和右下角分别是最小和最大的元素。很自然的，我们可以想到以下解题思路：首先选取数组中右上角的数字，如果该数字等于要查找的数字，查找过程结束；如果该数字大于要查找的数字，删除该列；如果该数字小于要查找的数字，删除该行。这样一来，每一次判断都可以缩小查找的范围，直到找到所要查找的数字，或者查找范围为空。具体代码实现如下：

C/C++实现
``` C
bool Find(int* matrix, int rows, int columns, int number) {
    bool find = false;
    if (matrix != NULL && rows > 0 && columns > 0) {
        int row = 0;
        int column = columns - 1;
        while (row < rows && column >= 0) {
            if (matrix[row * columns + column] == number) {
                find = true;
                break;
            }
            else if (matrix[row * columns + column] > number) {
                -- column;
            }
            else {
                ++ row;
            }
        }
    }
    return find;
}
```

Python实现
``` Python
def Find(list, number):
    find = False
    rows, columns = len(list), len(list[0])
    if (rows > 0 and columns > 0):
        row, column = 0, columns - 1
        while (row < rows and column >= 0):
            if (list[row][column] == number):
                find = True
                break
            elif (list[row][column] > number):
                column -= 1
            else:
                row += 1
    return find
```

完整代码及测试请移步[Github](https://github.com/floperry/CodeEveryday/tree/master/offer/03-Find-In-Matrix)。关于本题更具体的讲解，可参考[链接-二维数组中的查找](https://github.com/gatieme/CodingInterviews/tree/master/003-%E4%BA%8C%E7%BB%B4%E6%95%B0%E7%BB%84%E4%B8%AD%E7%9A%84%E6%9F%A5%E6%89%BE)。
