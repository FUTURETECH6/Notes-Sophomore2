# First Normal Form

多值属性的处理方式

1. Use multi fields, as person(pname, … , phon1, phon2, phon3, …);
2. Use a separate table, as phone(pname, phone);
3. Use single field, as person(pname, …, phones,…) 类型看是原子的，处理方式看不是

Draw 

# Pitfalls

## Decomposition

Lossy Decomposition：分解完再自然连接后元组反而增加了(比如不同系的同名老师)

## Goal — Devise a Theory for the Following

1) Decide whether a particular relation R is in “good” form. 	-No redundant 
2) In the case that a relation R is not in “good” form, decompose it into a set of relations {R1, R2, ..., Rn } such that 
each relation is in good form 
the decomposition is a lossless-join decomposition
Our theory is based on:

* functional dependencies (函数依赖)
* multivalued dependencies (多值依赖)

# Functional Dependencies

$$
\alpha \subset R, \beta \subset R(都可以是多个attributes)
\\
\alpha \rightarrow \beta
\\
\Longrightarrow t1[\alpha] = t2 [\alpha] \Rightarrow t1[\beta]  = t2 [\beta] (t1、t2是元组)
$$

称 β is functionally dependent on α , α functionally determines β

## Key

特殊的，对于super key
$$
K \rightarrow R
$$
对于candidate key
$$
K \rightarrow R
\\
no\ \alpha \subset K, \alpha \rightarrow R(不存在K的真子集α使得α依赖于R)
$$

## 平凡(Trivial)与非平凡

平凡的：$\alpha \rightarrow \beta,{\rm if}\ \beta \subseteq \alpha$

非平凡的：$\alpha \rightarrow \beta,{\rm if}\ \beta \nsubseteq \alpha$

## Armstrong’s Axioms

自反律：${\rm if}\ \beta \subseteq \alpha, {\rm then}\ \alpha \rightarrow \beta$

Augmentation增补律：加上其他也成立

Transitivity传递律

**Example**：PPT8.21

## Closure (属性集的)闭包

FD的集合的闭包F^+^：利用Armstrong公理找出的所有

伪代码

```pseudocode

```



属性的集合的闭包α^+^

## Canonical Cover 正则覆盖



### Extraneous Attributes



计算的伪代码

```pseudocode

```



# Decomposition

目标和特性

## Dependency preservation

