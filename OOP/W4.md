```c++
int i = 0; double d = 0.0;
int *pi = &i;
double *pd = &d;

pi = pd;		// Both not ok
void *pv = pd;	// Both ok
pi = pv;		// C ok, C++ not ok
```





# Class

**Objects = Attributes(Data) + Services(Operations)**



构造函数：

一个构造函数也没有时编译器会生成一个空函数作为构造函数，但只要有一个自己写的构造函数之后就不会了，因此如果该构造函数有参数，新定义的类必须带参数进行初始化，否则报错`no matching constructor for initialization of 'A'`



```cpp
#include ""	// 先在当前再在某个其他地方申明了的地方
#include <>	// 直接在指定的地方
```



## Header

Tips for header

1. One class declaration per header file

2. Same prefix with source file.

3. The contents of a header file is surrounded with

    \#ifndef #define... #endif

