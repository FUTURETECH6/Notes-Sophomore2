# Chapter 1

```python
# 单行输入多个数字
a, b, c = map(int, input().split())

# type()显示变量类型

# +@, -@是sign运算，+-@,-+@同-@；++@,--@同+@

# 关于int()
>>> int("123.4")
ValueError: invalid literal for int() with base 10: '123.4'
>>> int(-123.9)	# 切掉小数部分
-123

# 关于eval()
>>> eval("12")
12
>>> eval("12.34")
12.34
>>> eval(12)
TypeError: eval() arg 1 must be a string, bytes or code object

# 关于id()
>>> x = 3
>>> print(id(x))
>>> x = 7
>>> print(id(x))
4451259600
4451259728

# 关于sum()
sum = sum(1/(2*i-1) for i in range (1, n+1))
```

\! `<<` 优先级低于+/-

UTF-8 是 Unicode 的实现方式之一。是**变长**字符集

类型在使用过程中可以改变：`x = 3; x = '123'`

| 运算符                     | 描述                                                       | Addition                                                     |
| :------------------------- | :--------------------------------------------------------- | ------------------------------------------------------------ |
| `+@, -@`                   | 正负号                                                     |                                                              |
| `**`                       | 指数 (最高优先级)                                          | `3**2**3==6561==3**8`<br />**从右向左**3\^(2\^3)             |
| ~~`~ + -`~~                | ~~按位翻转, 一元加号和减号 (最后两个的方法名为 +@ 和 -@)~~ |                                                              |
| `* / % //`                 | 乘，除，取模和取整除                                       | <u>浮点数(无论是除数还是被除数)整除完还是浮点数</u>：<br />`type(1.1//1)==type(2//1.1)==<class 'float'>` |
| `+ -`                      | 加法减法                                                   |                                                              |
| `>> <<`                    | 右移，左移运算符                                           | 课程不要求                                                   |
| `&`                        | 位 'AND'                                                   | 课程不要求                                                   |
| `^ |`                      | 位运算符                                                   | 课程不要求                                                   |
| `<= < > >=`                | 比较运算符                                                 |                                                              |
| `<> == !=`                 | 等于运算符                                                 |                                                              |
| `= %= /= //= -= += *= **=` | 赋值运算符                                                 |                                                              |
| `is, is not`               | 身份运算符                                                 | 课程不要求                                                   |
| `in, not in`               | 成员运算符                                                 | 课程不要求                                                   |
| `not, and, or`             | 逻辑运算符                                                 | not > and > or                                               |

# Chapter 2

## 浮点、格式

```python
# 保留位数+四舍五入
print("%.2f" % a)
print("f(%.1f) = %.1f" % (x, s))	# 多个
round(a, 2)
print(format(1234.56789, "5.3f"))	# 1234.567	# 超过场宽了
print("Avg is {:.1f}".format(avg))

# 逗号分隔
a, b, c = map(int, input().split(','))

# 列表输入转化
myList = list(map(int,input().split()))

# range()
range(lower_bound, upper_bound, step)
```

各种type见第二章习题

`chr()`返回当前值对于的ASCII字符

~~字符串以\0标志字符串结束~~

**Regax**

```python
>>> import sys
>>> a=complex(1,2); a
(1+2j)
>>> 1+2J # 大小写都可以
(1+2j)
>>> 1+2j
(1+2j)
>>> a.real	# 不是函数没有()；是变量成员？
1.0
>>> a.imag
2.0
```

==单斜杠就是浮点除==

**负数的取整和求余**

* 向左取整`-3 // 2 == -2`
* 向左取余`-9 % 4  ==  3`



**浮点数**的误差：特别注意==取等的判断==

```python
2.1 - 2.0 != 0.1
round(2.1 - 2.0, 1) == 0.1
round(2.675, 2) == 2.67 != 2.68	# 这个也注意

>>> round(18.67, -1)			# !!!!直接输出会保留一位但是还是float
20.0
>>> round(12345.6789, -3)
12000.0
>>> type(round(12345.6789, -3))
<class 'float'>

>>> print("%.32f" % 2.675)
2.67499999999999982236431605997495
```







math库

```python
math.pow(x, y)
pow(x, y[, z])	# 自带的，pow(x,y)%z
```



string

```python
# 独特操作是"括'，'括"，转义也可以
# 下标：正向0到n-1，反向-1到-n
>>> str = "abcdefghijklmn"
>>> str[0: 9: 2]	# 
'acegi'
>>> str[-1: -9: -2]	# 注意步长相应变负
'nljh'
>>> str.replace('cde', '12'); str	# 只换第一个
'ab12fghijklmn'
```

## 关系运算、逻辑运算

