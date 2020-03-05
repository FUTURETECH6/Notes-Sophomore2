



MIPS **convention** for registers
* \$s0, \$s1, ... for registers corresponding to variables in C
* \$t0, \$t1, ... for temporary registers



==特别注意位置！！==

==Load：reg $\Longleftarrow$ memory==：`lw $to, 32($s3)`

==Store：reg $\Longrightarrow$ memory==：`sw $to, 48($s3)`





基址寄存器：`lw $t0, 8($s3)	# word $s3[2]`==这个偏移量必须是常量，也不能不加(原址也得加个0)==

`word`到底是多大与指令集有关：<u>**MIPS的一个word是4字节**</u>

要用变址得求出要用的地方的地址：

```assembly
# g = h + A[i];
# g, h, i ---- $s1, $s2, $s4; base address of A ---- $s3
add $t1, $s4, $s4	# temp reg $t1 = 2 * i
add $t1, $t1, $t1	# temp reg $t1 = 4 * i
add $t1, $t1, $s3	# $t1 = address of A[i] (4 * i + $s3)
lw  $t0, 0($t1)		# temp reg $t0 = A[i]
add $s1, $s2, $t0	# g = h + A[i]
```





### Memory Alignment

不足1字补到1字

```c
struct{
    int a;
    char b;
    char c[2];
    char d[3];
    float e;
}
```

|      |        |        |        |
| ---- | ------ | ------ | ------ |
| e    | e      | e      | e      |
| d[0] | d[1]   | d[2]   | No Use |
| c[0] | c[1]   | No Use | No Use |
| b    | No Use | No Use | No Use |
| a    | a      | a      | a      |

为什么不弄成连续？因为这样存有可能一个变量(如e)要被分到多个word，因为总线一次只能读一个word，因此e不能被一次读出



## Stack

栈：从高地址往低地址(临时)；堆：从低地址往高地址(长用)

汇编中可以越界操作不会报错



## MIPS字段格式

**(不同类型格式不同但长度相同)**

* Reg型

    |        | op   | rs   | rt   | rd   | shamt | funct |
    | ------ | ---- | ---- | ---- | ---- | ----- | ----- |
    | bits： | 6    | 5    | 5    | 5    | 5     | 6     |
    | add    | 0    |      |      |      |       | 32    |

    当只有一个源寄存器时优先使用rt？
    Ex. 

* Immediate

    | op   | rs   | rt目的 | const or address |
    | ---- | ---- | ------ | ---------------- |
    | 6    | 5    | 5      | 16               |


