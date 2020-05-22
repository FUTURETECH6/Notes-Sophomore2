# Intro

**Three characteristics**

* Behavior
    * Input (read once), output (write only, cannot read) ,or storage (can be reread and usually rewritten)
* Partner 对象
    * Either a human or a machine is at the other end of the I/O device, either feeding data on input or reading data on output.
        * 比如说键盘人也不可能摁太快，所以bus不需要太快，但是CPU就不一样了
* Data rate
    * The **peak** rate at which data can be transferred between the I/O device and the main memory or processor.
        * 峰值/平均值



**Throughput**

**Measure of IO bandwidth**

1. How much data can we move through the system in a certain time?
    For example, in many supercomputer applications, most I/O requires are for long streams of data, and transfer bandwidth is an important characteristic.
2. How many I/O operations can we do per unit of time? 
    For example, National Income Tax Service mainly processes <u>large number of small files</u>.
    * Response time (e.g., workstation and PC)
    * Both throughput and response time (e.g., ATM, FileServer, WebServer)

# Disk

* floppy disks
* hard disks
    * larger
    * higher density
    * higher data rate
    * more than one platter



**The organization of hard disk**

* platters(盘面): disk consists of a collection  of platters, each of which has two recordable disk surfaces
    * tracks(磁道): each disk surface is divided into concentric circles
        * sectors(扇区): each track is in turn divided into sectors, which is the smallest unit that can be read or written
* cylinder(柱面)：在所有盘面的某一给定位置下，所有读写头下面的轨道所形成的的柱面



<u>**Disk Read Time**</u>

* Seek time：把磁头定位到正确的磁道
    * 一般使用avg，且实际avg会是厂商手册的25%\~33%
    * C盘在磁头开始移动的位置，字母越大越远
* Rotational latency：寻道后，等待扇区转过来的时间
    * 使用avg，即<u>转半圈所需时间</u>
* Transfer time：传输一块(一般是一个sector，新磁盘是多个sectors)所需的时间
* Controller time：磁盘内部的IO控制器消耗的世界

Ex.

Access Time = Seek time + Rotational Latency + Transfer time + Controller Time

\\                       = 6ms + 0.5/10,000RPM +0.5KB/(50MB/sec) + 0.2ms = 9.2ms

## D, R, A

**Dependability Reliability Availability** 可依赖性，可靠性，可用性

> Def: Computer system **dependability** is the quality of delivered service such that reliance can justifiably be placed on this service. The service delivered by a system is its observed actual behavior as perceived by other system(s) interacting with this system’s users. Each module also has an ideal specified behavior, where a service specification is an agreed  description of the expected behavior. A system failure occurs when the actual behavior deviates from the specified behavior.



**Measure of Availability**

* MTTF(Mean Time To Failure)：磁盘平均故障时间
* MTTR(Mean Time To Repair)：服务中断修理时间
* MTBF(Mean Time Between Failures) = MTTF+ MTTR
* 可用性Availability=MTTF/(MTTF+MTTR)

**Three way to improve MTTF**

* Fault avoidance:
    preventing fault occurrence by construction(构建技术)
* Fault tolerance:
    using redundancy(冗余技术) to allow the service to comply with the service specification despite faults occurring, which applies primarily to <u>hardware faults</u>
* Fault forecasting:
    predicting the presence and creation of faults, which applies to hardware and software faults



## RAID

Redundant Arrays of Inexpensive Disks(Take *4 data disks* for example)

|       |                                              | 备注                                                         | CheckDisk                  | FailSurv？？ | Use    |
| ----- | -------------------------------------------- | ------------------------------------------------------------ | -------------------------- | ------------ | ------ |
| 0     | Non-redundant striped(条带化)                | 把数据分散到多个磁盘(条带化)                                 | 0                          | 0            | Widely |
| 1     | Mirroring                                    | 镜像拷贝                                                     | 4                          | 1            |        |
| ~~2~~ | ~~Memory-style ECC~~                         | 弃用                                                         | 3                          | 1            |        |
| 3     | Bit-interleaved(交叉) parity                 | 每次都要读所有盘                                             | 1                          | 1            |        |
| 4     | Block-interleaved parity                     | 小数据量读不必访问所有盘？？？，但是每次写都要更新校验盘(减速) | 1                          | 1            |        |
| 5     | Block-interleaved distributed(分布式) parity | 将校验信息分布到所有盘                                       | 1 (那为什么还要校验盘？？) | 1            | Widely |
| 6     | P+Q redundancy                               | 每个校验盘对数据盘和另外一个校验盘进行奇偶校验               | 2                          | 2            |        |

