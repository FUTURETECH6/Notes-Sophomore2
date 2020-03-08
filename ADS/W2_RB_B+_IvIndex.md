# RB

**性质**

* 根是黑的
* 叶是黑的
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

**最大树高**：2ln(N+1)：
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

* 父亲与叔叔均为红：父辈与爷爷调颜色，但需递归向上
* 父辈异色：先改颜色后改结构，不需再向上调整

**Delete**(删黑需调)

* 兄弟红：把兄弟变爷爷，换了个黑兄弟再继续



# B+

(B-Tree：非叶节点不为索引，也有Unique Key；B+Tree：非叶节点都是索引，Key可以和叶一样)

要求：每个有$\lceil$m/2$\rceil$ ~ m个儿子，根节点可以为leaf或有2-m个儿子

注意：m是分支个数不是一个节点中数据个数(最多m-1，=deg-1)

用途：建立数据库：叶节点存数据，非叶节点存索引

Depth(M,N)：O($\lceil \log_{\lceil M/2\rceil} N\rceil$)

T_{Find}(M,N) = M*O(logN)

**Insert**

超过m就分裂，向上插入新的索引，递归，若到根还超了则向上长一层

**Delete**

删掉，如果少于m/2，那么与兄弟合并，

* 如果合并后少于m，则没问题，删除上一层的一个索引，
* 如果超过m，则就去拿一个来放过来

递归



# Inverted File Index

Dict: Term + Times + Documents + Position

Token Analyzer + Vocabulary Scanner + Vocabulary Insertor

