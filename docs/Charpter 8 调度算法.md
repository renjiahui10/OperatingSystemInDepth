# 深入理解操作系统 第八章
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/79636134-e265-4770-90e3-5a609064717e)


>   第八章的主要内容是：调度算法(感觉清华这门课程前几章比较精彩,后续讲的有点混乱)

## 背景
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/3c7129cb-c934-4701-82b7-4442c2756fe1)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/3abf7b4e-440d-4091-a08a-3b33a4a23eed)

上图答：主要和running态相关的状态转换，例如：运行态到就绪态、就绪态到运行态和运行态到阻塞态等
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4ea066f5-470f-441c-a368-2c0e91458441)

### 上下文切换

-   切换CPU的当前任务, 从一个进程/线程到另一个
-   保存当前进程/线程在PCB/TCB中的执行上下文(CPU状态)
-   读取下一个进程/线程的上下文

### CPU调度

-   从就绪队列中挑选一个进程/线程作为CPU将要运行的下一个进程/线程
-   调度程序: 挑选进程/线程的内核函数(通过一些调度策略)
-   什么时候进行调度?

### 内核运行调度程序的条件(满足一条即可)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/76f9016e-3e8a-475a-a7eb-95df339f58ec)

**内核级的调度策略**
当一个进程执行系统调用，此时CPU交由操作系统控制，当系统调用返回时，如果还是原进程称为内核级不可抢占。如果以为某些事件的发生，进程可以切换成其他进程，系统调用返回时不再时原进程，称为内核级可以抢占。

## 调度准则
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/9df76a5a-e888-40f9-93f7-6e043c78c835)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4767e723-d3d3-4f7a-b2f5-973b977a51d2)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5d4c8622-0701-4bd0-901f-90089336b1ea)

从上面的折线图中可以看到，大部分的CPU计算时间在8ms以内，所以要选择合适的时间片基本单位
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/9c6d3233-4ed4-469c-bcfa-0f2f1e835e75)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/811f5698-6bd6-45b8-ae8e-558034264fa5)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/86efb49a-4eb8-4167-a65d-4e0ab73a815b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/dfacb40f-69ec-4d73-9f06-505b3281d59e)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/ad7f870d-4a00-4ef3-afe8-9e82071aae0b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/76ac8885-6303-47c2-ab9e-c11070cdd1a3)
**评价指标**
-CPU使用率: CPU处于忙状态所占时间的百分比

-吞吐量: 在单位时间内完成的进程数量

-周转时间: 一个进程从初始化到结束,包括所有等待时间所花费的时间

-等待时间: 进程在就绪队列中的总时间

-响应时间: 从一个请求被提交到产生第一次相应所花费的总时间

各指标在操作系统上的表现:

低延迟调度增加了交互式表现(如果移动了鼠标,但是屏幕中的光标却没动,我们可能会重启电脑)

操作系统需要保证低吞吐量不受影响(我想要结束长时间的编程,所以操作系统必须不时进行调度,即使存在许多交互任务)

吞吐量是操作系统的计算带宽

响应时间是操作系统的计算延迟

## 调度算法
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/493bfdc3-1050-4090-8ad4-7f79ad16862b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/90b4b14b-0ae9-42d2-a060-c8235348479b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/324209d7-69bd-4622-aab8-74bf59cf0503)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/b9731918-c0d8-4845-aac4-f400cb01622c)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/ed0165bb-a791-405f-8530-9832ae12a0d5)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/29521697-c29c-400c-ba9d-e4ac083c2e69)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/9dde6c5d-7cb9-43e6-9007-4e731e883460)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/ee2bb0cd-0a0b-46ed-b0af-7c07576a9f95)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4813ad2f-dd03-4405-9c97-af3af32de591)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/ca2a85a1-bcfe-4557-8012-8aeed1290c50)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/c8242aeb-9499-43af-990c-e7893f64621a)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/b2791d3b-fb23-4b06-9c13-969b9d7999f5)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/3ff083e6-a964-416c-8edc-3460becd3885)

目前系统大概10ms一个时间片，上面的时间片轮转算法结合了时间片和先来先服务
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5c4fc848-f1b2-4298-9a6d-024fb70a16f0)

上面的算法结合了时间片轮转算法和先来先服务算法，前台（交互）要求时间片段，后台计算量比较大，可能需要先来先服务算法。
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/015e4ecd-f4c1-4070-95ba-2bf0aab3aad7)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/7014f086-9194-4179-befb-410e77feb0af)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/9e093c84-19ce-44d4-a9d7-b9849016d826)

多级反馈队列是是多级队列的改进，
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8e899521-e9cf-451a-b1b9-83ca931a772b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/967a30a1-60a7-4931-b2c4-e4cd175ab889)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/99bbe00b-556e-4615-b7d4-51e56d254e8f)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e5e83d36-c075-4799-93b3-97cfd5c6acd4)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/bf7fb207-6c50-4855-a08c-7b3aabe081bc)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/1a543334-872d-441d-84ba-8be2005b3891)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/fa9772bc-b3f4-41bc-bcf2-6864c73fa236)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/3e62b7b9-6d06-4fd8-a320-1343f2797722)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5a223fcb-a32f-4829-8f85-f31bde146def)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/b5494e43-4ea0-4066-b858-ea8ab9bb0838)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/6d5831f4-9e3f-4ae5-a4c0-d8a5bad9194a)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/2b32f127-ed6f-4376-b748-55828a2ffc3e)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4937548e-b097-4837-86d7-f844aa2175c7)

