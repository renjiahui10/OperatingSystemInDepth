# 深入理解操作系统 第四章 操作系统的非连续内存分配 

>   第四章的主要内容是：操作系统的非连续内存分配

第三章介绍的是连续内存管理, 即 : 操作系统加载到内存以及程序加载到内存中时, 分配一块连续的空闲(内存)块. 但是容易出现碎片问题, 这一章介绍的非连续内存分配可以有效的减少碎片的出现.

## 非连续内存分配的必要性

### 连续内存分配的缺点

1.  分配给一个程序的物理内存是连续的
2.  内存利用率低
3.  有外碎片 / 内碎片的问题

### 非连续内存分配的优点

1.  一个程序的物理地址空间是非连续的

2.  更好的内存利用和管理

3.  允许共享代码与数据(共享库等...)

4.  支持动态加载和动态链接

### 非连续内存分配的缺点

1.  建立虚拟地址(逻辑地址）和物理地址的转换难度大

    *   软件方案（用软件来实现为一个应用程序分配不连续内存空间，这份开销是十分大的，能否和硬件系统（CPU内部和内存管理相关的硬件）结合来完成硬件支持的非连续内存管理的）

    *   硬件方案(采用硬件方案) : 分段 / 分页

## 非连续内存分配

### 分段(Segmentation)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d3208e25-517d-4ad8-94b2-8bd42674f252)

上图中左边虚拟的逻辑地址空间是连续的，但是分段之后可以有效的隔离开来，可以把堆放在特定的地址，用特定的权限管理起来，同时，可以将运行栈、程序数据、库、用户程序等分离出来，可以让用户代码段和主程序段能共享，相互之间访问，有些数据可以相互访问。左右两边需要由建立对应的映射关系

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/46713d92-264e-4261-9b6f-29ee0c585396)


**段 :** 在程序中会有来自不同文件的函数 ; 在程序执行时, 不同的数据也有不同的字段, 比如 : 堆 / 栈 / .bss / .data 等

**分段 : ** 更好的分离和共享

程序的分段地址空间如下图所示 : 

<img src="https://i.loli.net/2020/11/11/NAnzHKj5tEM416h.jpg"/>

**分段寻址方案**

逻辑地址空间连续，但是物理地址空间不连续，使用映射机制进行关联（**编写一个应用程序，使用的是一维的逻辑地址，一维逻辑地址怎么能和分段的物理地址对应呢？**）
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/fb5d719d-5e86-4a2b-bab2-a8341658e255)

**上图中下方的两种地址方式都是逻辑地址**

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8abf5b49-8ad6-473d-b8e4-d1f727575557)



一个段 : 一个内存"块"

程序访问内存地址需要 : 一个二维的二元组(s, addr) → (段号, 地址)

操作系统维护一张段表, 存储( 物理地址中的起始地址, 长度限制)

物理地址 : 段表中的起始地址 + 二元组中的偏移地址

### 分页(Paging)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d372c915-5a24-47c3-b7a6-4d15e6e98c2a)
分页和分段很相似，在分页中也有页号和偏移，两者主要的区别是，段的长度时可变的，而页（页帧）的长度时不可变的
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8b8342ac-1829-408a-b8cb-441afbf9ae7f)
帧——物理内存的组织和布局方式
页——逻辑地址的组织和布局方式



#### 分页地址空间



划分物理内存至固定大小的帧(Frame)

-   大小是2的幂, 512 / 4096 / 8192

划分逻辑地址空间至相同大小的页(Page)

-   大小是2的幂, 512 / 4096 / 8192

建立方案 → 转换逻辑地址为物理地址(pages to frames)

-   页表
-   MMU / TLB

**帧(Frame)**
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/bc683769-1078-405c-8e77-e40bb4e1c96e)

物理内存被分割为大小相等的帧. 一个内存物理地址是一个二元组(f, o) → (帧号, 帧内偏移)

帧号 : F位, 共有2^F个帧

帧内偏移 : S位, 每帧有2^S个字节

物理地址 = 2^S * f + o

(例子 : 16-bit地址空间, 9-bit(512 byte) 大小的页帧 物理地址 = (3,6) 物理地址 = 2^9 * 3 + 6 = 1542)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/48c6a66f-8c4c-4f15-bf04-04fc0e3e1345)


