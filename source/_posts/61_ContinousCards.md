---
title: 剑指_ContinousCards
date: 2019-03-18
tags: code
---
#### 扑克牌的顺子
---
题目：从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王可以看成任意数字。
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
#include <cstdlib>

int Compare(const void* arg1, const void* arg2);

bool IsContinuous(int* numbers, int length)
{
    if (numbers == nullptr || length < 1)
        return false;

    qsort(numbers, length, sizeof(int), Compare);

    int numberOfZero = 0;
    int numberOfGap = 0;

    // 统计数组中0的个数
    for (int i = 0; i < length && numbers[i] == 0; ++i)
        ++numberOfZero;

    // 统计数组中的间隔数目
    int small = numberOfZero;
    int big = small + 1;
    while (big < length) {
        // 两个数相等，有对子，不可能是顺子
        if (numbers[small] == numbers[big])
            return false;

        numberOfGap += numbers[big] - numbers[small] - 1;
        small = big;
        ++big;
    }

    return (numberOfGap > numberOfZero) ? false : true;
}

int Compare(const void* arg1, const void* arg2)
{
    return *(int*)arg1 - *(int*)arg2;
}
```