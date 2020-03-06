****



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

> 为什么不弄成连续？因为这样存有可能一个变量(如e)要被分到多个word，因为总线一次只能读一个word，因此e不能被一次读出



## Stack

栈：从高地址往低地址(临时)；堆：从低地址往高地址(长用)

汇编中可以越界操作不会报错



## MIPS字段格式

**(不同类型格式不同但<u>长度相同(32bit)</u>)**

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

Ex.6

```assembly
lw  $t0, 1200($t1)
add $t0, $s2, $t0
sw  $to, 1200($t1)
```

| op         | rs   | rt   | rd    | shamt | funct  |
| ---------- | ---- | ---- | ----- | ----- | ------ |
| 35(100011) | 9    | 8    | 00000 | 10010 | 110000 |
| 0          | 18   | 8    | 8     | 0     | 32     |
| 43(101011) | 9    | 8    | 00000 | 10010 | 110000 |

注意`lw`和`sw`的op差别很小，这样便于硬件设计

~~????1200的二进制不是0b10 1000 0000吗~~



## 部分汇编指令

<u><b><code>PC</code></b>寄存器指向当前行，类似于x86中的<code>IP</code></u>



```assembly
slt $t0, $s0, $s1		# SetLessThan?
						# $t0 gets 1 if $s0 < $s1 ( a < b)
						# $s0-$s1<0 ,test sign bit =1
bne $t0, $zero, Less	# go to Less if $t0 != 0(that is, if a < b)

# 为什么不直接
blt $s0, $s1, Less
```



**Switch**的汇编

```c
switch (k) {
	case0: f = i + j; break; /*k=0*/
	case1: f = g + h; break; /*k=1*/
	case2: f = g - h; break; /*k=2*/
	case3: f = i - j; break; /*k=3*/
}
```

```asm
# 要点
# Jump with register content
jr $r
# jump address table
$r ← $t4 + 4 * K
```

## 函数调用

Six steps for execution of the procedure 

1. Main program to place parameters in place where the procedure can access them
2. Transfer control to the procedure
3. Acquire the storage resources needed for the procedure (保护过程要用的寄存器)
4. Perform the desired task
5. Return result value to main program
6. Return control to the point of origin (即断点)

**Registers for procedure calling**

\$a0 ~ \$a3: four argument registers to pass parameters

\$v0 ~ \$v1: two value registers to return values

\$ra: one return address register to return to origin point

**Instruction for procedures: jal ( jump-and-link )**

`jal` ProcedureAddress

类似x86中的`call`?get

作用

1. Next instruction address (PC+4 )Save to \$ra (\$31)
2. Procedure Address move to PC(instruction address REG.)
3. return control to the caller using `Jr $ra` (\$31)

**Using more registers**

* Stack: ideal data structure for spilling registers
* Stack Operation :Push, pop ; Stack pointer: \$sp (\$29)

