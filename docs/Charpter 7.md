 # 深入理解操作系统 第七章
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/93f4952a-9b45-454f-b759-c36d48b8296a)


>   第七章的主要内容是：进程

## 进程(process)描述

进程的描述属于进程的静态描述部分，进程的状态是进程的动态表述部分
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/952549ae-8c0c-4173-a153-624d1c95d220)

### 进程定义
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/b39dac29-580f-4e5e-815f-79689daaf269)

程序是静态的，程序跑起来才是一个进程（process），

### 进程的组成
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/82954859-1fbf-45cf-a670-e520c5d7d522)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/55054787-1c98-4c22-ab07-0b46d00baa6c)

上图中通过调用关系，一个进程可以包括多个程序是因为，一个进程代表要完成一个特定功能，而某个特定的功能是由多个程序协同完成的


![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/20e75ac2-5ffd-4f6b-b825-61a7a9ec176b)


上图中，进程由核心态是指应用程序在执行访问硬盘等操作时，是通过系统调用，由操作系统内核控制CPU去访问硬盘的（此时CPU处于核心态），然后返回系统调用结果（之前课程也有详细的介绍————系统调用，异常，中断）


 ![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/99a6d104-9b07-45f9-a39d-d4736befa44b)



### 进程的特点

**动态性** : 可动态地创建, 结果进程;

**并发性** : 进程可以被独立调度并占用处理机运行; (并发:一段内有多个进程执行, 并行:一时刻有多个进程在执行（**多CPU才能做到并行**）)

**独立性** : 不同进程的工作不相互影响;(页表是保障措施之一)

**制约性** : 因访问共享数据, 资源或进程间同步而产生制约.

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e90f024e-b931-4fc9-914b-80baa944ad22)

上图中，当多个进程在CPU中执行时，需要操作系统调度，选择哪个进程占用CPU执行。执行进程A，不能占用CPU太长时间，需要去执行B,这取决与操作系统调度的策略和算法（包括说进程A和B有逻辑的先后顺序，必须先执行完A才能执行B）


![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a1bf28de-8bf4-4416-94fa-5a6ca54d29d7)

>   抛出了一个问题 : 如果你要设计一个OS, 怎么样来实现其中的进程管理机制?
 ![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/aca96a25-67e4-445c-8fbb-27886966964d)


### 进程控制结构
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e18c8fd7-a71b-4637-b066-c7f53fd7fdf8)

描述进程的数据结构 : 进程控制块 (Process Control Block)

操作系统为每个进程都维护了一个PCB, 用来保存与该进程有关的各种状态信息.
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/cd3f59bf-44b2-437b-9330-2dcd2fbfeb0f)

**进程控制块 :** 操作系统管理控制进程运行所用的信息集合.

进程的创建 : 为该进程生成一个PCB

**进程的终止 :** 回收它的PCB

**进程的组织管理 :** 通过对PCB的组织管理来实现

(PCB具体包含什么信息? 如何组织的（有很多的进程，怎么有效组织他们的进程控制块）? 进程的状态转换（进程什么时候开始的，什么时候结束的，中间是否被切换了）?)

**PCB有以下三大类信息 :**

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/9b6b448e-2866-4856-85d0-2043f109959a)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4a42f718-b068-4f67-a554-06a665e39419)

-   进程标志信息. 如本进程的标识（一个PCB唯一表示了一个进程，会有一个进程控制块的ID，或者说一个进程的ID，这个表示可以反映出哪一个程序在执行，或者执行了多少次）, 本进程的产生者标识(父进程标识). 用户标识，
-   处理机（CPU）状态信息保存区（主要指寄存器） : 保存进程的运行现场信息 :
    -   用户可见寄存器. 用户程序可以使用的数据, 地址等寄存器
    -   控制和状态寄存器. 如程序计数器(PC), 程序状态字(PSW)
    -   栈指针. 过程调用, 系统调用, 中断处理和返回时需要用到它
