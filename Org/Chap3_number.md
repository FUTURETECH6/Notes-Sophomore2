# 数

## 符号拓展

note: `addi` still sign-extends!

note: `sltu, sltiu` for unsigned comparisons

溢出可能的后果：

`-127(1000 0001)+(-2)(1111 1110) = +127(0111 1111)`

常见情况

|      |      | 结果 |
| ---- | ---- | ---- |
| A+B  | ++   | <0   |
| A+B  | --   | \>0  |
| A-B  | +-   | <0   |
| A-B  | -+   | \>0  |

**lb、lbu**： Loads a byte into the lowest 8 bits of a register，剩下24位是正负拓展

**sltu、sltiu**：

注意单源指令用的位段不一定是rs：`sll $t2, $s0, 3	#sll rd,rt,i`；目的寄存器也不一定是rd：`slti rt, rs, immediate`



互补的二补码作为无符号数相加=模，如3+-3 = 0011 + 1101 = 10000(此即为补码运算的正常溢出)

补码的补码是自己(1101 -> 1011 -> 1101)

## Biased Notation

```
8-bit biased-127 notation:
11111111 = 255 - 127 = +128
00000000 =   0 - 127 = -127
01111111 = 127 - 127 = 0
```



# ALU

| ALU Control Lines        | Function         |
| ------------------------ | ---------------- |
| 000                      | And              |
| 001                      | Or               |
| 010                      | Add              |
| 110                      | Sub              |
| 111                      | Set on less than |
| 符号控制、逻辑代数控制、 |                  |





### ALU加速

**Carry look-ahead adder (CLA)**

C~i+1~	 =bi ci + ai ci + ai bi 

​           =ai bi +(ai $\oplus$ bi )ci

Generate gi = ai bi

Propagate pi = ai $\oplus$ bi / ai + bi (都可以)

c~i+1~ = gi + pi ci

c1 = g0 + (p0 * c0)
c2 = g1 + p1*c1 = g1 + (p1 * g0) + (p1 * p0 * c0)
c3 = g2 + p2*c2 = g2 + (p2 * g1) + (p2 * p1 * g0) + (p2 * p1 * p0 * c0)
c4 = g3 + p3*c3 = g3 + (p3 * g2) + (p3 * p2 * g1) + (p3 * p2 * p1 * g0) + (p3 * p2 * p1 * p0 * c0)

一般不会look ahead太多位，四位挺好的：

```pseudocode
c4 =  g3  + p3*g2   + p3*p2*g1    + p3*p2*p1*g0     + p3*p2*p1*p0*c0
c8 =  g7  + p7*g6 	+ p7*p6*g5    + p7*p6*p5*g4     + p7*p6*5*p4*c4
c12 = g11 + p11*g10	+ p11*p10*g9  + p11*p10*p9*g8   + p11*p10*p9*p8*c8
c16 = g15 + p15*g14	+ p15*p14*g13 + p15*p14*p13*g12 + p15*p14*p13*p12*c12
```

