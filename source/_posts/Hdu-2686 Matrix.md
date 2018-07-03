---
title: Hdu-2686 Matrix
date: 2016-12-28 16:31:57
mathjax: true
tags: 
    - Muti-treaded 
    - DP
categories: Algorithm
---

*类型：多线程DP*

### 描述：

Yifenfei very like play a number game in the n*n Matrix. A positive integer number is put in each area of the Matrix.
Every time yifenfei should to do is that choose a detour which frome the top left point to the bottom right point and than back to the top left point with the maximal values of sum integers that area of Matrix yifenfei choose. But from the top to the bottom can only choose right and down, from the bottom to the top can only choose left and up. And yifenfei can not pass the same area of the Matrix except the start and end. 

### Input

The input contains multiple test cases.
Each case first line given the integer n (2<n<30) 
Than n lines,each line include n positive integers.(<100)

### output

For each test case output the maximal values yifenfei can get.

### sample Input

2
10 3
5 10
3
10 3 3
2 5 3
6 7 10
5
1 2 3 4 5
2 3 4 5 6
3 4 5 6 7
4 5 6 7 8
5 6 7 8 9

### Samput Output

28
46
80

### 题解：

这个题目是一道 **多线程DP** ，题目简单的可理解为，在矩阵中选择两条不相交的最优路径，最优理解为使路径上数字的和最大。简单分析之后，可以发现此题由原来方向为==左上 -> 右下 -> 左上== 的路径可转换为两条方向为==左上 -> 右下== 的路径同时出发，且不相交，**多线程 ** 得到体现。

规定dp\[x~1~]\[y~1~][x~2~]\[y~2~]表示为两条路径的终点分别为（x~1~ ，y~1~）,（x~2~ ,y~2~），原矩阵数据存在MAP\[]\[]中。对于当前时刻的状态，因为一个点的位置的前一个状态有两种可能性，所以一共有四种可能性，状态转移方程具体为：

dp\[x~1~]\[y~1~][x~2~]\[y~2~] = max(dp\[x~1~-1]\[y~1~][x~2~-1]\[y~2~],dp\[x~1~-1]\[y~1~][x~2~]\[y~2~-1],dp\[x~1~]\[y~1~-1][x~2~-1]\[y~2~],dp\[x~1~]\[y~1~-1][x~2~]\[y~2~-1]) + MAP\[x~1~]\[y~1~]

当x~1~ != x~2~ 或 y~1~ != y~2~ 时dp\[x~1~]\[y~1~][x~2~]\[y~2~] += MAP\[x~2~]\[y~2~]

分析下时间复杂度为O(n^4^)，空间复杂度为O(n^4^)。如何优化呢？

有这么一个事实，当明确一个终点的距离原点的距离后，如果知道该终点的横左边，那么由距离可知唯一的纵左边，并且两个不同的重点有唯一相同属性**到原点的距离** ，即x~1~ + y~1~ = x~2~ + y~2~。

那么我们可以设一个变量L，用来存坐标与原点的距离。dp\[l]\[x~1~][x~2~] 则可以简单的表示出原来的dp\[x~1~]\[y~1~][x~2~]\[y~2~] ，但是空间与时间复杂度变为O(n^3^).

### 代码：

```c++
include <iostream>
include <cstdio>
include <cstring>
using namespace std;

int max(int a,int b,int c,int d){
    int tmp1 = max(a,b);
    int tmp2 = max(c,d);
    return max(tmp1,tmp2);
}

int main()
{
    int n;
    int MAP[40][40],dp[80][40][40];

    while (scanf("%d",&n) != EOF ){
        memset(MAP,0,sizeof(MAP));
        memset(dp,0,sizeof(dp));
        for (int i = 1; i <= n; ++i){
            for (int j = 1; j <= n; ++j){
                scanf("%d",&MAP[i][j]);
            }
        }

        for (int l = 1; l <= (2 * n - 1); ++l)
            for (int x1 = 1; x1 <= n; ++x1)
                for (int x2 = 1; x2 <=n; ++x2){
                    int tmp_y1 = l - x1 + 1,tmp_y2 = l - x2 + 1;
                    dp[l][x1][x2] = max(dp[l-1][x1-1][x2-1],dp[l-1][x1][x2],
                                        dp[l-1][x1-1][x2],dp[l-1][x1][x2-1]) + MAP[x1][tmp_y1];
                    if (x1 != x2 || tmp_y1 != tmp_y2)
                        dp[l][x1][x2] += MAP[x2][tmp_y2];
                }
                
        cout << dp[2*n-1][n][n]<<endl;
    }
    return 0;
}
```