-   进程控制信息
    -   调度和状态信息. 用于操作系统调度进程并占用处理机使用（描述该进程识处于等待的状态，还是就绪的状态，描述该进程当前的执行现状）.
    -   进程间通信信息. 为支持进程间与通信相关的各种标志、信号、信件等, 这些信息都存在接收方的进程控制块中.
    -   存储管理信息. 包含有指向本进程映像存储空间的数据结构（本进程占用了多少内存，是不是要回收，是不是分配新的内存）.
    -   进程所用资源. 说明由进程打开、使用的系统资源，如打开的文件等.
    -   有关数据结构的链接信息. 进程可以连接到一个进程队列中, 或连接到相关的其他进程的PCB.

**进程的组织方式**
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/97d2a24a-e456-4191-9647-21202d385ce0)

链表 : 同一状态的进程其PCB成一链表, 多个状态对应多个不同的链表.(各状态的进程形成不同的链表 : 就绪链表, 阻塞链表)

索引表 : 同一状态的进程归入一个index表(由index指向PCB), 多个状态对应多个不同的index表(各状态的进行形成不同的索引表 : 就绪索引表, 阻塞索引表)

## 进程状态(state)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0c15674f-351f-4e9a-b556-47a73db52103)

### 进程的生命期管理
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f68f175d-cc99-4dd1-8889-e5dec1987ea3)

#### 进程创建

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f1cc5b4d-e100-4312-a787-b64b6752e650)

引起进程创建的3个主要事件 :

-   系统初始化（操作系统在初始时创建一个进程（inti进程），这个进程负责创建其他新的进程）;
-   用户请求创建一个新进程（向操作系统发出请求，由操作系统创建）;
-   正在运行的进程执行了创建进程的系统调用（也是向操作系统发出请求，由操作系统创建）.


#### 进程运行（进程由就绪态转变为运行态）

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8c0bb8f1-02b6-443e-a316-c92c31fc19aa)

操作系统从可以执行的进程（就绪进程）中选择一个执行
内核选择一个就绪的进程, 让它占用CPU并执行

(为何选择?如何选择?如何从一系列就绪态的进程中选择一个执行？涉及到操作系统的调度问题)

#### 进程等待(阻塞)（阻塞态和就绪态不一样。阻塞态需要的数据还没有到，此时即便把占用CPU也没办法执行。就绪态是现在可以执行，但是在排队等CPU）

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/bf0b8b0f-e976-4eb2-bc5e-2f26edd0f531)

在以下情况下, 进程等待(阻塞):

1.  请求并等待系统服务, 无法马上完成
2.  启动某种操作, 无法马上完成
3.  需要的数据没有到达

进程只能自己阻塞自己（进程自身触发等待）, 因为只有进程自身才能知道何时需要等待某种事件的发生.

#### 进程唤醒（从等待态变成就绪态）

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/422c0acf-d783-4256-b969-d84db78f8299)

唤醒进程的原因 :

1.  被阻塞进程需要的资源可被满足
2.  被阻塞进程等待的事件到达
3.  将该进程的PCB插入到就绪队列
进程变成就绪态以后就可以被操作系统调度，占用CPU去执行了。
进程只能被别的进程或操作系统唤醒（**因为它现在没有占用CPU,没法唤醒自己**）

#### 进程结束

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5b31674a-9874-466e-841b-918921b87d50)

在以下四种情况下, 进程结束 :

-   正常退出(自愿)（运行结束，自愿退出）
-   错误退出(自愿)（运行出现严重错误，无法继续执行，自愿退出）
-   致命错误(强制性)（比如访问其他进程的内容，当应用程序使用系统调用访问硬盘时，CPU由操作系统控制，操作系统检测到该进程访问其他进程的数据时，操作系统将它杀死）
-   被其他进程杀死(强制性)（比如管理性质的进程可以杀死其他进程）

### 进程状态变化模型

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/df1c2fa8-1926-4a0a-86c6-3e6668ebac20)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/7d41cf11-cfcb-4ee7-8265-043509c37044)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/71122cad-5d84-450f-b358-5f2cdf8750e9)

**可能的状态变化如下 :**

