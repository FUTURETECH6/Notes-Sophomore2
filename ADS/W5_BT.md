



https://blog.csdn.net/weixin_40170902/article/details/80856799



## MiniMax Strategy

例如井字棋，设**评估函数**W为当前状态下某方的可能赢的三条(Win Line)有W个，则f(P) = W_Computer - W_Me，我要使fP尽可能小

评估函数很重要

另外的评估函数：$\begin{cases}赢了：1\\平局：0\\输了：-1\end{cases}$



http://inst.eecs.berkeley.edu/~cs61b/fa14/ta-materials/apps/ab_tree_practice/

* α：max节点最大推算评估值
* β：min界定最小推算评估值



蒙特卡洛树搜索



Game Trees

