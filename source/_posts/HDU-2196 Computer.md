---
title: HDU-2196 Computer
date: 2016-12-28 16:31:57
tags: 树形DP
categories: Algorithm
---

*类型： 树形DP 、树的最长路*

### Description

A school bought the first computer some time ago(so this computer's id is 1). During the recent years the school bought N-1 new computers. Each new computer was connected to one of settled earlier. Managers of school are anxious about slow functioning of the net and want to know the maximum distance Si for which i-th computer needs to send signal (i.e. length of cable to the most distant computer). You need to provide this information. 

​							 ![hdu_2196](C:\Users\AndyFrancis\Desktop\hdu_2196.jpg)

Hint: the example input is corresponding to this graph. And from the graph, you can see that the computer 4 is farthest one from 1, so S1 = 3. Computer 4 and 5 are the farthest ones from 2, so S2 = 2. Computer 5 is the farthest one from 3, so S3 = 3. we also get S4 = 4, S5 = 4. 

### Input

Input file contains multiple test cases.In each case there is natural number N (N<=10000) in the first line, followed by (N-1) lines with descriptions of computers. i-th line contains two natural numbers - number of computer, to which i-th computer is connected and length of cable used for connection. Total length of cable does not exceed 10^9. Numbers in lines of input are separated by a space.

### Output

For each case output N lines. i-th line must contain number Si for i-th computer (1<=i<=N).

### Sample Input

5
1 1
2 1
3 1
1 1

### Sample Output

3
2
3
4
4

### 题解：

此题的意思为：树中一共n个节点，输出每个节点到其中任意一个节点的最远花费的值。

有这么一个定理：

> 首先假设树的最长路的两个叶子节点为v1,v2，那么现有结论，从任意一点u出发走到的最远的点一定是（v1,v2）中的一点，然后再从v1或者v2出发走到的最远点一定是v2或者v1。

因此，经过三次搜索就能找到任意一点最长路径或者说是话费。

### 代码：

```c++
include <iostream>
include <cstdio>
include <algorithm>
include <vector>
include <queue>
include <cmath>
include <cstring>
using namespace std;

const int MAX = 10010;

struct Tree{
    int v;
    int dis;
};
vector <struct Tree > tree[MAX];
int ans[MAX];
int max_len,root;

init(){
    memset(ans,0,sizeof(ans));
    memset(tree,0,sizeof(tree));
}

void trees(int u,int v,int len){ // v为当前节点，u为父节点
    if(max_len < len ){
        max_len = len;
        root = v;
    }

    for(int i = 0;i < tree[v].size();i++){
        struct Tree new_v = tree[v][i];
        if(new_v.v == u)
            continue;
        trees(v,new_v.v,len + new_v.dis);
        ans[new_v.v] = max (ans[new_v.v],new_v.dis + len);
    }
}

int main()
{
    int n;
    while(scanf("%d",&n) != EOF){
        init();
        int max_w = 0, w = 1;
        for(int i = 2;i <= n; i++){
            int tmp,tmp_d; cin >> tmp >> tmp_d;
            tree[tmp].push_back(Tree{i,tmp_d});
            tree[i].push_back(Tree{tmp,tmp_d});
        }

        max_len = 0;
        trees(-1,1,0);
        trees(-1,root,0);
        trees(-1,root,0);

        for(int i = 1;i <= n; i++){
            cout << ans[i] <<endl;
        }
    }
    return 0;
}
```

