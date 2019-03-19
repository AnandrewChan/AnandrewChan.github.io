---
title: 剑指_LongestSubstringWithoutDup
date: 2019-03-18
tags: code
---
#### 面试题48：最长不含重复字符的子字符串
---
题目：请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。假设字符串中只包含从'a'到'z'的字符。
<!--more-->
---
方法一：蛮力法
方法二：动态规划
```
/*******************************************************************
Copyright(c) 2016, Harry He
All rights reserved.

Distributed under the BSD license.
(See accompanying file LICENSE.txt at
https://github.com/zhedahht/CodingInterviewChinese2/blob/master/LICENSE.txt)
*******************************************************************/


#include <iostream>
#include <string>

// 方法一：蛮力法
bool hasDuplication(const std::string& str, int position[]);

int longestSubstringWithoutDuplication_1(const std::string& str)
{
    int longest = 0;
    int* position = new int[26];
    for (int start = 0; start < str.length(); ++start) {
        for (int end = start; end < str.length(); ++end) {
            int count = end - start + 1;
            const std::string& substring = str.substr(start, count);
            if (!hasDuplication(substring, position)) {
                if (count > longest)
                    longest = count;
            } else
                break;
        }
    }

    delete[] position;
    return longest;
}

bool hasDuplication(const std::string& str, int position[])
{
    for (int i = 0; i < 26; ++i)
        position[i] = -1;

    for (int i = 0; i < str.length(); ++i) {
        int indexInPosition = str[i] - 'a';
        if (position[indexInPosition] >= 0)
            return true;

        position[indexInPosition] = indexInPosition;
    }

    return false;
}

// 方法二：动态规划
int longestSubstringWithoutDuplication_2(const std::string& str)
{
    int curLength = 0;
    int maxLength = 0;

    int* position = new int[26];
    for (int i = 0; i < 26; ++i)
        position[i] = -1;

    for (int i = 0; i < str.length(); ++i) {
        int prevIndex = position[str[i] - 'a'];
        if (prevIndex < 0 || i - prevIndex > curLength)
            ++curLength;
        else {
            if (curLength > maxLength)
                maxLength = curLength;

            curLength = i - prevIndex;
        }
        position[str[i] - 'a'] = i;
    }

    if (curLength > maxLength)
        maxLength = curLength;

    delete[] position;
    return maxLength;
}
```