NULL → New : 一个新进程被产生出来执行一个程序，操作系统创建PCB

New → Ready: 当进程创建完成并初始化后, 一切就绪准备运行时, 变为就绪状态，PCB中数据机构初始化完成，进程可以执行了，过程不会持续很久

Ready → Running  : 处于就绪态的进程被操作系统进程调度程序选中后（有调度算法）, 就分配到CPU上来运行

Running → Exit   : 当进程表示它已经完成或者因出错, 当前运行进程会由操作系统作结束处理

Running → Ready  : 处于运行状态的进程在其运行过程中, 由于分配它的处理机时间片用完而让出CPU,有操作系统来完成（此时CPU有应用程序控制，操作系统怎么起作用的呢？是时钟，（**应该是时钟发出中断请求了**））

Running → Blocked: 当进程请求某样东西且必须等待时，例如等待定时器的到达，读写文件等

Blocked → Ready  : 当进程要等待某事件到来时, 它从阻塞状态变到就绪状态，由操作系统完成。

### 进程挂起

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e0b1a7c5-f3e6-4969-a1f5-2fba7f4849f1)


进程挂起, 为了合理且充分地利用系统资源.

进程在挂起状态时, 意味着进程没有占用内存空间, 处在挂起状态的进程映像在磁盘上.(把进程放到磁盘上)

**两种挂起状态**

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/77674a05-726a-459c-86a5-956b1ee377e4)

1.  阻塞挂起状态 : 进程在外存并等待某事件的出现;
2.  就绪挂起状态 : 进程在外存, 但只要进入内存, 即可运行.

**与挂起相关的状态转换**

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e6b38874-3813-4d21-85d6-9ab98fec4587)


**解挂, 激活 :** 把一个进程从外存转到内存; 可能有以下几种情况 :

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5754fb77-1949-4a4c-8f75-7217d4e39206)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/03763196-5ce9-4349-84c7-5d65d43a568b)


![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e3a0ccc3-daea-4081-9f75-b4d3bcfe6b08)

上图答案如下：状态队列

### 状态队列

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/6c6aa8a1-c321-456d-b219-88d8450b7f4a)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/06487ba8-a05e-455d-a97a-bcdfa04a08b8)

上图中存在多个就绪队列，是因为就绪队列也可能分优先级（就绪队列一可以优先调用，就绪队列二其次等），阻塞队列也分多个（也存在优先级，根据阻塞等待的事件不同会排不同的队列，事件1有一个队列，事件2有一个队列，当事件1满足时，等待事件1的所有进程就有可能从阻塞态转变为就绪态，但需要当事件1只能满足一个进程时，只能把这一个进程从阻塞态变成就绪态，如果事件1产生以后，所有这些等待事件1的进程都可以得到满足，我们需要把所有的进程从阻塞状态变成就绪状态）

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/cd3def44-d8a2-4805-b5c7-06bbc1b9163e)

（**个人想法：进程挂起和虚拟内存管理技术的区别，虚拟内存管理是将某个程序的一部分调入内存中运行，其余部分留下，等有需要的时候在调入内存，此时仍然存放在内存中的部分程序是死的，而进程挂起是将虚拟内存管理技术中放入内存中的那部分程序暂时放置在内存中，不仅程序还包括了进程控制块（PBC),他们都暂时放入了内存呢中。**）


## 线程(thread)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/9335ffa0-ae33-45ce-b708-a02c53bb31cf)

### 为什么使用线程?

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/1a80bedb-efc6-4560-b9a3-e7bfeca0cc36)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/9c190218-309f-4e82-9874-9385e8b3e4b8)

问题: 播放出来的声音能否连贯? 各个函数之间不是并发执行, 影响资源的使用效率.
由于函数Read()会访问硬盘，速度较慢，播放出的声音可能是断断续续的。


```cpp
//单进程方式
while(1){
	Read();
	Decompress();
	Play();
}
//问题: 播放出来的声音能否连贯? 各个函数之间不是并发执行, 影响资源的使用效率.
```
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a5170eae-3bbe-4297-9475-d3ebc74207e0)

