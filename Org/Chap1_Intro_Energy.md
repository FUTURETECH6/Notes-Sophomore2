

==**内容补充**(第五版书中没有)==

* 加法器进位方法
* Booth算法
* 微程序



一个指令用一个微程序实现







> 1. 磁道坏了，怎么办
>
>     ```
>     隔离
>     ```
>
> 2. 哪个磁盘最快
>
>     1. C 盘快还是 E 盘快
>
>     ```
>     不一定，取决于是否是同一种存储介质
>     ```
>
> 3. 怎么设计 undelete 软件
>
>     ```
>     重建索引？
>     ```
>
> 4. 硬盘格式化为什么可以恢复
>
>     1. 手机通讯录格式化了，怎么恢复
>
>     ```
>     数据没有被覆盖
>     用恢复软件
>     ```
>
> 5. flash 是 U 盘、 SD 、 SSD 的基础器件，读写 10-100 万次就坏了，为什么能作为笔记本硬盘
>
>     ```
>     体积小；
>     并且可以通过将一块flash分成多块，遇到坏了的就隔离来规避风险
>     ```
>
> 6. 硬盘容量一样，性能一样么? 
>
>     ```
>     不一样，速度不一样
>     ```
>
> 7. 硬盘灯为啥经常闪
>
>     ```
>     在进行读写操作
>     ```



定义性能：

* **Response time** (execution time): the total time required to complete a task
* **Throughput** (bandwidth): number of tasks completed per unit time

例如ATM机用户操作间隔长，就不需要Throughput来衡量



CPU(执行)时间(CPU exe time)：执行某一任务在CPU上所花费的时间

* 用户CPU时间：程序本身
* 系统CPU时间：操作系统



时钟周期：**clock cycle**, **clock rate**, **tick**, clock tick, clock period 





1. program的CPU执行时间=CPU clock cycles(周期数) for a program * clock cycle time
2. CPU clock cycles(周期数) for a program = Instructions for a program(指令数) * average clock cycles per instruction (CPI)
3. CPU time = Instruction count(指令数) * CPI * Clock cycle time

$\Large \rm \frac{秒数}{程序}=\frac{指令数}{程序} \times \frac{时钟周期数}{指令数}(CPI) \times \frac{秒数}{时钟周期数}(clk\_time)$



Static 能耗来源：occurs because of leakage current that flows even when a transistor is off.

Dynamic能耗来源：energy that is consumed when transistors switch states from 0 to 1 and vice versa.

**SP = IU**

**DP = CU^2^f/2**	(==到底有没有2==)



Frequency ∝ Clock_Rate

Dynamic Energy(能耗，非功耗) ∝ Capacitive load(负载电容，The capacitive load per transistor is a function of both <u>the number of transistors connected to an output</u> (called the <u>fanout</u>) and <u>the technology</u>, which determines the capacitance <u>of both wires and transistors</u>.) ×􏰁 Voltage^2^

> $\rm E = qU/2 = \frac{CU^2}{2d}{}$

0 → 1 → 0 or 1 → 0 → 1 的Energy(方波) ∝ 1/2 􏰁 Capacitive load ×􏰁 Voltage^2^

single transistor(单个晶体管)的Power(功耗) ∝ 1/2 ×􏰁 Capacitive load 􏰁× Voltage^2^ ×􏰁 **Frequency switched**(开关频率，is a function of the clock rate)



static power与dynamic power



作业 1.5；1.8