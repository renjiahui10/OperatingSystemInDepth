# 深入理解操作系统 第九章

>   第九章的主要内容是：同步互斥
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5f9b442c-2088-4436-b926-7e1fdfe7dd25)
https://blog.csdn.net/liming0931/article/details/82902084#:~:text=%E5%90%8C%E6%AD%A5%20%EF%BC%9A%E6%98%AF%E6%8C%87%E6%95%A3%E6%AD%A5%E5%9C%A8%E4%B8%8D%E5%90%8C%E4%BB%BB%E5%8A%A1%E4%B9%8B%E9%97%B4%E7%9A%84%E8%8B%A5%E5%B9%B2%E7%A8%8B%E5%BA%8F%E7%89%87%E6%96%AD%EF%BC%8C%E5%AE%83%E4%BB%AC%E7%9A%84%E8%BF%90%E8%A1%8C%E5%BF%85%E9%A1%BB%E4%B8%A5%E6%A0%BC%E6%8C%89%E7%85%A7%E8%A7%84%E5%AE%9A%E7%9A%84%E6%9F%90%E7%A7%8D%E5%85%88%E5%90%8E%E6%AC%A1%E5%BA%8F%E6%9D%A5%E8%BF%90%E8%A1%8C%EF%BC%8C%E8%BF%99%E7%A7%8D%E5%85%88%E5%90%8E%E6%AC%A1%E5%BA%8F%E4%BE%9D%E8%B5%96%E4%BA%8E%E8%A6%81%E5%AE%8C%E6%88%90%E7%9A%84%E7%89%B9%E5%AE%9A%E7%9A%84%E4%BB%BB%E5%8A%A1%E3%80%82%20%E6%9C%80%E5%9F%BA%E6%9C%AC%E7%9A%84%E5%9C%BA%E6%99%AF%E5%B0%B1%E6%98%AF%EF%BC%9A%E4%B8%A4%E4%B8%AA%E6%88%96%E4%B8%A4%E4%B8%AA%E4%BB%A5%E4%B8%8A%E7%9A%84%E8%BF%9B%E7%A8%8B%E6%88%96%E7%BA%BF%E7%A8%8B%E5%9C%A8%E8%BF%90%E8%A1%8C%E8%BF%87%E7%A8%8B%E4%B8%AD%E5%8D%8F%E5%90%8C%E6%AD%A5%E8%B0%83%EF%BC%8C%E6%8C%89%E9%A2%84%E5%AE%9A%E7%9A%84%E5%85%88%E5%90%8E%E6%AC%A1%E5%BA%8F%E8%BF%90%E8%A1%8C%E3%80%82%20%E6%AF%94%E5%A6%82%20A,%E4%BB%BB%E5%8A%A1%E7%9A%84%E8%BF%90%E8%A1%8C%E4%BE%9D%E8%B5%96%E4%BA%8E%20B%20%E4%BB%BB%E5%8A%A1%E4%BA%A7%E7%94%9F%E7%9A%84%E6%95%B0%E6%8D%AE%E3%80%82%20%E6%98%BE%E7%84%B6%EF%BC%8C%E5%90%8C%E6%AD%A5%E6%98%AF%E4%B8%80%E7%A7%8D%E6%9B%B4%E4%B8%BA%E5%A4%8D%E6%9D%82%E7%9A%84%E4%BA%92%E6%96%A5%EF%BC%8C%E8%80%8C%E4%BA%92%E6%96%A5%E6%98%AF%E4%B8%80%E7%A7%8D%E7%89%B9%E6%AE%8A%E7%9A%84%E5%90%8C%E6%AD%A5%E3%80%82%20%E4%B9%9F%E5%B0%B1%E6%98%AF%E8%AF%B4%E4%BA%92%E6%96%A5%E6%98%AF%E4%B8%A4%E4%B8%AA%E4%BB%BB%E5%8A%A1%E4%B9%8B%E9%97%B4%E4%B8%8D%E5%8F%AF%E4%BB%A5%E5%90%8C%E6%97%B6%E8%BF%90%E8%A1%8C%EF%BC%8C%E4%BB%96%E4%BB%AC%E4%BC%9A%E7%9B%B8%E4%BA%92%E6%8E%92%E6%96%A5%EF%BC%8C%E5%BF%85%E9%A1%BB%E7%AD%89%E5%BE%85%E4%B8%80%E4%B8%AA%E7%BA%BF%E7%A8%8B%E8%BF%90%E8%A1%8C%E5%AE%8C%E6%AF%95%EF%BC%8C%E5%8F%A6%E4%B8%80%E4%B8%AA%E6%89%8D%E8%83%BD%E8%BF%90%E8%A1%8C%EF%BC%8C%E8%80%8C%E5%90%8C%E6%AD%A5%E4%B9%9F%E6%98%AF%E4%B8%8D%E8%83%BD%E5%90%8C%E6%97%B6%E8%BF%90%E8%A1%8C%EF%BC%8C%E4%BD%86%E4%BB%96%E6%98%AF%E5%BF%85%E9%A1%BB%E8%A6%81%E5%AE%89%E7%85%A7%E6%9F%90%E7%A7%8D%E6%AC%A1%E5%BA%8F%E6%9D%A5%E8%BF%90%E8%A1%8C%E7%9B%B8%E5%BA%94%E7%9A%84%E7%BA%BF%E7%A8%8B%EF%BC%88%E4%B9%9F%E6%98%AF%E4%B8%80%E7%A7%8D%E4%BA%92%E6%96%A5%EF%BC%89%EF%BC%81%20%E5%9B%A0%E6%AD%A4%E4%BA%92%E6%96%A5%E5%85%B7%E6%9C%89%E5%94%AF%E4%B8%80%E6%80%A7%E5%92%8C%E6%8E%92%E5%AE%83%E6%80%A7%EF%BC%8C%E4%BD%86%E4%BA%92%E6%96%A5%E5%B9%B6%E4%B8%8D%E9%99%90%E5%88%B6%E4%BB%BB%E5%8A%A1%E7%9A%84%E8%BF%90%E8%A1%8C%E9%A1%BA%E5%BA%8F%EF%BC%8C%E5%8D%B3%E4%BB%BB%E5%8A%A1%E6%98%AF%E6%97%A0%E5%BA%8F%E7%9A%84%EF%BC%8C%E8%80%8C%E5%90%8C%E6%AD%A5%E7%9A%84%E4%BB%BB%E5%8A%A1%E4%B9%8B%E9%97%B4%E5%88%99%E6%9C%89%E9%A1%BA%E5%BA%8F%E5%85%B3%E7%B3%BB%E3%80%82
## 背景
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/edbfe406-e058-4a46-9470-df2260eb123b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8e21cbad-9b33-447c-a29e-53ed4a63b0ee)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f556298b-f7a1-4455-ae10-f7d00c9bd1fb)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/803e255a-1c55-43ee-aaae-c501c28e9ac2)

