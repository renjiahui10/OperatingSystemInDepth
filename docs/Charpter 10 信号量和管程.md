![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/61013b0e-9ae9-4480-9952-68561731c303)# 深入理解操作系统 第十章
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/9c693a1c-b064-4eef-865f-59fc9526dbb1)


>   第十章的主要内容是：信号量和管程
信号量和管程都是操作系统提供的同步方法
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f215e041-3b38-424c-ae33-74c7ae404da7)

信号量和管程都属于高层次的编程抽象方法	
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/861f4402-95de-4937-8052-3646c2bf2d10)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/1d7e96b8-36c9-47b6-bffb-d8e62f7f22e8)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/9174d339-4707-4549-a23e-ee27e4343f79)

## 信号量
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/7e85d4b6-6c0c-485d-97ac-63d0b18e6d2a)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/60d39b33-5408-44e5-b63c-308dbe160274)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/54c853cd-6360-4208-8973-9f6a10af389d)

上述整个P操作和V操作在操作系统的保护下是原子性操作，保证了信号量的正确实施
信号量的抽象数据类型

-   一个整形(sem),具有两个原子操作
-   P(): sem减一,如果sem<0,等待,否则继续
-   V(): sem加一,如果sem≤0,唤醒一个等待的P

信号量是整数

信号量是被保护的变量

-   初始化完成后,唯一改变一个信号量的值的办法是通过P()和V()
-   操作必须是原子

P()能够阻塞,V()不会阻塞

我们假定信号量是公平的

-   没有线程被阻塞在P()仍然堵塞如果V()被无限频繁调用(在同一个信号量)
-   在实践中,FIFO经常被使用

两个类型信号量

-   二进制信号量: 可以是0或1
-   计数信号量: 可以取任何非负数
-   两者相互表现(给定一个可以实现另一个)

信号量可以用在2个方面

-   互斥
-   条件同步(调度约束——一个线程等待另一个线程的事情发生)

## 信号量使用
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0cdb14c5-ce68-4551-8e43-11a81b148834)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5330ee1a-c493-48e9-b6d8-c793d871dc71)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/340938d4-cd37-4820-a498-4e540e43a122)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/9e2005a2-6107-46d0-961d-5bee26157a42)

所谓的条件同步是指一个进程的某个模块的运行必须以另外一个进程的某个模块的运行结束为条件。比如说，B进程的X模块是准备数据q，A进程的N模块是处理数据q。
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/565fda46-de51-429f-bed1-ba9f5abd3662)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/95469ad6-c2cf-4487-856a-ad12925e172e)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/49012c76-b13c-4e97-9194-28eb872b1a10)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/2cdce5a2-0f37-41db-b6ba-0c737726a432)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a118934f-a250-4fdb-abd3-5522cb8f5484)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/23cf9a67-2ac2-40ac-bf73-ccbe62822163)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/68ddce2d-bda9-41e3-9ccf-fae5250a4b21)
（**我自己的理解：我觉得信号量本质上就是用两个原子操作（申请和释放）来维护一个变量（有标识作用）**）
1.  用二进制信号量实现的互斥

    ```cpp
    mutex = new Semaphore(1);
    
    mutex->P();
    ...
    mutex->V();
    ```

2.  用二进制信号量实现的调度约束

    ```cpp
    condition = new Semaphore(0);
    
    //Thread A
    ...
    condition->P(); //等待线程B某一些指令完成之后再继续运行,在此阻塞
    ...
    
    //Thread B
    ...
    condition->V(); //信号量增加唤醒线程A
    ...
    ```

