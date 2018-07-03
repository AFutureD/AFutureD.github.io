---
title: SYNU-2015-training-题解
date: 2017-02-12 13:23:16
mathjax: true
tags: 
    - 题解
categories: Algorithm
---

*题解*

## A - Til the Cows Come Home 

### 描述

Bessie is out in the field and wants to get back to the barn to get as much sleep as possible before Farmer John wakes her for the morning milking. Bessie needs her beauty sleep, so she wants to get back as quickly as possible. 

Farmer John's field has N (2 <= N <= 1000) landmarks in it, uniquely numbered 1..N. Landmark 1 is the barn; the apple tree grove in which Bessie stands all day is landmark N. Cows travel in the field using T (1 <= T <= 2000) bidirectional cow-trails of various lengths between the landmarks. Bessie is not confident of her navigation ability, so she always stays on a trail from its start to its end once she starts it.

Given the trails between the landmarks, determine the minimum distance Bessie must walk to get back to the barn. It is guaranteed that some such route exists.

### Input

* Line 1: Two integers: T and N 
* Lines 2..T+1: Each line describes a trail as three space-separated integers. The first two integers are the landmarks between which the trail travels. The third integer is the length of the trail, range 1..100.

### Output

* Line 1: A single integer, the minimum distance that Bessie must travel to get from landmark N to landmark 1.

### Sample Input

>5 5
1 2 20
2 3 30
3 4 20
4 5 20
1 5 100

### Sample Output

>90

### 简述题意与思路

这题是一个很裸的最短路源点为`n`,终点为 `1`。套一下dijkstra的模版就好了。

### 代码

```c++
include <iostream>
include <cstring>
include <vector>
include <queue>
include <stdio.h>
using namespace std;
const int INF = 0x3f3f3f3f;
struct Edge {
    int vertex, weight;
};
class Graph {
private:
    int n;
    vector<Edge> * edges;
    bool * visited;
public:
    int * dist;
    Graph (int input_n) {
        n = input_n;
        edges = new vector<Edge>[n];
        dist = new int[n];
        visited = new bool[n];
        memset(visited, 0, n * sizeof(bool));
        memset(dist, 0x3f, n * sizeof(int));
    }
    ~Graph() {
        delete[] dist;
        delete[] edges;
        delete[] visited;
    }
    void insert(int x, int y, int weight) {
        edges[x].push_back(Edge{y, weight});
        edges[y].push_back(Edge{x, weight});
    }
    void dijkstra(int v) {
        dist[v] = 0;
        for(int i = 0;i < n;i++){
            int min_dist = INF,min_vertex;
            for(int j = 0;j < n; j++){
                if(!visited[j] && dist[j] < min_dist){
                    min_dist = dist[j];
                    min_vertex = j;
                }
            }
            visited[min_vertex] = 1;

            for(int j = 0; j < edges[min_vertex].size() ;j++ ){
                Edge tmp = edges[min_vertex][j];
                if( min_dist + tmp.weight < dist[tmp.vertex]){
                    dist[tmp.vertex] = min_dist + tmp.weight;
                }
            }
        }
    }
};

int main() {
    int t, n;
    scanf("%d%d",&t,&n);
    Graph g(n);
    for (int i = 0; i < t; i++) {
        int a, b, c;
        scanf("%d%d%d",&a,&b,&c);
        g.insert(a - 1, b - 1, c);
    }
    g.dijkstra(n-1);
    cout << g.dist[0] << endl;
}
```

## B - Heavy Transportation

### 描述

Background 
Hugo Heavy is happy. After the breakdown of the Cargolifter project he can now expand business. But he needs a clever man who tells him whether there really is a way from the place his customer has build his giant steel crane to the place where it is needed on which all streets can carry the weight. 
Fortunately he already has a plan of the city with all streets and bridges and all the allowed weights.Unfortunately he has no idea how to find the the maximum weight capacity in order to tell his customer how heavy the crane may become. But you surely know. 

Problem 
You are given the plan of the city, described by the streets (with weight limits) between the crossings, which are numbered from 1 to n. Your task is to find the maximum weight that can be transported from crossing 1 (Hugo's place) to crossing n (the customer's place). You may assume that there is at least one path. All streets can be travelled in both directions.

### Input

