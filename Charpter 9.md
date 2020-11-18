<p align='center'>
<a href="https://oxygenpanda.github.io/" target="_blank"><img alt="Website" src="https://img.shields.io/badge/博客-劳振煜的知識倉儲-faf2f2.svg?style=flat-square&logo=Blogger"></a>
<a href="https://www.github.com/OXygenPanda" target="_blank"><img src="https://img.shields.io/badge/Github-@劳振煜-f3e1e1.svg?style=flat-square&logo=GitHub"></a>
<a href="https://i.loli.net/2020/11/11/SBZ2mFJGKLjUtTO.jpg" target="_blank"><img src="https://img.shields.io/badge/微信-@OXygen-f1d1d1.svg?style=flat-square&logo=WeChat"></a>


# 深入理解操作系统 第九章

>   第九章的主要内容是：同步

## 背景

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
