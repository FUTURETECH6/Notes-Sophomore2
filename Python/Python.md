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



format

```python

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

==Python中逻辑表达式~~不再~~将数作为bool型处理了==

| 运算符 | 逻辑表达式 | 描述                                                         | 实例                    |
| :----- | :--------- | :----------------------------------------------------------- | :---------------------- |
| and    | `x and y`  | 布尔"与" - 如果 x 为 False，x and y 返回 False，否则它返回 y 的计算值。<br />在布尔上下文中从左到右演算表达式的值，如果布尔上下文中的**所有值都为真，那么 and 返回最后一个值。**<br/>如果布尔上下文中的**某个值为假，则 and 返回第一个假值** | (a and b) 返回 20。     |
| or     | `x or y`   | 布尔"或" - 如果 x 是非 0，它返回 x 的值，否则它返回 y 的计算值。<br />如果**有一个值为真，or 立刻返回该值**<br/>如果**所有的值都为假，or 返回最后一个假值** | (a or b) 返回 10。      |
| not    | `not x`    | 布尔"非" - 如果 x 为 True，返回 False 。如果 x 为 False，它返回 True。<br />将数视作bool | not(a and b) 返回 False |

`ord("")`查询Unicode编码，只能单字符

