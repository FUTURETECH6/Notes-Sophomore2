# STL

## Container

**分类**

* 顺序容器
    vector, deque,list
* 关联容器
    set, multiset, map, multimap
* 容器适配器
    stack, queue, priority_queue

**顺序容器和关联容器中都有的成员函数**

* begin 返回指向容器中第一个元素的迭代器
* end 返回指向容器中最后一个元素后面的位置的迭代器
* rbegin 返回指向容器中最后一个元素的迭代器
* rend 返回指向容器中第一个元素前面的位置的迭代器
* erase 从容器中删除一个或几个元素
* clear 从容器中删除所有元素

**容器与迭代器**

| 容器           | 迭代器类别 |
| -------------- | ---------- |
| vector         | 随机       |
| dequeue        | 随机       |
| list           | 双向       |
| set/multiset   | 双向       |
| map/multimap   | 双向       |
| stack          | 不支持     |
| queue          | 不支持     |
| priority_queue | 不支持     |

*注： 有的算法，例如sort, binary_search需要通过随机访问迭代器来访问容器中的元素，那么list以及关联容器就不支持该算法!*

### list

除了基础的还支持的函数有：push_front, pop_front, sort, remove(删除和指定值相等的所有元素), unique[删除所有和前一个元素相同的元素(要做到元素不重复，则unique之前还需要 sort)], merge, reverse, splice(在指定位置前面插入另一链表中的一个或多个元素，并在另 一链表中删除被插入的元素)

## 函数对象

重载类的`()`运算符可以让函数对象看上去像函数，以`greater`为例

```cpp
template<class T>
struct greater : public binary_function<T, T, bool> {
    bool operator()(const T& x, const T& y) const {
        return x > y;
    }
};

template<class Arg1, class Arg2, class Result>
struct binary_function {
    typedef Arg1 first_argument_type;
    typedef Arg2 second_argument_type;
    typedef Result result_type;
};
```



## 关联容器

默认是用less模板作为比较器的，因此自定义类需要重载`<`运算符

### set/multiset

RB树，所以是有序的

```cpp
template<class Key, class Pred = less<Key>, class A = allocator<Key> >	// 缺省模板参数Pred和A
class multiset { ...... };
```

| multiset的成员函数                                           |
| ------------------------------------------------------------ |
| iterator find(const T & val);<br />在容器中查找值为val的元素，返回其迭代器。如果找不到，返回end()。 |
| iterator insert(const T & val);<br />将val插入到容器中并返回其迭代器。 |
| void insert( iterator first,iterator last);<br />将区间[first,last)插入容器。 |
| int count(const T & val);<br />统计有多少个元素的值和val相等。 |
| iterator lower_bound(const T & val);<br />查找一个最大的位置 it,使得[begin(),it) 中所有的元素都比 val 小。 |
| iterator upper_bound(const T & val);<br />查找一个最小的位置 it,使得[it,end()) 中所有的元素都比 val 大。 |
| pair<iterator,iterator> equal_range(const T & val);<br />同时求得lower_bound和upper_bound。 |
| iterator erase(iterator it);<br/>删除it指向的元素，返回其后面的元素的迭代器(Visual studio 2010上如此，但是在 C++标准和Dev C++中，返回值不是这样)。 |

### map/multimap

RB树，所以是有序的

map/multimap容器里放着的都是pair模版类的对象，且按first从小 到大排序

```cpp
template<class Key, class T, class Pred = less<Key>, class A = allocator<T> >	// 注意比较器是less<key>
class multimap {
    ...
    typedef pair<const Key, T> value_type;
    ...
};
```

## 容器适配器

### stack

stack 是后进先出的数据结构，只能插入，删除，访问栈顶的元素。 

可用 vector, list, deque来实现。<u>缺省情况下，用deque实现。</u>用 vector和deque实现，比用list实现性能好。

```cpp
template<class T, class Cont = deque<T> >
class stack {
    .....
};
```

操作：push pop top(返回引用)

### queue

和stack 基本类似，可以用 list和deque实现。缺省情况下用deque实现。

```cpp
template<class T, class Cont = deque<T> >
class queue {
    ......
};
```

### priority_queue

和 queue类似，可以用vector和deque实现。缺省情况下用 vector实现。

```cpp
template <class T, class Container = vector<T>, class Compare = less<T> >
class priority_queue {
    ...
};
```