第一章到第八章内容, 到目前为止

-   多道程序设计: 现代操作系统的重要特性
-   并行很有用(为什么?) 提示: 多个并发实体: CPU IO 用户 等
-   进程,线程: 操作系统抽象出来用于支持多道程序设计
-   CPU调度: 实现多道程序设计的机制
-   调度算法: 不同的策略

独立的线程:

-   不和其他线程共享资源或状态
-   确定性: 输入状态决定结果
-   可重现: 能够重现起始条件, IO
-   调度顺序不重要

合作线程:

-   在多个线程中共享状态
-   不确定性
-   不可重现

不确定性和不可重现意味着bug可能是间歇性发生的

进程,线程;计算机,设备需要合作

合作优点:

1.  共享资源
    -   一台电脑,多个用户
    -   一个银行存款余额,多台ATM机
    -   嵌入式系统
2.  加速
    -   IO操作和计算可以重叠
    -   多处理器
3.  模块化
    -   将大程序分解成小程序 gcc会调用cpp,cc1,cc2,as,ld
    -   使系统易于扩展

程序可以调用函数fork()来创建一个新的进程

-   操作系统需要分配一个新的并且唯一的进程ID
-   因此在内核中,这个系统调用会运行 new_pid = next_pid++;
-   翻译成机器指令:
    -   Load next_pid Reg1
    -   STORE Reg1 new_pid
    -   INC Reg1
    -   STORE Reg1 next_pid
 
