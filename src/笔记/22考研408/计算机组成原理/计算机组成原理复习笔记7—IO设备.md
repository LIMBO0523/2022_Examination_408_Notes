# 输入/输出设备

## 概念

I/O设备就是可以将数据输入到计算机，或者可以接收计算机输出数据的外部设备

- 鼠标、键盘――**输入设备**
- 显示器、打印机――**输出设备**
- 硬盘、光盘――**即可输入、又可输出的设备**(有的教材称为:**外存设备**）

I/O接口:又称**I/O控制器（I/O Controller)、设备控制器**，负责协调主机与外部设备之间的数据传输

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210911222039619.png" alt="image-20210911222039619" style="zoom:80%;" />

## I/O控制方式

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210911222130764.png" alt="image-20210911222130764" style="zoom:80%;" />

数据流:键盘→I0接口的数据寄存器→数据总线→CPU某寄存器→主存（变量i的对应位置)

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210911223045201.png" alt="image-20210911223045201" style="zoom:80%;" />

1. **程序查询方式**:CPU不断轮询检查I/O控制器中的“状态寄存器”，检测到状态为“已完成”之后，再从数据寄存器取出输入数据

2. **程序中断方式**:等待键盘I/O时CPU可以先去执行其他程序，键盘I/O完成后I/O控制器向CPU发出中断请求，CPU响应中断请求，并取走输入数据

   > 思考:对于快速I/O设备，如“磁盘”，每准备好一个字就给CPU发送一次中断请求，会导致什么问题?
   >
   > 答:CPU需要花大量的时间来处理中断服务程序，CPU秘用率严重下降。

3. **DMA控制方式**:主存与高速I/O设备之间有一条直接数据通路（DMA总线)。**CPU向DMA接口发出“读/写”命令，并指明主存地址、磁盘地址、读写数据量等参数。**

   DMA控制器自动控制磁盘与主存的数据读写，**每完成一整块数据读写（如1KB为一整块)，才向CPU发出一次中断请求。**<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210911222850079.png" alt="image-20210911222850079" style="zoom:80%;" />

4. 通道控制方式：通道可以识别并执行一系列通道指令，通道指令种类、功能通常比较单一

   <img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210911223221465.png" alt="image-20210911223221465" style="zoom:80%;" />

   1. CPU向通道发出I/O指令。指明通道程序在内存中的位置,并指明要操作的是哪个I/O设备。CPU就可以去做其他事情
   2. 通道执行内存中的通道程序，控制I/O设备完成一系列任务
   3. **通道执行完规定的任务后，向CPU发出中断请求**==（DMA完成一整块数据读写）==，之后CPU对中断进行处理

   > 有的商用中型机、大型机可能会接上超多的I/O设备，如果都让CPU来管理，那么CPU就太累了...
   >
   > 通道是具有特殊功能的处理器,能对I/O设备进行统一管理

## I/O系统基本组成

1. I/O硬件包括**外部设备、I/O接口、I/O总线**等。

2. I/O软件包括**驱动程序、用户程序、管理程序、升级补丁**等。

   1. I/O指令——CPU指令的一部分<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210911223850098.png" alt="image-20210911223850098" style="zoom:80%;" />

   2. 通道指令——通道能识别的指令

      **通道程序**提前编制好**放在主存中**

      在含有通道的计算机中，CPU执行I/O指令对通道发出命令，由通道执行一系列通道指令，代替CPU对I/O设备进行管理

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210911224021465.png" alt="image-20210911224021465" style="zoom:80%;" />

## 外部设备

### 输入设备

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210911224104567.png" alt="image-20210911224104567" style="zoom:80%;" />

### 输出设备

#### 显示屏

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210911224241005.png" alt="image-20210911224241005" style="zoom:80%;" />

#### 打印机

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210911224604996.png" alt="image-20210911224604996" style="zoom:80%;" />

### 外存储器

计算机的外存储器又称为辅助存储器，目前主要使用磁表面存储器。
所谓“磁表面存储”，是指把某些磁性材料薄薄地涂在金属铝或塑料表面上作为载磁体来存储信息。磁盘存储器、磁带存储器和磁鼓存储器均属于磁表面存储器。

