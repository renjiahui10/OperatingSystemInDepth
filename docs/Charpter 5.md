# 深入理解操作系统 第五章

>   第五章的主要内容是：操作系统的虚拟内存管理技术
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/53d53a7f-09b8-4715-bc97-0c41c96f7ae1)


## 虚拟内存的起因
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0fd552c9-ff31-4a49-a61c-30e582abf21f)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/6e0ddafc-3d82-49f5-aefd-293a97196857)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/33b9e410-f089-4040-84c9-31a43a5a7536)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8a0d7db9-62b6-44ed-9965-0dafb6b19dcd)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/3d7ea014-3812-40fd-9c4b-81fed84088aa)

使用硬盘/磁盘使更多的程序在有限的内存中运行

理想的存储器 : 更大更快更便宜和非易失性的存储区

## 覆盖技术

如果是程序太大, 超出了内存的容量, 可以采用手动的概率(overlay)技术, 只把需要的指令和数据保存在内存当中
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f474454a-7de5-43ee-887a-fab86675318b)


目的 : 是在较小的可用内存中运行较大的程序, 常用于多道程序系统, 与分区存储管理配合使用.

原理 :

把程序按照其自身逻辑结构, 划分为若干个功能上相互独立的程序模块, 那些不会同时执行的模块共享同一块内存区域, 按时间先后来运行.

-   必要部分(常用功能)的代码和数据常驻内存;
-   可选部分(不常用功能)在其他程序模块中实现, 平时存放在外存中, 在需要用到时才装入内存;
-   不存在调用关系的模块不必同时装入到内存, 从而可以相互覆盖, 即这些模块共用一个分区.

>   也就是说,程序松耦合的部分可以按需装入内存,不需要的时候放在外存中,多个不常用部分共用一个分区.

实例 :
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5855e2b5-3a65-4bf3-9e98-c39fb31404f1)

A(20k) ____B(50k) ____ D(30k)
        | ____ C(30k) ____ E(20k)
                             |____ F(40k)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4a46a69d-ccd0-4f57-bb52-a8a429624716)

因此不需要将整个程序190k的数据全部放入内存中, 而是划分为 常驻区(20k) 覆盖区0(50k) 覆盖区1(40k) 压缩至了110k的内存空间使用
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e9939c4b-09ee-4a78-be5f-a6a7169ac950)
八九十年代的Turbo Pascal软件支持覆盖技术
缺点 :

-   由程序员来把一个大的程序划分为若干个小的功能模块, 并确定各个模块之间的覆盖关系, 费时费力, 增加了编程的复杂度;
-   覆盖模块并从外存装入内存, 实际上是以时间延长来换取空间节省.

## 交换技术

如果是程序太多, 超过了内存的容量, 可以采用自动的交换(swapping)技术, 把暂时不能执行的程序送到外存中
（**早期的操作系统unix提供的一种方法，当多个程序在内存中时，可以让操作系统（而不是程序员）进行管理，将暂时不使用的程序整个导入到内存中去**）
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a95332e7-33ba-4906-8960-1a55069ba7ee)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/24938c65-1d48-4f49-acf7-5f1ee5dd7a30)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/801a596f-3b6b-49ba-8385-13d0063f6de3)

## 覆盖技术和交换技术的对比

特点 :

-   覆盖只能发生在那些相互之间没有调用关系的程序模块之间, 因此程序员必须给出程序内的各个模块之间的逻辑覆盖结构.
-   交换技术是以在内存中的程序大小为单位进行的, 它不需要程序员给出各个模块之间的逻辑覆盖结构.
-   换言之, 交换发生在内存中程序与管理程序或操作系统之间, 而覆盖则发生在运行程序的内部.

在内存不够用的情形下, 可以采用覆盖技术和交换技术, 但是 :

-   覆盖技术 : 需要程序要自己把整个程序划分为若干个小的功能模块, 并确定各个模块之间的覆盖关系, 增加了程序员的负担.
-   交换技术 : 以进程作为交换的单位, 需要把进程的整个地址空间都换入换出, 增加了处理器的开销.

## 虚拟内存管理技术

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f8b54e68-a125-4bc2-be33-3ad6543e5451)


![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4a70fe8b-244f-4e23-9a6d-a4444548c91d)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0d9e296f-89f2-435f-ac21-f4bbdea521c5)