**举例说明**
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/127abe81-a32e-4300-a958-9bc1f7d2e29b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/998a17e6-f28c-421d-83b1-2b0507831482)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/43273788-2545-4945-8725-3d9345af8c36)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0ad1ca85-d17f-4efc-88a1-eaa8627a9ffb)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0a9021b8-ed3a-4650-b9fb-f255f52a0287)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8d27a0ed-aaf8-49c8-9b95-634b363cb4ec)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4c166dfd-6be1-4eb9-b5cb-d14ab305320e)

**现实生活中举例**
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/b8b41266-5811-4add-b9da-541e0b0b5b84)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5be52d60-6958-41fc-9044-0291a053038c)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0bf7dfa0-6d91-4f6a-b86a-f4095fc68d74)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/7b46e0fe-3bdb-4fd8-b689-e404da0f12cb)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/3e321a07-25be-4825-93c3-19d20b95cbf4)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/84d8a3d8-d0da-4941-b6ee-691b86897595)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/ec09002a-5fe6-4d7d-ab73-d2dc7945842f)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/2d88be61-668e-45be-b9a7-6dd2081cd44a)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/de3f8b71-8829-4eba-b0b4-2eeb29625d90)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/b4bed24d-539f-466b-aa68-8b581ac6f8eb)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/c9ffa014-6dda-43b3-874d-ab93ab4f1279)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/25363d14-e7a3-4a1d-ab51-e079f3e7aec6)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/26bae3b7-032a-412e-9a79-d4c06bdf1299)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/1b570932-7ca8-49ca-bdce-dfcd37281b15)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/31889559-15a8-44fd-af90-5950f9b8ce40)

上述方案即使只有一个进程在操作，也没有办法把面包买回来。
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/1c3120a6-b70f-462e-93e8-4ef64420dc45)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/250c277e-d464-42c8-8366-8d75a0205fdc)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/605f6970-c22b-4592-a519-4370c0c11e14)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/475f6905-5d0b-4ed5-a8aa-c4efffdf554c)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5d8c729a-a9ef-4150-b362-e01003e18488)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/52998574-209a-48a1-a6a5-2900569c9d9b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/562b80b0-beea-4453-bbab-d77500a0071c)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/2f5326aa-cc39-4150-bf56-8f655f4a7419)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f2b7fa20-b460-4aa8-bb51-eb0ffdef9171)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/28e3770d-25a2-4095-9917-420f40748f1b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5eb48494-9261-4fdb-9282-58b66040e032)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/b7b225b0-61f3-4fcd-8fa6-105ca54d5853)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0aabe29d-b0e5-4162-857e-93f790c0793b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/cef92ec6-15b5-40ad-80ca-009709aa98b4)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/6bc4d138-287c-4e35-8bbe-02ede5f0a0c9)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4cc45674-5ab6-4d2c-9130-6b398d24787c)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8e428b2d-a997-4c8f-a611-a4076466547d)

在操作系统中由于存在资源共享，出现了互斥、死锁、饥饿等现象。




所谓的原子操作也就是指，一组必须绑定在一起执行的操作，不能中断的只执行一部分一组操作中的一部分而转去执行其他操作。
假设两个进程并发执行

-   如果next_pid等于100, 那么其中一个进程得到的ID应该是100, 另一个进程的ID应该是101, next_pid应该增加到102
-   可能在INC前进行了上下文切换, 最终导致两个进程的pid都是100,而next_pid也是101

无论多个线程的指令序列怎样交替执行,程序都必须正常工作

-   多线程程序具有不确定性和不可重现的特点
-   不经过专门设计,调试难度很高

不确定性要求并行程序的正确性

-   先思考清楚问题,把程序的行为设计清楚
-   切忌给予着手编写代码,碰到问题再调试

## 一些概念

前面的现象称为Race Condition(竞态条件)