3.  一个线程等待另一个线程处理事情

    比如生产东西或消费东西(生产者消费者模式),互斥(锁机制)是不够的

    有界缓冲区的生产者-消费者问题

    -   一个或者多个生产者产生数据将数据放在一个缓冲区里
    -   单个消费者每次从缓冲区取出数据
    -   在任何一个时间只有一个生产者或消费者可以访问该缓冲区

    正确性要求

    -   在任何一个时间只能有一个线程操作缓冲区(互斥)
    -   当缓冲区为空时,消费者必须等待生产者(调度,同步约束)
    -   当缓存区满,生产者必须等待消费者(调度,同步约束)

    每个约束用一个单独的信号量

    -   二进制信号量互斥
    -   一般信号量 fullBuffers
    -   一般信号了 emptyBuffers

    ```cpp
    class BoundedBuffer{
    		mutex = new Semaphore(1);
    		fullBuffers = new Semaphore(0);   //说明缓冲区初始为空
     		emptyBuffers = new Semaphore(n);  //同时可以有n个生产者来生产
    };
    
    BoundedBuffer::Deposit(c){
    		emptyBuffers->P();
    		mutex->P();
    		Add c to the buffer;
    		mutex->V();
    		fullBuffers->V();
    }
    
    BoundedBuffer::Remove(c){
    		fullBuffers->P();
    		mutex->P();
    		Remove c from buffer;
    		mutex->V();
    		emptyBuffers->V();
    }
    ```

## 信号量实现

使用硬件原语

-   禁用中断
-   原子指令

类似锁

-   禁用中断

```cpp
class Semaphore{
		int sem;
		WaitQueue q;
};

Semaphore::P(){
		--sem;
		if(sem < 0){
				Add this thread t to q;
				block(p);
		}
};

Semaphore::V(){
		++sem;
		if(sem <= 0){
				Remove a thread t from q;
				wakeup(t);
		}
}
```

信号量的双用途

-   互斥和条件同步
-   但等待条件是独立的互斥

读,开发代码比较困难

-   程序员必须非常精通信号量

容易出错

-   使用的信号量已经被另一个线程占用
-   忘记释放信号量

不能够处理死锁问题

## 管程
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/ccc9af49-a781-4524-9776-f70edc7db412)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5ff836e9-6999-4765-a748-d336a0562410)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a50b4d61-fc30-4df1-9578-071493e5d0c0)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/f2ae0ba2-162f-4e25-8a32-e79146517394)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d6f7ea9c-171f-4989-b2fc-725a1f23af70)

当条件变量个数是0时，管程退化成临界区
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/4062cd2a-d9d2-400c-8751-85ad99843b8d)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/ff364109-39cc-4397-b16f-7dec40da5ecf)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/478128a9-a447-473f-8bc2-221387116982)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/87c160a5-af22-45d4-9b1c-6ef12b7e56fa)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/5b0d83b5-0f84-4cb6-808f-30167a721461)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/29e00918-c9d0-4d49-b3c6-704dbfa90b03)


目的: 分离互斥和条件同步的关注

什么是管程

-   一个锁: 指定临界区
-   0或者多个条件变量: 等待,通知信号量用于管程并发访问共享数据

一般方法

-   收集在对象,模块中的相关共享数据
-   定义方法来访问共享数据

Lock

-   Lock::Acquire() 等待直到锁可用,然后抢占锁
-   Lock::Release() 释放锁,唤醒等待者如果有

Condition Variable

-   允许等待状态进入临界区
    -   允许处于等待(睡眠)的线程进入临界区
    -   某个时刻原子释放锁进入睡眠
-   Wait() operation
    -   释放锁,睡眠,重新获得锁放回
-   Signal() operation(or broadcast() operation)
    -   唤醒等待者(或者所有等待者),如果有

实现

-   需要维持每个条件队列
-   线程等待的条件等待signal()

```cpp
class Condition{
		int numWaiting = 0;
		WaitQueue q;
};

Condition::Wait(lock){
		numWaiting++;
		Add this thread t to q;
		release(lock);
		schedule(); //need mutex
		require(lock);
}

Condition::Signal(){
		if(numWaiting > 0){
				Remove a thread t from q;
				wakeup(t); //need mutex
				numWaiting--;
		}
}
```

管程解决生产者-消费者问题

