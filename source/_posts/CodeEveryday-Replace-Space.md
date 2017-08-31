---
title: 一天一道算法题-替换空格
date: 2017-08-30 19:33:05
tags: [Python, C/C++, 算法]
---

今天介绍的算法题为替换空格，题目来源剑指offer，在线编程可上[牛客OJ](https://www.nowcoder.com/practice/4060ac7e3e404ad1a894ef3e17650423?tpId=13&tqId=11155&tPage=1&rp=1&ru=/ta/coding-interviews&qru=/ta/coding-interviews/question-ranking)。
<!--more-->

### 题目描述

请实现一个函数，把字符串中的每个空格替换成"%20"。例如输入"We are happy."，则输出"We%20are%20happy."。

### 解题思路

最直观的思路是从头到尾扫描字符串，每一次碰到空格字符的时候做替换。由于是把1个字符替换成3个字符，必须要把空格后面的所有字符都后移两个位置。假设字符串的长度是$n$，对每个空格字符，需要移动后面的$O(n)$个字符，因此对含有$O(n)$个空格的字符串而言，总的时间效率是$O(n^2)$。在这种替换方式中，我们发现很多字符都移动了很多次。有没有方法可以减少移动的次数呢？答案是肯定的。我们换一种方式，把从前向后替换改成从后往前替换。同时，可以先遍历一遍字符串来统计出字符串中空格的个数，并以此确定替换后字符串的长度。通过这种方式，所有的字符都只需要移动一次，算法总的时间效率为$O(n)$。具体代码实现如下：

C/C++实现

``` C
void ReplaceSpace(char string[], int length) {
    if (string == NULL || length <= 0) {
        return;
    }

    int oldLength = 0;
    int numberOfSpace = 0;
    int i = 0;
    while (string[i] != '\0') {
    	++ oldLength;
    	if (string[i] == ' ') {
            ++ numberOfSpace;
    	}
    	++ i;
    }

    int newLength = oldLength + numberOfSpace * 2;
    int indexOfOld = oldLength;
    int indexOfNew = newLength;

    while (indexOfOld >= 0 && indexOfNew > indexOfOld) {
    	if (string[indexOfOld] == ' ') {
            string[indexOfNew--] = '0';
            string[indexOfNew--] = '2';
            string[indexOfNew--] = '%';
    	}
    	else {
            string[indexOfNew--] = string[indexOfOld];
    	}
    	-- indexOfOld;
    }
}
```

Python实现

``` Python
def ReplaceSpace(string):
    list = list(string)
    numberOfSpace = len([x for x in list if x == ' '])
    oldLength, newLength = len(list), len(list) + numberOfSpace * 2
    [s.append('') for i in range(numberOfSpace * 2)]
    indexOfOld, indexOfNew = oldLength - 1, newLength -1

    while (indexOfOld >= 0 and indexOfNew > indexOfOld):
        if (list[indexOfOld] == ' '):
            list[indexOfNew-2], list[indexOfNew-1], list[indexOfNew] = '%20'
            indexOdNew -= 3
        else:
            list[indexOfNew] = list[indexOfOld]
            indexOfNew -= 1
        indexOfOld -= 1

    print(''.join(list))
```

完整代码及测试请移步[Github](https://github.com/floperry/CodeEveryday/tree/master/offer/04-Replace-Space)。关于本题更具体的讲解，可参考[链接-替换空格](https://github.com/gatieme/CodingInterviews/tree/master/004-%E6%9B%BF%E6%8D%A2%E7%A9%BA%E6%A0%BC)。

### Tips

合并两个数组（包括字符串）时，如果从前往后复制每个数字（或字符）需要重复移动数字（或字符）多次，那么我们可以考虑从后往前复制，这样就能减少移动的次数，从而提高效率。


