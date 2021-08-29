# Java并发  
## 线程和进程  
### 线程同步的方式  
1. 信号：类似进程间的信号处理  
2. 锁机制：互斥锁、读写锁和自旋锁  
3. 条件变量：使用通知的方式解锁，与互斥锁配合使用    
4. 信号量  

#### 典型的锁机制  
1. 读写锁  
2. 互斥锁  
3. 条件变量  
4. 自旋锁  

线程锁：互斥锁，条件变量，自旋锁  

> 互斥锁属于sleep-waiting类型的锁。  
> 自旋锁属于busy-waiting类型的锁。  
> 互斥锁是在抢锁失败的情况下主动放弃CPU进入睡眠状态直到锁的状态改变时再唤醒  
> 互斥锁在加锁操作时涉及上下文的切换  

### Java线程通信的方式  
- 共享内存模型  
- volatile 告知程序任何对变量的读需要从主内存中获取，写必须同步刷新回主内存，保证所有线程对变量访问的可见性。  
- synchronized 确保多个线程在同一时刻只能有一个处于方法或同步块中，保证线程对变量访问的原子性、可见性和有序性。  
- join()：在线程中调用另一个线程的 join() 方法，会将当前线程挂起，而不是忙等待，直到目标线程结束。  
- wait() notify() notifyAll()  
- ThreadLocal 是线程共享变量，但它可以为每个线程创建单独的副本，副本值是线程私有的，互相之间不影响。  


### 进程同步的方式  
1. 临界区  
2. 同步与互斥   
3. 信号量  
4. 管程  

### 进程间通信的方式  
1. 管道  
2. 命名管道  
3. 共享内存  
4. 消息队列  
5. 信号量  
6. 套接字  


### 线程的生命周期  
新建，可运行，阻塞，无期限阻塞，限期阻塞，死亡  
> 有几种阻塞队列？  

### sleep()和wait()的区别    
sleep()方法会休眠当前正在执行的线程  
wait()方法使得线程等待某个条件满足，线程在等待时会被挂起，当其他线程的运行使得这个条件满足时，其它线程会调用 notify() 或者 notifyAll() 来唤醒挂起的线程。  
使用 wait() 挂起期间，线程会释放锁。  
**wait() 和 sleep() 的区别**  
- wait() 是 Object 的方法，而 sleep() 是 Thread 的静态方法；  
- wait() 会释放锁，sleep() 不会。  

### execute()方法和submit()的区别  


## sychronized  
JVM 实现的 synchronized  
synchronized 关键字解决的是多个线程之间访问资源的同步性，synchronized关键字可以保证被它修饰的方法或者代码块在任意时刻只能有一个线程执行。  

###  sychronized实现  
synchronized 属于 重量级锁，效率低下。  
因为监视器锁（monitor）是依赖于底层的操作系统的 Mutex Lock 来实现的，Java 的线程是映射到操作系统的原生线程之上的。  
如果要挂起或者唤醒一个线程，都需要操作系统帮忙完成，而操作系统实现线程之间的切换时需要从用户态转换到内核态，这个状态之间的转换需要相对比较长的时间，时间成本相对较高。  
在 Java 6 之后 Java 官方对从 JVM 层面对 synchronized 较大优化，如自旋锁、适应性自旋锁、锁消除、锁粗化、偏向锁、轻量级锁等技术来减少锁操作的开销。    

### sychronized底层原理    
synchronized 同步语句块的实现使用的是 monitorenter 和 monitorexit 指令，其中 monitorenter 指令指向同步代码块的开始位置，monitorexit 指令则指明同步代码块的结束位置。

synchronized 修饰的方法并没有 monitorenter 指令和 monitorexit 指令，取得代之的确实是 ACC_SYNCHRONIZED 标识，该标识指明了该方法是一个同步方法。

不过两者的本质都是对对象监视器 monitor 的获取。

#### 同步语句块  
synchronized 同步语句块的实现使用的是 monitorenter 和 monitorexit 指令，  
其中 monitorenter 指令指向同步代码块的开始位置，  
monitorexit 指令则指明同步代码块的结束位置。  
当执行 monitorenter 指令时，线程试图获取锁也就是获取 对象监视器 monitor 的持有权。  
在执行monitorenter时，会尝试获取对象的锁，如果锁的计数器为 0 则表示锁可以被获取，获取后将锁计数器设为 1 也就是加 1。  
在执行 monitorexit 指令后，将锁计数器设为 0，表明锁被释放。如果获取对象锁失败，那当前线程就要阻塞等待，直到锁被另外一个线程释放为止。  

