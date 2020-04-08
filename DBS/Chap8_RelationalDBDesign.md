# First Normal Form

原子：如果某个域中元素被认为是不可分的，则这个域称为是原子的

- 非原子域的例子:
    ― 复合(composite)属性:名字集合
    ― 多值(multi-value)属性:电话号码
    ― 复杂数据类型:面向对象的
- 如果关系模式R的所有属性的域都是原子的，则R称为属于**第一范式 (1NF)**
- 非原子值存储复杂并易<u>导致数据冗余</u>(redundant)
    - 我们假定所有关系都属于第一范式

- 原子性实际上是由域元素在数据库中**如何被使用**决定的
    - 例，字符串通常会被认为是不可分割的
    - **但是**假设学生被分配这样的标识号:CS0012或EE1127，如果前两个字母表示系，后四位数字表示学生在该系内的唯一号码，则这样的标识号不是原 子的
    - 当采用这种标识号时，是不可取的。因为这需要额外的编程，而且信息 是在应用程序中而不是在数据库中编码

## 非原子值处理

**复合属性**

1. 让每个子属性本身成为成为一个属性

**多值属性的处理方式**

1. Use multi fields, as person(pname, … , phon1, phon2, phon3, …);
2. Use a separate table, as phone(pname, phone);
3. Use single field, as person(pname, …, phones,…) 类型看是原子的，处理方式看不是

# Pitfalls

关系数据库设计要求我们找到一个“好的”关系模式集合。一个坏的 设计可能导致

- redundant storage
- insert / delete / update anomalies
- inability to represent certain information

**数据冗余**

如将instructor和department合并，则所有同个系的老师的系的信息都会被存储多次

**增删改异常**

更新：如一个计算机从玉泉搬到紫金港了，那instructor里所有妓院老师的dept_building都得改

增删：刚入职的任奎没有系，本来一个dept=NULL就行了，现在得有一堆信息是NULL，就很难处理

## Decomposition

**模式分解**

例，可以将关系模式(A,B,C,D)分解为:(A,B)和(B,C,D)，或(A,C,D)和 (A,B,D)，或(A,B,C)和(C,D)，或(A,B)、(B,C)和(C,D)，或 (A,D)和 (B,C,D)。例，将关系模式inst_dept分解为:

- instructor(ID,name,dept_name,salary)
- department(dept_name,building,budget)

**Lossy Decomposition**：分解完再自然连接后元组反而增加了(比如不同系的同名老师，连接完每个人都会跟着跟自己同名的所有老师的系)，这里的有损并不是指数据丢失，而是数据多余

**Lossless-join decomposition**：

## Goal — Devise a Theory for the Following

1) Decide whether a particular relation R is in “good” form. 	-No redundant 
2) In the case that a relation R is not in “good” form, decompose it into a set of relations {R1, R2, ..., Rn } such that 
each relation is in good form 
the decomposition is a lossless-join decomposition
Our theory is based on:

* functional dependencies (函数依赖)
* multivalued dependencies (多值依赖)

# Functional Dependencies

## Definition

$$
\alpha \subset R, \beta \subset R(都可以是多个attributes)
\\
\alpha \rightarrow \beta
\\
\Longrightarrow t1[\alpha] = t2 [\alpha] \Rightarrow t1[\beta]  = t2 [\beta] (t1、t2是元组)
$$

称 β is functionally dependent on α , α functionally determines β

其实就类似于函数关系$x\rightarrow f(x)$

例如对于下面的实例

| A    | 1    | 1    | 3    |
| ---- | ---- | ---- | ---- |
| B    | 4    | 5    | 7    |

A不能决定B，但是B可以决定A

## 与Key关系

特殊的，对于super key
$$
K \rightarrow R
$$
对于candidate key
$$
K \rightarrow R
\\
\nexists\ \alpha \subset K, \alpha \rightarrow R(不存在K的真子集α，使得α依赖于R)
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

