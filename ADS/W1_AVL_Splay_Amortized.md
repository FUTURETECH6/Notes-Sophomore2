# AVL, Splay

## Feb24

# AVL

树的高度用边/节点个数来衡量(考试会用单节点时的高度来标明)

BF(Balance Factor)：左子树高减右

在保持平衡的同时要注意二叉树条件`L<M<R`的保持

<u>多个冲突调离trouble maker(只有一个，即新插入的)最近的那个trouble finder(被破坏节点，可能有多个)</u>；通过递归的方式从下往上找trouble finder即可



# Splay

为什么全部用单旋转(Zig)不行：不会显著改变path上其他节点的深度甚至有可能会往下推

Zig：仅在parent为根节点时进行

ZigZag/ZagZig：同LR/RL

ZigZig/ZagZag：reverse

# Amortized(均摊) Analysis

均摊时间：log

例子：12 345，连续3次操作均值的上界为amortized bound，worst5，amortzied4，average3

## Aggregate analysis聚合分析

n个操作序列worst是T(n)，则摊还代价为T(n)/n

pop()和multiPop()都需要push()来支持，三种栈操作的代价都是O(n)/n=O(1)

## Accounting method核算法

Ex. 20个人的生产队，初始估计的为amortized cost，实际需要食品量为actual cost，差值就是credit

$\rm \Large T_{amortized}= \frac{\sum \hat c_i(\geqslant \sum c_i)}{n}{}$

只要有办法保证估计的肯定不小于实际的就可以了，例如push=2

## Potential method势能法

D表示状态

credit = Φ(D_i)-Φ(D_{i-1})

其中Φ()为势能函数

$\large\displaystyle \sum \hat c_i = \sum c_i+\Phi(D_n)-\Phi(D_0)$