The first line contains the number of scenarios (city plans). For each city the number n of street crossings (1 <= n <= 1000) and number m of streets are given on the first line. The following m lines contain triples of integers specifying start and end crossing of the street and the maximum allowed weight, which is positive and not larger than 1000000. There will be at most one street between each pair of crossings.

### Output

The output for every scenario begins with a line containing "Scenario #i:", where i is the number of the scenario starting at 1. Then print a single line containing the maximum allowed weight that Hugo can transport to the customer. Terminate the output for the scenario with a blank line.

### Sample Input

>1
3 3
1 2 3
1 3 4
2 3 5

### Sample Output

>Scenario #1:
4

### 简述题意与思路

**题意：** n个点m条边，边的权值为最大承载量。要求输出从1点到n点能运送货物的最大重量。
**思路：** 这题可以用prime和dijkstra两种方法，即最大生成树和最短路。prime很裸，,disjkstra需要稍加变形，由于在训练dijkstra，我就选择这个了。
对于dijkstra，其松弛操作由原来的修改为最短路径改为修改为较大权值。且选点操作，改为选取边权较大的点了。

### 代码

```c++
include <iostream>
include <cstring>
include <vector>
include <queue>
include <stdio.h>
using namespace std;
const int INF = -1;
struct Edge {
    long long vertex, weight;
};
class Graph {
private:
    int n;
    vector<Edge> * edges;
    bool * visited;
public:
    long long * dist;
    Graph (int input_n) {
        n = input_n;
        edges = new vector<Edge>[n];
        dist = new long long[n];
        visited = new bool[n];
        memset(visited, 0, n * sizeof(bool));
        memset(dist, 0, n * sizeof(long long));
    }
    ~Graph() {
        delete[] dist;
        delete[] edges;
        delete[] visited;
    }
    void insert(int x, int y, int weight) {
        edges[x].push_back(Edge{y, weight});
        edges[y].push_back(Edge{x, weight});
    }
    void dijkstra(int v) {
        dist[v] = 0;

        for(int i=0;i<edges[0].size(); i++){
            dist[edges[0][i].vertex] = edges[0][i].weight;
        }

        visited[0]=1;
        
        for(int i = 0;i < n;i++){
            long long min_dist = INF,min_vertex;
            for(int j = 0;j < n; j++){ 
                if(!visited[j] && dist[j] > min_dist){
                    min_dist = dist[j];
                    min_vertex = j;
                }
            }
            
            visited[min_vertex] = 1;
            
            for(int j = 0; j < edges[min_vertex].size() ;j++ ){
                Edge tmp = edges[min_vertex][j];
                if( min(min_dist , tmp.weight) > dist[tmp.vertex]){
                    dist[tmp.vertex] = min(min_dist , tmp.weight);
                }
            }
        }
    }
};

int main() {
    int T;
    scanf("%d",&T);
    for(int t = 1; t <= T; t++){
        int n, m;
        scanf("%d%d",&n, &m);
        Graph g(n);
        for (int i = 0; i < m; i++) {
            int a, b, c;
            scanf("%d%d%d",&a,&b,&c);
            g.insert(a - 1, b - 1, c);
        }
        g.dijkstra(0);
        printf("Scenario #%d:\n%lld\n\n",t,g.dist[n-1]);
    }
}
```

## C - Wireless Network 

### 描述
    
An earthquake takes place in Southeast Asia. The ACM (Asia Cooperated Medical team) have set up a wireless network with the lap computers, but an unexpected aftershock attacked, all computers in the network were all broken. The computers are repaired one by one, and the network gradually began to work again. Because of the hardware restricts, each computer can only directly communicate with the computers that are not farther than d meters from it. But every computer can be regarded as the intermediary of the communication between two other computers, that is to say computer A and computer B can communicate if computer A and computer B can communicate directly or there is a computer C that can communicate with both A and B. 

In the process of repairing the network, workers can take two kinds of operations at every moment, repairing a computer, or testing if two computers can communicate. Your job is to answer all the testing operations. 

### Input

The first line contains two integers N and d (1 <= N <= 1001, 0 <= d <= 20000). Here N is the number of computers, which are numbered from 1 to N, and D is the maximum distance two computers can communicate directly. In the next N lines, each contains two integers xi, yi (0 <= xi, yi <= 10000), which is the coordinate of N computers. From the (N+1)-th line to the end of input, there are operations, which are carried out one by one. Each line contains an operation in one of following two formats: 
1. "O p" (1 <= p <= N), which means repairing computer p. 
2. "S p q" (1 <= p, q <= N), which means testing whether computer p and q can communicate. 