上图中使用多进程的方式，可能会解决以上问题（例如先多执行几次Read()函数，然后再去执行Decompress()函数），但是又引出了新的问题（上图中下方小字部分）

```cpp
//多进程
//进程1
while(1){
	Read();
}
//进程2
while(1){
	Decompress();
}
//进程3
while(1){
	Play();
}
//问题: 进程之间如何通信,共享数据?另外,维护进程的系统开销较大:
//创建进程时,分配资源,建立PCB;撤销进程时,回收资源,撤销PCB;进程切换时,保存当前进程的状态信息
```
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/c499be7a-fcc3-4b4f-aafd-dea41e1c7ac1)


（而进程之间是不共享相同的地址空间的，进程A想访问进程B的地址空间，需要操作系统做一次进程之间的通信）
这实体就是线程.

### 什么是线程

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/9137a723-dc1f-431f-9a69-ce63ec029847)

线程是进程当中的一条执行流程.

从两个方面重新理解进程:

1.  从资源组合的角度: 进程把一组相关的资源组合起来,构成了一个资源平台(环境),包括地址空间(代码段,数据段),打开的文件等各种资源;
2.  从运行的角度: 代码在这个资源平台上的一条执行流程(线程).

线程 = 进程 - 共享资源（进程除了需要管理资源以外，还需要一系列的线程来完成执行的过程）
一个进程内可以包含多个线程，多个线程共享该进程提供的各种资源（代码，数据，内存）
每一个线程有自己的线程控制块（thread control block,TCB),负责管理和执行流程相关的信息，包括PC（程序计数器）、SP（堆栈）、寄存器等

### 线程的优缺点

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d3aa3107-03b6-4fb7-846c-c9f405dcbba3)

线程的优点:

-   一个进程中可以同时存在多个线程;
-   各个线程之间可以并发地执行;
-   各个线程之间可以共享地址空间和文件等资源.

线程的缺点:

-   一个线程崩溃, 会导致其所属进程的所有线程崩溃.(给它了"权限"就得有更高的"责任")

-   线程所需的资源

当需要性能要求且程序执行流程相对固定的情况下（比如天气预报），可以使用线程的方式。
当性能要求不高，而安全性要求较高时，可以使用进程的方式。在互联网上的应用（比如浏览器）可以使用进程的方式，比如在浏览器中打开一个网页可以用一个线程的方式，但是如果使用线程的方式，如果有一个网页崩溃（恶意代码或写的不好），就会导致整个浏览器崩溃。比如说google chrome的每个页面就是使用进程的方式

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d269f29a-8d08-4894-b511-52fe662a5680)

早期的MS-DOS是单进程单线程的，unix是多进程单线程的，window NT和linux是多进程和一个进程中的多线程的。

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d55fff93-6419-45a1-8991-25d30e881856)

code、data、files多个线程之间共享，每个线程有各自独立的registers和stack，各自独立的Register和stack保存了每个进程的执行流程和控制流。



### 线程和进程的比较

-   进程是资源（**内存、打开的文件、网络**）分配单位, 线程是CPU调度单位;
-   进程拥有一个完整的资源平台, 而线程只独享必不可少的资源, 如寄存器和栈;
-   线程同样具有就绪,阻塞和执行三种基本状态,同样具有状态之间的转换关系;
-   线程能减少并发执行的时间和空间开销:
    -   线程的创建时间比进程短;(直接利用所属进程的一些状态信息)
    -   线程的终止时间比进程短;(不需要考虑把这些状态信息给释放)
    -   同一进程内的线程切换时间比进程短;(同一进程不同线程的切换不需要切换页表)
    -   由于同一进程的各线程之间共享内存和文件资源, 可直接进行不通过内核的通信.(直接通过内存地址读写资源)

### 线程的实现

主要有三种线程的实现方式:
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/aeab6095-ac38-4607-8e43-9e3c6e1f4b85)