```cpp
class BoundedBuffer{
		Lock lock;
		int count = 0;  //buffer 为空
		Condition notFull, notEmpty;
};

BoundedBuffer::Deposit(c){
		lock->Acquire();    //管程的定义:只有一个线程能够进入管程
		while(count == n)
				notFull.Wait(&lock); //释放前面的锁
		Add c to the buffer;
		count++;
		notEmpty.Signal();
		lock->Release();
}

BoundedBuffer::Remove(c){
		lock->Acquire();
		while(count == 0)
				notEmpty.Wait(&lock);
		Remove c from buffer;
		count--;
		notFull.Signal();
		lock->Release();
}
```

开发,调试并行程序很难

-   非确定性的交叉指令

同步结构

-   锁: 互斥
-   条件变量: 有条件的同步
-   其他原语: 信号量

怎么样有效地使用这些结构

-   制定并遵循严格的程序设计风格,策略

## 经典同步问题

1.  读者-写者问题
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a8491b55-5015-4530-8ad9-fe25db87d43e)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/1753c20a-308c-4bbb-a5ab-0f3d762751c1)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/951e731e-9578-4be6-a61d-1ed5a044e71f)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/ea462331-d942-4c7e-8fa2-25369b44256f)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/15885121-e1d7-4d76-b11f-542e48128340)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/9bfe2c8d-ae41-4b63-ba24-5d6d1d505804)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e3e62e93-e24e-44ae-ba44-607f845f7310)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/6610d91c-b67e-4e3a-a6fb-f890d417592c)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/53ce75ad-de43-4108-a70c-0f6357f5f269)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/a029d289-0c72-4c4c-b05f-a4c2280c488d)

上述方法是读者优先：读者1开始读，之后来了写者1等待，之后又来了几个读者，此时写者需要继续等待，等所有读者读完，写着才可以写
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/e83ec6bd-4cb2-41e2-a4ee-5cc7a3fcba63)

还又读者和写者公平地位的。

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/28f3f3a8-ed69-4cff-b364-0c090667f1f2)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/537ec091-ff1c-4fb6-83f9-15a6afdcde1c)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/81cf78cc-815f-4ea7-be9f-4dae5bf2a0f7)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0cd3a596-f459-4fbf-8465-fec3b57f0ae9)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/02869096-b2eb-4a2e-844c-06c31947d661)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/454a137c-9139-4dc8-a573-455dcbfd0803)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/7dcb7108-cba0-4a49-b84d-68e8aeb40d60)

![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/d37cbba8-23e8-437a-9e2f-bbd403ddfc02)

    动机: 共享数据的访问

    两种类型的使用者: 读者(不修改数据) 写者(读取和修改数据)

    问题的约束:

    -   允许同一时间有多个读者,但在任何时候只有一个写者
    -   当没有写者时,读者才能访问数据
    -   当没有读者和写者时,写者才能访问数据
    -   在任何时候只能有一个线程可以操作共享变量

    多个并发进程的数据集共享

    -   读者: 只读数据集;他们不执行任何更新
    -   写者: 可以读取和写入

    共享数据

    -   数据集
    -   信号量CountMutex初始化为1
    -   信号量WriteMutex初始化为1
    -   整数Rcount初始化为0(当前读者个数)

    读者优先设计

    只要有一个读者处于活动状态, 后来的读者都会被接纳.如果读者源源不断的出现,那么写者使用处于阻塞状态.

    ```cpp
    //信号量实现
    //writer
    sem_wait(WriteMutex);
    write;
    sem_post(WriteMutex);
    
    //reader
    sem_wait(CountMutex);
    if(Rcount == 0)
    		sem_wait(WriteMutex); //确保后续不会有写者进入
    ++Rcount;
    read;
    --Rcount;
    if(Rcount == 0)
    		sem_post(WriteMutex); //全部读者全部离开才能唤醒写者
    sem_post(CountMutex);
    ```

    写者优先设计

    一旦写者就绪,那么写者会尽可能的执行写操作.如果写者源源不断的出现的话,那么读者就始终处于阻塞状态.

    ```cpp
    //writer
    Database::Write(){
    		Wait until readers/writers;
    		write database;
    		check out - wake up waiting readers/writers;
    }
    //reader
    Database::Read(){
    		Wait until no writers;
    		read database;
    		check out - wake up waiting writers;
    }
    
    //管程实现
    AR = 0; // # of active readers
    AW = 0; // # of active writers
    WR = 0; // # of waiting readers
    WW = 0; // # of waiting writers
    Condition okToRead;
    Condition okToWrite;
    Lock lock;
    //writer
    Public Database::Write(){
    		//Wait until no readers/writers;
    		StartWrite();
    		write database;
    		//check out - wake up waiting readers/writers;
    		DoneWrite();
    }
    
    Private Database::StartWrite(){
    		lock.Acquire();
    		while((AW + AR) > 0){
    				WW++;
    				okToWrite.wait(&lock);
    				WW--;		
    		}
    		AW++;
    		lock.Release();
    }
    
    Private Database::DoneWrite(){
    		lock.Acquire();
    		AW--;
    		if(WW > 0){
    				okToWrite.signal();
    		}
    		else if(WR > 0){
    				okToRead.broadcast(); //唤醒所有reader 
    		}
    		lock.Release();
    }
    
    //reader
    Public Database::Read(){
    		//Wait until no writers;
    		StartRead();
    		read database;
    		//check out - wake up waiting writers;
    		DoneRead();
    }
    
    Private Database::StartRead(){
    		lock.Acquire();
    		while(AW + WW > 0){    //关注等待的writer,体现出写者优先
    				WR++;
    				okToRead.wait(&lock);
    				WR--;
    		}
    		AR++;
    		lock.Release();
    }
    
    private Database::DoneRead(){
    		lock.Acquire();
    		AR--;
    		if(AR == 0 && WW > 0){  //只有读者全部没有了,才需要唤醒
    				okToWrite.signal();
    		}
    		lock.Release();
    }
    ```