>   分页和分段的最大区别 : 这里的 S 是一个固定的数, 而分段中的长度限制不定

**页(Page)**
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8326a9a6-1ccb-4ee9-8383-b30d045bf9f0)

上图中页号和帧号的大小可能不匹配，页的数量可以比帧的数量大（用于虚拟内存）
但是页内偏移和帧内偏移是一致的

#### 页寻址方案
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f5270b3e-59cf-4c78-a100-c2b1cb36c3b0)
上图中页的数量比帧的数量大
页表由操作系统维护和创建, 是在操作系统初始化时，页表保存了页号和帧号之间的对应关系之间的映射关系
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/abacd349-1ab7-4ae1-8783-92eef35d2153)



-   逻辑地址空间应当大于物理内存空间
-   页映射到帧
-   页是连续的虚拟内存
-   帧是非连续的物理内存(有助于减少碎片的产生)
-   不是所有的页都有对应的帧

### 页表(Page Table)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4f41ce10-32c4-4c24-a5d0-5912e8cabd96)
怎么来实现页表，怎么能实现的更加高效，更加节省空间，是由操作系统和硬件相互配合完成的目标，

#### 页表概述
页表其实就是一个大数组
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/503f92c6-9b78-4db1-8d89-6e64791753e3)



每一个运行的程序都有一个页表

-   属于程序运行状态, 会动态变化
-   PTBR : 页表基址寄存器

**转换流程**

CPU根据程序的page的页号的若干位, 计算出索引值index, 在页表中搜索这个index, 得到的是帧号, 帧号和原本的offset组成物理地址.

页表中还有一些特殊标志位（标志这个页表项是否合法————也就是该逻辑地址是否对应真实的物理地址，该页表项对应的内存是否读过或写过

-   dirty bit,
-   resident bit, (0 : 对应的物理页帧在内存中不存在 ; 1 : 存在)
-   clock / reference bit

**转换实例**
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/b3d63371-7811-4488-9480-bd83ed3b94ad)


16位地址的系统

-   32KB的物理内存
-   每页的 1024 byte

逻辑地址空间 : (4, 0) ... (3, 1023)

页表 :

Flags |  Frame nums

1 0 1    0 0 0 0 0          → 内存访问异常(可能要杀死程序)

0 1 1    0 0 1 0 0           → 页帧是4 偏移是 1023 → 物理地址 (4, 1023)

#### 分页机制的性能问题（时间问题和空间问题）
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8af1e414-69ab-413b-927d-216eef06ef05)


#### 转换后备缓冲区(TLB————CPU中MMU的一部分，是一块相关存储器（关联存储器）————可以并发的快速查找，但是容量有限)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/20e398f7-5e83-4a1d-b7c6-62ffc786ec7e)


缓解时间问题

Translation Look-aside Buffer(TLB) 是一个缓冲区. CPU中有快表TLB(可以将经常访问的页表存放在这边)

缓存近期访问的页帧转换表项

-   TLB使用相关存储器实现, 具备快速访问性能
-   如果TLB命中, 物理页号可以很快被获取
-   如果TLB未命中, 对应的表项被更新到TLB中(x86的CPU由完全由硬件实现, 其他的可能是由操作系统实现）
**TLB未命中（TLB缺失）CPU回去访问页表，这样会导致速度变慢，但是这种情况出现频率不高，因为32位的操作系统，页的大小是4k，如果一页中的每一项全部访问的话，4k次访问内存才可能出现一次TLB缺失**

#### 二级/多级页表

解决空间问题 

时间换空间
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f01c96a8-2f00-4453-bf6e-076c108d3df5)
一级页表的页表项存储着二级页表的起始地址

二级页表

-   将页号分为两个部分, 页表分为两个, 一级页号对应一级页表, 二级页号对应二级页表.
-   一级页号查表获得在二级页表的起始地址, 地址加上二级页号的值, 在二级页表中获得帧号
-   节约了一定的空间, 在一级页表中如果resident bit = 0, 可以使得在二级页表中不存储相关index,而只有一张页表的话, 这一些index都需要保留

