 # 深入理解操作系统 第六章
 很好的一个帖子（https://blog.csdn.net/lililuni/article/details/83685463）
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f1796830-c6f2-4d49-addf-523ab04fccc6)


>   第六章的主要内容是：操作系统的虚拟内存管理技术中的页面置换算法

## 功能与目标
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8e3fb048-9251-44e4-b232-43aa8f1207de)

功能 : 当缺页中断发生, 需要调入新的页面而内存已满时, 选择内存当中哪个物理页面被置换.

目标 : 尽可能地减少页面的换进换出次数(即缺页中断的次数). 具体来说, 把未来不再使用的或短期内较少使用的页面换出, 通常只能在局部性原理指导下依据过去的统计数据来进行预测.

页面锁定 : 用于描述必须常驻内存的操作系统的关键部分或时间关键的应用进程. 实现的方式是 : 在页表中添加锁定标记位(lock bit).

## 实验设置与评价方法

实例 :

记录一个进程对页访问的一个轨迹
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/b2fcc9f0-800d-4f3d-ae9a-3648443fad4a)

-   举例 : 虚拟地址跟踪(页号, 偏移)...
    -   (3,0) (1,9) (4,1) (2,1) (5,3) (2,0) ...
-   生成的页面轨迹
    -   3, 1, 4, 2, 5, 2, 1, ...

模拟一个页面置换的行为并且记录产生页缺失数的数量

-   更少的缺失, 更好的性能
## 页面置换算法的分类
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/54d7b4d6-6f13-47c9-b848-80f5a7e34548)


## 局部页面置换算法

### 最优页面置换算法
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4c310aa3-eb6c-4b96-a658-9384279f60e6)

基本思路 : 当一个缺页中断发生时, 对于保存在内存当中的每一个逻辑页面, 计算在它的下一次访问之前, 还需等待多长时间, 从中选择等待时间最长的那个, 作为被置换的页面.

这是一种理想情况, 在实际系统中是无法实现的, 因为操作系统无法知道每一个页面要等待多长时间以后才会再次被访问.

可用作其他算法的性能评价的依据.(在一个模拟器上运行某个程序, 并记录每一次的页面访问情况, 在第二遍运行时即可使用最优算法)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f552b910-d571-4329-bdd8-6043825e6cc3)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d01ea84c-6532-4840-bcb8-5749e10b2cc5)


### 先进先出算法
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/2c2f43d1-0811-4467-9c2e-2ae7340ed9e3)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/1baf3839-5dbb-483c-abae-30f4363e365c)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8e3bcfe3-009f-495b-9779-be502be32c98)

### 最近最久未使用算法
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/c26f9227-6bc8-4fe4-b4be-0b7dc65de53e)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/695e459c-ccab-477c-93fc-43c30c7e6365)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/9f3669b4-e9d4-4a74-ad0a-711b06e15dad)

LRU(Least Recently Used)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/7e348c67-89cd-4767-b5d7-790cb664b8d0)

不管使用以上那种算法，每次访问一个页面时都需要查找链表或堆栈中是否已经存在，这些开销是非常大的。操作系统要尽可能的高效，同时耗时也要尽可能的少（简洁）。**LRU算法确实达到了较好的效果，但是比较耗时，不方便在操作系统中实现**
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a30a1081-288a-4282-9fa3-409699a245a9)

### 时钟页面置换算法
Clock页面置换算法，LRU的近似，对FIFO的一种改进
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/878bac0b-8aa8-4eaa-8a6d-a0a76a4ae756)

需要用到页表项的访问位, 当一个页面被装入内存时, 把该位初始化为0. 然后如果这个页面被访问, 则把该位置设为1;这个置1的过程是由硬件来完成的，当然软件也可以主动的完成置1和置0的操作。
流程 :
如果访问页在物理内存中, 访问位置1.
如果不在物理页, 从指针当前指向的物理页开始, 如果访问位0, 替换当前页, 指针指向下一个物理页; 如果访问位为1, 置零以后访问下一个物理页再进行判断. 如果所有物理页的访问位都被清零了, 又回到了第一次指针所指向的物理页进行替换.
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/1f6cf432-5db0-4144-ac47-39091b5467f0)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/683a1b6a-7255-4577-b888-4b7900f74b87)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4677f7fa-a493-4e14-96fb-bf3ec5db14e6)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/2fa47422-1e18-404f-abaf-deda56da34ac)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/96137597-bab3-4d10-a58c-cb1ed562937b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/b947cbb4-a4be-449d-b0e3-67d3026a71a6)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/60bef8e8-870f-4c8e-8fc9-d55331be0f51)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4a7595f2-7012-427c-a411-781fcf5bbd81)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/2bd497c9-3305-43f9-9ae3-a84da684985e)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/6b4c44c1-babb-41db-8623-993ee1696fea)