2.  哲学家就餐问题(学习自 [github.com/cyc2018](http://github.com/cyc2018))
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/0169d50a-2daf-4ec0-8b3a-77b58c533a9a)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/131df432-099b-45c1-978a-9b57b37e680c)
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/b841ddac-68ac-418b-8b5a-54b7f0770515)

如果五个人同时拿左边的刀叉，就会出现死锁问题，谁也吃不到饭，但是都在等其他人放下某个刀叉
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/69108393-4c55-45db-bc10-72151e6dd8e5)

上图是个极端的方案——同时只能有一个科学家就餐
![image](https://github.com/renjiahui10/OperatingSystemInDepth/assets/114166264/496f5635-c61a-4731-8f90-6247bff722d1)

    共享数据:

    -   Bowl of rice(data set)
    -   Semaphone fork [5] initialized to 1

    ```cpp
    #define N 5
    #define LEFT (i + N - 1) % N // 左邻居
    #define RIGHT (i + 1) % N    // 右邻居
    #define THINKING 0
    #define HUNGRY   1
    #define EATING   2
    typedef int semaphore;
    int state[N];                // 跟踪每个哲学家的状态
    semaphore mutex = 1;         // 临界区的互斥，临界区是 state 数组，对其修改需要互斥
    semaphore s[N];              // 每个哲学家一个信号量
    
    void philosopher(int i) {
        while(TRUE) {
            think(i);
            take_two(i);
            eat(i);
            put_two(i);
        }
    }
    
    void take_two(int i) {
        down(&mutex);
        state[i] = HUNGRY;
        check(i);
        up(&mutex);
        down(&s[i]); // 只有收到通知之后才可以开始吃，否则会一直等下去
    }
    
    void put_two(i) {
        down(&mutex);
        state[i] = THINKING;
        check(LEFT); // 尝试通知左右邻居，自己吃完了，你们可以开始吃了
        check(RIGHT);
        up(&mutex);
    }
    
    void eat(int i) {
        down(&mutex);
        state[i] = EATING;
        up(&mutex);
    }
    
    // 检查两个邻居是否都没有用餐，如果是的话，就 up(&s[i])，使得 down(&s[i]) 能够得到通知并继续执行
    void check(i) {         
        if(state[i] == HUNGRY && state[LEFT] != EATING && state[RIGHT] !=EATING) {
            state[i] = EATING;
            up(&s[i]);
        }
    }
    ```