* 磁表面存储器的优点:
  1. 存储容量大，位价格低;
  2. 记录介质可以重复使用;
  3. 记录信息可以长期保存而不丢失，甚至可以脱机存档;
  4. 非破坏性读出，读出时不需要再生。
* 磁表面存储器的缺点:
  1. 存取速度慢;
  2. 机械结构复杂;
  3. 对工作环境要求较高。

**外存储器既可以作为输入设备，也可以作为输出设备。(既可以存数据，也可以读数据)**

磁盘驱动器:核心部件是磁头组件和盘片组件，温彻斯特盘是一种可移动头固定盘片的硬盘存储器。

磁盘控制器:是硬盘存储器和主机的接口，主流的标准有IDE、SCSI、SATA等。

#### 磁盘设备的组成

1. 存储区域
   一块硬盘含有若干个记录面，每个**记录面**划分为若干条**磁道**，而每条磁道又划分为若干个**扇区**，扇区（也称块）是**磁盘读写的最小单位**，也就是说磁盘**按块存取**。
   1. **磁头数**：即记录面数，表示硬盘总共有多少个磁头，磁头用于读取/写入盘片上记录面的信息，一个记录面对应一个磁头。
   2. **柱面数**：表示硬盘每一面盘片上有多少条磁道。在一个盘组中，不同记录面的相同编号（位置）的诸磁道构成一个圆柱面。
   3. **扇区数**：表示每一条磁道上有多少个扇区。
2. 硬盘存储器：硬盘存储器由磁盘驱动器、磁盘控制器和盘片组成

#### 磁盘的性能指标

1. 磁盘的容量:一个磁盘所能存储的字节总数称为磁盘容量。磁盘容量有**非格式化容量和格式化容量之分**。

   > 非格式化容量是指磁记录表面可以利用的磁化单元总数。
   > 格式化容量是指按照某种特定的记录格式所能存储信息的总量。

2. 记录密度:记录密度是指盘片单位面积上记录的二进制的信息量，通常以道密度、位密度和面密度表示。

   > ==道密度是沿磁盘半径方向单位长度上的磁道数;==
   >
   > ==位密度是磁道单位长度上能记录的二进制代码位数;==
   >
   > ==面密度是位密度和道密度的乘积。==
   >
   > 注意:磁盘所有磁道记录的信息量一定是相等的,并不是圆越大信息越多，故每个磁道的位密度都不同。

3. 平均存取时间

   > <font color='red'>**$平均存取时间=寻道时间（磁头移动到目的磁道）
   > 旋转延迟时间（磁头定位到所在扇区）+
   > 传输时间（传输数据所花费的时间）$**</font><img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210912200319401.png" alt="image-20210912200319401" style="zoom:80%;" />