The input will not exceed 300000 lines. 

### Output

For each Testing operation, print "SUCCESS" if the two computers can communicate, or "FAIL" if not.

### Sample Input

>4 1
0 1
0 2
0 3
0 4
O 1
O 2
O 4
S 1 4
O 3
S 1 4

### Sample Output

>FAIL
SUCCESS

### 简述题意与思路

**题意：** 给出电脑数量n和距离d，给出n个电脑的坐标。给出操作 O 和 P，O操作表示修复电脑，P操作表示测试两台电脑是否可以通讯，对于P操作输出结果FAIL或者SUCCESS。
**思路：** 简单的并查集题，对每个O进行Union操作；对于P进行查询集合操作，如果集合相同择SUCCESS，否则FAIL。其中Union进行的条件是，俩电脑距离是否小于d。

### 代码

由于为AC，稍后贴出。注意题意与思路可能会改变。


## D - The Suspects 

### 描述

Severe acute respiratory syndrome (SARS), an atypical pneumonia of unknown aetiology, was recognized as a global threat in mid-March 2003. To minimize transmission to others, the best strategy is to separate the suspects from others. 
In the Not-Spreading-Your-Sickness University (NSYSU), there are many student groups. Students in the same group intercommunicate with each other frequently, and a student may join several groups. To prevent the possible transmissions of SARS, the NSYSU collects the member lists of all student groups, and makes the following rule in their standard operation procedure (SOP). 
Once a member in a group is a suspect, all members in the group are suspects. 
However, they find that it is not easy to identify all the suspects when a student is recognized as a suspect. Your job is to write a program which finds all the suspects.

### Input

The input file contains several cases. Each test case begins with two integers n and m in a line, where n is the number of students, and m is the number of groups. You may assume that 0 < n <= 30000 and 0 <= m <= 500. Every student is numbered by a unique integer between 0 and n−1, and initially student 0 is recognized as a suspect in all the cases. This line is followed by m member lists of the groups, one line per group. Each line begins with an integer k by itself representing the number of members in the group. Following the number of members, there are k integers representing the students in this group. All the integers in a line are separated by at least one space. 
A case with n = 0 and m = 0 indicates the end of the input, and need not be processed.

### Output

For each case, output the number of suspects in one line.

### Sample Input

>100 4
2 1 2
5 10 13 11 12 14
2 0 1
2 99 2
200 2
1 5
5 1 2 3 4 5
1 0
0 0

### Sample Output

>4
1
1

### 简述题意与思路

**题意：** 很简单给出人数n和组数m。有m组人群，其中序号为0的那个人为疑似感染患者，输出疑似感染人群的人数。对于那m组人群需要进行合并。
**思路：** 首先当m为0时，直接输出1，应为只有0号一个人。其他情况，对于m组进行并查集的Union算法，把属于同一组的人标为同一个组。然后输出号0所在那个组的人数就好了。

### 代码

```c++
include <iostream>
include <cstring>
include <vector>
include <queue>
include <stdio.h>
using namespace std;
int const MAX = 30000 +10;
int mem[MAX];
int son[MAX],pre[MAX];

void init(){
    for (int i = 0; i < MAX; ++i)
    {
        pre[i] = i;
        son[i] = 1;
    }
}

int Find_Set(int x)
{
    return x == pre[x] ? x : Find_Set(pre[x]);;
}

void Union(int x, int y){
    int root1= Find_Set(x), root2= Find_Set(y);;
    if(root1 != root2){
        pre[root2] = root1;
        son[root1] += son[root2];
    }
}

int main(){

    int n,m;
    while(scanf("%d%d",&n,&m) && (n || m)){
        init();
        if(m == 0){
            cout << 1 <<endl;
            continue;
        }

        for(int i = 0; i < m;i++)
        {
            int  c,one;
            scanf("%d%d",&c,&one);
            for(int j= 1;j < c;j++){
                int two;
                scanf("%d",&two);
                Union(one,two);
            }
        }
        printf("%d\n",son[Find_Set(0)]);
    }
    return 0;
}
```