#### 修饰方法  
synchronized 修饰的方法并没有 monitorenter 指令和 monitorexit 指令，取得代之的确实是 ACC_SYNCHRONIZED 标识，该标识指明了该方法是一个同步方法。    
JVM 通过该 ACC_SYNCHRONIZED 访问标志来辨别一个方法是否声明为同步方法，从而执行相应的同步调用。  


### sychronized的三种使用方法  
1. 修饰实例方法：作用于当前对象实例加锁，进入同步代码前要获得当前对象实例的锁  
2. 修饰静态方法：给当前类加锁  
3. 修饰代码块：this|object获得给定对象的锁，类.class表示获得当前class的锁  

总结：
- synchronized 关键字加到 static 静态方法和 synchronized(class) 代码块上都是是给 Class 类上锁。  
- synchronized 关键字加到实例方法上是给对象实例上锁。  

### sychronized和ReentrantLock的区别  

#### 1. 都是可重入锁  
#### 2. synchronized依赖于JVM而ReentrantLock依赖于API  
#### 3. ReentrantLock有一些高级功能：等待可中断，可实现公平锁，可实现选择性通知  
#### 4. 新版本 Java 对 synchronized 进行了很多优化，例如自旋锁等  

> 公平锁是指多个线程在等待同一个锁时，必须按照申请锁的时间顺序来依次获得锁。  


## ReentrantLock  
ReentrantLock 是 java.util.concurrent（J.U.C）包中的锁。  
一个 ReentrantLock 可以同时绑定多个 Condition 对象。  

# J.U.C   
## AQS  
AQS是一个用来构建锁和同步器的框架，这个类在java.util.concurrent.locks包下面。  
AQS 核心思想是：  
如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。  
如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制 AQS 是用 CLH 队列锁实现的，即将暂时获取不到锁的线程加入到队列中。  


### AQS组件  
Semaphore(信号量)-允许多个线程同时访问： synchronized 和 ReentrantLock 都是一次只允许一个线程访问某个资源，Semaphore(信号量)可以指定多个线程同时访问某个资源。  
CountDownLatch （倒计时器）： CountDownLatch 是一个同步工具类，用来协调多个线程之间的同步。这个工具通常用来控制线程等待，它可以让某一个线程等待直到倒计时结束，再开始执行。  
CyclicBarrier(循环栅栏)： CyclicBarrier 和 CountDownLatch 非常类似，它也可以实现线程间的技术等待，但是它的功能比 CountDownLatch 更加复杂和强大。主要应用场景和 CountDownLatch 类似。       
它要做的事情是，让一组线程到达一个屏障（也可以叫同步点）时被阻塞，直到最后一个线程到达屏障时，屏障才会开门，所有被屏障拦截的线程才会继续干活。CyclicBarrier 默认的构造方法是 CyclicBarrier(int parties)，其参数表示屏障拦截的线程数量，每个线程调用 await() 方法告诉 CyclicBarrier 我已经到达了屏障，然后当前线程被阻塞。  

### CountDownLatch  
用来控制一个或者多个线程等待多个线程。

维护了一个计数器 cnt，每次调用 countDown() 方法会让计数器的值减 1，减到 0 的时候，那些因为调用 await() 方法而在等待的线程就会被唤醒。

### CyclicBarrier  
用来控制多个线程互相等待，只有当多个线程都到达时，这些线程才会继续执行。  
和 CountdownLatch 相似，都是通过维护计数器来实现的。线程执行 await() 方法之后计数器会减 1，并进行等待，直到计数器为 0，所有调用 await() 方法而在等待的线程才能继续执行。  

CyclicBarrier 和 CountdownLatch 的一个区别是，CyclicBarrier 的计数器通过调用 reset() 方法可以循环使用，所以它才叫做循环屏障。  
CyclicBarrier 有两个构造函数，其中 parties 指示计数器的初始值，barrierAction 在所有线程都到达屏障的时候会执行一次。  

### Semaphore  
Semaphore 类似于操作系统中的信号量，可以控制对互斥资源的访问线程数。  

### FutureTask  
FutureTask 可用于异步获取执行结果或取消执行任务的场景。当一个计算任务需要执行很长时间，那么就可以用 FutureTask 来封装这个任务，主线程在完成自己的任务之后再去获取结果。  