执行top 操作时，返回的是最大元素的**常**引用(因为不允许修改，否则位置就乱了)

## Algorithm

### 不变序列算法

**for_each**(注意是const)

```cpp
template<class InIt, class Fun>
Fun for_each(InIt first, InIt last, Fun f);
// 对[first,last)中的每个元素 e ,执行 f(e) , 要求 f(e)不能改变e。
```

**count**

```cpp
 template<class InIt, class T>
size_t count(InIt first, InIt last, const T& val);
// 计算[first,last) 中等于val的元素个数
```

**count_if**

```cpp
template<class InIt, class Pred>
size_t count_if(InIt first, InIt last, Pred pr);
// 计算[first,last) 中符合pr(e) == true 的元素 e的个数
```

**min_element/max_element**

都是以`<`作为比较器

### 变值算法

* for_each
    对区间中的每个元素都做某种操作
* copy
    复制一个区间到别处
* copy_backward
    复制一个区间到别处，但目标区前是从后往前被修改的
* transform
    将一个区间的元素变形后拷贝到另一个区间
    
    ```cpp
    template<class InIt, class OutIt, class Unop>
    OutIt transform(InIt first, InIt last, OutIt x, Unop uop);
    ```
    
* swap_ranges
    交换两个区间内容
* fill
    用某个值填充区间
* fill_n
    用某个值替换区间中的n个元素
* generate
    用某个操作的结果填充区间
* generate_n
    用某个操作的结果替换区间中的n个元素
* replace
    将区间中的某个值替换为另一个值
* replace_if
    将区间中符合某种条件的值替换成另一个值
* replace_copy
    将一个区间拷贝到另一个区间，拷贝时某个值要换成新值拷过 去
* replace_copy_if 将一个区间拷贝到另一个区间，拷贝时符合某条件的值要换成 新值拷过去

### 删除算法

> 删除算法会删除一个容器里的某些元素。这里所说的“删除”，并不会使容器里的元素减少，其工作过程是:将所有应该被删除的元素看做空位子，然后用留下的元素从后往前移，依次去填空位子。元素往前移后，它原来的位置也就算是空位子，也应由后面的留下的元素来填上。最后，没有被填上的空位子，维持其原来的值不变。<u>删除算法不应作用于关联容器。</u>

* remove
    删除区间中等于某个值的元素
    
* remove_if
    删除区间中满足某种条件的元素
    
* remove_copy
    拷贝区间到另一个区间。等于某个值的元素不拷贝 
    
* remove_copy_if 拷贝区间到另一个区间。符合某种条件的元素不拷贝

* unique
    删除区间中**连续相等**的元素，只留下一个(可自定义比较器)
    
    ```cpp
    template<class FwdIt>
    FwdIt unique(FwdIt first, FwdIt last);	// 用 == 比较是否等
    template<class FwdIt, class Pred>
    FwdIt unique(FwdIt first, FwdIt last, Pred pr);	// 用 pr 比较是否等
    ```
    
    自己写个程序就会发现实际上就是把重复的移到后边了，并且返回值就是指向第一个被移到后面的树的迭代器
    
    ```cpp
    vector<int> vi{1, 2, 3, 4, 4, 5, 3, 45, 4, 5, 23, 5, 23, 5, 13, 5, 23, 525, 35, 25, 23, 523, 5};
    sort(vi.begin(), vi.end());
    auto iEnd = unique(vi.begin(), vi.end());
    for (auto i = vi.begin(); i != iEnd; i++)
        cout << *i << " ";
    cout << endl;
    for (auto &i : vi)
        cout << i << " ";
    /*
    1 2 3 4 5 13 23 25 35 45 523 525 
    1 2 3 4 5 13 23 25 35 45 523 525 5 13 23 23 23 23 25 35 45 523 525 
    */
    ```
    
* unique_copy
    拷贝区间到另一个区间。连续相等的元素，只拷贝第一个到目标区 间 (可自定义比较器)

### 变序算法

* rotate_copy 将区间以首尾相接的形式进行旋转后的结果拷贝到另一个区间， 源区间不变

* next_permutation
    将区间改为下一个排列(可自定义比较器)

* prev_permutation
    将区间改为上一个排列(可自定义比较器)

* random_shuffle
    随机打乱区间内元素的顺序
    用之前要初始化伪随机数种子

    ```cpp
    srand(unsigned(time(NULL)));	//#include <ctime>
    ```

