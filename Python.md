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



| 运算符                     | 描述                                                   | Addition              |
| :------------------------- | :----------------------------------------------------- | --------------------- |
| `**`                       | 指数 (最高优先级)                                      | `3**2**3==6561==3**8` |
| `~ + -`                    | 按位翻转, 一元加号和减号 (最后两个的方法名为 +@ 和 -@) |                       |
| `* / % //`                 | 乘，除，取模和取整除                                   |                       |
| `+ -`                      | 加法减法                                               |                       |
| `>> <<`                    | 右移，左移运算符                                       |                       |
| `&`                        | 位 'AND'                                               |                       |
| `^ |`                      | 位运算符                                               |                       |
| `<= < > >=`                | 比较运算符                                             |                       |
| `<> == !=`                 | 等于运算符                                             |                       |
| `= %= /= //= -= += *= **=` | 赋值运算符                                             |                       |
| `is is not`                | 身份运算符                                             |                       |
| `in not in`                | 成员运算符                                             |                       |
| `not and or`               | 逻辑运算符                                             |                       |

# Chapter 2

```python
# 保留位数+四舍五入
print("%.2f" % a)
print("f(%.1f) = %.1f" % (x, s))	# 多个
round(a, 2)

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