系统缺陷: 结果依赖于并发执行或者时间的顺序,时间

-   不确定性
-   不可重现

怎么样避免竞态?

Atomic Operator(原子操作)

原子操作是指一次不存在任何终端或者失败的执行

-   该执行成功结束
-   或者根本没有执行
-   并且不应发生任何部分执行的状态

实际上操作往往不是原子的

-   有些看上去是原子操作,实际上不是
-   连x++这样的简单语句,实际上是由三条指令构成的
-   有时候甚至连单条假期指令都不是原子的(Pipeline,super-scalar,out-of-order,pape fault)

临界区(Critical section)是指进程中的一段需要访问共享资源并且当另一个进程处于相应代码区域时便不会被执行的代码区域（**每个进程中访问临界资源的那段代码称为临界区（Critical Section）**）

互斥(Mutual exclusion)是指当一个 进程处于临界区并访问共享资源时,没有其他进程会处于临界区并且访问任何相同的共享资源

死锁(Dead lock)是指两个或以上进程,在相互等待完成特定任务,而最终没法将自身任务进行下去

饥饿(Starvation)是指一个可执行的进程,被调度器持续忽略,以至于虽然处于可执行状态却不被执行

## 临界区
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/09b8c061-4148-4832-9276-380b8434534e)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/405e951d-8dd3-44fd-b3f9-5517d6e64dad)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/fe5950c7-cca6-455c-9c55-dd6a247ff62b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/336fa979-845d-4030-a91c-69cad1889ea5)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0e39d7dc-3b2f-4891-853b-7a7c4de95f82)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/274e2f01-ea9d-40bb-a114-b371b9d2df10)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/13b49cea-41c6-4aba-a3fd-7e0295ae4006)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/853f2a3a-dfa5-47a3-8127-6ba064b00785)


互斥: 同一时间临界区中最多存在一个线程

Progress: 如果一个线程想要进入临界区,那么它最终会成功

有限等待: 如果一个线程i处于入口区,那么在i的请求被接受之前,其他线程进入临界区的时间是有限制的

无忙等待(可选): 如果一个进程在等待进入临界区,那么在它可以进入之前会被挂起

## 方法1:禁用硬件中断
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0e2081d1-4582-443a-a58f-82b01ba168d7)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/238029c8-22d6-41d3-9835-c051fe345fe1)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e1715434-d02c-4ee6-ac22-aa49a5311603)

由于整个系统都停下来为这个程序停下来服务，由于中断也关闭了，如果这个进程此时出现问题，整个系统就崩溃了，cpu的控制权也回不到操作系统中了。
没有中断,没有上下文切换,因此没有并发

-   硬件将中断处理延迟到中断被启用之后
-   大多数现代计算机体系结构都提供指令来完成

进入临界区

-   禁用中断

离开临界区

-   开启中断

一旦中断被禁用,线程就无法被停止

-   整个系统都会为你停下来
-   可能导致其他线程处于饥饿状态

要是临界区可以任意长怎么办?

-   无法限制响应中断所需的时间(可能存在硬件影响)

要小心使用,适合于较小的操作

## 方法2:基于软件的解决方案
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/62754971-0c50-4f8b-88d8-c04e746dcf5d)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/fa16c4bd-1a18-4a34-a9a8-87e8ee8532c0)

上图中whlie(turn!=1)；是一个完整代码，表示当turn！=1是空循环；
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/93af7f29-51c6-4291-8d05-6c3f231a08f6)

上图中，当线程Ti在刚刚判断完flag(j)==1之后发生中断，跳转到线程Tj，可能发生两线程同时进入临界区。
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/7b13a74c-49d7-43cb-8cd9-dfae3aa4d936)

上图中，在线程Ti贴标签表示想进入临界区时，如果发生中断转向线程Tj，线程Tj也贴了标签，此时两个线程就都无法进入临界区了
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/6b1e6a10-8cc1-4c41-b622-daec55def071)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8da95c8c-596c-4f5f-8d60-438a50b63435)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/dd7bc7f0-d22a-4ba9-8f82-52017ea5e2ac)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d13006c3-2519-4a63-aa67-e66de1a1df1b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/88a75d7e-1b2a-41b1-b133-375ba2cfe8b1)

