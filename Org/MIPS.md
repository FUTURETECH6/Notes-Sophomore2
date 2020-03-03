```assembly
add tar,b,c	#

```

32个寄存器：`$s0–$s7, $t0–$t9, $zero, $a0–$a3, $v0–$v1, $gp, $fp, $sp, $ra, $at`

**常见指令，P87**

```assembly
注意操作数位置与意义的对应
beq $s1,$s2,25	#if ($s1 == $s2) go to PC + 4 + 100
slt $s1,$s2,$s3	#if ($s2 < $s3) $s1 = 1; else $s1 = 0
```