多级页表

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a3d58376-361a-43c9-ad27-c38693a1a367)

-   通过把页号分为k个部分, 来实现多级间接页表, 建立一棵页表"树"

#### 反向页表
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/90aab1b1-2862-450d-9357-f610841e39a6)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/ddf16fac-a3c5-4079-ba02-66c4ac75075a)

解决大地址空间问题

目的 : 根据帧号获得页号

反向页表只需要存在一张即可

-   有大地址空间(64-bits), 前向映射页表变得繁琐. 比如 : 使用了5级页表
-   不是让页表与逻辑地址空间的大小相对应, 而是当页表与物理地址空间的大小相对应. 逻辑地址空间增长速度快于物理地址空间

##### 基于页寄存器(Page Registers)的方案
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8254694d-734b-4b75-b4fc-8e6758526fca)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f714208e-16bd-477e-bf22-cedb521476bb)


存储 (帧号, 页号) 使得表大小与物理内存大小相关, 而与逻辑内存关联减小.

每一个帧和一个寄存器关联, 寄存器内容包括 :

-   resident bit : 此帧是否被占用
-   occupier : 对应的页号 p
-   protection bits : 保护位

实例 :

-   物理内存大小是 : 4096 * 4096 = 4K * 4KB = 16 MB
-   页面大小是 : 4096 bytes = 4 KB
-   页帧数 : 4096 = 4 K
-   页寄存器使用的空间(假设8 bytes / register) : 8 * 4096 = 32 Kbytes
-   页寄存器带来的额外开销 : 32K / 16M = 0.2%
-   虚拟内存大小 : 任意

优势 :

-   转换表的大小相对于物理内存来说很小
-   转换表的大小跟逻辑地址空间的大小无关

劣势 :

-   需要的信息对调了, 即根据帧号可以找到页号
-   如何转换回来? (如何根据页号找到帧号)
-   在需要在反向页表中搜索想要的页号

##### 基于关联内存(associative memory)的方案
也就是基于关联存储器（相关存储器————可以并行的快速查找）
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/73f09a5c-6334-414b-b578-37793ee32b2b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/aa9c38ad-cfae-4328-b540-31399d554a42)


关联存储器硬件设计复杂, 导致容量不大, 需要放置在CPU中

-   如果帧数较少, 页寄存器可以被放置在关联内存中
-   在关联内存中查找逻辑页号
    -   成功 : 帧号被提取
    -   失败 : 页错误异常 (page fault)
-   限制因素:
    -   大量的关联内存非常昂贵(难以在单个时钟周期内完成 ; 耗电)

##### 基于哈希(hash)的方案

哈希函数 : h(PID, p)  PID当前运行程序的标识

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a8e3339f-d778-4edd-95cd-a090bb7064c9)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/31bbf81a-7b1c-402d-85b0-683dd67e43c0)

在反向页表中通过哈希算法来搜索一个页对应的帧号

-   对页号做哈希计算, 为了在帧表中获取对应的帧号（哈希函数的输出是帧号，哈希函数的计算可以用硬件加速）
-   页 i 被放置在表 f(i) 位置, 其中 f 是设定的哈希函数
-   为了查找页 i , 执行下列操作 :
    -   计算哈希函数 f(i) 并且使用它作为页寄存器表的索引, 获取对应的页寄存器
    -   检查寄存器标签是否包含 i, 如果包含, 则代表成功, 否则失败
好处：
1.不受制于逻辑地址空间和虚拟地址空间的限制，因为之和物理地址空间相关，容量可以做的很小
2.之前的每个应用程序都有一个页表（也就是说，每个应用程序对应的逻辑地址空间可能是独立的），而由于反向页表使用的是帧号（对应物理地址）作为索引的，并不相互冲突（和多少个进程没有关系），所以整个系统只需要一个页表就可以了，所以相对来说节省空间，但是需要一个硬件支持的哈希计算方式
**由于每个应用程序对应的逻辑地址空间独立，在整个系统使用同一个反向页表的情况下，可能出现一个page number对应多个frame number号的情况，这也是为啥哈希函数的输入多了一个PID的原因，见下图**
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/95547544-e6d7-450f-95bc-3c30b9602ce6)

