---
title: HDU-2444 The Accomodation of Students
date: 2016-12-28 16:31:57
mathjax: true
tags: 
    - 图论
    - 二分图
    - 最大匹配
categories: Algorithm
---

*类型： 二分图的判断和最大匹配*

### Problem Description：

There are a group of students. Some of them may know each other, while others don't. For example, A and B know each other, B and C know each other. But this may not imply that A and C know each other.

Now you are given all pairs of students who know each other. Your task is to divide the students into two groups so that any two students in the same group don't know each other.If this goal can be achieved, then arrange them into double rooms. Remember, only paris appearing in the previous given set can live in the same room, which means only known students can live in the same room.

Calculate the maximum number of pairs that can be arranged into these double rooms.

### Input：

For each data set:
The first line gives two integers, n and m(1<n<=200), indicating there are n students and m pairs of students who know each other. The next m lines give such pairs.

Proceed to the end of file.

### Output：

If these students cannot be divided into two groups, print "No". Otherwise, print the maximum number of pairs that can be arranged in those rooms.

### Sample Input：

4 4
1 2
1 3
1 4
2 3
6 5
1 2
1 3
1 4
2 5
3 6

### Sample Output：

No
3

### 题解：

这道题要先判断图是不是二分图，如果不是的话，就直接输出No,是的话就求最大匹配，
建边是双向的所以要除以2。
先判断能否构成二分图，判断二分图用交叉染色法从某个未染色的点出发把此点染成白
色，该点周围的点染成黑色黑色周围的又染成白色，若走到某个点已经染色并且它相邻
点的颜色与它一样则不是二分图，而是有奇数圈的图可以这样理解，染白色既加入X集
合，黑色既加入Y集合若某个点即是X集合又是Y集合，那说明不是二分图。

其次用匈牙利算法计算最大匹配，是通过DFS搜增广路来计算最大匹配。通过搜增广路扩展匹配路径。具体为：

1. 定义一个空集U，放入起始点。
2. 搜索相邻的点，判断该点是否是一条匹配线的一点，不是则把那一邻点加入集合U；是，则对此点DFS搜索一个相邻的点，直到该点不在一条匹配线上。由此得到一条线段，匹配线与非匹配相交替，非匹配比匹配线多1，反过来看（把匹配看成非匹配的、非匹配看成匹配的），则是匹配比非匹配多一条。
3. 循环步骤2，如果不能继续则结束。

### Code：

```c++
include"stdio.h"
include"string.h"
include"iostream"
include"queue"
using namespace std;
define N 205
int mark[N],link[N],map[N][N],color[N],n;
int find(int a)       //匈牙利算法
{
	int i;
	for(i=1;i<=n;i++){
		if(!mark[i]&&map[a][i]){
			mark[i]=1;
			if(!link[i]||find(link[i])){ //若i已经配对，则查找和i配对的那个元素是否还能和其他元素配对
				link[i]=a; //若可以则把i配给a
				return 1;
			}
		}
	}
	return 0;
}
int judge()        //判断是否是二分图
{
	int i,cur;
	queueq;       //队列声明
	q.push(1);       //把1加入队列
	while(!q.empty()){
		cur=q.front();      //取队首元素
		q.pop();             //删除队首元素
		for(i=1;i<=n;i++){
			if(map[cur][i]){      //1和2、3认识则把2、3均标记为2；  
				if(color[i]==-1){
					color[i]=1-color[cur];
					q.push(i);
				}
				else if(color[i]==color[cur])      //接下来到2出对时，color[3]=color
					return 0;
			}
		}
	}
	return 1;
}
int main()
{
	int i,j,m,ans;
	while(scanf("%d%d",&n,&m)!=-1){
		memset(link,0,sizeof(link));
		memset(map,0,sizeof(map));
		memset(color,-1,sizeof(color));
		while(m--){
			scanf("%d%d",&i,&j);
			map[i][j]=map[j][i]=1;
		}
		if(judge()==0){
			printf("No\n");
			continue;
		}
		ans=0;
		for(i=1;i<=n;i++){
			memset(mark,0,sizeof(mark));
			ans+=find(i);
		}
		printf("%d\n",ans/2);
	}
	return 0;
}
```

