![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/2a7f5dca-4790-40f2-b903-34e144693397)# 深入理解操作系统 第十二章

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e76cc8a2-3917-4b44-9bfa-a2283108ad6b)

>   第十二章的主要内容是：文件管理

## 基本概念


### 文件系统和文件
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/945b9458-864a-41cc-9da0-fad1b110f916)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/fc9d4b6a-aa27-42c1-b965-0c180ec616a6)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/20cd6476-e114-4b67-851e-31d856029f95)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/6df2b301-cbe2-483e-8a0e-4381086740a1)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/33f143a1-08b2-4a63-9061-02215e7e7557)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/97601738-8681-4fa2-9ad5-08e1cd00d522)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/7837fb1c-4494-4e3d-8ce0-305d7ba3395a)


文件系统: 一种用于持久性存储的系统抽象

-   在存储上: 组织,控制,导航,访问和检索数据
-   在大多数计算机系统包含文件系统
-   个人电脑,服务器,笔记本电脑
-   ipod,tivo,机顶盒,手机,电脑
-   google可能也是由一个文件系统构成的

文件: 文件系统中的一个单元的相关数据在操作系统中的抽象

文件系统的功能:

-   分配文件磁盘空间
    -   管理文件块(哪一块属于哪一个文件)
    -   管理空闲空间(哪一块是空闲的)
    -   分配算法(策略)
-   管理文件集合
    -   定位文件及其内容
    -   命名: 通过名字找到文件的接口
    -   最常见: 分层文件系统
    -   文件系统类型(组织文件的不同方式)
-   提供的便利及特征
    -   保护: 分层来保护数据安全
    -   可靠性,持久性: 保持文件的持久即使发生崩溃,媒体错误,攻击等

文件和块:

文件属性: 名称,类型,位置,大小,保护,创建者,创建时间,最久修改时间...

文件头: 在存储元数据中保存了每个文件的信息,保存文件的属性,跟踪哪一块存储块属于逻辑上文件结构的哪个偏移

### 文件描述符（打开的文件在内存中维护的相关信息）
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e7ab74c3-7396-4c3e-a187-7f6155475ad6)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a330a26d-e0a3-4984-9701-7cd5e5b27b86)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/9ccffe82-1a7d-4145-8a2d-e82a8dbca95b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f82b2813-ec33-400a-ad38-8d25b1a19789)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/898be466-4436-49c4-84f8-39f16ec83150)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5fa44753-b3fc-45fd-9e6a-00a54304dbe9)

文件的用户视图和系统视图：也就是说文件从用户和系统的角度来讲是什么样子的（文件的用户视图是指从用户视图看到的文件是什么样子的，系统视图是指从操作系统看到的文件是什么样子的），也就是说从操作系统看，文件只是简单的字节序列，并不考虑文件的内部结构（数据结构，是jpg图片还是txt文档，这是由用户进程考虑的）
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/11007c69-ca60-4269-a82b-a05d557b8645)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8e2ec624-1506-456b-83fa-004074744479)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0d94e69f-5bef-4db0-bc1f-84ee0ffd98fd)

磁盘访问的最小单位是块，即便是读写一个字节也得把整个块读出来。
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/607e6605-9543-4dfb-a28b-ef573a431190)

上述三种访问模式都是指在同一个文件中寻找该文件中不同的内容。
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/05225f8e-535b-4fe7-a5fa-abe7bc9344af)

上图中无结构和简单记录结构都是操作系统层面的，复杂结构是由应用程序来识别的。**但是，操作系统接口层面上一定程度可以识别文件是可执行文件还是简单的文本文件**
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/37b523c1-b7e8-4de9-9de1-076c47fe8b7a)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d2fd5cc9-90c4-4371-b28f-324de1c472ba)

文件使用模式:

使用程序必须在使用前先"打开"文件

```cpp
f = open(name, flag);
...
... = read(f, ...);
...
close(f);
```

内核跟踪每个进程打开的文件:

-   操作系统为每个进程维护一个打开文件表
-   一个打开文件描述符是这个表中的索引

需要元数据来管理打开文件:

文件指针: 指向最近的一次读写位置,每个打开了这个文件的进程都这个指针

文件打开计数: 记录文件打开的次数 - 当最后一个进程关闭了文件时,允许将其从打开文件表中移除

文件磁盘位置: 缓存数据访问信息

访问权限: 每个程序访问模式信息

用户视图: 持久的数据结构

系统访问接口:

字节的集合(UNIX)

系统不会关心你想存储在磁盘上的任何的数据结构

操作系统内部视角:

