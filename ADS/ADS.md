# 易混淆

**NPL**：当前节点到没有两个子节点(**至少一个为NULL**)的节点的最短路径长，<u>自平衡的依据</u> (左式堆)

**bh(x)**：从x节点开始往下的**黑**节点个数(所有同路一样的) (红黑树)



# 诡异题目

To implement a binomial queue, <u>the subtrees of a binomial tree</u> are linked in <u>increasing</u> sizes. **False**

<u>For potential method, a good potential function should always assume its **minimum** at the start of the sequence</u> **True**

In an AVL tree, it is impossible to have this situation that the balance factors of **a** node and both of its children are all +1. **False**

The difference between aggregate analysis and accounting method is that the later one assumes that the amortized costs of the operations may differ from each other. **True**

问B+数度为n的节点最多最少有？个的基本上都是完满n叉🌲



# 复杂度整理

树

|       | 访问                                                         | 插入(amo) | M次插入                     | 删除(amo) |
| ----- | ------------------------------------------------------------ | --------- | --------------------------- | --------- |
| AVL   | O(logN)                                                      |           |                             |           |
| Splay | O(logN)                                                      |           |                             |           |
| RB    | O(logN)                                                      | O(1)      | O(M+N)                      | O(1)      |
| B+    | O(logN)<br />(深度为$O(\lceil \log_{\lceil M/2\rceil} N\rceil)$) |           | $O(\frac{M}{\log M}\log N)$ |           |

堆

|               | 访问最小        | 插入(amo) | M次插入  | 删除最小(amo) | Merge   |
| ------------- | --------------- | --------- | -------- | ------------- | ------- |
| Leftiest      | O(1)            |           |          | O(logN)       |         |
| Skew          | O(1)            |           | O(MlogN) | O(logN)       |         |
| BinomialQueue | O(logN) or O(1) | O(1)      |          | O(logN)       | O(logN) |

DP: 课件样例都是O(N^3^)，产品组装为O(N)

