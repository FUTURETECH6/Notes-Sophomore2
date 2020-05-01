# RB

**性质**

* 根是黑的
* (叶是黑的，NIL、不存数据
* 每个通路上黑色数量一样
* <font color = "#FF0000">红节点</font>的两个子都是黑
    * 不能两个连续红
    * 最长路不会超过最短路两倍

```cpp
class T{
  public:
    struct Node{color, key, left, right, p/* p for what? */};
    
  private:
    struct Node *nil;
}
T::T(){
    nil.color = BLACK;
    // 其他属性无所谓
}
```



**bh(x)**：从x节点开始往下的**黑**节点个数(所有同路一样的)

<u>bh(t) >= h(tree)/2</u>

<u>**最大树高**：2ln(N+1)</u>：
$$
Proof:
\\
1
\\
sizeof(x) >= 2^{bh(x)}-1
\\
hx=0, sizeof(x) = 0
\\
h(x) = k+1, bh(child) = bh(x) || bh(x)-1
\\
h(child) <= k, sizeof(child) >= 2^{bh(child)}-1>= 2^{bh(x)-1}-1
\\
1+2sizeof(child)>=2^{bh(x)}-1
\\
2
\\
bh(Tree)>=h(Tree)/2
\\
sizeof(root) = N >= s^{bh(Tree)}-1 >=
$$

**Insert**(加红再调)

* T_amo = O(1)
* 父亲与叔叔均为红：父辈与爷爷调颜色，但需递归向上
* 父辈异色：先改颜色后改结构，不需再向上调整

**Delete**(删黑需调)

* T_amo：𝑚 consecutive insertions in a tree of 𝑛 nodes is 𝑂(𝑛+𝑚) (Tarjan 1985)."
    * 所以均摊时间也是O(1)
* 兄弟红：把兄弟变爷爷，换了个黑兄弟再继续

http://web.stanford.edu/class/archive/cs/cs166/cs166.1146/lectures/05/Small05.pdf 93页

**摊还分析**

n个节点的树连续m次插入的balance需要O(m+n)：记账法：插入一个节点的时候，给红色的节点额外的1个credit，

# B+

(B-Tree：非叶节点不为索引，也有Unique Key；B+Tree：非叶节点都是索引，Key可以和叶一样)

要求：每个有$\lceil$m/2$\rceil$ ~ m个儿子，根节点可以为leaf或有2-m个儿子

注意：m是分支个数不是一个节点中数据个数(最多m-1，=deg-1)

用途：建立数据库：叶节点存数据，非叶节点存索引

**Insert**

超过m就分裂，向上插入新的索引，递归，若到根还超了则向上长一层

**Delete**

删掉，如果少于m/2，那么与兄弟合并，

* 如果合并后少于m，则没问题，删除上一层的一个索引，
* 如果超过m，则就去拿一个来放过来

递归

**效果分析**

$Depth(M,N) = O(\lceil \log_{\lceil M/2\rceil} N\rceil)$

$T_{Insert}(M,N) = O(M\log_MN) = O(\frac{M}{\log M}\log N)$

$T_{Find}(M,N) = O(\log M \log_MN) = O(\log N)$



# Inverted File Index

为什么叫Inverted？<u>由Inverted File可以倒推出原文</u>

https://blog.csdn.net/Woolseyyy/article/details/51559937

Dict: Term + Times + Documents + Position

Token Analyzer + Vocabulary Scanner + Vocabulary Insertor



Term Frequent

TF-IDF(TF-Inverse Document Frequency)



Distributed Indexing

* **Term-partitioned index**(按term分类)
    A~C D~F ............ X~Z
* **Document-partitioned index**(按文件号)
    1~ 10001~ 90001~ 10000 20000 100000



**Precision 查准率**

**Recall 查全率**



**User happiness**

- Data Retrieval **Performance** Evaluation (after establishing correctness)
    - Response <u>time</u>
    - Index <u>space</u>
- Information Retrieval Performance Evaluation
    - How relevant is the answer set?