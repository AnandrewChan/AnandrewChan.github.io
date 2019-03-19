---
title: 构建乘积数组ConstuctArray
date: 2019-03-19
tags:
- code
- array
- 剑指
---
#### 构建乘积数组
---
题目：给定一个数组A[0, 1, …, n-1]，请构建一个数组B[0, 1, …, n-1]，其中B中的元素B[i] =A[0]×A[1]×… ×A[i-1]×A[i+1]×…×A[n-1]。不能使用除法。
<!--more-->
思路：
B[i]的值可以看作下图的矩阵中每行的乘积。
下三角用连乘可以很容求得，上三角，从下向上也是连乘。
先算下三角中的连乘，即我们先算出B[i]中的一部分，然后倒过来按上三角中的分布规律，把另一部分也乘进去。
```
class Solution {
public:
    vector<int> multiply(const vector<int>& A)
    {

        int n = A.size();
        vector<int> B0(n, 1);
        vector<int> B1(n, 1);

        for (int i = 1; i < n; ++i) {
            B0[i] = B0[i - 1] * A[i - 1];
        }
        for (int i = n - 2; i >= 0; --i) {
            B1[i] = B1[i + 1] * A[i + 1];
        }

        vector<int> B(n, 1);
        for (int i = 0; i < n; ++i) {
            B[i] = B0[i] * B1[i];
        }

        return B;
    }
};
```
原书代码
```
#include <cstdio>
#include <vector>

using namespace std;

void BuildProductionArray(const vector<double>& input, vector<double>& output)
{
    int length1 = input.size();
    int length2 = output.size();

    if (length1 == length2 && length2 > 1) {
        output[0] = 1;
        for (int i = 1; i < length1; ++i) {
            output[i] = output[i - 1] * input[i - 1];
        }

        double temp = 1;
        for (int i = length1 - 2; i >= 0; --i) {
            temp *= input[i + 1];
            output[i] *= temp;
        }
    }
}
```