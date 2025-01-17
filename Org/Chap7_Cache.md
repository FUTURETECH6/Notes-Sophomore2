[TOC]

* SRAM：Cache(比寄存器慢
* DRAM：Mem

| Memory  technology | Typical  access time   | Cost  per GByte (2004) |
| ------------------ | ---------------------- | ---------------------- |
| SRAM               | 0.5-5ns                | \$4000-\$10,000        |
| DRAM               | 50-70ns                | \$100-\$200            |
| Magnetic  disk     | 5,000,000-20,000,000ns |                        |

# 层次化设计

* 时间相关性(temporal locality)
    * 一个东西被用了，大概率过一会会被再用
    * 循环结构
* 空间相关性(spatial locality)
    * 一个东西被用了，周围的东西大概率过一会会被再用
    * 顺序

Our initial focus:  two levels (upper, lower)

* block: minimum unit of data (block) for transfers
* hit: data requested is in the upper level(比如在Cache中hit了就不用去Memory找)
    * **Hit Time**: The time to access the upper level of the memory hierarchy, which includes the time needed to determine whether the access is a hit or a miss. 在upper找到的时间(包括决定是否hit的时间)
* miss: data requested is not in the upper level
    * DMC方法中Cache中所需的单元格被占用了也叫miss
    * **Miss Penalty**: The time to replace a block in the upper level with the corresponding block from the lower level, plus the time to deliver this block to the processor. 
    * ==miss也需要加上hit time：如90%hit，10%miss，总时间为：HitTime\*100% + MissPenalty\*10%==

```mermaid
graph LR
subgraph CPU
	Reg
	Cache_L1
end
Reg
-->Cache_L1
-->Cache_L2
-->Memory
-->Disk
```

# Cache

## Mapping Method

首先两个问题

* 如何知道数据在不在里面？
* 如果知道不在里面，要怎么找？要用哪个位置来访导入的数据

==Index位数全部是根据Set(显式或隐式)而不是Block来算的，offset是byte不是word==

### Direct Mapped(2L)

**分块**：例如1024Word(4Byte/Word)的内存，分为256块，则第40个Byte的主存地址为000010(块号)\[10(块内字号)00(字内地址)\](偏移地址)

Memory中的整个Block的内容被全部存在Cache中

`Tag = BlockAddr_in_Memory // BlockNum_in_Cache`

`CacheIndex = BlockAddr_in_Memory % BlockNum_in_Cache`

`Offset = ByteAddr_in_Memory % BlockSize`

<img src="assets/image-20200415110455204.png" style="zoom:33%;" />

虽然容易不均匀，但是速度非常快(访问一次即可)



<img src="assets/image-20200415111200115.png" style="zoom:33%;" />

Tag是指示要找的东西的Memory对应在Block中的Byte地址(例如下面例题中就是4位的)

Index是指示Block地址的(例如下面例题中就是10位的)

`{tag, index}`合起来就是实际的地址(必须是按Byte而不是Word)

`Addr_bit = tag_bit + block_bit + byteOffset_bit + valid_bit`

<img src="assets/image-20200415111451881.png" style="zoom: 40%;" />

<img src="assets/image-20200415111248532.png" style="zoom: 40%;" />

> How many total bits are required for a direct-mapped **cache** 16KB of data and 4-word blocks, assuming a 32-bit address?
>
> * ~~16KB=4KWord=2^12^ words~~
> * ~~One block=4 words = 2^2^  words~~ 
> * Number of blocks (index bit) = 2^12^ ÷ 2^2^ = 2^10^ blocks(Cache中的BlockNum和IndexSize是相等的，因此index是10位的)
> * Data bits of block =4×32=128 bits\
> * <u>Tag bits  = address - index - blockSize(byte offset width) =32 - 10 - 4 =18 bits</u>  ？？(这个并不能由其他的算出来，因为没告诉你映射到的Memory多大，只能用地址线的位数来反推，由此再推出Memory最大能到 2^18^ *16KB = 4GB) ==Valid和Dirty不算在地址位里==
> * Valid bit = 1 bit
> * Total Cache size = 2^10^ × (128+18+1)= 2^10^×147= 147 Kbits= 18.4KB
> * It is about [1.15](147/128=18.4/16) times as many as needed just for the data

**特点**

* Cache利用率低
* **替换速度快**
* **块冲突率高**
* 淘汰算法简单
* 适合于大容量Cache[低利用率、淘汰不至于太复杂]

Ex. 1-word Block

<img src="assets/image-20200421200008644.png" style="zoom:33%;" />

### Fully-Associative(1L)

**分块**：例如1024Word(4Byte/Word)的内存，分为256块，则第61个Byte的主存地址为0011(块号)1101(偏移地址)

Tag直接存储`BlockAddr_in_Memory`，🈚️Index (因此扫描巨慢

**特点**

* Cache利用率高
* **替换速度慢**
* **块冲突率低**
* 淘汰算法复杂
    * Random, LRU, FIFO
* 适合于小容量Cache[高利用率、不至于因为淘汰复杂而导致时间很慢]

Ex. 1-word Blocks(Assume cache has 4 blocks)

<img src="assets/image-20200421200051843.png" style="zoom:33%; " />

### Set-Associative(3L)

一个Cache多组，每组多块

`SetIndex_in_Cache = SetIndex_in_Memory % SetNum_in_Cache`

\\                                     `= (BlockAddr_in_Memory / BlockNum_in_Set) % SetNum_in_Cache`

**n-way set associative**：一个set有n个block

Ex. 2-Way Set-Associative Cache(1 word/block; 2 blocks/set; 4 blocks/cache; hence 2 sets per cache)

<img src="assets/image-20200421200135739.png" style="zoom:33%;" />

### 映射方式区别

[映射方式对性能的影响](# 映射方式对性能的影响)

* DM：浙江代表只能住201
* FA：浙江代表随便住
* SA：浙江代表给5个房间
    * 即：任意mod组数为k的都可以进到index=k的set，然后再看要去替换这个set里的哪个block

---

<img src="assets/image-20200417090807882.png" style="zoom: 50%;" />

### 物理地址

| Block_Addr      | Block_Addr     |                  |
| --------------- | -------------- | ---------------- |
| Tag             | Index          | offset           |
| 第几个set/block | (Index of Set) | 块内的byte index |
|                 | Index of Block |                  |

* The **Index** field selects
    * The set, in case of a set-associative cache
    * The block, in case of a direct-mapped cache
    * Has as many bits as `log2(#sets)` for set-associative caches, or `log2(#blocks)` for direct-mapped caches
    * ==Index位数全部是根据Set(显式或隐式)而不是Block来算的==
* The **Byte Offset** field selects
    * The byte within the block
    * Has as many bits as `log2(size of block)`
* The **Tag** is used to find the matching block within a set or in the cache
    * Has as many bits as `Address_size – Index_size – Byte_Offset_Size`



#### ※Ex.

CacheSize = 64KB; 4 words/block; 4 bytes/word; physical address: 32bits

|                                       | tag  | index                 | offset |
| ------------------------------------- | ---- | --------------------- | ------ |
| Directed-Mapped (1-way ass)           | 16   | 12 (1 block对应1 set) | 4      |
| Fully-Associative (block_num-way ass) | 28   | 0 (只有1个set)        | 4      |
| 2-way associative                     | 17   | 11                    | 4      |
| 4-way associative                     | 18   | 10                    | 4      |



## Cache R/W

### Handling Read

先找对应index的，如果V是低电平或者tag中的内容不对，则是miss

**Read miss**(L1用哈佛，因为混在一起miss概率会加大)

* instruction cache miss
* data cache miss

**instruction cache miss**：4steps

1. **stall(挂起) the CPU**: Send the original PC value (current PC-4) to the memory. (等你去内存找回来我可以先去完成其他一堆程序了)
2. **fetch block from memory**: Instruct main memory to perform a read and wait for the memory to complete its access.
3. **deliver to cache**(这步别漏了): Write the cache entry, putting the data from memory in the data portion of the entry, writing the upper bits of the address (from the ALU) into the tag field, and turning the valid bit on.
4. **restart CPU read**: Restart the instruction execution at the first step, which will <u>refetch the instruction again</u>, this time <u>finding it in the cache</u>. (Read Hit)

冷启动：开机的时候，OS为了开机会把Cache全部写满，这时候会很慢；之后OS就在Cache中常驻了因此速度就快了 (论DDR4的重要性

### Handling Write

* Write hits:  Difference Strategy
    * write-through: Ensuring Consistent (总是写到内存，(一写到底through))
        * Write the data into both the memory the cache
        * Strategy ---- writes always update both the cache and the memory
        * **[write buffer](# write buffer)**，可以用这个来稍微减小对速度的影响
        * 不需要dirty，有valid就够了
    * write-back: Cause Inconsistent (之后再写到内存)
        * Wrote the data into only the data cache
        * Strategy ---- write back data from the cache to memory later (later一般是指程序结束之后)
        * Fast，两者相差很大用这种
        * 需要加一个[dirty](# 脏位)位来进行判断
        * **避免误写的方法**：用两个周期(先检查是否命中再写)；弄个写缓冲(在正常周期内将新数据放入缓冲，如果命中了再写入)
* Write misses(写东西，tag对不上):
    * Write allocate (收集)
        * read the entire block from memory into the cache(==Write allocate得先read==), then write the word using \-back or \-through(hit策略)
        * <u>The block is loaded into the cache on a miss before anything else occurs.</u> 
        * 看脏位
            * 1：先把原来东西写到内存，再用新值冲掉
            * 0：没被写过，直接冲
    * Write around (绕开？no write allocate)
        * The block is only written to main memory
        * It is not stored in the cache.
    * In general, **<u>write-back caches use write-allocate</u>**(之后要hit必须cache里有东西；chache和mem差距大), and **<u>write-through caches use write-around</u>**.(无所谓，反正hit了也要写到mem，跟cache没啥关系；mem和cache差距小)

#### 脏位

* The bit indicates that its associated block of cache has been modified and has not been saved to memory yet. [What does 'dirty' mean in the context of caching? - Quora](https://www.quora.com/What-does-dirty-mean-in-the-context-of-caching)
* 为什么需要dirty？因为cache上同一个index位置存储的块的tag一定(对于miss的情况，hit则没有脏位之说了)是不同的
* CPU向Cache上的某Block写过东西之后Block的dirtyBit被置位。之后要更新Cache的时候，如果dirty是1就必须先把当前的写到Memory(此时复位？)再更新，如果是0就直接更新。
* 此处的脏位(Cache->Mem)和虚拟内存中的(Mem->Disk)差不多，就是位置不一样而已



## Replacement

* Random：简单

* FIFO：计算量大，还得有硬件来计算时间

* LRU(Least Recently Used)：计算量大，还得每次都更新谁是最近使用的

    * The most commonly used scheme is least recently used (LRU), which we used in the previous example. In an LRU scheme, the block replaced is the one that has been unused for the longest time.

        For a two-way set associative cache, the LRU can be implemented easily. We could keep a single bit in each set. We <u>set the bit whenever a specific block in the set is referenced</u>, and reset the bit whenever another block is referenced.

        As associativity increases, implementing LRU gets harder.

## Memory Organization

**空间相关性**

。。。。

**三种策略**

<img src="assets/image-20200421214324844.png" style="zoom: 25%;" />

---

假设：1个clk发送地址；15个clk访问初始化 (找到地址)；1个clk传1 word数据；4 words/block, 4 bytes/word

### 1-word-wide

因此1word数据要17clk，传1 block要`Miss Penalty = 1 + 4 ×(1 + 15) = 65clk`(地址只用传一次，为什么？内存管理会自增吗？)

${\rm Bandwidth} 带宽 = \frac{16B}{65clk} \approx \frac14$

<u>带宽表示一个周期能传多少字节的数据</u>

### Wide

2Words的数据宽

需要`Miss Penalty = 1 + 2 ×(15 + 1) = 33clk` (发地址一次，(读+传)两次)

${\rm Bandwidth} 带宽 = \frac{16B}{33clk} \approx 0.48$

4words的数据宽

需要`Miss Penalty = 1 + 1 × (15 + 1) = 17clk` (发地址一次，(读+传)一次)

${\rm Bandwidth} 带宽 = \frac{16B}{17clk} \approx 0.98$

### Four-way interleaved

每次都把其他的准备好

需要`Miss Penalty = 1 + 15 + (4 × 1) = 20clk`(发地址一次，读一次(一次完成四个)，传出四次)

${\rm Bandwidth} 带宽 = \frac{16B}{20clk} \approx 0.8$

> 为什么比Wide好？明明带宽更高？
> \\	因为加总线很贵

注意四个内存块的地址

<img src="assets/image-20200422105323845.png" style="zoom:33%;" />

## Cache性能

### write buffer

与write through配合使用，本来要直接写到主存的东西先写到这，然后处理器继续干活。之后写缓冲会自己写到主存然后释放空间，除非写缓冲满了，处理器必须停下来知道写缓冲有空出一个位置来。另外，需要注意的是如果完成写操作的时间要比指令产生写指令的平均周期长，那再多写缓冲也没用，必然是要爆的。

### Block大小对性能影响

Block👆，Index👇

| Program | Block size in  words | Instruction  miss rate | Data miss rate | Effective  combined miss rate |
| ------- | -------------------- | ---------------------- | -------------- | ----------------------------- |
| gcc     | 1                    | 6.1%                   | 2.1%           | 5.4%                          |
|         | 4                    | 2.0%                   | 1.7%           | 1.9%                          |
| spice   | 1                    | 1.2%                   | 1.3%           | 1.2%                          |
|         | 4                    | 0.3%                   | 0.6%           | 0.4%                          |

### 评估性能

* Miss Penalty影响冷启动速度之类的
* Miss Rate

两种启动方式

* 冷启动慢，但是开机之后都很流畅
* 冷启动快，但是开机之后得不断加载所以会卡[](# 映射方式)

`CPU time = (CPU execution clock cycles + Memory-stall clock cycles) × Clock cycle time`

读操作阻塞的周期：`Read_stall_cycles = (Reads/Program) × Read_miss_rate × Read_miss_penalty`

写操作阻塞的周期：`Write_stall_cycles = [(Writes/Program) × Write_miss_rate × Write_miss_penalty] + Write_buffer_stalls` (写缺失+写缓冲阻塞)

写缓冲区阻塞：`Write_buffer_stalls`：个人理解应该指的是连续多次写操作中，下一次得等上一次写完才可以写。取决于频率和write的时机(比如你要是能保证mem任意时刻写一次的时间之内不会遇到其他的写要求，那就不存在这个问题)，因此没办法量化计算。

存储器阻塞时钟周期数：`Memory-stall clock cycles `

\\            `= (Read-stall cycles + Write-stall cycles)`

\\            `= (Memory_accesses/Program) × Miss_rate × Miss_penalty`(忽略写缓冲区阻塞，共用读写的MissRate和MissPenalty)

\\            `= (Instructions/Program) × (Misses/Instruction) × Miss_penalty`(这第一个括号应该是只考虑内存中的指令)

<u>中文P271例题加深理解</u>

<img src="assets/image-20200909150135343.png" style="zoom:50%;" />

#### ※映射方式对性能的影响

先看[这里](# 映射方式)

Ex. Given the following sequence of block addresses: 0,8,0,6,8, find the number of misses for each cache organization. (each consisting of four one-word blocks. )

**direct-mapped**: 5 misses

| Memory block | Hit or miss | Set 0   | Set 1   | Set 2   | Set 3   |
| ------------ | ----------- | ------- | ------- | ------- | ------- |
|              |             | Block 0 | Block 1 | Block 2 | Block 3 |
| 0            | Miss        | M[0]    |         |         |         |
| 8            | Miss        | M[8]    |         |         |         |
| 0            | Miss        | M[0]    |         |         |         |
| 6            | Miss        | M[0]    |         | M[6]    |         |
| 8            | Miss        | M[8]    |         | M[6]    |         |

**fully-associative**: 3 misses

| Memory block | Hit or miss | Set 0   | Set 0  | Set 0  | Set 0  |
| ------------ | ----------- | ------- | ------- | ------- | ------- |
|              |             | Block 0 | Block 1 | Block 2 | Block 3 |
| 0            | Miss        | M[0]     |         |         |           |
| 8            | Miss        | M[0]     | M[8]    |         |           |
| 0            | Hit         | M[0]     | M[8]    |         |           |
| 6            | Miss        | M[0]     | M[8]    | M[6]    |           |
| 8            | Hit         | M[0]     | M[8]    | M[6]    |           |

**2-way associative**: 4 misses

| Memory block | Hit or miss | Set 0   | Set 0  | Set 1  | Set 1  |
| ------------ | ----------- | ------- | ------- | ------- | ------- |
|              |             | Block 0 | Block 1 | Block 2 | Block 3 |
| 0            | Miss        | M[0]     |         |         |           |
| 8            | Miss        | M[0]     | M[8]    |         |           |
| 0            | Hit    | M[0]     | M[8]    |         |           |
| 6            | Miss        | M[0]     | M[6]    |         |           |
| 8            | Miss        | M[8]     | M[6]    |         |           |

### <!--计算-->

> <!--Assume:-->
>
> * <!--instruction cache miss rate = 2%-->
> * <!--data cache miss rate = 4%-->
> * <!--CPI without any memory stalls = 2-->
> * <!--miss penalty = 100 cycles-->
> * <!--The frequency of all loads and stores in gcc is 36%,as we see in Figure 3.26, on page 288.-->
>
> <!--Question: How faster a processor would run with a perfect cache?-->
>
> <!--Answer:-->
>
> 1. <!--Instruction miss cycles = I×2%×100 =2.00I-->
> 2. <!--Data miss cycles = I×<u>36%</u>×4%×100 =1.44I-->
> 3. <!--Total memory-stall cycles = 2.00I+ 1.44I =3.44I-->
> 4. <!--CPI with stall = CPI with perfect cache + total memory-stalls-->
>     <!--\\                      = (2 + 3.44 )I = 5.44I-->
> 5. <!--CPU_time_with_stall/CPU_time_with_perfect_cache(perfect is ) = (I×CPI_stall×Clock_cycle)/(I×CPI_perfect×Clock_cycle) = CPI_stall/CPI_perfect = 5.44/2 = 2.72-->

## 优化性能

### 降低Miss Rate

### 降低Miss Penalty

#### 多级缓存

Ex. CPI = 1.0(无miss), f = 5GHz, 整体缺失率(此时仅有L1)为2%, 100ns DRAM access; Adding L2 cache, 5ns access time, 整体缺失率(此时变为L1+L2)降为0.5%(是全部的missrante不是L2的)

Miss penalty to main memory is `(100ns)/(0.2ns/clk) = 500clk`

Miss penalty with levels of cache without access main memory is `(5ns)/(0.2ns/clk) = 25clk`

The **CPI** with Two level of cache is `1.0 + primary_stall_per_inst + secondary_stall_per_inst = 1 + 2% × 25 + 0.5% × 500 = 1 + 0.5 + 2.5 = 4.0`

本来是`1 + 2% × 500 = 11.0`，显然快了很多