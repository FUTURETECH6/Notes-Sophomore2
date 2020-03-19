# 编译

## Link

**FIGURE 2.13** The MIPS memory allocation for program and data

\$pc从`0x400 000`开始向高

\$gp堆顶寄存器(Stack for Static Data)从`0x1000 8000`到`0x1000 0000`

\$sp栈顶寄存器(Stack for Dynamic Data)从`0x7fff fffc`开始往低

Static和Dynamic没有实际的分界，compiler会视具体应用来操作

看书上(中文P127)例题

那道题offset里存的是0xFFFF8000和0XFFFF8020，然后直接溢出完事





## Load

调用动态链接库



## 宏

$at专门给宏用的

<img src = "assets/macro.png">

# 符号拓展

note: `addi` still sign-extends!

note: `sltu, sltiu` for unsigned comparisons

溢出可能的后果：

`-127(1000 0001)+(-2)(1111 1110) = +127(0111 1111)`

常见情况

|      |      |      |
| ---- | ---- | ---- |
| A+B  | ++   | <0   |
| A+B  | --   | \>0  |
| A-B  | +-   | <0   |
| A-B  | -+   | \>0  |

