调度的时机发生在：A程序发生中断或者系统调用，有用户态转变为内核态，执行完中断或系统调用处理以后，有内核态返回用户态时，来执行调度，也就是上图中的步骤2
-   **FCFS(先来先服务)**

    First come, First Served

    如果进程在执行中阻塞,队列中的下一个会得到CPU

    优点: 简单

    缺点:

    -   平均等待时间波动较大
    -   花费时间少的任务可能排在花费时间长的任务后面
    -   可能导致IO和CPU之间的重叠处理(CPU密集型进程会导致IO设备闲置时,IO密集型进程也在等待)

-   **SPN(SJF) SRT(短进程优先(短作业优先)短剩余时间优先)[最优平均等待时间]**

    Shortest Process Next(Shortest Job First) Shortest Remaining Time

    选择预测的完成时间来将任务入队

    可以是抢占的或者是不可抢占的

    可能导致饥饿

    -   连续的短任务流会使场任务饥饿
    -   短任务可用时的任何场任务的CPU时间都会增加平均等待时间

    需要预测未来

    -   怎么预估下一个CPU突发的持续时间
    -   简单的解决: 询问用户
    -   如果用户欺骗就杀死进程
    -   如果不知道怎么办?

-   **HRRN(最高响应比优先)**

    Highest Response Ratio Next

-   **Round Robin(轮循)**

    使用时间切片和抢占来轮流执行任务

    在叫做量子(或者时间切片)的离散单元中分配处理器

    时间片结束时,切换到下一个准备好的进程

    花销: 额外的上下文切换

    时间量子太大:

    -   等待时间过长
    -   极限情况退化成FCFS

    时间量子太小:

    -   反应迅速
    -   吞吐量由于大量的上下文切换开销受到影响

    目标:

    -   选择一个合适的时间量子
    -   经验规则: 维持上下文切换开销处于1%以内

-   **Multilevel Feedback Queues(多级反馈队列)**

    优先级队列中的轮循

    就绪队列被划分成独立的队列: 比如前台(交互),后台(批处理)

    每个队列拥有自己的调度策略: 比如前台(RR),后台(FCFS)

    调度必须在队列间进行:

    -   固定优先级: 先处理前台,然后处理后台;可能导致饥饿
    -   时间切片: 每个队列都得到一个确定的能够调度其进程的CPU总时间;比如80%使用RR的前台,20%使用FCFS的后台

    一个进程可以在不同的队列中移动

    例如,n级优先级-优先级调度在所有级别中,RR在每个级别中

    -   时间量子大小随优先级级别增加而增加
    -   如果任务在当前的时间量子中没有完成,则降到下一个优先级

    优点: CPU密集型任务的优先级下降很快;IO密集型任务停留在高优先级

-   **Fair Share Scheduling(公平共享调度)**

    FSS控制用户对系统资源的访问

    -   一些用户组比其他用户组更重要
    -   保证不重要的组无法垄断资源
    -   未使用的资源按照每个组所分配的资源的比例来分配
    -   没有达到资源使用率目标的组获得更高的优先级

## 评价方式

确定性建模: 确定一个工作量,然后计算每个算法的表现

队列模型: 用来处理随机工作负载的数学方法

实现/模拟: 建立一个允许算法运行实际数据的系统;最灵活,最具一般性

## 实时调度

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/59b1fc9d-1102-4d1c-9e26-39d3dc865298)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/ee9ec5d8-f2d8-48da-a5fd-4e1d39c83762)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/eb5e149f-4c3a-40a1-90a0-cd1a5351b1f1)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/07486c6a-45f1-472c-b02b-63dd0985bb8e)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0b6e85a1-95b7-4b10-8c41-ff960c963614)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5cc0b31a-ce20-4554-8a2b-8de04e1f56af)




-   实时系统

    定义: 正确性依赖于其时间和功能两方面的一个操作系统

    性能指标: 时间约束的及时性;速度和平均性能相对不重要

    主要特征: 时间约束的可预测性

    分类:

    -   强实时系统: 需要在保证时间内完成重要的任务,必须完成
    -   弱实时系统: 要求重要的进程的优先级更高,尽量完成,并非必须

    任务(工作单元): 一次计算,一次文件读取,一次信息传递等

    属性: 去的进展所需要的资源;定时参数.

-   单调速率(RM)

    -   最佳静态优先级调度
    -   通过周期安排优先级
    -   周期越短优先级越高
    -   执行周期最短的任务

-   截止日期最早优先(EDF)

    -   最佳的动态优先级调度
    -   Deadline越早优先级越高
    -   执行Deadline最早的任务

## 多处理器调度
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e6d36c12-3d04-4a86-aece-a95834b173e3)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/1c080f7a-7b2f-4d85-86a2-3c40e932be0a)


多处理器的CPU调度更复杂:

-   多个相同的单处理器组成一个多处理器
-   优点: 复杂共享

对称多处理器(SMP)

-   每个处理器运行自己的调度程序
-   需要在调度程序中同步

## 优先级反转
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/fe1158f9-0e9a-438c-a91a-366637a55a10)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d1bd3a05-7782-44c0-89f4-ce399160bea2)

上图中，优先级T1>T2>T3,临界区：每个进程中访问临界资源的那段代码称为临界区（Critical Section）（临界 资源是一次仅允许一个进程使用的共享资源）。每次只准许一个进程进入临界区， 进入后不允许其他进程进入。不论是硬件临界资源，还是软件临界资源，多个进程 必须互斥地对它进行访问。（https://blog.51cto.com/yang/2892072）
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/1b77532d-8a92-411f-965a-88f2eb6f0ca3)

可以发生在任务基于优先级的可抢占的调度机制中

当系统内的环境强制使高优先级任务等待低优先级任务时发生