满足进程Pi和Pj之间互斥的经典的基于软件的解决方法(1981年)

使用两个共享数据项

-   int turn; //指示该谁进入临界区
-   bool flag[]; //指示进程是否准备好进入临界区

进入临界区:

```cpp
flag[i] = true;
turn = j;
while(flag[j] && turn == j);
```

退出临界区:

```cpp
flag[i] = false;
```

实例:

```cpp
do{
	flag[i] = true;
	turn = j;
	while(flag[j] && turn == j);
	CRITICAL SECTION
	flag[i] = false;
	REMAINDER SECTION
}while(true);
```

Bakery 算法(N个进程的临界区)

-   进入临界区之前,进程接收一个数字
-   得到的数字最小的进入临界区
-   如果进程Pi和Pj收到相同的数字,那么如果i<j,Pi先进入临界区,否则Pj先进入临界区
-   编号方案总是按照枚举的增加顺序生成数字

Dekker算法(1965): 第一个针对双线程例子的正确解决方案

Bakery算法(1979): 针对n线程的临界区问题解决方案

复杂: 需要两个进程的共享数据项

需要忙等待: 浪费CPU时间

没有硬件保证的情况下无真正的软件解决方案: Perterson算法需要原子的LOAD和STORE指令

## 方法3:更高级的抽象
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d38be0a3-fd10-4ed3-96da-d7a12193d336)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/c4e9f530-7590-4897-bd69-a373f2ee146f)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/48d11710-c689-4c38-9c1c-c93136336795)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/24aaa227-51f2-4576-83bd-e6ad4b520e17)

**我的理解：应该是针对某一个临界区有一个锁**
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/af12ea7e-356b-43ec-944a-4d8cd8250bec)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/20bff2eb-1554-43a9-9f8e-2d7a4077fdd0)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/cbdea8a0-fcf8-4800-9f83-97987811ea3e)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8cdea554-ac7e-4b9e-be51-e1eff435d49f)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/49b3c91f-743a-4357-aca2-13854af583fc)

之前提到的禁用中断的实现同步互斥的方法只能适合于单处理器，因为多处理器没法一般不使用把全部处理器都中断的方式，只要有一个处理器没用中断，就有可能存在两个处理器同时访问一个临界区。
硬件提供了一些原语
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f6b91f76-4ea8-4afc-ab14-a6b6e5a4840b)

-   像中断禁用, 原子操作指令等
-   大多数现代体系结构都这样

操作系统提供更高级的编程抽象来简化并行编程

-   例如,锁,信号量
-   从硬件原语中构建

锁是一个抽象的数据结构

-   一个二进制状态(锁定,解锁),两种方法
-   Lock::Acquire() 锁被释放前一直等待,然后得到锁
-   Lock::Release() 锁释放,唤醒任何等待的进程

使用锁来编写临界区

-   前面的例子变得简单起来:

    ```cpp
    lock_next_pid->Acquire();
    new_pid = next_pid++;
    lock_next_pid->Release();
    ```

大多数现代体系结构都提供特殊的原子操作指令

-   通过特殊的内存访问电路
-   针对单处理器和多处理器

Test-and-Set 测试和置位

-   从内存中读取值
-   测试该值是否为1(然后返回真或假)
-   内存值设置为1

交换

-   交换内存中的两个值

```cpp
bool TestandSet(bool *target){
		bool rv = *target;
		*target = true;
		return rv;
}

void Exchange(bool *a, bool *b){
		bool tmp = *a;
		*a = *b;
		*b = tmp;
}
```

-   总结

    锁是更高等级的编程抽象

    -   互斥可以使用锁来实现
    -   通常需要一定等级的硬件支持

    常用的三种实现方法

    -   禁用中断(仅限于单处理器)
    -   软件方法(复杂)
    -   原子操作指令(单处理器或多处理器均可)

    可选的实现内容:

    -   有忙等待
    -   无忙等待