* partition
    把区间内满足某个条件的元素移到前面，不满足该条件的移到后面

* stable_patition
    把区间内满足某个条件的元素移到前面，不满足该条件的移到后面。而且对这两部分元素，分别保持它们原来的先后次序不变

###  排序算法

排序算法需要随机访问迭代器的支持，因而不适用于关联容器和list。

* sort
    快排。将区间从小到大排序(可自定义比较器)。
* stable_sort
    归并。将区间从小到大排序，并保持相等元素间的相对次序(可自定义比较器)。
* partial_sort
    对区间部分排序，直到最小的n个元素就位(可自定义比较器)。
* partial_sort_copy
    将区间前n个元素的排序结果拷贝到别处。源区间不变(可自定义比 较器)。
* nth_element
    对区间部分排序，使得第n小的元素(n从0开始算)就位，而且比它 小的都在它前面，比它大的都在它后面(可自定义比较器)。 
* make_heap
    使区间成为一个“堆”(可自定义比较器)。
* push_heap
    将元素加入一个是“堆”区间(可自定义比较器)。
* pop_heap
    从 “堆”区间删除堆顶元素(可自定义比较器)。
* sort_heap
    将一个“堆”区间进行排序，排序结束后，该区间就是普通的有序区间，不再是 “堆”了(可自定义比较器)。

###  有序区间算法

> 有序区间算法要求所操作的区间是已经从小到大排好序的，而 且需要随机访问迭代器的支持。所以有序区间算法不能用于关 联容器和list。

* binary_search
    判断区间中是否包含某个元素。
* includes
    判断是否一个区间中的每个元素，都在另一个区间中。
* lower_bound
    查找最后一个不小于某值的元素的位置。
* upper_bound
    查找第一个大于某值的元素的位置。
* equal_range
    同时获取lower_bound和upper_bound。
* merge
    合并两个有序区间到第三个区间。
    五个参数，下同
* set_union
    将两个有序区间的并拷贝到第三个区间
* set_intersection
    将两个有序区间的交拷贝到第三个区间
* set_difference
    将两个有序区间的差拷贝到第三个区间
* set_symmetric_difference
    将两个有序区间的对称差拷贝到第三个区间
* inplace_merge
    将两个连续的有序区间原地合并为一个有序区间

## bitset

# C++11

## 初始化

```cpp
int arr[3]{1, 2, 3};
vector<int> iv{1, 2, 3};
map<int, string> mp{{1, "a"}, {2, "b"}};
string str{"Hello World"};
int * p = new int[20]{1,2,3};
```

## decltype

```cpp
int i;
double t;
struct A { double x; };
const A* a = new A();
decltype(a) x1; // x1 is A *
decltype(i) x2; // x2 is int
decltype(a->x) x3; // x3 is double
decltype((a->x)) x4 = t; // x4 is double&
```

## smart pointer

## for

## 右值引用和move

```cpp

class A{};
A & r1 = A();	//错误，无名临时变量 A() 是右值，因此不能初始化左值引用 r1
A && r2 = A();	//正确，因 r2 是右值引用
```

```cpp
#include <cstring>
#include <iostream>
#include <string>

using namespace std;

class String {
  public:
    char *str;

    String() : str(new char[1]) { str[0] = 0; }
    String(const char *s) {
        str = new char[strlen(s) + 1];
        strcpy(str, s);
    }

    String(const String &s) {
        cout << "copy constructor called" << endl;
        str = new char[strlen(s.str) + 1];
        strcpy(str, s.str);
    }

    String &operator=(const String &s) {
        cout << "copy operator= called" << endl;
        if (str != s.str) {
            delete[] str;
            str = new char[strlen(s.str) + 1];
            strcpy(str, s.str);
        }
        return *this;
    }

    // move constructor
    String(String &&s) : str(s.str) {
        cout << "move constructor called" << endl;
        s.str    = new char[1];
        s.str[0] = 0;
    }

    // move assigment
    String &operator=(String &&s) {
        cout << "move operator= called" << endl;
        if (str != s.str) {
            delete[] str;
            str      = s.str;
            s.str    = new char[1];
            s.str[0] = 0;
        }
        return *this;
    }

    ~String() { delete[] str; }
};

template <class T>
void MoveSwap(T &a, T &b) {
    T tmp(move(a));  // std::move(a)为右值，这里会调用move constructor
    a = move(b);     // move(b)为右值，因此这里会调用move assigment
    b = move(tmp);   // move(tmp)为右值，因此这里会调用move assigment
}

int main() {
    // String & r = String("this"); // error
    String s;
    s = String("ok");  // String("ok")是右值
    cout << "******" << endl;
    String &&r = String("this");
    cout << r.str << endl;
    String s1 = "hello", s2 = "world";
    MoveSwap(s1, s2);
    cout << s2.str << endl;
    return 0;
}
/*
move operator= called
******
this
move constructor called
move operator= called
move operator= called
hello
*/
```