关系运算符

```python
>>> 1<3<5
True
>>> 3<5>2	# == 3 < 5 and 5 > 2
True
>>> "Hello" > "world" # 'H' < 'w'
False
# 仍然有短路原则，即能确定解就不往下了，所以即使后面有语法错误也不会报错
>>> 3 < 5 and 'a' > 3
False
>>> 3 > 5 or 'a' > 3
  File "<stdin>", line 1, in <module>
TypeError: '>' not supported between instances of 'str' and 'int'
    
>>> ((2>=2) or (2<2)) and 2	# 为什么啊啊啊啊啊啊啊啊啊
2
>>> True and 99
99
```

==关系表达式返回值只有`True`和`False`==

~~==Python中逻辑表达式不再将数作为bool型处理了==~~

| 运算符 | 逻辑表达式 | 描述                                                         | 实例                    |
| :----- | :--------- | :----------------------------------------------------------- | :---------------------- |
| and    | `x and y`  | 布尔"与" - 如果 x 为 False，x and y 返回 False，否则它返回 y 的计算值。<br />在布尔上下文中从左到右演算表达式的值，如果布尔上下文中的**所有值都为真，那么 and 返回最后一个值。**<br/>如果布尔上下文中的**某个值为假，则 and 返回第一个假值** | (a and b) 返回 20。     |
| or     | `x or y`   | 布尔"或" - 如果 x 是非 0，它返回 x 的值，否则它返回 y 的计算值。<br />如果**有一个值为真，or 立刻返回该值**<br/>如果**所有的值都为假，or 返回最后一个假值** | (a or b) 返回 10。      |
| not    | `not x`    | 布尔"非" - 如果 x 为 True，返回 False 。如果 x 为 False，它返回 True。<br />将数视作bool | not(a and b) 返回 False |

Ex.

`0 and 1 or not 2 < True`：`0and1`返回0，`not 2 < True`返回True，`0 or True`返回第一个真值`True`

`ord("")`查询Unicode编码，只能单字符

## 基本语句

### 序列赋值

，以下这种只能给单字符字符串

```python
>>> a, b = "34"
>>> a
'3'
>>> b
'4'

>>> a, b = "123"
ValueError: too many values to unpack (expected 2)
>>> a, b = "1234"
ValueError: too many values to unpack (expected 2)
```

不等长赋值

这里是当作**列表**操作的而不是字符串

```python
>>> i, *j = "1234"
>>> i
'1'
>>> j
['2', '3', '4']


>>> *a, b = [1, 2, 3]
>>> a
[1, 2]
>>> b
3
```



**range**

三个参数都是`int`

start为0可缺省，但此时步长也必须缺省(否则`stop, step`会被当成`start, stop`)

### sum

```python
>>> sum(range(1, 101))
5050
>>> sum([1, 3, 7])
11
>>> sum(1/i for i in range(1,21))
3.597739657143682
>>> sum([1/i for i in range(1,21)])
3.597739657143682
```

### 列表推导式

要加中括号

```python
求6+66+...

tmp = 0
for i in range(1, n+1):
    a += tmp + 6
    tmp *= 10

for i in range(1, n+1):
    a += int('6' * i)

a = sum([int('6' * i) for i in range(1, n+1)])

myList = [2 * number for number in [1,2,3,4,5]]

# 加条件
[expression for item in iterable if condition]
[i for i in range(1,8) if i % 2 == 1]	# 求1到7奇数的列表
[exp1 if condition else exp2 for item in iterable]
# [if condition exp1 else exp2 for item in iterable]本质是这样但是不能这么写，否则condition和exp1会没有keyword分隔
[1 if i%2 == 1 else 0 for i in range(0,10)]

# 二维列表
print([[i for i in range(1+j*3, 4+j*3)] for j in range(0, 3)])
```



## 数据容器：序列

### 序列操作

| 操作       | **描述**                                 |                                        |
| ---------- | ---------------------------------------- | -------------------------------------- |
| X1+X2      | 联接序列X1和X2，生成新序列               |                                        |
| X\*n       | 序列X重复n次，生成新序列                 |                                        |
| X[i]       | 引用序列X中下标为i的成员                 | -1开始导数                             |
| X[i:j]     | 切片：引用序列X中下标为i到j-1的子序列    |                                        |
| X[i: j: k] | 引用序列X中下标为i到j-1的子序列，步长为k | i，j越界没关系，如果没东西会反应为空集 |
| X[::-1]    | 倒序切片                                 |                                        |
| len(X)     | 计算序列X中成员的个数                    |                                        |
| max(X)     | 序列X中的最大值                          | 字符串的通过Unicode来比较              |
| min(X)     | 序列X中的最小值                          | 可以通过`ord()`查询Unicode             |
| v in X     | 检查v是否在序列X中，返回布尔值           |                                        |
| v not in X | 检查v是否不在序列X中，返回布尔值         |                                        |