如果想要在有限容量的内存中, 以更小的页粒度为单位装入更多更大的程序, 可以采用自动的虚拟存储技术


![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/42e4ebe9-5973-400c-8459-4be986985041)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a169584d-1145-4dda-b1f8-ea2e412f460a)

**要实现高效的虚拟内存管理机制，除了需要“操作系统和硬件”（分段或分页）以外，还需要应用程序很好的满足局部性原理**



![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/bf8fbbbd-f414-495c-ba95-f803a91486ce)

上图中，通过硬盘的补充，使得我们可以在内存中跑多个程序，而且每个程序都会认为自己占用了4GB的内存，



-   基本特征

    -   大的用户空间 : 通过把物理内存和外存相结合, 提供给用户的虚拟内存空间通常大于实际的物理内存, 即实现了这两者的分离. 如32位的虚拟地址理论上可以访问4GB, 而可能计算机上仅有256M的物理内存, 但硬盘容量大于4GB.
    -   部分交换 : 与交换技术相比较, 虚拟存储的调入和调出是对部分虚拟地址空间进行的;
    -   不连续性 : 物理内存分配的不连续性, 虚拟地址空间使用的不连续性.

-   虚拟页式内存管理

    页式内存管理

    页表 : 完成逻辑页到物理页帧的映射

    根据页号去页表中寻找索引, 先查看 resident bit 是否为0, 0表示不存在, 1表示映射关系存在, 获得帧号加上原本的偏移, 获得了物理地址.

    虚拟页式内存管理

    -   大部分虚拟存储系统都采用虚拟页式存储管理技术, 即在页式存储管理的基础上, 增加请求调页和页面置换功能.

    -   基本思路

        -   当一个用户程序要调入内存运行时, 不是将该程序的所有页面都装入内存, 而是只装入部分的页面, 就可启动程序运行.
        -   在运行的过程中, 如果发现要运行的程序或要访问的数据不再内存, 则向系统发出缺页的中断请求, 系统在处理这个中断时, 将外存中相应的页面调入内存, 使得该程序能够继续运行.

    -   页表表项

        逻辑页号 | 访问位 | 修改位 | 保护位 | 驻留位 | 物理页帧号

        驻留位 : 表示该页是在内存中还是在外存.

        保护位 : 表示允许对该页做何种类型的访问, 如只读, 可读写, 可执行等

        修改位 : 表示此页在内存中是否被修改过. 当系统回收该物理页面时, 根据此位来决定是否把它的内容写回外存

        访问位 : 如果该页被访问过(包括读写操作), 则设置此位. 用于页面置换算法.

    -   缺页中断处理过程 :

        1.  如果在内存中有空闲的物理页面, 则分配一物理页帧f, 然后转第4步; 否则转到第2步;
        2.  采用某种页面置换算法, 选择一个将被替换的物理页帧f, 它所对应的逻辑页为q, 如果该页在内存期间被修改过, 则需要把它写回外存;
        3.  对q所对应的页表项修改, 把驻留位置为0;
        4.  将需要访问的页p装入到物理页面f当中;
        5.  修改p所对应的页表项的内容, 把驻留位置为1, 把物理页帧号置为f;
        6.  重新运行被中断是指令.

        >   在何处保存未被映射的页?
        >
        >   -   能够简单地识别在二级存储器中的页
        >   -   交换空间(磁盘或者文件) : 特殊格式, 用于存储未被映射的页面

        后备存储(二级存储) :

        -   一个虚拟地址空间的页面可以被映射到一个文件(在二级存储中)的某个位置
        -   代码段 : 映射到可执行二进制文件
        -   动态加载的共享库程序段 : 映射到动态调用的库文件
        -   其他段 : 可能被映射到交换文件(swap file)

    -   虚拟内存性能

        为了便于理解分页的开销, 使用有效存储器访问时间 effective memory access time (EAT)

        EAT = 访存时间 * 页表命中几率 + page fault处理时间 * page fault几率

        实例 :

        访存时间 : 10 ns

        磁盘访问时间 : 5 ms

        参数 p  = page fault 几率

        参数 q = dirty page 几率(对页面写操作)

        EAT = 10\*(1-p) + 5000000\*p\*(1+q)