### 二次机会算法
页表项的flag位：resident bit（存在位） accessed bit（used bit——访问位）  dirty bit（有过写操作）
当应用程序对某个页访问之后，如果是写操作，硬件会将used_bit和dirty_bit都置1，如果是读操作，硬件仅会将used_bit置1
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a2ba8cf8-eff1-42d9-ac9b-9cbcd775490a)

因为考虑到时钟页面置换算法, 有时候会把一些 dirty bit 为1(有过写操作)的页面进行置换, 这样的话, 代价会比较大. 因此, 可以结合访问位和脏位一起来决定应该置换哪一页.
这样会使得经过读操作的页面可以经过多次时钟头的扫描，也就是说, 替换的优先级, 没有读写也没写过, 那么直接走, 如果写过或者访问过, 那么给你一次机会, 如果又写过, 又访问过, 那么久给你两次机会.使得被写过的页被替换出去的概率减少，减少了访问硬盘的时间。
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/51d582db-0bb8-4a22-8978-348a88456209)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/dec8588e-664d-4a93-aed3-1c9bc28a8a1d)


### 最不常用算法

Least Frequently used, LFU
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/65586ca3-f376-4686-a9f3-b67f6929e952)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f4c7b0c8-5a9d-41b2-ad8f-74d0a28d8a0d)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e54f504c-1a30-4ecd-b118-7ec0ec3d0d97)


### Belady现象(科学家名字)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/28d0bca2-9be5-48ec-ac1e-ff32eec597f6)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/7c002473-291d-4375-9fb3-e31ba418e071)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f919bdcc-53f7-4e3b-8148-0604bedd8ef4)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/83afcf29-f492-4b76-bf1b-3d2c300e6442)


### LRU / FIFO 和 Clock 的比较
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/83f94442-fea0-496e-a529-eb9a6c14bacc)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/7932c61a-24ce-4049-8261-914ced4616c5)


## 全局页面置换算法
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4e4938f3-5527-4218-beda-029e5b01995d)
局部页面置换算法都假定为每个程序分配固定的物理页帧，然而程序在运行过程中，需要的物理页帧的数量是不断变化的，程序运行刚开始可能需要很多物理页帧，但是随后该程序不需要了那么多物理页帧了，如果继续为该程序分配固定数量的物理页帧的话会影响操作系统的性能。而全局页面置换算法可以动态地调整为每个进程分配的物理页帧的数量。
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/6ef836c2-d9f1-4113-8fa0-0b9ea6778aca)

### 工作集模型
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/22d3bbfe-21f3-4be0-b1cf-b7b4600e6dc5)


### 工作集
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f9d4529b-17a2-42c3-89af-e849c9ebc256)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/7d323e12-70fe-4a4a-ae46-45e8d9fbe4a7)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8c73a000-67f2-47ae-bd7e-ab72cbe4f567)


### 常驻集
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/6af4c970-3561-4e71-99e2-e84618aef9b0)

### 工作集页置换算法

当工作集窗口在滑动过程中, 如果页面不在集合中, 那么就会直接丢失这个不在窗口中页面, 而不会等待缺页中断再丢弃.

### 缺页率置换算法

可变分配策略 : 常驻集大小可变. 例如 : 每个进程在刚开始运行的时候, 先根据程序大小给它分配一定数目的物理页面, 然后在进程运行过程中, 再动态地调整常驻集的大小.

-   可采用全局页面置换的方式, 当发生一个缺页中断时, 被置换的页面可以是在其他进程当中, 各个并发进程竞争地使用物理页面.
-   优缺点 : 性能较好, 但增加了系统开销.
-   具体实现 : 可以使用缺页率算法来动态调整常驻集的大小.

缺页率 : 表示 "缺页次数 / 内存访问次数"

影响因素 : 页面置换算法, 分配给进程的物理页面数目, 页面本身的大小, 程序的编写方法.

### 抖动问题

-   如果分配给一个进程的物理页面太少, 不能包含整个的工作集, 即常驻集 属于 工作集, 那么进程将会造成很多的缺页中断, 需要频繁的在内存与外存之间替换页面, 从而使进程的运行速度变得很慢, 我们把这种状态称为 "抖动".
-   产生抖动的原因 : 随着驻留内存的进程数目增加, 分配给每个进程的物理页面数不断就减小, 缺页率不断上升. 所以OS要选择一个适当的进程数目和进程需要的帧数, 以便在并发水平和缺页率之间达到一个平衡.
