---
title: HDU-1851 A Simple Game
date: 2016-12-28 16:31:57
mathjax: true
tags: 
    - 博弈
    - SG函数
categories: Algorithm
---

*类型： 博弈，SG函数*

### 描述：

Agrael likes play a simple game with his friend Animal during the classes. In this Game there are n piles of stones numbered from 1 to n, the 1st pile has M1 stones, the 2nd pile has M2 stones, ... and the n-th pile contain Mn stones. Agrael and Animal take turns to move and in each move each of the players can take at most L1 stones from the 1st pile or take at most L2 stones from the 2nd pile or ... or take Ln stones from the n-th pile. The player who takes the last stone wins.

After Agrael and Animal have played the game for months, the teacher finally got angry and decided to punish them. But when he knows the rule of the game, he is so interested in this game that he asks Agrael to play the game with him and if Agrael wins, he won't be punished, can Agrael win the game if the teacher and Agrael both take the best move in their turn?

The teacher always moves first(-_-), and in each turn a player must takes at least 1 stones and they can't take stones from more than one piles.

### INPUT：

The first line contains the number of test cases. Each test cases begin with the number n (n ≤ 10), represent there are n piles. Then there are n lines follows, the i-th line contains two numbers Mi and Li (20 ≥ Mi > 0, 20 ≥ Li > 0). 

### OUTPUT：





Your program output one line per case, if Agrael can win the game print "Yes", else print "No". 

### Sample input：

2
1
5 4
2
1 1
2 2

### sample output：

Yes
No

### 题解：

此题有两种办法一种是nim+bash，还有一种是通过SG函数来解决