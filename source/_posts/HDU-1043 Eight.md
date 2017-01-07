---
title: HDU-1043 Eight
date: 2016-12-28 16:31:57
tags: 
    - 搜索
    - A*算法
categories: Algorithm
---

*类型： A\*算法*

## EXAMPLE

### description

The 15-puzzle has been around for over 100 years; even if you don't know it by that name, you've seen it. It is constructed with 15 sliding tiles, each with a number from 1 to 15 on it, and all packed into a 4 by 4 frame with one tile missing. Let's call the missing tile 'x'; the object of the puzzle is to arrange the tiles so that they are ordered as: 

1   2   3   4
5   6   7   8
9   10 11 12
13 14 15  x

where the only legal operation is to exchange 'x' with one of the tiles with which it shares an edge. As an example, the following sequence of moves solves a slightly scrambled puzzle: 

 1   2   3   4       1   2   3   4      1   2   3   4       1   2   3   4
 5   6   7   8       5   6   7   8      5   6   7   8       5   6   7   8
 9   x  10 12      9  10  x  12     9  10 11 12     9  10 11 12
13 14 11 15    13 14 11 15    13 14  x  15    13 14 15  x

The letters in the previous row indicate which neighbor of the 'x' tile is swapped with the 'x' tile at each step; legal values are 'r','l','u' and 'd', for right, left, up, and down, respectively. 

Not all puzzles can be solved; in 1870, a man named Sam Loyd was famous for distributing an unsolvable version of the puzzle, and 
frustrating many people. In fact, all you have to do to make a regular puzzle into an unsolvable one is to swap two tiles (not counting the missing 'x' tile, of course). 

In this problem, you will write a program for solving the less well-known 8-puzzle, composed of tiles on a three by three 
arrangement. 

### Input

You will receive, several descriptions of configuration of the 8 puzzle. One description is just a list of the tiles in their initial positions, with the rows listed from top to bottom, and the tiles listed from left to right within a row, where the tiles are represented by numbers 1 to 8, plus 'x'. For example, this puzzle 

1 2 3 
x 4 6 
7 5 8 

is described by this list: 

1 2 3 x 4 6 7 5 8 

### Output

You will print to standard output either the word ``unsolvable'', if the puzzle has no solution, or a string consisting entirely of the letters 'r', 'l', 'u' and 'd' that describes a series of moves that produce a solution. The string should include no spaces and start at the beginning of the line. Do not print a blank line between cases. 

### Sample Input

2  3  4  1  5  x  7  6  8

### Sample Output

ullddrurdllurdruldr

## 题解

这题核心思想是使用A*算法，并且需要通过hash确定每种状态的序号，以用来访问数组来确定时候被访问过和父节点的存储。

首先hash的核心算法是康托展开。其次A\*算法使用估值函数确定最优节点，以放入队列中进行搜索。A\*算法的参数有三个f、h、g，f = h + g，g为已花费的代价（题中指已经走过的步数），h为估算的代价（题中指在无障碍的情况下最少还需要多少步到达终点目标），正式地，这个距离叫做曼哈顿距离。

最后一点，每次数字的移动会使状态的逆序数改变，但是不改变奇偶性。

## 代码

```c++
include<iostream>
include<cstdio>
include<cstring>
include<queue>
include<cmath>
using namespace std;

struct node     //状态
{
    int a[10];
    int f, h, g;
    int x;      //x在的位置

    friend bool operator < (node a, node b)
    {
        return a.f > b.f;
    }
};

priority_queue<node>que;
int fac[10];
//46233
struct
{
    int father;
    char dir;
}vis[362881];

int get_h(int a[])
{
    int h = 0;
    for(int i = 0; i < 8; i++)
    {
        if(a[i])
            h += fabs((a[i]-1)/3 - i/3) + fabs((a[i]-1)%3 - i%3);
    }
    return h;
}

int Hash(int a[])
{
    int ans = 0;
    for(int i = 0; i < 9; i++)
    {
        int tmp = 0;
        for(int j = i+1; j < 9; j++)
        {
            if(a[i] > a[j]) tmp++;
        }
        ans += tmp*fac[8-i];
    }
    return ans+1;
}

void prin(int n)
{
//    printf("n=%d\n", n);
    if(vis[n].father!=-1)
    {
        prin(vis[n].father);
        printf("%c", vis[n].dir);
    }
}

void SWAP(int &x, int &y)
{
    int t = x;
    x = y;
    y = t;
}

int dir[4][2] = { {1, 0}, {-1, 0}, {0, -1}, {0, 1} };
char dd[] = "dulr";

bool is(int a[])
{
    int ans = 0;
    for(int i = 0; i < 9; i++)
    {
        if(a[i])
        for(int j = i+1; j < 9; j++)
        {
            if(a[i] > a[j] && a[j])
                ans++;
        }
    }
    return !(ans & 1);
}

void debug(int a[])
{
    for(int i = 0; i < 3; i++)
    {
        for(int j = 0; j < 3; j++)
        {
            printf("%d ", a[i*3+j]);
        }
        printf("\n");
    }
    printf("\n");
}

int bfs(node star)
{
    while(!que.empty()) que.pop();
    que.push( star );
    star.h = get_h( star.a );    star.g = 0;
    star.f = star.g + star.h;
    vis[ Hash( star.a ) ].father = -1;
    while(!que.empty()){
        node tmp = que.top(); que.pop();
        int father = Hash(tmp.a);
        for(int i = 0; i < 4; i++){
            int x = dir[i][0] + tmp.x/3;
            int y = dir[i][1] + tmp.x%3;
            if(0 <= x && x < 3 && 0 <= y && y < 3){
                node s = tmp; s.x = x*3+y;
                SWAP( s.a[ tmp.x ], s.a[ s.x ] );
                s.g++;
                s.h = get_h( s.a ); s.f = s.h + s.g;
                int son = Hash(s.a);

                if(son == 46234){
                    vis[ son ].father = father;
                    vis[ son ].dir = dd[i];
                    prin(46234);printf("\n");
                    return 0;
                }
                if(!vis[ son ].father && is(s.a)){
                    vis[ son ].father = father;
                    vis[ son ].dir = dd[i];
                    que.push( s );
                }
            }
        }
    }
    return 1;
}


int main(void)
{
    int i;
    fac[1] = 1;
    for(i = 2; i < 10; i++) fac[i] = fac[i-1]*i;
    node star;
    char in[2];
//    freopen("ou.txt", "w", stdout);
    while(~scanf("%s", in))
    {
        memset(vis, 0, sizeof(vis));
        if(in[0] == 'x')
        {
            star.a[0] = 0;
            star.x = 0;
        }
        else star.a[0] = in[0] - '0';
        for(i = 1; i < 9; i++)
        {
            scanf("%s", in);
            if(in[0] == 'x')
            {
                star.a[i] = 0;
                star.x = i;
            }
            else star.a[i] = in[0] - '0';
        }
        if(!is(star.a))
        {
            printf("unsolvable\n");continue;
        }
        if(Hash(star.a) == 46234) {printf("\n"); continue;}
        if(bfs(star))
        {
            printf("unsolvable\n");
        }
    }
    return 0;
}

```