访问操作：

```python
>>> print([2, 3, 5, 7, 11, 13][1:-3])
[3, 5]
>>> print([2, 3, 5, 7, 11, 13][2:])
[5, 7, 11, 13]
>>> print([2, 3, 5, 7, 11, 13][:3])
[2, 3, 5]
>>> print([2, 3, 5, 7, 11, 13][:-2])
[2, 3, 5, 7]
>>> print([2, 3, 5, 7, 11, 13][-1:0:-1])
[13, 11, 7, 5, 3]

>>> print([2, 3, 5, 7, 11, 13][3::-1])
[7, 5, 3, 2]
>>> print([2, 3, 5, 7, 11, 13][3:-5:-1])
[7, 5]
>>> print([2, 3, 5, 7, 11, 13][3:-5:1])
[]
>>> print([2, 3, 5, 7, 11, 13][:])
[2, 3, 5, 7, 11, 13]
>>> print([2, 3, 5, 7, 11, 13][])
  File "<stdin>", line 1
    print([2, 3, 5, 7, 11, 13][])
                               ^
SyntaxError: invalid syntax
```





### 字符串

`TypeError: 'str' object does not support item assignment`

==str是只读的数据类型==

`"Hello"`

在前面加r则转义符不工作(实际上是吧`\`变成`\\`)

```python
>>> r"hello\nworld"
'hello\\nworld'
```

#### %

```python
>>> print("%4d" % 9)	# 设置场宽，默认右对齐
   9
>>> print("%-4d" % 9)	# 改为左对齐，注意他会补空格
9   
>>> print("%04d" % 9)	# 0表示补0左对齐
0009
>>> print("%+03d" % 9)	# 0表示补0左对齐
+09
>>> print("%-03d" % 9)	# 注意他会补空格
9  
>>> print("%+d" % 9)	# +表示输出sign
+9
>>> print("%+d" % -9)	# +表示输出sign
-9
```



#### format

**^**, **<**, **>** 分别是居中、左对齐、右对齐，后面带宽度， **:** 号后面带填充的字符，只能是一个字符，不指定则默认是用空格填充。

**+** 表示在正数前显示 **+**，负数前显示 **-**； （空格）表示在正数前加空格

b、d、o、x 分别是二进制、十进制、八进制、十六进制。

```python
>>> "{1} {0} {1}".format("hello", "world")  # 设置指定位置
'world hello world'

>>> print('value 为: {0.value}'.format(my_value))  # "0" 是可选的
value 为: 6
    
>>> print("{:.2f}".format(3.1415926));
3.14

print("f({0:.1f}) = {1:.1f}".format(x, s))	# ~~不加索引(0, 1)则按顺序~~规范一点好
print("f(%.1f) = %.1f" % (x, s))
```

```python
>>> '{0:*>10}'.format(10)		# 右对齐
'********10'
>>> '{0:*<10}'.format(10)		# 左对齐
'10********'
>>> '{0:*^10}'.format(10)		# 居中对齐
'****10****'
>>> '{0:.2f}'.format(1/3)
'0.33'
>>> '{0:b}'.format(10)			# 二进制
'1010'
>>> '{0:o}'.format(10)			# 八进制
'12'
>>> '{0:x}'.format(10)			# 16进制
'a'
>>> '{:,}'.format(12345678901)	# 千分位格式化
'12,345,678,901'



print(11 * '*')
print('*' + "{:^9}".format("Hello") + "*")	# 居中的妙用
print(11 * '*')
***********
*  Hello  *
***********
```

#### 常用函数

| 字符串常用方法或函数          | 解释                                                         |                                                              |
| ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| S.title()                     | 字符串S首字母大写                                            |                                                              |
| S.lower()                     | 字符串S变小写                                                |                                                              |
| S.upper()                     | 字符串S变大写                                                |                                                              |
| S.strip(),S.rstrip(),lstrip() | 删除前后空格，删除右空格，删除左空格                         |                                                              |
| S.find(sub[,start[,end]])     | 在字符串S中查找sub子串首次出现的位置，找不到则返回-1         | 必须完整出现，且范围为半开半闭的`[start, end)`<br />如`"0123456789".find("678", 6, 8)`输出也是-1 |
| S.replace(old,new)            | 在字符串S中用new子串替换old子串                              |                                                              |
| S.join(X)                     | 将序列X用S连接合并成字符串                                   |                                                              |
| S.split(sep=None)             | 将字符串S拆分成列表<br />缺省参数的话<u>即使多个空格</u>也会被视作一个sep | `>>> '3//11//2018'.split('/')`<br />`['3', '', '11', '', '2018']` |
| S.count(sub[,start[,end]])    | 计算sub子串在字符串S中出现的次数                             |                                                              |
| re.split(" +", sentense)      | 正则表达式匹配多个空格，返回值是列表                         |                                                              |

还有其他is开头的判断函数，注意有空格的陷阱

https://www.runoob.com/python3/python3-string.html

```python
# 字符串排序新方法，其中join() 方法用于将序列中的元素以指定的字符连接生成一个新的字符串。
print("".join(sorted(dict)))	# 调用sorted()会自动将dict类型转为list

str = "-";
seq = ("a", "b", "c"); # 字符串序列
print str.join( seq );	# a-b-c

```





```python
# *的使用
>>> a = "Hello,"
>>> b = "world!"
>>> print([a, b])
['Hello,', 'world!']
>>> print([*a, *b])
['H', 'e', 'l', 'l', 'o', ',', 'w', 'o', 'r', 'l', 'd', '!']
```

### 列表

`['H', 'e', 'l', 'l', 'o']`

**列表的直接赋值传递指针**，切片用于序列的实拷贝

```python
# 这里是个指针？
>>> a = [2, 3, 5, 7, 11, 13]
>>> b = a
>>> b[0] = 1
>>> a
[1, 3, 5, 7, 11, 13]
# 切片的使用
>>> a = [2, 3, 5, 7, 11, 13]
>>> b = a[:]
>>> b[0] = 1
>>> a
[2, 3, 5, 7, 11, 13]
```

#### 常用函数

| **列表的常用方法或函数**              | **描述**                                                   |                           |
| ------------------------------------- | ---------------------------------------------------------- | ------------------------- |
| **L.append(x)**                       | 在列表L尾部追加x                                           |                           |
| **L.clear()**                         | 移除列表L的所有元素                                        |                           |
| **L.count(x)**                        | 计算列表L中x出现的次数                                     |                           |
| **L.copy()**                          | 列表L的备份                                                |                           |
| **L.extend(x)**                       | 将列表x扩充到列表L中                                       | `a.extend()[1,2])`        |
| ==**L.index(value[,start[,stop]])**== | 计算在指定范围内value的下标，找不到则报错，得先写个`if in` | 不是find()，这是字符串的  |
| **L.insert(index,x)**                 | 在下标index的位置插入x                                     | 若index不存在，则加到最后 |
| **L.pop(index)**                      | 返回并删除下标为index的元素，默认是最后一个                |                           |
| **L.remove(value)**                   | 删除值为value的第一个元素                                  |                           |
| **L.reverse()**                       | 倒置列表L                                                  |                           |
| **L.sort()**                          | 对列表元素排序                                             |                           |
| **S.join(L)**                         | 用S将L(L中元素必须是string类型)连起来                      |                           |

### 元组

`('H', 'e', 'l', 'l', 'o')`

==完全只读==

#### 常用函数

| 元组常用方法和函数 | 描述                |
| ------------------ | ------------------- |
| T.count(x)         | 计算x元素出现的次数 |
| T.index(x)         | 计算X元素的下标     |



## 随机函数

| **函数名**                                | **含义**                                                     | **示列**                      |
| ----------------------------------------- | ------------------------------------------------------------ | ----------------------------- |
| random.random()                           | 返回一个介于左闭右开[0.0,  1.0)区间的**浮点数**              | random.random()               |
| random.uniform(a,  b)                     | 返回一个介于 [a，b] 的浮点数。                               | random.uniform(1,10)          |
| random.randint(a,b）                      | 返回 [a,b] 的一个随机整整。                                  | random.randint(15,30）        |
| random.randrange(  [start], stop[, step]) | 从指定范围内，获取一个随机数                                 | random.randrange(10,  30, 2)  |
| random.choice(sequence)                   | 从序列中获取一个随机元素                                     | random.choice([3,78,43,7]）   |
| random.shuffle(x)                         | 用于将一个列表中的元素打乱,即将列表内的元素随机排列          | random.shuffle(l)  , l是序列  |
| random.sample(sequence,  k)               | 从指定序列中随机获取长度为k的序列并随机排列                  | random.sample([1,4,5,89,7],3) |
| random.seed(n)                            | 对随机数生成器进行初始化的函数，n代表随机种子。参数为空时，随机种子为系统时间 | random.seed(2)                |

产生随机密码，老师的方法太复杂了，可以这样

```python
import random as rd
import string

pwLength = 10

pool = ""
pool += string.digits
pool += string.ascii_letters
print("".join(rd.sample(pool, pwLength)))
```