块的集合(块是逻辑转换单元,而扇区是物理转换单元)

块大小<> 扇区大小: 在UNIX中, 块的大小是 4KB

当用户说: 给我2-12字节空间时会发生什么?

获取字节所在的快

返回快内对应部分

如果要写2-12字节?

获取块

修改块内对应部分

写回块

在文件系统中的所有操作都是在整个块空间上进行的: `getc()` `putc()` 即使每次只访问1字节的数据,也会缓存目标数据4096字节(一个磁盘块)

用户怎么访问文件: 在系统层面需要知道用户的访问模式

顺序访问: 按字节依次读取(几乎所有的访问都是这种方式)

随机访问: 从中间读写(不常用,但是仍然重要,如: 虚拟内存支持文件,内存页存储在文件中;更加快速,不希望获取文件中间的内容的时候也必须先获取块内所有字节)

内容访问: 通过特征

文件内部结构:

无结构: 单词,比特的队列

简单记录结构: 列,固定长度,可变长度

复杂结构: 格式化的文档(word, PDF), 可执行文件, ...

多用户系统中的文件共享是很必要的

访问控制:

谁能够获得哪些文件的哪些访问权限

访问模式: 读,写,执行,删除,列举等

文件访问控制列表(ACL):

<文件实体, 权限>

UNIX模式:

<用户|组|所有人,读|写|可执行>

用户ID识别用户,表明每个用户所允许的权限及保护模式

组ID允许用户组成组,并指定了组访问权限

指定多用户,客户如何同时访问共享文件:

和过程同步算法相似

因磁盘IO和网络延迟而设计简单

UNIX文件系统(UFS)语义:

对打开文件的写入内容立即对其他打开同一文件的其他用户可见

共享文件指针允许多用户同时读取和写入文件

会话语义:

写入内容只有当文件关闭时可见

锁:

一些操作系统和文件系统提供该功能

### 目录
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f171a806-e3b3-496c-a371-45d9ba9fd6e5)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d3cfb641-bd7c-4823-895f-3701321d04d7)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/b0f8aece-1c54-42a8-a949-56f90424e5a5)

文件以目录的方式组织起来

目录是一类特殊的文件: 每个目录都包含了一张表<name, pointer to file header>

目录和文件的树形结构: 早期的文件系统是扁平的(只有一层目录)

层次名称空间: /spell/mail/prt/first  /programs/p/list

典型操作:

搜索文件

创建文件

删除文件

枚举目录

重命名文件

在文件系统中遍历一个路径

操作系统应该只允许内核模式修改目录: 确保映射的完整性,应用程序能够读目录(ls)

文件名的线性列表,包含了指向数据块的指针: 编程简单,执行耗时

Hash表 - hash数据结构的线性表: 减少目录搜索时间,碰撞,固定大小

名字解析: 逻辑名字转换成物理资源(如文件)的过程:

在文件系统中: 到实际文件的文件名(路径)

遍历文件目录直到找到目标文件

举例: 解析"/bin/ls":

读取root的文件头(在磁盘固定位置)

读取root的数据块: 搜索bin项

读取bin的文件头

读取bin的数据块: 搜索ls项

读取ls的文件头

当前工作目录:

每个进程都会指向一个文件目录用于解析文件名

允许用户指定相对路径来代替绝对路径

一个文件系统需要先挂载才能被访问

一个未挂载的文件系统被挂载在挂载点上

### 文件别名

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/dead4a45-35bb-4bef-a9cf-68c01abc029f)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/9d72dd5a-4cfe-46cf-937a-4ca979caabcf)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f65ba059-d726-487f-bc56-811b6a04c12a)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e29ef0dc-cbc9-41da-97d8-18d6b426d22a)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/19125e30-5e01-4af1-9168-ec9fac254e7d)


两个或多个文件名关联同一个文件:

硬链接: 多个文件项指向一个文件

软链接: 以快捷方式指向其他文件

通过存储真实文件的逻辑名称来实现

如果删除一个有别名的文件会如何呢? : 这个别名将成为一个悬空指针

Backpointers 方案:

每个文件有一个包含多个backpointers的列表,所以删除所有的Backpointers

backpointers使用菊花链管理

添加一个间接层: 目录项数据结构

链接: 已存在文件的另外一个名字(指针)

链接处理: 跟随指针来定位文件

我们如何保证没有循环呢?

只允许到文件的链接, 不允许在子目录的链接

每增加一个新的链接都用循环检测算法确定是否合理

限制路径可遍历文件目录的数量

### 文件系统种类
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/6c35a352-255f-4a30-8d80-11f2543070e4)

