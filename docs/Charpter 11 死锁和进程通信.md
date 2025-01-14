![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5e665f93-074a-475f-8381-031d30ea1db4)# 深入理解操作系统 第十一章

>   第十一章的主要内容是：死锁和进程通信
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/11a6a184-7572-40e4-a32f-0cb51802e253)

## 死锁问题
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/2e1988ef-673a-4998-ab59-27dc52155055)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f44f62fe-1824-456d-add6-fd1a87b32761)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/6a0cf092-03a9-4fdd-8b85-ce8995cdfdb6)


![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/3df19d93-9f60-4d93-954c-a934079da0fa)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0074e8fb-855f-4f69-ba09-e5ec40c43216)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f31dfb7e-195b-4311-98d0-b386f3a8126d)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/95248d31-f5ed-474a-bdd8-16fa50b2bc03)

消耗资源由一个进程产生，一个进程使用。

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/1b8d838c-50bd-4414-a786-848a77c8512b)

<img width="575" alt="image" src="https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/ecddd4c9-e5a2-4d98-ad5f-7394e1c605f2">


![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/01141b2c-339e-4925-8432-536d03749bf9)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/85dd1f40-1538-4228-bc7e-5a7941a0a366)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d3e75fb5-38c8-4621-a061-744d7e73553e)


## 系统模型


资源类型R1,R2,..,Rm(CPU, memory space, IO devices)

每个资源类型Ri有Wi个实例.

每个进程使用资源如下:

- require,get ← free resource
- use,hold    ← requested,used resource
- release     ← free resource

可重复使用的资源

- 在一个时间只能有一个进程使用且不能被删除
- 进程获得资源,后来释放由其他进程重用
- 处理器,IO通道,主和副存储器,设备和数据结构,如文件,数据库和信号量
- 如果每个进程拥有一个资源并请求其他资源,死锁可能发生

使用资源

- 创建和销毁
- 在IO缓存区的中断,信号,消息,信息
- 如果接收消息阻塞可能会发生死锁
- 可能少见的组合事件会引起死锁

资源分配图

一组顶点V和边E的集合

- V有两种类型:
    - P={P1,P2,...,Pn},集合包括系统中的所有进程
    - R={R1,R2,...,Rm},集合包括系统中的所有资源类型
- requesting,claiming edge - directed edge Pi → Rj
- assignment,holding  edge - directed edge Rj → Pi

基本情况

如果图中不包含循环:

- 没有死锁

如果图中包含循环:

- 如果每个资源类只有一个实例,那么死锁
- 如果每个资源类有几个实例,可能死锁

## 死锁特征
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a8463c0d-c9a2-43ea-874a-fd2362d48bf7)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a7b85f93-6fb1-4646-84f1-9f29400b6104)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/7d27f634-d39b-40c6-9309-877c29462acc)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/c50bb46c-efa3-4e85-9fba-0b67d41701be)

死锁出现一定会出现以下四个条件,但是出现以下四个条件不一定死锁:

- 互斥: 在一个时间只能有一个进程使用资源
- 持有并等待: 进程保持至少一个资源正在等待获取其他进程持有的额外资源
- 无抢占: 一个资源只能被进程资源释放,进程已经完成了它的任务之后
- 循环等待: 存在等待进程集合{P0,P1,...,Pn},P0正在等待P1所占用的资源,P1正在等待P2占用的资源...Pn-1在等待Pn的资源,Pn正在等待P0所占用的资源

## 死锁处理方法
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4a43c321-10a2-40cc-a444-b40ac818c256)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/6926e1f3-6619-487a-bf8c-9cc38b2c0dfb)

常见方法

- 确保系统永远不会进入死锁状态
- 运行系统进入死锁状态,然后恢复.
- 忽略这个问题,假装系统中从来没有发生死锁,用于大多数操作系统,包括UNIX

### Deadlock Prevention    预防

限制申请方式

- 互斥 - 共享资源不是必须的,必须占用非共享资源（例如像打印机，之前只能互斥访问，现在加一个缓存，可以允许多个进程访问打印机缓存，同时打印机按顺序从缓存中读取数据打印
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a5628c46-1b26-4546-9b18-45ccea25441a)

- 占用并等待 - 必须保证当一个进程请求的资源,它不持有任何其他资源
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/7d4b8f4a-9069-4438-a942-9ca9ad1bfb63)

    - 需要进程请求并分配其所有资源,它开始执行之前或允许进程请求资源仅当进程没有资源
    - 资源利用率低,可能发生饥饿
- 无抢占 -
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/35eb73b1-c6c9-4b1c-9cf1-66a20f5dd2db)

    - 如果进程占有某些资源,并请求其他不能被立即分配的资源,则释放当前正占有的资源
    - 被抢占资源添加到资源列表中
    - 只有当它能够获得旧的资源以及它请求新的资源,进程可以得到执行
