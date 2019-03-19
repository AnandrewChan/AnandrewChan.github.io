---
title: 剑指_LastNumberInCircle
date: 2019-03-18
tags: code
---
#### 圆圈中最后剩下的数字
---
题目：0, 1, …, n-1这n个数字排成一个圆圈，从数字0开始每次从这个圆圈里删除第m个数字。求出这个圆圈里剩下的最后一个数字。
<!--more-->
```
/*******************************************************************
Copyright(c) 2016, Harry He
All rights reserved.

Distributed under the BSD license.
(See accompanying file LICENSE.txt at
https://github.com/zhedahht/CodingInterviewChinese2/blob/master/LICENSE.txt)
*******************************************************************/


#include <cstdio>
#include <list>

using namespace std;

// ====================方法1====================
int LastRemaining_Solution1(unsigned int n, unsigned int m)
{
    if (n < 1 || m < 1)
        return -1;

    unsigned int i = 0;

    list<int> numbers;
    for (i = 0; i < n; ++i)
        numbers.push_back(i);

    list<int>::iterator current = numbers.begin();
    while (numbers.size() > 1) {
        for (int i = 1; i < m; ++i) {
            current++;
            if (current == numbers.end())
                current = numbers.begin();
        }

        list<int>::iterator next = ++current;
        if (next == numbers.end())
            next = numbers.begin();

        --current;
        numbers.erase(current);
        current = next;
    }

    return *(current);
}

// ====================方法2====================
int LastRemaining_Solution2(unsigned int n, unsigned int m)
{
    if (n < 1 || m < 1)
        return -1;

    int last = 0;
    for (int i = 2; i <= n; i++)
        last = (last + m) % i;

    return last;
}
```