-   用户线程  : 在用户空间实现; POSIX Pthreads, Mach C-threads, Solaris threads
-   内核线程  : 在内核中实现; Windows, Solaris, Linux
-   轻量级进程: 在内核中实现,支持用户线程; Solaris
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/754c7a64-2ce2-4da5-aa27-f65d19b6e3db)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/b0044dba-11ca-42c5-8039-409171585e47)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/377c8932-e1bc-4c39-b6b8-50a382d520c4)


**用户线程**

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/42070c48-43a7-4ae5-940e-df1285fe864b)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/b3030318-12c9-492b-ad56-c13ca784c8e0)

操作系统只能看到进程, 看不到线程, 线程的TCB在线程库中实现;用户线程在应用态的应用线程库来实现和管理。（**线程控制块TCB在用户空间实现，操作系统看不到线程，线程的调度和管理操作系统不参与，在用户空间实现的线程机制, 它不依赖于操作系统的内核, 由一组用户级的线程库函数来完成线程的管理, 包括进程的创建,终止,同步和调度等.

-   由于用户线程的维护由相应的进程来完成(通过线程库函数),不需要操作系统内核了解用户进程的存在,可用于不支持线程技术的多进程操作系统;
-   如果某个进程被设置成等待状态，那进程所包含的线程都不能执行了
-   每个进程都需要它自己私有的线程控制块(TCB)列表,用来跟踪记录它的各个线程的状态信息(PC,栈指针,寄存器),TCB由线程库函数来维护;
-   用户线程的切换也是由线程库函数来完成,无需用户态/核心态切换,所以速度特别快;
-   允许每个进程拥有自定义的线程调度算法.

用户线程的缺点:

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d06904f3-3264-46f5-b5b1-7c1b9d196f7b)

-   阻塞性的系统调用如何实现?如果一个线程发起系统调用而阻塞,则整个进程在等待;
-   当一个线程开始运行时,除非它主动地交出CPU的使用权,否则它所在的进程当中的其他线程将无法运行;（因为用户态的线程库函数没法主动打断当前用户线程的执行，但是操作系统在管理进程或在内核线程下管理线程时去能力去打断当前正在执行的进程或线程，是通过中断（主要是时钟中断）的方式，在实现的，操作系统管理中断，时钟中断发生时，CPU控制权交给操作系统，操作系统可以按时间片的方式让不同的进程线程轮流执行）
-   由于时间片分配给进程,所以与其他进程比,在多线程执行时,每个线程得到的时间片较少,执行会较慢.


**内核线程**

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/fc908d09-de9e-4c0c-80d1-fa244e9db5e8)

操作系统能够看到进程也可能看到线程，由操作系统管理起来的线程叫内核线程,线程控制块（TCB)在内核中实现的;
在这种内核线程的方式下，CPU的调度单位时线程而不是进程，此时进程主要完成资源的管理。
内核线程是在操作系统的内核当中实现的一种线程机制,由操作系统的内核来完成线程的创建,终止和管理，此时.

-   在支持内核线程的操作系统中,由内核来维护进程和线程的上下文信息(PCB和TCB);一个进程PCB管理一个线程TCB列表，管理一系列属于该进程的线程。但是具体调度由TCB完成。 
-   线程的创建,终止和切换都是通过系统调用,内核函数的方式来进行,由内核来完成,因此系统开销较大;
-   在一个进程当中,如果某个内核线程发起系统调用而被阻塞,并不会影响其他内核线程的运行;
-   时间片分配给线程,多线程的进程获得更多CPU时间;
-   Windows NT 和 Windows 2000/XP 支持内核线程.

**轻量级进程**

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e957ece7-15ca-4054-9d1c-b615562f20d8)

它是内核支持的用户线程.一个进程可以有一个或多个轻量化进程,每个量级进程由一个单独的内核线程来支持.(Solaris,Linux)

## 上下文切换

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0291bf46-ef0b-41ba-9eed-24cbe58d3297)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/cefca48a-0a59-44c4-9b72-c76653f73dec)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0be215f5-03b7-44fb-bc6d-1e48bda4e1c3)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/53540477-2889-4d89-b998-70193f993c38)