为什么会有这么多文件系统呢：不同的文件系统由于存储的数据不同，会做不同的优化，使用场景的不同也会做不同的优化，比如说光盘的文件系统是一次性写入多次读出，大多数文件系统是多次读入多次读出，同时不同的文件系统的安全要求也是不同的，安全级别越高，访问效率也会相对下降，对于安全级别不高的，可以减弱安全机制，提高访问效率，
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/ce89294b-4c53-4a68-8556-5d64a1d6e130)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/cbc75e01-78e1-475e-9d02-a66d78bd2f38)

磁盘文件系统: 文件存储在数据存储设备上,如磁盘; 例如: FAT,NTFS,ext2,3,ISO9660等

数据库文件系统: 文件根据其特征是可被寻址的; 例如: WinFS

日志文件系统: 记录文件系统的修改,事件; 例如: journaling file system

网络,分布式文件系统: 例如: NFS,SMB,AFS,GFS

特殊,虚拟文件系统

## 虚拟文件系统
虚拟文件系统的提出是为了面对有多种不同的文件系统，而在操作系统中希望对上提供一个统一的接口这个问题而提出的
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5b2017ea-486a-4d6a-82eb-b44bf6a20dba)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/7190150a-982e-4bcc-9253-c30e4fa02846)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0367b184-3f55-4d4b-bf36-81dae73b991b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/6bf4c34b-bbc9-44c9-80a8-107b4b51e1c9)

每个文件都有一个文件控制块
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/86c6f355-9f8a-4e01-9f0c-3bf94fefe864)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/b0ad53d9-c600-4474-b205-8239123b5cc3)

上图中vol代表卷控制块，dir代表目录项，file代表文件控制块，data block代表具体的文件数据。
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/c6e9b515-9a06-46c4-a204-fb50819bdf8c)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/51e87c0e-0816-4e1b-8a91-7c68f74ba05a)

卷控制块在磁盘上的位置是固定的
分层结构:

顶层: 文件,文件系统API

上层: 虚拟(逻辑)文件系统 (将所有设备IO,网络IO全抽象成为文件,使得接口一致)

底层: 特定文件系统模块

目的: 对所有不同文件系统的抽象

功能:

提供相同的文件和文件系统接口

管理所有文件和文件系统关联的数据结构

高效查询例程,遍历文件系统

与特定文件系统模块的交互

数据结构:

卷[第四声]控制块(UNIX: "superblock")

每个文件系统一个

文件系统详细信息

块,块大小,空余块,计数,指针等

文件控制块(UNIX: "vnode" or "inode")

每个文件一个

文件详细信息

许可,拥有者,大小,数据库位置等

目录节点(Linux: "dentry")

每个目录项一个(目录和文件)

将目录项数据结构及树形布局编码成树形数据结构

指向文件控制块,父节点,项目列表等

其中: 卷控制块(每个文件系统一个),文件控制块(每个文件一个),目录节点(每个目录项一个)

持续存储在二级存储中: 在分配在存储设备中的数据块中

当需要时加载进内存:

卷控制块: 当文件系统挂载时进入内存

文件控制块: 当文件被访问时进入内存

目录节点: 在遍历一个文件路径时进入内存

## 数据块缓存
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a0814e01-0e3c-4ecd-8b04-b870f065f9cf)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/2bc41597-a8b2-4157-bd55-dd3e8a1633ac)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/3ef1ca86-9d89-4798-8ea8-007a4815e4fd)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4e5ceb37-ad6a-48f5-a523-bb926bdf42e7)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8d81b1bd-1ec9-41cf-a07e-229303626db7)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a707b123-d3ca-419e-b751-94fc4b7bf767)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/9d02888f-abd4-4f55-95c9-50f6e4386a47)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8979482d-fbbf-4cce-8db8-75e7cf99d9ef)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0accf6c4-0cf9-4948-9a8e-446f33e804c4)

数据块按需读入内存:

提供 `read()` 操作

预读: 预先读取后面的数据块

数据块使用后被缓存:

假设数据将会再次被使用

写操作可能被缓存和延迟写入

两种数据块缓存方式:

普通缓冲区缓存

页缓存: 同一缓存数据块和内存页

分页要求: 当需要一个页时才将其载入内存

支持存储: 一个页(在虚拟地址空间中)可以被映射到一个本地文件中(在二级存储中)

## 打开文件的数据结构

打开文件描述:

每个被打开的文件一个

文件状态信息

目录项,当前文件指针,文件操作设置等

打开文件表:

一个进程一个

一个系统级的

每个卷控制块也会保存一个列表

所以如果有文件被打开将不能被卸载

