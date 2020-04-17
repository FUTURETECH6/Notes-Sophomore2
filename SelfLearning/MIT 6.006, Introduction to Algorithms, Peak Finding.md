#  DP

## Fibonacci Numbers

Naive recursive algorrithm: 

```pseudocode
f(n):
    if n <= 2: f = 1;
    else return f(n-1) + f(n-2)
```

每个会被重复调用了两次

是exponential time的：

>  用D&C的复杂度证明$\rm T(N) = T(N-1) + T(N-2) + \Theta(1)$
>
> T(N) >= 2T(N-2) $\Longrightarrow$ T(N) >= 2^N/2^ $\Longrightarrow$ T(N) = Ω(2^N/2^)

### Memorized DP(Top down)

<u>DP的技巧：Memorize：put one in **dict**, and check before compute</u>

```pseudocode
fib(n):
    if n in memo:
        return memo[n]
    if n <= 2:
        f = 1
    else:
        f = fib(n-1) + fib(n-2)
    memo[n] = f
    return f
```

```cpp
// CXY
Type Fib(int n) {
    if (n < list.size())
        return list[n];
    Type f = Fib(n - 1) + Fib(n - 2);
    list.push_back(f); // 递归调用确保了这个必是从小到大push的
    return f;
}
```

复杂度是linear：

> memorized calls θ(1)
>
> nomemorized calls is n个
>
> 非递归工作花费时间是$\Theta(1)$
>
> <u>ignore 递归调用花的时间</u>
>
> 所以总共是 nx1+1 = n

所以DP的本质就是recurrsion + memoration

time = subproblemNum * timePerSubpromble(recursion excludes)

### Bottom-up DP

```python
def fib(n):
    for k in range(1, n+1):
        if k <= 2:
            f = 1;
        else:
            f = fib[k-1] + fib[k-2] # 因为是bottomUp，所以确保了这两个一定存在
        fib[k] = f
    return fib[n]
```

本质上和Memorized是一样的？就是没有调用了，所以更save space

## Shortest Path

naive：

DP = recurrsion + memoration + guess

don't knwo how? guess and find the best one

用纯递归做最短路的时间花费。。。

### Memorized

用DFS， $S_k(s, v) = \min_{(u,v)\in E}(S_{k-1}(s,u) + w(u,v))$

如果遇到有绕回原点的路，则因为原点此时还不在memo里，因此会继续递归，造成无限循环内存爆炸

但是避 开这种情况的话， T = O(E+V)

**<u>Subpromble dependencies should be acyclic</u>**

> time for subgraph δ(s,v) = indegree(v) + 1 (==1是哪里来的？为什么是indeg不是outdeg==)
>
> $\Longrightarrow$ total = ∑~V~ (indegree + 1)= E + V



subprobl

