# 深入理解操作系统 第九章

>   第九章的主要内容是：同步互斥
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5f9b442c-2088-4436-b926-7e1fdfe7dd25)

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

临界区(Critical section)是指进程中的一段需要访问共享资源并且当另一个进程处于相应代码区域时便不会被执行的代码区域

互斥(Mutual exclusion)是指当一个 进程处于临界区并访问共享资源时,没有其他进程会处于临界区并且访问任何相同的共享资源

死锁(Dead lock)是指两个或以上进程,在相互等待完成特定任务,而最终没法将自身任务进行下去

饥饿(Starvation)是指一个可执行的进程,被调度器持续忽略,以至于虽然处于可执行状态却不被执行

## 临界区

互斥: 同一时间临界区中最多存在一个线程

Progress: 如果一个线程想要进入临界区,那么它最终会成功

有限等待: 如果一个线程i处于入口区,那么在i的请求被接受之前,其他线程进入临界区的时间是有限制的

无忙等待(可选): 如果一个进程在等待进入临界区,那么在它可以进入之前会被挂起

## 方法1:禁用硬件中断

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

硬件提供了一些原语

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