一些操作系统和文件系统提供该功能

调节对文件的访问

强制和劝告:

强制 - 根据锁保持情况和需求拒绝访问

劝告 - 进程可以查找锁的状态来决定怎么做

## 文件分配

大多数文件都很小:

需要对小文件提供强力的支持

块空间不能太小

一些文件非常大:

必须支持大文件(64-bit 文件偏移)

大文件访问需要相当高效

如何为一个文件分配数据块

分配方式:

连续分配

链式分配

索引分配

指标:

高效: 如存储利用(外部碎片)

表现: 如访问速度

连续分配:

文件头指定起始块和长度

位置,分配策略: 最先匹配,最佳匹配,...

优势: 文件读取表现好;高效的顺序和随机访问

劣势: 碎片;文件增长问题

链式分配:

文件以数据块链表方式存储

文件头包含了到第一块和最后一块的指针

优势: 创建,增大,缩小很容易;没有碎片

劣势: 不可能进行真正的随机访问;可靠性

索引分配:

为每个文件创建一个名为索引数据块的非数据数据块(到文件数据块的指针列表)

文件头包含了索引数据块

优势: 创建,增大,缩小很容易;没有碎片;支持直接访问

劣势: 当文件很小时,存储索引的开销大;处理大文件难

## 空闲空间列表

跟踪在存储中的所有未分配的数据块

空闲空间列表存储在哪里?

空闲空间列表的最佳数据结构怎么样?

用位图代表空闲数据块列表: 11111101101110111 如果 i = 0表明数据块i是空闲的,反之是分配的

使用简单但是可能会是一个big vector:

160GB disk → 40M blocks → 5MB worth of bits

然而,如果空闲空间在磁盘中均匀分布,那么再找到"0"之前需要扫描 磁盘上数据块总数 / 空闲块的数目

需要保护:

指向空闲列表的指针

位图:

必须保存在磁盘上;在内存和磁盘拷贝可能有所不同;不允许block[i]在内存中的状态为bit[i]=1而在磁盘中bit[i]=0

解决:

在磁盘上设置bit[i] = 1; 分配block[i]; 在内存中设置bit[i] = 1

## 多磁盘管理 - RAID

通常磁盘通过分区来最大限度减小寻道时间:

一个分区是一个柱面的集合

每个分区都是逻辑上独立的磁盘

分区: 硬件磁盘的一种适合操作系统指定格式的划分

卷: 一个拥有一个文件系统实例的可访问的存储空间(通常常驻在磁盘的单个分区上)

使用多个并行磁盘来增加: 吞吐量(通过并行),可靠性和可用性(通过冗余)

RAID - 冗余磁盘阵列: 各种磁盘管理技术;RAID levels: 不同RAID分类,如RAID-0,RAID-1,RAID-5

实现: 在操作系统内核: 存储,卷管理; RAID硬件控制器(IO)

RAID-0

数据块分成多个子块, 存储在独立的磁盘中: 和内存交叉相似

通过更大的有效块大小来提供更大的磁盘带宽

RAID-1

可靠性成倍增长

读取性能线性增加(向两个磁盘写入,从任何一个读取)

RAID-4

数据块级磁带配有专用奇偶校验磁盘: 允许从任意一个故障磁盘中恢复

条带化和奇偶校验按byte-by-byte或者bit-by-bit: RAID-0,4,5: block-wise ;RAID-3: bit-wise

RAID-5

每个条带快有一个奇偶校验块,允许有一个磁盘错误

RAID-6

两个冗余块,有一种特殊的编码方式,允许两个磁盘错误

## 磁盘调度

读取或写入时,磁头必须被定位在期望的磁道,并从所期望的扇区开始

寻道时间: 定位到期望的磁道所花费的时间

旋转延迟: 从扇区的开始处到到达目的处花费的时间

平均旋转延迟时间 = 磁盘旋转一周时间的一半

寻道时间是性能上区别的原因

对单个磁盘,会有一个IO请求数目

如果请求是随机的,那么会表现很差

FIFO:

按顺序处理请求

公平对待所有进程

在有很多进程的情况下,接近随机调度的性能

最短服务优先:

选择从磁臂当前位置需要移动最少的IO请求

总是选择最短寻道时间

skan:

磁臂在一个方向上移动,满足所有为完成的请求,直到磁臂到达该方向上最后的磁道

调换方向

c-skan:

限制了仅在一个方向上扫描

当最后一个磁道也被访问过了后,磁臂返回到磁盘的另外一端再次进行扫描

c-loop(c-skan改进):

磁臂先到达该方向上最后一个请求处,然后立即反转