\*parity：奇偶校验



**RAID**

* 主要目的：用多个小磁盘替代掉一个大磁盘

* 弊端
    * Array Reliability of N disks = Reliability of 1 disk / N，例如72个6年的组在一起变成1个月，因此需要经常更换
* 特点
    * 文件条带化：
    * Availability很高：因为有冗余，即使某个部件坏掉了都能继续用
    * 容易坏：如上
    * 数据可以从阵列中冗余的部分恢复
        * Capacity penalty to store
        * Bandwidth penalty to update

### Level

[Standard RAID levels - Wikipedia](https://en.wikipedia.org/wiki/Standard_RAID_levels)

* **RAID0**
    * 只是把数据分散到多个磁盘(条带化)，没有redundancy
* **RAID1**
    * 完全拷贝
    * 最昂贵
    * 01和10的区别
* **RADI3**
    * 按字节进行条带化？
        * 无论多大规模的IO都需要所有盘
    * 对所有盘，按位进行奇偶校验存在checkDisk
        * 如果一个数据盘出错了，subtract P from sum of other disks to find missing information
    * 对RAID4的Inspiration
        * RAID 3 relies on parity disk to discover errors on Read
        * Every sector has an error detection field
        * Relies on error detection field to catch errors on read, not on the parity disk
        * Allows independent reads to different disks simultaneously
* **RAID4**
    * 按块进行条带化
    * 允许并行发生多个数据读取
    * 小数据量读不必访问所有盘
    * 小数据量写也不必访问所有盘
        * 只需要在目标数据盘上写，并利用旧的Check盘更新奇偶校验(`check' = (data xor data') xor check`)
    * 但是写操作不能并发进行：Writes to DataDisk also write to P disk
* **RAID5**
    * 支持了写操作的并发进行
        * 每个block的奇偶校验放在不同盘上了
        * 所以是否相当于每个盘都是数据盘？
* **RAID6**
    * 两个校验盘：每个校验盘对数据盘和另外一个校验盘进行奇偶校验

#### 图示

<img src="assets/2560px-RAID_3.svg.png" style="zoom:10%;" />

*注：一个颜色一个block，一个小块一个byte*

<img src="assets/2560px-RAID_4.svg.png" style="zoom:10%;" />

<img src="assets/2560px-RAID_5.svg.png" style="zoom:10%;" />

<img src="assets/2880px-RAID_6.svg.png" style="zoom:10%;" />

### Tech

* Disk Mirroring, Shadowing (RAID 1)
    * Logical write = two physical writes
* Parity Data Bandwidth Array (RAID 3)
* High I/O Rate Parity Array (RAID 5)
    * Logical write = 2 reads>？？ + 2 writes

Hot swapping：在系统运行时替换某个部件

# Network

Key characteristics of typical networks include the following

* Distance: 0.01 to 10,000 kilometers
* Speed: 0.001MB/sec to 100MB/sec
* Topology: Bus, ring, star, tree
* Shared lines: None (point-to-point) or shared (multidrop)

Type

* Local area network (LAN)
    * e.g., Ethernet
* Packet-switched network ,which are common in long-haul networks 
    * e.g., ARPANET
* TCP/IP is the key to interconnecting different networks
* The bandwidths of networks are probably growing faster than the bandwidth of any other type of device at present.

# Bus, Conc

不能全是专线，否则代价太高了

**Difficulty**

* may be bottleneck
    * 数据多会阻塞
* length of the bus
    * 长度决定了频率
* number of devices
* trade-offs (fast bus accesses and high bandwidth，二者不可兼得，就像公交车人多会使上下车慢)
* support for many different devices
* cost



**Bus transaction**

* Def：一系列的总线操作，包括一个请求，也可能包括一个相应，二者均可能携带数据。一个事务由一个请求发起，可能包括多个独立的总线操作
* include two parts
    1. sending the address
    2. receiving or sending the data
* two operations
    * input: inputting data from the device <u>to memory</u>
    * output: outputting data to a device <u>from memory</u>

## Type

**传输内容**

* **Control lines**, which are used to signal requests and acknowledgments, and to indicate what types of information is on the data lines.
* **Data lines**, which carry information (e.g., **data**(注意地址不是控制总线的), addresses, and complex commands) between the source and the destination.

**连接位置**

* processor-memory 处理器-内存总线
    * (short high speed, custom design)
* backplane 背板总线
    * 使处理器，内存，IO设备能接在单根总线上
    * (high speed, often standardized, e.g., PCI)
* I/O
    * (lengthy, different devices, standardized, e.g., SCSI)
    * 通过pm/bp总线连接到内存上而不直接相连

<img src="assets/image-20200519110342839.png" style="zoom:50%;" />

**标准**

* SCSI (small computer system interface)
* PCI  (peripheral component interconnect)
* IPI   (intelligent peripheral interface)
* IBMPC-AT   IBMPC-XT
* ISA	EISA

**通信方式**

* **Synchronous** bus use a clock and a synchronous protocol, fast and small but every device must operate at same rate and clock skew(时钟偏差) requires the bus to be short
* **Asynchronous** bus don’t use a clock and instead use handshaking
* Handshaking protocol
    * Our example ,which illustrates how asynchronous buses use handshaking, assumes there are 3 control lines.
        * ReadReq(读请求): Used to indicate a read request for memory. The <u>address is put on the data lines</u> at the same time.
        * DataRdy(数据就绪): Used to indicate that data word is now ready on the data lines.
        * Ack(应答): Used to acknowledge the ReadReq or the DataRdy signal of the other party(另一方)

Ex.

![](assets/image-20200519111959047.png)

1. When memory saw the **ReadReq** line, it reads the address from the data bus, starts the memory read operation, then raises **Ack** to tell the device that the ReadReq signal has been seen.
2. I/O device saw the Ack line high and releases the **ReadReq** data lines.
3. Memory sees that ReadReq is low and drops the **Ack** line.
4. When the memory has the data ready, it places the data on the data lines and raises **DataRdy**.
5. The I/O device sees DataRdy, reads the data from the bus , and signals that it has the data by raising **Ack**. 
6. The memory sees Ack signals, drops **DataRdy**, and releases the data lines.
7. Finally, the I/O device, seeing DataRdy go low, drops the **Ack** line, which indicates that the transmission is completed.

也可以用FSM来表示这个过程

## 分配

> *Bus Arbitration* refers to the process by which the current bus master accesses and then leaves the control of the bus and passes it to the another bus requesting processor unit. The controller that has access to a bus at an instance is known as *Bus maste*r.

**Obtaining Access to the Bus**

* “Without any control, multiple device desiring to communicate could each try to assert the control and data lines for different transfers!”
* So,a bus master is needed. Bus masters initiate and control all bus requests.
    * e.g., <u>processor is always a bus master.</u>
* Example: the initial steps in a bus transaction with a single master (the processor). PPT6.46

 **Bus Arbitration**(仲裁)

[BUS Arbitration in Computer Organization - GeeksforGeeks](https://www.geeksforgeeks.org/bus-arbitration-in-computer-organization/)

* <u>Deciding which **bus master**(应该是which device？) gets to use the bus next</u>
* In a bus arbitration scheme, a device wanting to use the bus signals a bus request and is later granted the bus. 
* four bus arbitration schemes:
    * daisy chain arbitration (not very fair):菊花链，阻塞式级联
    * centralized, parallel arbitration (requires an arbiter),  e.g., PCI
    * self selection, e.g., NuBus used in Macintosh
    * collision detection, e.g., Ethernet

# Interface