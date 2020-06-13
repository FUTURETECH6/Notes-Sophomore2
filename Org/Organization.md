```assembly
add tar,b,c	#

```

32个通用寄存器：`$s0–$s7, $t0–$t9, $zero, $a0–$a3, $v0–$v1, $gp, $fp, $sp, $ra, $at`

| Name      | Register number | Usage                                        | Preserved on call? |
| --------- | --------------- | -------------------------------------------- | ------------------ |
| \$zero    | 0               | The constant value 0                         | n.a.               |
| \$at      | 1               | For assembler                                |                    |
| \$v0–\$v1 | 2–3             | Values for results and expression evaluation | no                 |
| \$a0–\$a3 | 4–7             | Arguments                                    | no                 |
| \$t0–\$t7 | 8–15            | Temporaries                                  | no                 |
| \$s0–\$s7 | 16–23           | Saved                                        | yes                |
| \$t8–\$t9 | 24–25           | More temporaries                             | no                 |
| \$k0-\$k1 | 26-27           | For OS                                       |                    |
| \$gp      | 28              | Global pointer                               | yes                |
| \$sp      | 29              | Stack pointer                                | yes                |
| \$fp      | 30              | Frame pointer                                | yes                |
| \$ra      | 31              | Return address                               | yes                |

**常见指令，P87**

```assembly
注意操作数位置与意义的对应
beq $s1,$s2,25	#if ($s1 == $s2) go to PC + 4 + 100
slt $s1,$s2,$s3	#if ($s2 < $s3) $s1 = 1; else $s1 = 0
```

---

sll/srl for logical

`sll $t2, $s0, 3	#sll rd, rt, sa`

---

互为补码的绝对值相加=模

---

ALU的移位运算是怎么实现的？`SLL rd, rt, sa`

全部都算好，用一个32位32路多路选择器来选择最后结果

[Barrel shifter](http://en.wikipedia.org/wiki/Barrel_shifter) 桶式移位器

[cpu - How are shifts implemented on the hardware level? - Stack Overflow](https://stackoverflow.com/questions/10932578/how-are-shifts-implemented-on-the-hardware-level)

---

j和b都是字地址

---

==不管需不需要一定要保护寄存器，反正写了也不吃亏==

保护寄存器套路(一般是在调用前，且是否需要保护参照上面的表)：

```assembly
subi $sp, $sp, 12
sw $s0, 8($sp)
sw $s2, 4($sp)
sw $s5, 0($sp)
```

恢复套路：

```assembly
lw $s0, 8($sp)
lw $s2, 4($sp)
lw $s5, 0($sp)
addi $sp, $sp, 12
```