### BlockingQueue  
- FIFO 队列 ：LinkedBlockingQueue、ArrayBlockingQueue（固定长度）  
- 优先级队列 ：PriorityBlockingQueue  
提供了阻塞的 take() 和 put() 方法：如果队列为空 take() 将阻塞，直到队列中有内容；如果队列为满 put() 将阻塞，直到队列有空闲位置。  
#### 使用 BlockingQueue 实现生产者消费者问题  
### ForkJoin  
主要用于并行计算中，和 MapReduce 原理类似，都是把大的计算任务拆分成多个小任务并行计算。  

## Java内存模型  
## 内存模型三大特性  
### 原子性  
被 volatile 修饰的 64 位数据（long，double）的读写操作划分为两次 32 位的操作来进行，即 load、store、read 和 write 操作可以不具备原子性。  
保证操作的原子性
#### 1. 使用原子类  
#### 2. AtomicInteger 能保证多个线程修改的原子性  
#### 3. 使用 synchronized 互斥锁来保证操作的原子性
它对应的内存间交互操作为：lock 和 unlock，在虚拟机实现上对应的字节码指令为 monitorenter 和 monitorexit。  

### 可见性  
可见性指当一个线程修改了共享变量的值，其它线程能够立即得知这个修改。Java 内存模型是通过在变量修改后将新值同步回主内存，在变量读取前从主内存刷新变量值来实现可见性的。  

保证操作的可见性：   
#### 1. volatile  
#### 2. synchronized，对一个变量执行 unlock 操作之前，必须把变量值同步回主内存。 
#### 3. final，被 final 关键字修饰的字段在构造器中一旦初始化完成，并且没有发生 this 逃逸（其它线程通过 this 引用访问到初始化了一半的对象），那么其它线程就能看见 final 字段的值。

### 有序性  
在本线程内观察，所有操作都是有序的。在一个线程观察另一个线程，所有操作都是无序的，无序是因为发生了指令重排序。  
#### 1. volatile 关键字通过添加内存屏障的方式来禁止指令重排，即重排序时不能把后面的指令放到内存屏障之前。  
#### 2. 可以通过 synchronized 来保证有序性，它保证每个时刻只有一个线程执行同步代码，相当于是让线程顺序执行同步代码。  

### 先行发生原则  
让一个操作无需控制就能先于另一个操作完成。  

1. 单一线程原则  
2. 管程锁定规则  
3. volatile 变量规则  
4. 线程启动规则  
5. 线程加入原则  
6. 线程中断规则  
7. 对象终结规则  
8. 传递性  

## 线程安全  

### 1. 不可变  
- final 关键字修饰的基本数据类型  
- String  
- 枚举类型  
- Number 部分子类，如 Long 和 Double 等数值包装类型，BigInteger 和 BigDecimal 等大数据类型。  

### 2. 互斥同步  
synchronized 和 ReentrantLock。  
### 3. 非阻塞同步  
CAS：比较并交换，CAS 指令需要有 3 个操作数，分别是内存地址 V、旧的预期值 A 和新值 B。当执行操作时，只有当 V 的值等于 A，才将 V 的值更新为 B。  
AtomicInteger：J.U.C 包里面的整数原子类 AtomicInteger 的方法调用了 Unsafe 类的 CAS 操作。  

### 4. 无同步方案  
栈封闭  
线程本地存储：可以使用 java.lang.ThreadLocal 类来实现线程本地存储功能。  
可重入代码  






## volatile  
声明为 volatile ，这就指示 JVM，这个变量是共享且不稳定的，每次使用它都到主存中进行读取。  
volatile 关键字 除了防止 JVM 的指令重排 ，还有一个重要的作用就是保证变量的可见性。  

### sychronized和volatile的区别
1. volatile 关键字是线程同步的轻量级实现，所以 volatile 性能肯定比synchronized关键字要好 。但是 volatile 关键字只能用于变量而 synchronized 关键字可以修饰方法以及代码块 。  
2. volatile 关键字能保证数据的可见性，但不能保证数据的原子性。synchronized 关键字两者都能保证。  
3. volatile 关键字主要用于解决变量在多个线程之间的可见性，而 synchronized 关键字解决的是多个线程之间访问资源的同步性。  

## 优化后的sychronized锁  
> CAS(比较并交换) 指令需要有 3 个操作数，分别是内存地址 V、旧的预期值 A 和新值 B。当执行操作时，只有当 V 的值等于 A，才将 V 的值更新为 B。  


