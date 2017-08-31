---
title: 一天一道算法题-从尾到头打印链表
date: 2017-08-31 16:00:23
tags: [Python, C/C++, 算法]
---

今天介绍的算法题为从尾到头打印链表，题目来源剑指offer，在线编程可上[牛客OJ](https://www.nowcoder.com/practice/d0267f7f55b3412ba93bd35cfa8e8035?tpId=13&tqId=11156&tPage=1&rp=1&ru=%2Fta%2Fcoding-interviews&qru=%2Fta%2Fcoding-interviews%2Fquestion-ranking)。
<!--more-->

### 题目描述

输入一个链表的头结点，从尾到头反过来打印出每个节点的值。

### 解题思路

看到题目后的第一反应就是将链表中节点的指针反转过来，改变链表的方向，然后从头到尾打印出链表的值就可以了。但是，打印通常是一个只读操作，并不希望打印时改变链表的结构，这种思路就不可行了。换一个思路，我们遍历链表的顺序是从头到尾，而打印链表的顺序是从尾到头，是典型的“后进先出”，进而想到可以用栈来实现。也就是说，每次遍历到一个节点时，我们先把该节点压入栈中，当遍历完成后，再依次将每一个节点从栈中弹出并打印即可。具体代码实现如下：

C/C++实现

``` C
struct ListNode {
    int value;
    ListNode* next;
};

vector<int> PrintListReversingly(ListNode* head) {
    stack<ListNode*> nodes;
    ListNode* pNode = head;

    while (pNode != NULL) {
        nodes.push(pNode);
        pNode = pNode->next;
    }

    vector<int> result;

    while (!nodes.empty()) {
        pNode = nodes.top();
        result.push_back(pNode->value);
        nodes.pop();
    }

    return result;
}
```

考虑到递归本质上就是一个栈结构，因此可以使用递归来实现。基于递归的代码看起来很简洁，但是当链表太长时，会加深函数调用的层级，从而导致函数调用栈溢出。从代码的鲁棒性看，基于循环实现的代码要更优。

C/C++实现（递归）

``` C
vector<int> PrintListReversingly_Recursively(ListNode* head) {
    vector<int> result;

    if (head != NULL) {
        if (head->next != NULL) {
            PrintListReversingly_Recursively(head->next);
        }

        result.push_back(head->value);
    }

    return result;
}
```

Python用类来实现链表的数据结构。以下代码用于实现一个Node类：

``` Python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
```

我们可以考虑两种方式来完成链表从尾到头的打印，具体代码实现如下：

1、新建list作为栈结构

``` Python
def PrintListReversingly(listNode):
    newList = []

    while (listNode is not None):
        newList.append(listNode.data)
        listNode = listNode.next

    return newList[::-1]
```

2、使用list的insert()函数

``` Python
def PrintListReversingly(listNode):
    newList = []

    while (listNode is not None):
        newList.insert(0, listNode.data)
        listNode = listNode.next

    return newList
```