- 循环等待 - 对所有资源类型进行排序,并要求每个进程按照资源的顺序进行申请
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d4f55c02-23b2-43c8-81bf-f4c26b813d57)


### Deadlock Avoidance     避免
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/568c8bce-e3c0-4be0-9e23-0ea2ee42bc3b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/cf8fe8db-78e2-460d-bc5c-7a9540766948)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d7a341b1-e897-4e2a-9ed7-5ead6b093b5f)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/68aac6d6-4a60-472b-a437-519af7eeac90)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/628dff9f-5201-475e-bf07-db2bcf601103)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/42560db1-8400-4cdc-9f6a-f3b6ea9fe334)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/46dd4fb9-0c39-40bf-90db-b09bc1a7f78a)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/36b3e979-af83-4346-96e0-3b7735699138)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a5a747ec-0221-4bdc-8cd6-85fd73338ae5)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/59b1d8d1-e7cf-4dd0-8ab3-aebc6f5cbb42)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/763d29d2-7579-4340-88ce-1d8f7efa8636)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/dcd4d79b-9478-48a8-b50e-c65c26d98c05)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/24a53fa1-2274-43c4-86de-5a274c992c4c)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/85236120-6720-449d-a98d-37fc2cac9e0c)

需要系统具有一些额外的先验信息提供

- 最简单和最有效的模式是要求每个进程声明它可能需要的每个类型资源的最大数目
- 资源的分配状态是通过限定提供与分配的资源数量,和进程的最大需求
- 死锁避免算法动态检查的资源分配状态,以确保永远不会有一个环形等待状态
- 当一个进程请求可用资源,系统必须判断立即分配是否能使系统处于安全状态
- 系统处于安全状态指: 针对所有进程,存在安全序列
- 序列<P1,P2,...,Pn>是安全的: 针对每个Pi,Pi要求的资源能够由当前可用的资源+所有的Pj持有的资源来满足,其中j<i.
    - 如果Pi资源的需求不是立即可用,那么Pi可以等到所有Pj完成
    - 当Pi完成后,Pi+1可以得到所需要的资源,执行,返回所分配的资源,并终止.
    - 用同样的方法,Pi+2,Pi+3和Pn能获得其所需的资源.
- 如果系统处于安全状态→无死锁
- 如果系统处于不安全状态→可能死锁
- 避免死锁: 确保系统永远不会进入不安全状态

### Deadlock Detection     检测

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/aeed9ce6-50da-43da-9051-21a22a0d7d10)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/631da06a-d9e0-4a08-bebc-520acb4af3e5)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/7de5dbb2-88cb-432f-9647-700677888482)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/351ec7d7-234d-471e-9581-941d43c3e976)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e8a44474-4813-4c19-b791-c481b4449e70)

也就是说操作系统检测死锁开销时很大的，所以操作系统一般不进行死锁检测。

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d9921f05-add8-4c9e-a8dd-4fa812ff5852)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/98d25253-771b-4e1f-b6e3-abd528810aab)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/43f7d6a1-7cd4-46f7-a55c-a1122a1c3634)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/ac14e9b4-9985-4f8c-ab7c-59d6d9a11068)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/037680db-968b-412f-afce-4a0e16737a86)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d1c566b2-ac04-443c-9840-fefc299fd9f5)

每个资源类型单一实例

Maintain wait-for graph

- 节点是进程
- Pi→Pj: Pi等待Pj

定期调用检测算法来搜索图中是否存在循环

算法需要n^2次操作,n是图中顶点的数目

数据结构:

- Available: 长度为M的向量表示每种类型可用资源的数量
- Allocation: 一个nxm矩阵定义了当前分配给各个进程每种类型资源的数量,如果Alocation[i, j] = k, 进程Pi拥有资源Rj的k个实例
- Request: 一个nxm矩阵表示各进程的当前请求.如果Request[i, j] = k,表示进程Pi请求k个资源Pj的实例

具体算法(跳过了,看视频)

检查算法使用

何时,使用什么样的频率来检测依赖于:

- 死锁多久可能会发生?
- 多少进程需要被回滚? one for each disjoint cycle

如果检测算法多次被调用,有可能是资源图有多个循环,所以我们无法分辨出多个可能死锁进程中的哪些"造成"死锁

### Recovery from Deadlock 恢复

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/c884d959-af5c-4655-ae80-082a9f0c48f6)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d15625bc-6476-4f55-9e9c-c86850e8bce8)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e422eb83-0bab-4609-a35f-e38c5c13cfe4)

终止所有的死锁进程

在一个时间内终止一个进程直到死锁消除

终止进程的顺序应该是:

- 进程的优先级
- 进程运行了多久以及需要多少时间才能完成
- 进程占用的资源
- 进程完成需要的资源
- 多少进程需要被终止
- 进程是交互还是批处理

选择一个受孩子 - 最小的成本

回滚 - 返回到一些安全状态,重启进程到安全状态