## 进程控制

### 创建进程
进程创建是操作系统提供给用户的一个系统调用、
在linux和unix系统中使用两个系统调用完成新进程的调用fork()和exec()
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d8c4afb3-cec2-4a3f-86a8-ab460f07d5ae)

exec()把新程序调入内存中，用新程序来重写当前进程（也就是新创建的子进程），当fork()和exec()执行完毕，就有两个进程存在了，并且新进程已经变成新的程序在运行了。
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5c929556-2f38-49aa-9146-cd9f9be46592)

上图中的exec("program",argc,arg0,arg1)里的“program”指可执行文件

上图中fork()系统调用函数返回时，就已经存在了两个进程（此时两个进程对应的程序也是一样的），这两个进程的PC(指令寄存器，程序计数器）都是指向fork()函数的下一行指令的（也就是if（pid==0）。然后两个一起往下执行，子进程中fork()的返回值pid为0，所以会执行exec()函数，将子进程改写成需要的进程。这样新进程创建完毕。父进程会跳过上述红色字体部分继续执行下面程序。
fork() 创建一个继承的子进程
 - 复制父进程的所有变量和内存
 - 复制父进程的所有CPU寄存器(有一个寄存器例外，也就是pid（程序ID）)

fork的返回值
 - 子进程的fork()返回0
 - 父进程的fork()返回子进程标识符（pid）
 - fork()可方便后续使用，子进程可使用getpid获取PID
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4dd6952f-ac26-4995-a04f-c00157b15a43)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4806e898-7413-4025-bb47-691934a09b91)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/de15b6e1-3b18-4fd3-9482-bf2a449eecf3)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/24378f46-f88d-4c93-9b4c-a07b922ad26a)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/269d8ddd-b1e0-46d6-aa4c-506d3a6d8c2d)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/bff27248-0130-48be-945d-704228d62e26)

下图中的fork()函数执行了几次
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a80676d9-150c-4d9f-89cd-fe4289096783)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/1d8f2e19-00aa-4503-8e71-b9a73135b71e)

故fork()函数共执行了7次，形成了最终共有八个子进程，上图中可以看到进程ID+1的顺序变化了，这是因为进程的调度问题，导致先创建了第三代的子进程1169，再去创建第二代的子进程1170.
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/51baf235-a4b7-41af-8cfd-743d14dd9465)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/847e741a-b87b-46c9-9fd5-35200294efa1)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/7de09c0b-a45b-436a-b845-1ba4bb06c9fa)

fork()的简单实现

-   对子进程分配内存
-   复制父进程的内存和CPU寄存器到子进程
-   开销昂贵

在99%的情况下,我们在调用fork()之后调用exec()

-   在fork()操作中内存复制是没有作用的
-   子进程将可能关闭打开的文件和连接
-   开销因此是最高的
-   为什么不能结合它们在一个调用中(OS/2, windows)?

vfork()

-   一个创建进程的系统调用,不需要创建一个同样的内存映像
-   一些时候称为轻量级fork()
-   子进程应该几乎立即调用exec()
-   现在不再使用如果我们使用 copy on write 技术

### 加载和执行进程

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/dbd38ade-05e6-4618-a228-c9f2545af15f)

上图中，在子进程中，开始从红色的两行代码开始执行，但是执行完exec()函数以后，由于子进程中的代码已经被“calc“覆盖了，所以也就不再继续执行红色的第二行程序了。

fork()函数执行失败时，返回一个负数。执行成功时父进程返回子进程ID，子进程返回0.

系统调用exec()加载程序取代当前运行的进程

exec()调用允许一个进程"加载"一个不同的程序并且在main开始执行(事实上 _start)

它允许一个进程指定参数的数量(argc)和它字符串参数数组(argv)

如果调用成功(相同的进程,不同的程序)

代码,stack,heap重写