4. 数据传输率:磁盘存储器在单位时间内向主机传送数据的字节数，称为数据传输率。

   > 假设磁盘转数为r(转/秒），每条磁道容量为N个字节，则数据传输率为D~r~=r*N

#### 磁盘地址

主机向磁盘控制器发送寻址信息,磁盘的地址一般如图所示:<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210912200504135.png" alt="image-20210912200504135" style="zoom:80%;" />

#### 硬盘的工作过程

硬盘的主要操作是寻址、读盘、写盘。每个操作都对应一个控制字，硬盘工作时，第一步是取控制字，第二步是执行控制字。

硬盘属于机械式部件，其读写操作是**串行**的，不可能在同一时刻既读又写，也不可能在同一时刻读两组数据或写两组数据。

#### 磁盘阵列

RAID (Redundant Array of Inexpensive Disks，廉价冗余磁盘阵列）是将多个独立的物理磁盘组成一个独立的逻辑盘，数据在多个物理盘上分割交叉存储、并行访问，具有更好的存储性能、可靠性和安全性。

RAID的分级如下所示。在RAID1～RAID5的几种方案中，无论何时有磁盘损坏，都可以随时拔出受损的磁盘再插入好的磁盘，而数据不会损坏。

- RAIDO:无冗余和无校验的磁盘阵列。

  <img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210912200758807.png" alt="image-20210912200758807" style="zoom:80%;" />

  逻辑上相邻的两个扇区在物理上存到两个磁盘,类比第三章“低位交叉编址的多体存储器”

- RAID1:镜像磁盘阵列。

  <img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210912200841488.png" alt="image-20210912200841488" style="zoom:80%;" />

  存两份数据

- RAID2:采用纠错的海明码的磁盘阵列。

  逻辑上连续的几个bit物理上分散存储在各个盘中4bit信息位+3bit海明校验位——可纠正一位错

- RAID3:位交叉奇偶校验的磁盘阵列。

- RAID4:块交叉奇偶校验的磁盘阵列。

- RAID5:无独立校验的奇偶校验磁盘阵列。

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210912201005484.png" alt="image-20210912201005484" style="zoom:80%;" />

## I/O方式

### 程序查询方式

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913194024578.png" alt="image-20210913194024578" style="zoom:80%;" />

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913194049502.png" alt="image-20210913194049502" style="zoom:80%;" />

- **预置传送参数**：CPU执行初始化程序，并预置传送参数:设置计数器、设置数据首地址
- **启动外设**：向I/O接口发送**命令字**，启动I/O设备
- **取外设状态**：CPU从接口读取设备状态信息
- **外设准备就绪**：CPU不断查询I/o设备状态,直到外设准备就绪
- **传送一次数据**：一般为一个字
- **修改传送参数：**修改地址和计数器参数
- **传送完否**：判断传送是否结束(一般计数器为0时结束)

- 优点:接口设计简单、设备量少。
- 缺点:CPU在信息传送过程中要花费很多时间用于查询和等待,而且在一段时间内只能和一台外设交换信息,效率大大降低。

### 程序中断方式

**程序中断**是指在计算机执行现行程序的过程中，出现某些急需处理的异常情况或特殊请求，CPU暂时中止现行程序，而转去对这些异常情况或特殊请求进行处理，在处理完毕后CPU又自动返回到现行程序的断点处,继续执行原程序。

工作流程：

1. 中断请求
   中断源向CPU发送中断请求信号。
2. 中断响应
   响应中断的条件。
   中断判优:多个中断源同时提出请求时通过中断判优逻辑响应一个中断源。
3. 中断处理
   **中断隐指令**，**中断服务程序**。

每个中断源向CPU发出中断请求的时间是随机的。
为了记录中断事件并区分不同的中断源，中断系统需对每个中断源设置中断请求标记触发器INTR,当其状态为“1”时,表示中断源有请求。
这些触发器可组成中断请求标记寄存器，该寄存器可集中在CPU中，也可分散在各个中断源中。

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913195034324.png" alt="image-20210913195034324" style="zoom:80%;" />

CPU响应中断必须满足以下3个条件:

1. 中断源有中断请求。
2. CPU允许中断即开中断。
3. 一条指令执行完毕，且没有更紧迫的任务。

中断判优的实现：

中断判优既可以用**硬件实现**，也可用**软件实现**:
**硬件实现**是通过**硬件排队器**实现的，它既可以设置在CPU中，也可以分散在各个中断源中;**软件实现**是通过**查询程序**实现的。

1. 硬伴故障中断属于最高级，其次是软件中断;
2. 非屏蔽中断优于可屏蔽中断;
3. DMA请求优于I/O设备传送的中断请求
4. 高速设备优于低速设备;
5. 输入设备优于输出设备;
6. 实时设备优于普通设备。

中断隐指令：

中断隐指令的主要任务:

1. **关中断**。在中断服务程序中，为了保护中断现场（即CPU主要寄存器中的内容)期间不被新的中断所打断，必须关中断，从而保证被中断的程序在中断服务程序执行完毕之后能接着正确地执行下去。

2. **保存断点**。为了保证在中断服务程序执行完毕后能正确地返回到原来的程序，必须将原来程序的断点（即程序计数器（PC）的内容）保存起来。可以存入堆栈，也可以存入指定单元。

3. **引出中断服务程序**。引出中断服务程序的实质就是取出中断服务程序的入口地址并传送给程序计数器（PC) 

   1. 软件查询法

   2. 硬件向量法：

      由硬件产生向量地址
      再由向量地址找到入口地址<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913200001435.png" alt="image-20210913200001435" style="zoom:80%;" />