第 41 行重载了一个移动赋值号。它和第 23 行的复制赋值号的区别在于，其参数是右值引用。在移动赋值号函数中没有执行深复制操作，而是直接将对象的 str 指向了参数 s 的成员变量 str 指向的地方，然后修改 s.str 让它指向别处，以免 s.str 原来指向的空间被释放两次。

该移动赋值号函数修改了参数，这会不会带来麻烦呢？答案是不会。因为移动赋值号函数的形参是一个右值引用，则调用该函数时，实参一定是右值。右值一般是无名临时变量，而无名临时变量在使用它的语句结束后就不再有用，因此其值即使被修改也没有关系。

第 65 行，如果没有定义移动赋值号，则会导致复制赋值号被调用，引发深复制操作。临时无名变量`String("ok")`是右值，因此在定义了移动赋值号的情况下，会导致移动赋值号被调用。移动赋值号使得 s 的内容和 `String("ok")` 一致，然而却不用执行深复制操作，因而效率比复制赋值号高。

虽然移动赋值号修改了临时变量 `String("ok")`，但该变量在后面已无用处，因此这样的修改不会导致错误。

第 57 行使用了 C++ 11 中的标准模板 move。move 能接受一个左值作为参数，返回该左值的右值引用。因此本行会用定义于第 28 行、以右值引用作为参数的移动构造函数来初始化 tmp。该移动构造函数没有执行深复制，将 tmp 的内容变成和 a 相同，然后修改 a。由于调用 MoveSwap 本来就会修改 a，所以 a 的值在此处被修改不会产生问题。

第 58 行和第 59 行调用了移动赋值号，在没有进行深复制的情况下完成了 a 和 b 内容的互换。对比使用引用的 Swap 函数。

Swap 函数执行期间会调用一次复制构造函数，两次复制赋值号，即一共会进行三次深复制操作。而利用右值引用，使用 MoveSwap，则可以在无须进行深复制的情况下达到相同的目的，从而提高了程序的运行效率。

## unordered_map

哈希

## regex

## lambda

````cpp
//形式:
[外部变量访问方式说明符](参数表) ->返回值类型
{
    语句组
}

[]：不使用任何外部变量
[=]：以传值的形式使用所有外部变量
[&]：以引用形式使用所有外部变量
[x, &y]：x以传值形式使用，y以引用形式使用
[=,&x,&y]：x,y以引用形式使用，其余变量以传值形式使用
[&,x,y]：x,y以传值的形式使用，其余变量以引用形式使用

“->返回值类型”也可以没有， 没有则编译器自动判断返回值类型。
````

Ex1.

```cpp
int main() {
    int x = 100, y = 200, z = 300;
    cout << [](double a, double b) { return a + b; }(1.2, 2.5) << endl;
    // 或者function<int(int)> ff = 
    auto ff = [=, &y, &z](int n) {
        cout << x << endl;
        y++;
        z++;
        return n * n;
    };
    cout << ff(15) << endl;
    cout << y << ", " << z << endl;
}
/*
3.7
100
225
201, 301
*/
```

Ex2.

```cpp
int main() {
    int a[4] = {4, 2, 11, 33};
    sort(a, a + 4, [](int x, int y) -> bool { return x % 10 < y % 10; });
    for_each(a, a + 4, [](int x) { cout << x << " "; });
}
/*
11 2 33 4 
*/
```

Ex3.

```cpp
int main() {
    vector<int> a{1, 2, 3, 4};
    int total = 0;
    for_each(a.begin(), a.end(), [&](int &x) {
        total += x;
        x *= 2;
    });
    cout << total << endl;  //输出 10
    for_each(a.begin(), a.end(), [](int x) { cout << x << " "; });
    return 0;
}
/*
10
2 4 6 8 
*/
```

## 多线程

`#include <thread>`

## 类型强制转换