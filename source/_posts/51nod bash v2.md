---
title: 51nod bash v2
date: 2016-12-28 16:31:57
tags: 
    - 博弈
    - 巴什博奕
categories: Algorithm
---

### 描述：
有一堆石子共有N个。A B两个人轮流拿，A先拿。每次只能拿1，3，4颗，拿到最后1颗石子的人获胜。假设A B都非常聪明，拿石子的过程中不会出现失误。给出N，问最后谁能赢得比赛。例如N = 2。A只能拿1颗，所以B可以拿到最后1颗石子。

### Input
第1行：一个数T，表示后面用作输入测试的数的数量。（1 <= T <= 10000)第2 - T + 1行：每行1个数N。(1 <= N <= 10^9)

### Output
共T行，如果A获胜输出A，如果B获胜输出B。

### Input示例
3234
### Output示例
BAA
### 题解：

从1个石子推下去发现7个一循环

### 代码

```c++
include<stdio.h>
int main()
{
    freopen("a.txt","r",stdin);
    long long N,K;
    int t;
    scanf("%d",&t);
    int bo[7]={0,1,0,1,1,1,1};
    while(t--){
        scanf("%d",&N);
        N%=7;
        printf("%d ",N);
        if(bo[N])
        	printf("A\n");
        else
        	printf("B\n");
    }
}
```