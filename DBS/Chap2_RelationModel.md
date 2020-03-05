### Attribute Type

* 1st NF
* 多值属性：电话多个
* 复合型属性：省市区
* null：是任何domain的成员，表示值未知或不存在



Schema：

Instance：Snapshot



### Relation Schema

$R(A_1, A_2, ..., A_3)$ is a relation schema

r(R) is a relation on the relation schema R

### Relation Instance

$t[A_i]$ e.g. $t[name]$

### Key

码

* superkey超码：一个或多个属性的**集合**，可以唯一地区分不同的元组
* candidate key候选码：任意真子集都不是超码(最小的superkey)，如`{ID}, {name, dept_name}`是，而`{ID, name}`不是候选码
* primary key主码：人为选出用来区分元组的候选码

#### Foreign Key 外键

(r中)是另外一个表(s)的**主键**的**属性**

把本表(r)称为referencing relation，另外一个表(s)称为referenced relation

如：

r: instructor(**ID**, naem, <u>dept_name</u>, salary);

s: department(**<u>dept_name</u>**, buliding, budget)

**作用**：一般的DBMS，要在r中插入时，外键必须在s中或为null，可以防止出错



# Query Language

Basic Operator

| Name                            |          | 作用                                                         | Ex                                                 |
| ------------------------------- | -------- | ------------------------------------------------------------ | -------------------------------------------------- |
| 选择 Select                     | $\sigma$ |                                                              | $\sigma_{\rm{salary \geqslant 85000}}(instructor)$ |
| 投影 Project                    | $\Pi$    | 取出某几个属性，重复的tuple会被去掉                          | $\Pi_{ID,salary}(instructor)$                      |
| 并 Union                        | $\cup$   | $r \cup s = \{t | t \in r {\ \rm or\ } t \in s\}$<br />1. r,s have same arity(等目)<br />2. attribute domains must be compatible | $\Pi_{name}(instructor) \cup \Pi_{name}(student)$  |
| 集合差<br />Set difference      | $-$      | $r - s = \{t | t \in r {\ \rm and\ } t \notin s\}$           |                                                    |
| 笛卡尔积<br />Cartesian product | $\times$ | $\sigma_{A=C}(r \times s)$                                   | $instructor \times department$                     |
| 重命名<br />Rename              |          | $\rho_{X(A_1,...,A_n)}(E)$                                   |                                                    |

例题：PPT 2.34-2.38



Additional Op

简化语句

| Name                     | Symbol                 |                                                              | Ex                            |
| ------------------------ | ---------------------- | ------------------------------------------------------------ | ----------------------------- |
| 交<br />Set Intersection |                        |                                                              |                               |
| 自然连接<br />Join       | $\Join$<br />$\bowtie$ | 属性同名的保留一份                                           | $instructor \Join department$ |
| 除<br />Division         | $\div$                 | 返回左边在右边元组都有组合的属性<br />$R=(A_1,...,A_m,B_1,...B_n)$<br />$S=(B_1,...,B_n)$<br />$R-S = (A_1,...Am)$<br />$r \div s = \{t|t\in \Pi_{R-S}(r) \and \forall u \in s (tu\in r)\}$<br />**属性**：$q = r \div s \Rightarrow q {\ \rm is\ the\ largest\ }\\{\rm relation\ satisfying\ }q\times s \subseteq r$ | 看书/ppt吧                    |
| 赋值<br />Assignment     | $\leftarrow$           |                                                              |                               |

## Summary

Union， set difference， Set intersection 为双目、等元运 算

Cartesian product， Natural join ， Division 双目运算

Project, select 为单运算对象

Priority(关系运算的优先级)：

Project, Select, Cartesian Product(times), Join, division, Intersection, Union, difference



### Extended

**Generalized Projection**：使用算数表达式

**Aggregate Functions and Operations**：avg，min，max，sum，count

**Outer Join**：左右全



## Null

根据SQL的处理方式：

聚合函数忽略，分组、去重的时候同其他

arithmetic：涉及`null`结果为`null`

logical：涉及`null`结果为`unknown`，`unknown`不同于`null`而类似于dontCare

`P is unknown` evalutes to be true (P evaluates to `unknown`)
