---
title: 伸展树——Splay Tree
date: 2017-08-18 19:43:57
mathjax: true
tags: 
    - Splay
categories: Algorithm
---

# 伸展树——Splay Tree 
## 简单介绍
**伸展树**（英语：Splay Tree）是一种二叉查找树，它能在$O(log(n))$内完成插入、查找和删除操作。它是由丹尼尔·斯立特（Daniel Sleator）和羅伯特·塔揚在1985年发明的。
在伸展树上的一般操作都基于伸展操作：假设想要对一个二叉查找树执行一系列的查找操作，为了使整个查找时间更小，被查频率高的那些条目就应当经常处于靠近树根的位置。于是想到设计一个简单方法， 在每次查找之后对树进行調整，把被查找的条目搬移到离树根近一些的地方。伸展树是一种自调整形式的二叉查找树，它会沿着从某个节点到树根之间的路径，通过一系列的旋转把这个节点搬移到树根去。
它的优势在于不需要记录用于平衡树的冗余信息。

## 目的

把重要的树节点转移树根。使在最坏的情况下,查询时间复杂度仍然为$O(log(n))$。

## 时间复杂度证明

具体证明请看维基百科的。 [Splay tree - Wikipedia](https://en.wikipedia.org/wiki/Splay_tree)

## 操作

### 基本操作：旋转

![](http://img.my.csdn.net/uploads/201210/10/1349877565_2986.png)

首先讲基本旋转之一右旋（zig）：

解释下上面这个图，对于右旋，P为目标节点，Q为P的父节点，A、B、C则是相关子节点。右旋操作可以将P节点变为Q的父节点，且保持原有的二叉查询树的特性，即对于某一节点，其左子树的所有节点的值都小于该节点而右子树的所有节点的值都大于该节点。

整个过程分为三步：

1. 将P的右子树B(如果有的话)作Q的左子树，即让B认Q作爹
2. 设Z为原本Q结点的父亲，让P认Z做爹(如果Z存在的话)，即将P作为Z的儿子(是左是右得由Q是Z的左儿子还是右儿子决定，要左右一致)
3. 将Q作为P的右子树，即让Q认P作爹。

####右旋代码：

```c++
void right_rotate(long P)
{
	long Q = father[P];
	long Z = father[Q];//这里的z与上文的Q相关，表示y的父节点。
    leftson[Q] = rightson[P];//第一步，将P的右子树B(如果有的话)作Q的左子树。
    if(rightson[P] != 0)
    	father[rightson[P]] = Q;//第一步，B认Q为父。
    father[P] = Z;//第二步，P认Z做爹。
    if(z != 0){
		if(leftson[Z] == y)//判断原来的Q是Z的左节点还是有节点。
        	leftson[Z] = P;//第二步,P做Z儿子。
     	else 
          	rightson[Z] = P;
    }
    rightson[P] = y;//第三步，Q作为P的右子树
  	father[Q] = P;//第三步，Q认P作爹
}
```



接着另一个旋转——左旋（zag）：

有了上面右旋的基础，就直接说明步骤了：

1. 将P的左子树B(如果有的话)作Q的右子树，即让B认Q作爹
2. 设Z为原本Q结点的父亲，让P认Z做爹(如果Z存在的话)，即将P作为Z的儿子(是左是右得由Y是Z的左儿子还是右儿子决定，要左右一致)
3. 将Q作为P的左子树，即让P认P作爹。

#### 左旋代码：

```c++
void left_rotate(long P)
{
	long Q = father[P];
	long Z = father[Q];
	rightson[Q] = leftson[P];
	if(leftson[P] != 0)
      	father[leftson[P]] = Q;
	father[P]=Z;
	if(Z != 0){
    	if(leftson[Z] == Q)
          	leftson[Z] = P;
      	else 
          	rightson[Z] = P;
	}
  	leftson[P] = Q;
  	father[Q] = P;
}
```



### 核心操作：spaly

把重要节点通过旋转操作转移为目标节点的父节点。一般为转移到根节点。

####Splay代码

```c++
void splaQ(long P,long Ancestry){
    while(father[P] != Ancestry){
        long Q = father[P];
        long Z = father[Q];
        if(Z == Ancestry){ //父节点为目标节点
            if(rightson[Q] == P)
                left_rotate(P);
            else
                right_rotate(P);
        }
        else{//由左旋和右旋组合产生四种情况
            if(rightson[Z] == Q && rightson[Q] == P){
                left_rotate(Q);
                left_rotate(P);
            }
            else if(rightson[Z] == Q && leftson[Q] == P){
                right_rotate(P);
                left_rotate(P);
            }
            else if(leftson[Z] == Q && leftson[Q] == P){
                right_rotate(Q);
                right_rotate(P);
            }
            else{
                left_rotate(P);
                right_rotate(P);
            }
        }
    }
    if(Ancestry == 0)
        root = P;
} 
```



### 插入操作 insert

1. 首先判断是否为空树。如果为空树，则初始化树。
2. 判断节点值与插入值是否相等，相等则对此节点做相关操作，如数量加一；不相等，执行第3步。
3. 比较节点值与插入值的大小，找的子节点。如果当前点节点不为空，执行第2步；为空则执行第4步。
4. 新建节点，树总节点数量（全局变量）加一，初始化父亲节点和子节点，进行splay操作。

#### insert代码

```c++
void insert(int x)
{  
    if (root==0){ //若当前为空树则直接加
        tree[++sz].num=x;  
        childsz[sz]=cnt[sz]=1; //size为该子树的节点数目，cnt为同一数值数字数目  
        root = sz; //sz为整棵树的节点数目
        return;    
    }   
    int now = root,fa = 0; //now,root等都为节点编号。
    while (1){  
        if (val[now] == x){ //相等，找到位置
            tree[now].cnt++;  
            splay(now);
            return;  
        }  
        fa = now;  
        now = (x>val[now]?rightson[now]:leftson[now]); //按大小找子节点
        if (now == 0){ //该点未被添加过，新建节点
            sz++; //节点新建了一个，作为节点编号
            val[sz] = x;
            cnt[sz] = childsize[sz] = 1;  
            father[sz] = fa;  
            (x > val[fa] ? rightson[fa] : leftson[fa]) = sz; //设置父亲                    
            splay(sz);
            return;
        }  
    }  
}
```



###查找操作

#### 查询某一值的排名

和其它二叉搜索树的操作基本一样。但是区别是：

1. 如果x比当前结点小，即应该向左子树寻找，ans不用改变（设想一下，走到整棵树的最左端最底端排名不就是1吗）。
2. 如果x比当前结点大，即应该向右子树寻找，ans需要加上左子树的大小以及根的大小（这里的大小指的是权值）。

不要忘记了再splay一下

##### find 代码

```c++
int find(int v){
    int ans = 0,now = root;
    while (1){
        if (v < val[now])
            now = leftch[now];
        else{
            ans += (leftch[now] ? size[leftch[now]] : 0); 
            //左子树存在的话就为左子树大小，否则为0。
            if (v == val[now]) {
                splay(now);
                return ans+1;
            }
            ans += cnt[now];//根的大小
            now = rightch[now];
        }
    }
    return ans;
}
```



#### 查询排名为x的点

如果当前点有左子树，并且x比左子树的大小小的话，即向左子树寻找；

否则，向右子树寻找：先判断是否有右子树，然后记录右子树的大小以及当前点的大小（都为权值），用于判断是否需要继续向右子树寻找。

##### findx 代码

```c++
int findx(int x){  
    int now = root;  
    while (1){  
        if (leftch[now] && x <= size[leftch[now]])  
            now = ch[now][0];  
        else{  
            int temp = (leftchch[now] ? size[leftch[now]] : 0) + cnt[now];  
            if (x <= temp)  
                return key[now];  
            x -= temp;
            now = rightch[now];  
        }  
    }  
} 
```



##感谢

http://blog.csdn.net/clove_unique/article/details/50630280

http://blog.csdn.net/skydec/article/details/20151805