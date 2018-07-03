---
title: Codeforces #400 C. Molly's Chemicals
date: 2018-07-03 14:18:14
mathjax: true
tags: 
    - 思维技巧
categories: Algorithm
---

*思维技巧*

## description

>Molly Hooper has n different kinds of chemicals arranged in a line. Each of the chemicals has an affection value, The i-th of them has affection value ai.
Molly wants Sherlock to fall in love with her. She intends to do this by mixing a contiguous segment of chemicals together to make a love potion with total affection value as a non-negative integer power of k. Total affection value of a continuous segment of chemicals is the sum of affection values of each chemical in that segment.
Help her to do so in finding the total number of such segments.

## Input

>The first line of input contains two integers, n and k, the number of chemicals and the number, such that the total affection value is a non-negative power of this number k. (1 ≤ n ≤ 105, 1 ≤ |k| ≤ 10).
Next line contains n integers a1, a2, ..., an ( - 109 ≤ ai ≤ 109) — affection values of chemicals.

## Output

>Output a single integer — the number of valid segments.

## Examples 
### input

>4 2
2 2 2 2

### output

>8

### input

>4 -3
3 -6 -3 12

### output

>3

## 题解

>首先输入n，k。之后输入n个数，求子序列和为 k ^ m ( m = 0,1,2,3 ...) 的数量。
>定义 sum[i] 为前i个数的和，则有 sum[i] - sum[j] == k ^ m, 但是算两次 sum[i] 和 sum[j] 时间复杂度就较大；我们来转化一下公式，使它变成 以下这样  sum[i] - k ^ m ==  sum[j] ，我们可以发现，如果存在 sum[i] - k ^ m 的值，那么就可以说明有这么一对 i ， j 使结果存在，但是时间复杂度为 o( m lg n )。 
 
## code

```c++
#include <cstdio>
#include <cstring>
#include <iostream>
#include <queue>
#include <map>
#include <cmath>
#include <algorithm>
#include <vector>
#include <cstdlib>
#include <cctype>
#include <set>
#define For(i,a,b) for(int (i)=(a);(i) < (b); ++(i))
#define Fors(i,a,b) for(int (i)=(a);(i) > (b); --(i))
#define sd(x) cout << "start debug:" << (x) << endl;
#define ed(x) cout << "end debug:" << (x) << endl;
#define rei(x) scanf("%d",&x)
#define rel(x) scanf("%lld",&x)
using namespace std;
typedef long long ll;
typedef pair<int,int> pii;
typedef vector<int> vi;
typedef queue<int> qi;
typedef long long LL;
const LL INF = 1e14;

int n;
LL k,sum[100050];
map<LL,LL>x;
set<LL>a;
set<LL>::iterator it;

int main()
{
    while(~scanf("%d %lld",&n,&k))
    {
        x.clear();
        a.clear();
        sum[0]=0;
        for(int i=1;i<=n;i++) scanf("%lld",&sum[i]),sum[i]+=sum[i-1];

        a.insert(1);
        LL temp=k;
        for(int i=1;i<=60;i++){
            if(temp>INF) break;
            a.insert(temp);
            temp*=k;
        }

        LL ans=0;
        x[0]=1;
        for(int i=1;i<=n;i++){
            for(it=a.begin();it!=a.end();it++) ans+=x[sum[i]-*it];
            x[sum[i]]++;
        }
        printf("%lld\n",ans);
    }
    return 0;
}
```