也可以先做个四位的ALU再把他们串联(均为1位

```pseudocode
G0= g3  + p3*g2   + p3*p2*g1    + p3*p2*p1*g0
G1= g7  + p7*g6   + p7*p6*g5    + p7*p6*p5*g4
G2= g11 + p11*g10 + p11*p10*g9  + p11*p10*p9*g8
G3= g15 + p15*g14 + p15*p14*g13 + p15*p14*p13*g12

P0= p3*p2*p1*p0
P1= p7*p6*p5*p4
P2= p11*p10*p9*p8
P3= p15*p14*p13*p12

C1 = c4  = G0+P0*c0
C2 = c8  = G1+P1*c4
C3 = c12 = G2+P2*c8
C4 = c16 = G3+P3*c12

C1=G0+P0*c0
C2=G1+P1*C1 = G1+P1*G0 + P1*P0*c0
C3=G2+P2*C2 = G2+P2*G1 + P2*P1*G0+ P2*P1*P0*c0
C4=G3+P3*C3 = G3+P3*G2 + P3*P2*G1+P3*P2*P1*G0 + P3*P2*P1*P0*c0
```

# 运算

## 乘法

### Ver1

第一个操作数是被乘数(multiplicand)，第二个是乘数(multiplier)

64ALU；64cand左移，32ier右移，64积不动；ier低位为0则Prod+=cand

<img src="assets/image-20200908163312048.png" style="zoom:33%;" /><img src="assets/image-20200908163427099.png" style="zoom:33%;" />

### Ver2

32ALU；32cand不动，32ier右移，64积右移；ier低位为1则Prod[high half]+=cand

<img src="assets/image-20200908163621411.png" style="zoom: 9%;" /><img src="assets/image-20200908163644722.png" style="zoom:33%;" />

### Ver3

32ALU；32cand不动，64积(初值为{32'b0, ier})右移；ier低位为1则Prod[high half]+=cand

习题3.13

<img src="assets/image-20200908164011142.png" style="zoom:9%;" /><img src="assets/image-20200908164027776.png" style="zoom:36%;" />

### signed乘

将符号数转化为非符号数，若两数异号再把结果变为其补码



### Booth算法

PPT65

multiplier连续n位(如a\~b, a<b)为1，则在第a位执行`sub Mcand*2^{a}`，a+1\~b执行sll，b+1位执行`add Mcand*2^{b+1}`(从0开始记位)

与Ver3结合可以做成状态机(最后一位 | 上一次右溢出位)：

* 1 0 	subtract multiplicand from left half
* 1 1 	no arithmetic operation
* 0 1 	add multiplicand to left half
* 0 0 	no arithmetic operation
* ALways Shift Right(sra)

Arithmetic shift right:

* keeps the **leftmost bit constant**
* no change of sign bit !

优势：代数运算少

更慢的特殊情况：0101

一样快：0110

| iteration | step           | Multiplicand | product          |
| --------- | -------------- | ------------ | ---------------- |
| 0         | Initial Values | 0010(2)      | 0000_/1101_0(-3) |
| 1         | 10: sub        | 0010         | 1110_/1101_0     |
|           | sra            | 0010         | 1111_0/110_1     |
| 2         | 01: add        | 0010         | 0001_0/110_1     |
|           | sra            | 0010         | 0000_10/11_0     |
| 3         | 10: sub        | 0010         | 1110_10/11_0     |
|           | sra            | 0010         | 1111_010/1_1     |
| 4         | 11: nop        | 0010         | 1111_010/1_1     |
|           | sra            | 0010         | 1111_1010/_1     |

==**好像有符号数乘法不能用负数当Mcand**==

13 = 0b001101, -21 = 0b101011

Use -21 as Mcand

| Iteration | Step       | Multiplicand | Product/Multiplier |
| --------- | ---------- | ------------ | ------------------ |
| 0         | Initialize | 101011       | 000000_001101_0    |
| 1         | 10: sub    | 101011       | 010101_001101_0    |
|           | sra        | 101011       | 001010_1\|00110_1  |
| 2         | 01: add    | 101011       | 110101_1\|00110_1  |
|           | sra        | 101011       | 111010_11\|0011_0  |
| 3         | 10: sub    | 101011       | 001111_11\|0011_0  |
|           | sra        | 101011       | 000111_111\|001_1  |
| 4         | 11: keep   | 101011       | 000111_111\|001_1  |
|           | sra        | 101011       | 000011_1111\|00_1  |
| 5         | 01: add    | 101011       | 101100_1111\|00_1  |
|           | sra        | 101011       | 110110_01111\|0_0  |
| 6         | 00: keep   | 101011       | 110110_01111\|0_0  |
|           | sra        | 101011       | 111011_001111\|_0  |

Use 13 as Mcand

| Iteration | Step       | Multiplicand | Product/Multiplier |
| --------- | ---------- | ------------ | ------------------ |
| 0         | Initialize | 001101       | 000000_101011_0    |
| 1         | 10: sub    | 001101       | 110011_101011_0    |
|           | sra        | 001101       | 111001_1/10101_1   |
| 2         | 11: keep   | 001101       | 111001_1/10101_1   |
|           | sra        | 001101       | 111100_11/1010_1   |
| 3         | 01: add    | 001101       | 001001_11/1010_1   |
|           | sra        | 001101       | 000100_111/101_0   |
| 4         | 10: sub    | 001101       | 110111_111/101_0   |
|           | sra        | 001101       | 111011_1111/10_1   |
| 5         | 01: add    | 001101       | 001000_1111/10_1   |
|           | sra        | 001101       | 000100_01111/1_0   |
| 6         | 10: sub    | 001101       | 110111_01111/1_0   |
|           | sra        | 001101       | 111011_101111/_1   |

上一种方法第五位差了个1。。。

## 除法

64被除数存到64余数里

### Ver1

32商左移，64余数不动，64除数(初值为`{32位除数, 32'b0}`)右移，64位ALU

做减法，余数-=除数比大小，若大于0则ok，商左移置1；小于0则rollback，商左移置0

<img src="assets/image-20200908172402957.png" style="zoom: 33%;" /><img src="assets/image-20200908172412772.png" style="zoom:33%;" />

### Ver2

32商左移，64余数左移，32除数不动，32位ALU

做减法，余数高32-=除数比大小，若大于0则ok，商该位置1；小于0则rollback(余数高32+除数)

<img src="assets/image-20200908172436437.png" style="zoom:33%;" />

### Ver3

省掉商寄存器

64余数左移，32除数不动，32位ALU

做减法，余数高32-=除数比大小，若大于0则将左移，低位平行置位1；小于0则rollback并左移，低位平行置位0

开始全部左移一位；

结束时高位右移一位，商在余数的低32位

<img src="assets/image-20200908172450076.png" style="zoom:33%;" /><img src="assets/image-20200908172458278.png" style="zoom:39%;" />

```
39/8 = 0010 0111 / 1000 = 0111|0100
```

| Round                 | Divisor | Remainder |
| --------------------- | ------- | --------- |
| 0                     | 1000    | 0010 0111 |
| <<+0                  |         | 0100 1110 |
| 1: <0                 |         | ...       |
| <<+0                  |         | 1001 1100 |
| 2: >0                 |         | 0001 1100 |
| <<+1                  |         | 0011 1001 |
| 3: <0                 |         | ...       |
| <<+0                  |         | 0111 0010 |
| 4: <0                 |         | ...       |
| <<+0                  |         | 1110 0100 |
| Done: shift high left |         | 0111 0100 |



## 浮点数

Standardized format IEEE 754

* Single precision 8 bit exp, 23 bit significand
* Double precision 11 bit exp, 52 bit significand

exp和significand两个是符号数，但不是补码，是偏码

Bias127(127偏码)：0表示-127，127表示0，254表示+127，255是特殊位

32位计算偏码是~~`-0x80000000`~~ -0x7FFFFFFF

$\Large \rm (-1)^{sign}\times(1+significand)\times 2 ^{exp - bias}$

1+significand(=fraction，尾数)是二进制，即整个浮点数实际上是<u>用**二进制**的科学计数法表示的小数</u>

**特殊的**：NaN，+\infin, -\infin, 0

exp和significand都为0：0(那1怎么办？1是2^0^，指数是01111111(偏码))

exp全为1，significand全为0：$\pm\infin$

exp全为1，significand不为0：其他(Not a Number)

最小的inf：2^128^；最小的非0：2^-149^；最小的指数位非0：2^-126^



例如-0.75为$\large\rm (-1)^1 \times (1 + 0.1000\ 0000\ 0000\ 0000\ 0000\ 000)\times 2^{(126 - 127)}$，所以exp为126，指数为0x4000000

```cpp
#include <cmath>
#include <iomanip>
#include <iostream>

std::string F2B(void *pa) {
    std::string out = "";
    int tmp         = *(int *)pa;
    int a           = tmp;
    for (int i = 0; i < 32; i++) {
        if (a & 0x80000000)
            out += "1";
        else
            out += "0";
        if (i == 0 || i == 8)
            out += " ";
        a = a << 1;
    }
    return out;
}

void printLine(float a) {
    std::cout << std::setw(12) << a << ": " << F2B(&a) << "\n";
}

int main(int argc, char const *argv[]) {
    printLine(0);
    printLine(pow(2, -0b01111111));
    printLine(1);
    printLine(pow(0xFFFFFFFFFFFFF, 20));
    printLine(pow(-0xFFFFFFFFFFFFF, 21));
    printLine(1.0 / 0.0);
    printLine(0.0 / 0.0);
    printLine(nanf(nullptr));

    printLine(pow(2, 126));
    printLine(pow(2, 127));
    printLine(pow(2, 128));
    printLine(pow(2, -126));
    printLine(pow(2, -127));
    printLine(pow(2, -149));
    printLine(pow(2, -150));
    return 0;
}

/* Output
           0: 0 00000000 00000000000000000000000
 5.87747e-39: 0 00000000 10000000000000000000000
           1: 0 01111111 00000000000000000000000
         inf: 0 11111111 00000000000000000000000
        -inf: 1 11111111 00000000000000000000000
         inf: 0 11111111 00000000000000000000000
         nan: 0 11111111 10000000000000000000000
         nan: 0 11111111 10000000000000000000000

 8.50706e+37: 0 11111101 00000000000000000000000
 1.70141e+38: 0 11111110 00000000000000000000000
         inf: 0 11111111 00000000000000000000000
 1.17549e-38: 0 00000001 00000000000000000000000
 5.87747e-39: 0 00000000 10000000000000000000000
  1.4013e-45: 0 00000000 00000000000000000000001
           0: 0 00000000 00000000000000000000000
*/
```



```c++
int main() {
    using namespace std;
    float f = -23.3125;
    std::cout << std::hex << *reinterpret_cast<uint32_t *>(&f);
}
```







**单精度**

| 31符号位 | 30    ……     23 exp | 22        ……          0      |
| -------- | ------------------- | ---------------------------- |
| 1        | 0111 1110           | 110 0000 0000 0000 0000 0000 |
| 1 bit    | 8 bits              | 23 bits                      |

**双精度**

| 31符号位 | 30     ……    20 exp | 19      ……           0   | 31 ... ... 0 Also fraction |
| -------- | ------------------- | ------------------------ | -------------------------- |
| 1        | 011 1111 1110       | 1100 0000 0000 0000 0000 |                            |
| 1bit     | 11 bits             | 20 bits                  | 32 bits                    |

### 浮点加法

假设只能存储4个十进制有效数和2个十进制指数

<img src="./assets/floatplus.png" style="zoom:50%;" />

y=0.5+(-0.4375) in binary

```perl
0.5     =  1.0002×2^-1  
-0.4375 = -1.1102×2^-2  
Step1:The fraction with less exponent is shifted right until matching
-1.110×2-2  → -0.111×2-1 
Step2:  Add the significands
       1.000×2-1 
  +) - 0.111×2-1
  -------------------
       0.001×2-1
Step3:  Normalize the sum and check for overflow or underflow
      0.001×2-1 → 0.010×2-2 → 0.100×2-3 → 1.000×2-4 
Step4: Round the sum
	 1.0002×2-4   = 0.062510
```

存在加法器不够大的可能，因为是以大的数字进行align的

存在浮点数吞吃：如10^6^+1，1极有可能被吞掉；还有$\displaystyle\sum_{i=1}^{100,000} \frac1x$会溢出，$\displaystyle\sum_{i=100,000}^{1} \frac1x$就不会

### 浮点乘法

指数直接相加即可，注意若是偏码表示的指数，需在结果上减去一个偏阶(如127、0x1FFFFFFF)

有效数也直接相乘，小数点仍在第一位后面(两个)

### 浮点除法

### Rounding



### Keyword

**Sign Magnitude**：符号表示法，如第一位放0/1，后面保留原码(-24 = 1001 1000)

**Biased Notation**：偏码表示法：如Bias127中0就是-127



浮点加法不符合结合律

(A+B)+C != A+(B+C)

T1 = A+B, T2 = T1 + C 不能保证 T2 == A + B + C



**3.13**

额外多保留两位guard和round

guard保护位：多保留的第一位，用于提高舍入精度

round舍入位：多保留的第二位，使中间结果满足浮点格式？(例如乘法结果有可能第一位为0(前导0)，这样就必须将结果左移一位，此时的guard就是移位前的round)

sticky：只要舍入位右边还有1，stk就为1，这样才能发现0.5和0.50000001之间的区别

```perl
3.13.1 (A+B)+C=
(A)  -1.1111111010*213+1.1111111010*213+1.0*20=1

(B)    (A+B)+C=
    1.1100101010*2^4+1.1010|100010*2^-2 + 1.1000010010*2^3

    1.1100101010
   +0.0000011010|101 (多的三位寄存器保存对齐过程的数据 Guard=1, round=0, Sticky=1) 
   =1.1101000100
     (No round)
 
3.13.2  A+(B+C)= A+1.1111111010*213+1.0*20
      (B+C)= 
      1.111111010
     +0.0000000000 (Guard=0, Round=0, Sticky=1) 
     =1.1111111010
        (No round)

3.13.2.b  A+(B+C)= 1.0100100110 11
      1.1000010010
     +0.0000110101 000 (Guard=0, round=0, Sticky=0)
     =1.1001000111   NO ROUND
  A:   1.1100101010 
C+B:   0.1100100011 10 (Guard=1, round=0, Sticky=0)
      10.1001001101 10   NORMALISE
       1.0100100110 110 (Guard=1,round=1, Sticky = 0)
       1.0100100111        ROUND UP
```



**nearest**

[Lecture notes - Floating Point Arithmetic](https://lost-contact.mit.edu/afs/cs.wisc.edu/sunx86_57/test_image/u/a/n/andrew/public/cs354/beyond354/arith.flpt.html)

总结：

```
00 下
01 下
10 下
11 上
```



```
Method 4.  round to nearest
use representation NEAREST to the desired value.
This works fine in all but 1 case:  where the desired value
is exactly half way between the two possible representations.

The half way case:
1000...   to the right of the number of digits to be kept,
then round toward nearest uses the representation that has
zero as its least significant bit.

Examples:

        1.1111   (1/4 of the way between, one is nearest)上
          |
  1.11    |    10.00
               ------


        1.1101   (1/4 of the way between, one is nearest)下
          |
  1.11    |    10.00
 ------


        1.0010  (the case of exactly halfway between)下
          |
  1.00    |    1.01
 -----


       -1.1101  (1/4 of the way between, one is nearest)下(负上)
          |
 -10.00   |    -1.11
               ------


       -1.0010  (the case of exactly halfway between)下(负上)
          |
 -1.01    |   -1.00
              -----

NOTE: this is a bit different than the "round to nearest" algorithm
(for the "tie" case, .5) learned in elementary school for decimal numbers.
```



**nearest even**

```
0|00 0
0|01 0
0|10 0
0|11 0
1|00 0
1|01 2
1|10 2
1|11 2
```



[ieee 754 - Rounding Floating Point Numbers after addition (guard, sticky, and round bits) - Stack Overflow](https://stackoverflow.com/questions/19146131/rounding-floating-point-numbers-after-addition-guard-sticky-and-round-bits)

[rounding - Guard, round, sticky bits (floating point) - Stack Overflow](https://stackoverflow.com/questions/45662113/guard-round-sticky-bits-floating-point)

```
  1.001 * 2^2
+
  1.0100,0000,0000,0000,0000,011 * 2^1
(=0.1010,0000,0000,0000,0000,0011 * 2^2)
=
  1.1100,0000,0000,0000,0000,0011 * 2^2(GRS=100, nearest even)
=
  1.1100,0000,0000,0000,0000,010 * 2^2
```

所以nearest even就是要让最后一位为0？