```cpp
int pid = fork(); //创建子进程
if(pid == 0) {    //子进程
	exec_status = exec("calc", argc, argv0,argv1,...);
	printf("Why would I execute?");
} else if(pid > 0) { //父进程
	printf("Whose your daddy?");
	...
	child_status = wait(pid);
}
```
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/38d3f29d-abd1-4905-971b-bfd2c4b56384)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e6c9bae7-6d74-4b5d-8e1e-9992a8891b00)

上图是执行exec()的结果。

**用户空间和内核空间变化情况**
原来的情况：
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/cee5b97e-aba0-4755-9579-71410562d64e)

执行完fork()以后：
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/8b68fd20-42c7-426b-99c8-eeda3effd42c)

执行完exec()函数以后：
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/157fe1a5-a941-462b-a4f0-23356a9062d3)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f5af8dc2-6bb3-490c-a4dc-fc7ed5ee4349)

copy on write(COW):写的时候再复制（执行复制时，先不复制，而是让他们同时指向一片内存区域，等他们其中一方进行写操作，改变这片内存区域的内容时，再考虑把这片区域复制以下）
当fork()函数采用COW技术时，父进程给子进程复制地址空间的时候，并没有全部复制给子进程，而只复制了指向父进程地址空间所需要的原数据（页表），他们指向同一块一直空间（也就是说父进程和子进程指向了同一块地址空间），当父进程或子进程在对共同指向的地址空间写的时候会触发一个异常，这个时候才对需要写的这个页进行复制，使父进程和子进程分别指向不同的页。






### 等待和终止进程

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e11cd9ba-5962-4520-a679-6be528a5d481)


wait()系统调用是被父进程用来等待子进程的结束，但是子进程运行结束以后直接调用exit()退出不就可以了吗，为什么要让父进程等子进程呢？
答：当一个进程执行完毕后，执行exit()系统调用，表明该进程执行完毕要退出了，那么执行完退出时，操作系统会根据情况将该进程占用的内存空间和文件资源（打开的文件等）释放和关闭。但是不会回收掉内核中的PCB（代表进程存在的唯一标识），也就是说执行exit()系统调用了，用户态的资源回收了，但是内核态的资源还没有回收（无法回收的原因：自己没办法拎起自己的头），而内核态资源PCB是通过父进程的wait()系统调用来实现的。
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/35c37e64-5019-4280-9256-f505a9f16d42)
这里所说的僵尸进程是指释放了用户态资源但是还没有释放内核态资源的子进程（再exit()执行完毕，而父进程的wait()还没有执行完毕）。

如果子进程结束之前，父进程已经死亡，是不是这个子进程的内核态资源就没法回收了呢？
答：进程之间都有父子关系，最原始的根进程（init process）会定期扫描PCB序列，查看是否存在僵尸进程，然后代替其父进程释放它的内核态资源（其PCB）

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/3fdeebc5-27a8-4eab-b05e-3eb81a17475e)

exec()执行时，进程需要处于running态，由于系统调用加载新程序，此时进程可能从running态转变成blocked态。



-   一个子进程向父进程返回一个值,所以父进程必须接受这个值并处理
-   wait()系统调用担任这个要求
    -   它使父进程去睡眠来等待子进程的结束
    -   当一个子进程调用exit()的时候,操作系统解锁父进程,并且将通过exit()传递得到的返回值作为wait调用的一个结果(连同子进程的pid一起)如果这里没有子进程存活,wait()立刻返回
    -   当然,如果这里有为父进程的僵尸等待,wait()立即返回其中一个值(并且解除僵尸状态)
-   进程结束执行之后,它调用exit()
-   这个系统调用:
    -   将这程序的"结果"作为一个参数
    -   关闭所有打开的文件,连接等等
    -   释放内存
    -   释放大部分支持进程的操作系统结构
    -   检查是否父进程是存活着的:
        -   如果是的话,它保留结果的值直到父进程需要它;在这种情况里,进程没有真正死亡,但是它进入了僵尸状态
        -   如果没有,它释放所有的数据结构,这个进程死亡
    -   清理所有等待的僵尸进程
-   进程终止是最终的垃圾收集(资源回收)