### 偏向锁  
偏向锁的思想是偏向于让第一个获取锁对象的线程，这个线程在之后获取该锁就不再需要进行同步操作，甚至连 CAS 操作也不再需要。  
当锁对象第一次被线程获得的时候，进入偏向状态，标记为 1 01。同时使用 CAS 操作将线程 ID 记录到 Mark Word 中，如果 CAS 操作成功，这个线程以后每次进入这个锁相关的同步块就不需要再进行任何同步操作。  
当有另外一个线程去尝试获取这个锁对象时，偏向状态就宣告结束，此时撤销偏向（Revoke Bias）后恢复到未锁定状态或者轻量级锁状态。   

### 轻量级锁  
轻量级锁是相对于传统的重量级锁而言，它使用 CAS 操作来避免重量级锁使用互斥量的开销。  
对于绝大部分的锁，在整个同步周期内都是不存在竞争的，因此也就不需要都使用互斥量进行同步，可以先采用 CAS 操作进行同步，如果 CAS 失败了再改用互斥量进行同步。  

### 自旋锁
自旋锁的思想是让一个线程在请求一个共享数据的锁时执行忙循环（自旋）一段时间，如果在这段时间内能获得锁，就可以避免进入阻塞状态。  

### 锁消除  
锁消除是指对于被检测出不可能存在竞争的共享数据的锁进行消除。  

### 锁粗化  



## 线程池  
线程池提供了一种限制和管理资源（包括执行一个任务）。 每个线程池还维护一些基本统计信息，例如已完成任务的数量。  
好处：降低资源消耗，提高响应速度，提高线程的可管理性  

### 线程池参数  
corePoolSize : 核心线程数线程数定义了最小可以同时运行的线程数量。  
maximumPoolSize : 当队列中存放的任务达到队列容量的时候，当前可以同时运行的线程数量变为最大线程数。  
workQueue: 任务队列，用来储存等待执行任务的队列  
  
keepAliveTime:当线程数大于核心线程数时，多余的空闲线程存活的最长时间  
unit : keepAliveTime 参数的时间单位。   
threadFactory :线程工厂，用来创建线程
handler :饱和策略。  


### 线程池种类  
- FixedThreadPool ： 该方法返回一个固定线程数量的线程池。  
- SingleThreadExecutor： 方法返回一个只有一个线程的线程池。  
- CachedThreadPool： 该方法返回一个可根据实际情况调整线程数量的线程池。  


### 饱和策略  


### 线程池大小确定  
多线程编程中一般线程的个数都大于 CPU 核心的个数，而一个 CPU 核心在任意时刻只能被一个线程使用，CPU 采取的策略是为每个线程分配时间片并轮转的形式。  
任务从保存到再加载的过程就是一次上下文切换。  

CPU 密集型任务(N+1)： 这种任务消耗的主要是 CPU 资源，可以将线程数设置为 N（CPU 核心数）+1，比 CPU 核心数多出来的一个线程是为了防止线程偶发的缺页中断，或者其它原因导致的任务暂停而带来的影响。一旦任务暂停，CPU 就会处于空闲状态，而在这种情况下多出来的一个线程就可以充分利用 CPU 的空闲时间。  
I/O 密集型任务(2N)： 这种任务应用起来，系统会用大部分的时间来处理 I/O 交互，而线程在处理 I/O 的时间段内不会占用 CPU 来处理，这时就可以将 CPU 交出给其它线程使用。因此在 I/O 密集型任务的应用中，我们可以多配置一些线程，具体的计算方法是 2N。  

CPU 密集型简单理解就是利用 CPU 计算能力的任务比如你在内存中对大量数据进行排序。单凡涉及到网络读取，文件读取这类都是 IO 密集型，这类任务的特点是 CPU 计算耗费时间相比于等待 IO 操作完成的时间来说很少，大部分时间都花在了等待 IO 操作完成上。  

### Executor  
1. 主线程首先要创建实现 Runnable 或者 Callable 接口的任务对象。  
2. 把创建完成的实现 Runnable/Callable接口的 对象直接交给 ExecutorService 执行:   
3. 如果执行 ExecutorService.submit（…），ExecutorService 将返回一个实现Future接口的对象  
4. 最后，主线程可以执行 FutureTask.get()方法来等待任务执行完成。主线程也可以执行 FutureTask.cancel来取消此任务的执行。  


## Threadlocal  
还不太清楚  

## Java怎么实现线程  
① 继承 Thread 类并重写 run 方法。实现简单，但不符合里氏替换原则，不可以继承其他类。

② 实现 Runnable 接口并重写 run 方法。避免了单继承局限性，实现解耦。

③实现 Callable 接口并重写 call 方法。可以获取线程执行结果的返回值，并且可以抛出异常。