饥饿 - 同一进程可能一直被选作受害者,包括回滚的数量

## IPC

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/01726e93-ccb0-40d6-aba3-9ccabfdedd00)

### 概述

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/88b51dd9-8d26-4bdc-a9e8-c430ac712661)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/90d6738a-70b9-44da-aa3b-ffd1daf88072)

进程通信的机制及同步

不使用共享变量的进程通信

IPC facility 提供2个操作:

- send(message) - 消息大小固定或者可变
- receive(message)

如果P和Q想通信,需要:

- 在它们之间建立通信链路
- 通过send/recevie交换消息

通信链路的实现

- 物理(例如,共享内存,硬件总线)
- 逻辑(例如,逻辑属性)

### 直接通信

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/cb0ea2a2-7681-40c6-ae89-6271cb4847ac)

进程必须正确的命名对方:

- send(P, message) - 发送消息到进程P
- receive(Q, message) - 从进程Q接收信息

通信链路的属性

- 自动建立链路
- 一条链路恰好对应一对通信进程
- 每对进程之间只有一个链路存在
- 链路可以是单向的,但通常是双向的

### 间接通信

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/55ef323a-a332-470a-a94a-9669b4118c93)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/16da29fc-6fbf-47e3-b274-42980317d5f0)

消息队列是由最开始发送消息的进程创建的。


![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/508a6c3e-8e1d-46da-86cb-c670a0c6a37c)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d32ef9e6-b93d-4fb8-8b3e-a3115c116f53)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e602dc8a-4042-4ae5-a595-b797e4b552b0)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e8da59d9-1bab-4eb3-84ea-b80df50278ae)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/358dbb8b-52de-4439-a86b-d144e865e9f5)

定向从消息队列接收消息

- 每个消息对垒都有一个唯一的ID
- 只有它们共享了一个消息队列,进程才能够通信

通信链路的属性

- 只有进程共享一个共同的消息队列,才建立链路
- 链接可以与许多进程相关联
- 每对进程可以共享多个通信链路
- 链接可以是单向或者双向

操作

- 创建一个新的消息队列
- 通过消息队列发送和接收消息
- 销毁消息队列

原语的定义如下:

- send(A, message)
- receive(A, message)

- 通信链路缓冲

    通信链路缓存大小:

    1. 0容量 - 0 message : 发送方必须等待接收方
    2. 有限容量 - n messages的有限长度 : 发送方必须等待,如果队列满
    3. 无限容量 - 无限长度 : 发送方不需要等待

### 信号

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5788df28-ce88-4388-a31c-c6881a699334)

例如像Ctrl+C可以是正在执行的进程停止下来，就是一个信号

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/ec46b6b2-d55a-48e0-b6ba-0abe42eadc4f)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/72a3aedf-0089-4f75-b866-3837f3705c4b)

signal()函数是注册信号历程的系统调用
信号Signal

- 软件中断通知事件处理
- Examples: SIGFPE, SIGKILL, SIGUSRI, SIGSTOP, SIGCONT

接收到信号时会发生什么?

- catch:  指定信号处理函数被调用
- ignore: 依靠操作系统的默认操作(abort, memory dump, suspend or resume process)
- mask:   闭塞信号因此不会传送(可能是暂时的,当处理同样类型的信号)

不足:

- 不能传输要交换的任何数据

### 管道

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/81d20c4b-2f29-42b7-9f39-7c19cdeaa7b1)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/9d5832c5-f056-44d3-9307-46a29ec09934)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/acc5a2b9-5196-4603-bfde-3b22cc5479f8)

数据交换

子进程从父进程继承文件描述符(0 stdin, 1 stdout, 2 stderr)

进程不知道(或不关心)从键盘,文件,程序读取或写入到终端,文件,程序.

例如: $ ls | more (两个进程, 管道是缓存,对于ls来说是stdout,对于more来说是stdin )

### 消息队列

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/85cfe559-b196-4673-b0a1-f952427317e8)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/3ba8ab6b-ca5d-46be-83c4-a6178570e402)

msgctl()系统调用:进程在结束时会释放占用的内存，但是如果想让消息队列继续存活下去，就可以使用msgctl()函数来控制。
消息队列按FIFO来管理消息

- message: 作为一个字节序列存储
- message queues: 消息数组
- FIFO &  FILO configuration

### 共享内存

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d09a3eea-22f1-4afa-8cb4-86261a4abf6a)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d756c8fa-a5c7-4a4e-9edb-cbe4e88e7008)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/3c6dddc6-fb2d-4277-abd2-ebca190a796e)

进程

- 每个进程都有私有地址空间
- 在每个地址空间内,明确地设置了共享内存段

优点

- 快速,方便地共享数据

不足

- 必须同步数据访问

最快的方法

一个进程写另一个进程立即可见

没有系统调用干预

没有数据复制

不提供同步

- Socket
