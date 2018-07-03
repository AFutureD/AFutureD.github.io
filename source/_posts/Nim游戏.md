---
title: 51nod Nim游戏
date: 2016-12-28 16:31:57
tags: 
    - 博弈
    - Nim游戏
categories: Algorithm
---

*类型：Nim游戏*

### 描述：

有N堆石子。A B两个人轮流拿，A先拿。每次只能从一堆中取若干个，可将一堆全取走，但不可不取，拿到最后1颗石子的人获胜。假设A B都非常聪明，拿石子的过程中不会出现失误。给出N及每堆石子的数量，问最后谁能赢得比赛。

#### 例如：

3堆石子，每堆1颗。A拿1颗，B拿1颗，此时还剩1堆，所以A可以拿到最后1颗石子。

### 题解：

定义P-position和N-position，其中P代表Previous，N代表Next。直观的说，上一次move的人有必胜策略的局面是P-position，也就是“后手可保证必胜”或者“先手必败”，现在轮到move的人有必胜策略的局面是N-position，也就是“先手可保证必胜”。

**更严谨的定义**是:

> 1.无法进行任何移动的局面（也就是terminal position）是P-position；
>
> 2.可以移动到P-position的局面是N-position；
>
> 3.所有移动都导致N-position的局面是P-position。

按照这个定义，如果局面不可能重现，或者说positions的集合可以进行拓扑排序，那么每个position或者是P-position或者是N-position，而且可以通过定义计算出来。

**结论**：对于一个Nim游戏的局面(a1,a2,...,an)，它是P-position当且仅当a1 ^ a2 ^ ... ^ an=0，其中^表示异或([xor](http://baike.sogou.com/lemma/ShowInnerLink.htm?lemmaId=64448407))运算。

**简单证明**

根据定义，证明一种判断position的性质的方法的正确性，只需证明三个命题：

1. 这个判断将所有terminal position判为P-position；
2. 根据这个判断被判为N-position的局面一定可以移动到某个P-position；
3. 根据这个判断被判为P-position的局面无法移动到某个P-position。

第一个命题，显然terminal position只有一个，就是全0，异或仍然是0。

第二个命题，对于某个局面(a1,a2,...,an)，若a1 ^ a2 ^ ... ^ an != 0，一定存在某个合法的移动，将ai改变成ai'后满足a1 ^ a2 ^ ... ^ ai' ^ ... ^ an = 0。不妨设a1 ^ a2 ^ ... ^ an = k，则一定存在某个ai，它的二进制表示在k的最高位上是1（否则k的最高位那个1是怎么得到的）。这时ai ^ k < ai一定成立。则我们可以将ai改变成ai' = ai ^ k，此时a1 ^ a2 ^ ... ^ ai' ^ ... ^ an = a1 ^ a2 ^ ... ^ an ^ k=0。

第三个命题，对于某个局面(a1,a2,...,an)，若a1 ^ a2 ^ ... ^ an = 0，一定不存在某个合法的移动，将ai改变成ai'后满足a1 ^ a2 ^ ... ^ ai' ^ ... ^ an = 0。因为异或运算满足消去率，由a1 ^ a2 ^ ... ^ an = a1 ^ a2 ^ ... ^ ai' ^ ... ^ an可以得到ai = ai'。所以将ai改变成ai'不是一个合法的移动。证毕。

根据这个定理，我们可以在O(n)的时间内判断一个Nim的局面的性质，且如果它是N-position，也可以在O(n)的时间内找到所有的必胜策略。Nim问题就这样基本上完美的解决了。

#### 附加：

当题目条件增加‘最多取k个’时，对每一堆石子mod(k+1)。

### 代码：

``` c++
include <stdio.h>
int main() {
  int n, x, r = 0;
  scanf( "%d", &n );
  while( n-- ) {
    scanf( "%d", &x );
    r ^= x;
  }
  printf( "%c\n", r == 0 ? 'B' : 'A' );
  return 0;
}
```