中断服务程序的主要任务：

1. 保护现场

   保存通用寄存器和状态寄存器的内容*（eg:保存ACC寄存器的值）*，以便返回原程序后可以恢复CPU环境。可使用堆栈，也可以使用特定存储单元。

2. 中断服务（设备服务）

   主体部分，如通过程序控制需打印的字符代码送入打印机的缓冲存储器中*(eg:中断服务的过程中有可能修改ACC寄存器的值)*

3. 恢复现场

   通过出栈指令或取数指令把之前保存的信息送回寄存器中*（eg:把原程序算到一般的ACC值恢复原样)*

4. 中断返回

   通过中断返回指令回到原程序断点处

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913200521013.png" alt="image-20210913200521013" style="zoom:80%;" />

多重中断：

- 单重中断:执行中断服务程序时不响应新的中断请求
- 多重中断:又称中断嵌套,执行中断服务程序时可响应新的中断请求

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913200647843.png" alt="image-20210913200647843" style="zoom:80%;" />

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913200753333.png" alt="image-20210913200753333" style="zoom:80%;" />

中断屏蔽技术主要用于多重中断，CPU要具备多重中断的功能，须满足下列条件

1. 在中断服务程序中提前设置开中断指令。
2. 优先级别高的中断源有权中断优先级别低的中断源。
   每个中断源都有一个屏蔽触发器，1表示屏蔽该中断源的请求，0表示可以正常申请，所有屏蔽触发器组合在一起，便构成一个屏蔽字寄存器，屏蔽字寄存器的内容称为屏蔽字。

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913200906117.png" alt="image-20210913200906117" style="zoom:80%;" />

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913201131508.png" alt="image-20210913201131508" style="zoom:80%;" />

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913201206335.png" alt="image-20210913201206335" style="zoom:80%;" />

### DMA方式

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913201259297.png" alt="image-20210913201259297" style="zoom:80%;" />

CPU向DMA控制器指明要输入还是输出;要传送多少个数据;数据在主存、外设中的地址。

传送前：

1. 接受==外设发出的DMA请求==(外设传送一个字的请求），并向CPU发出总线请求。
2.  CPU响应此总线请求，发出总线响应信号，接管总线控制权，进入DMA操作周期

传送时：

3. 确定传送数据的主存单元地址及长度，并能自动修改主存地址计数和传送长度计数。
4. 规定数据在主存和外设间的传送方向，发出读写等控制信号，执行数据传送操作

传送后：

5. 向CPU报告DMA操作的结束。

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913201749362.png" alt="image-20210913201749362" style="zoom:80%;" />

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913201820086.png" alt="image-20210913201820086" style="zoom:80%;" />

DMA方式具有下列特点:

1. 它使主存与CPU的固定联系脱钩，主存既可被CPU访问，又可被外设访问。
2. 在数据块传送时,主存地址的确定、传送数据的计数等都由硬件电路直接实现。
3. 主存中要开辟专用缓冲区，及时供给和接收外设的数据。
4.  DMA传送速度快，CPU和外设并行工作，提高了系统效率。
5.  DMA在传送开始前要通过程序进行预处理，结束后要通过中断方式进行后处理。

传送方式：

1. 停止CPU访问

   <img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913202028674.png" alt="image-20210913202028674" style="zoom:80%;" />

2. DMA与CPU交替访问：一个CPU周期分为C1和C2两个周期

   <img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913202056187.png" alt="image-20210913202056187" style="zoom:80%;" />

3. 挪用周期（周期窃取）

   <img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913202148524.png" alt="image-20210913202148524" style="zoom:80%;" />

DMA 访问主存有三种可能:

1. CPU此时不访存（不冲突)
2. CPU正在访存（存取周期结束让出总线)
3. CPU与 DMA同时请求访存（I/O访存优先)

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913202234832.png" alt="image-20210913202234832" style="zoom:80%;" />

<img src="C:\Users\LIMBO\AppData\Roaming\Typora\typora-user-images\image-20210913202250604.png" alt="image-20210913202250604" style="zoom:80%;" />

