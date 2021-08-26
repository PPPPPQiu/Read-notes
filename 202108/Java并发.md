# Java并发  

### 线程同步的方式  
1. 信号  
2. 锁机制  
3. 条件变量  
4. 信号量  

#### 典型的锁机制  
1. 读写锁  
2. 互斥锁  
3. 条件变量  
4. 自旋锁  

线程锁：互斥锁，条件变量，自旋锁  

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

#### 1.都是可重入锁  
#### 2.synchronized依赖于JVM而ReentrantLock依赖于API  
#### 3.ReentrantLock有一些高级功能：等待可中断，可实现公平锁，可实现选择性通知  

## volatile  
声明为 volatile ，这就指示 JVM，这个变量是共享且不稳定的，每次使用它都到主存中进行读取。  
volatile 关键字 除了防止 JVM 的指令重排 ，还有一个重要的作用就是保证变量的可见性。  

### sychronized和volatile的区别
1. volatile 关键字是线程同步的轻量级实现，所以 volatile 性能肯定比synchronized关键字要好 。但是 volatile 关键字只能用于变量而 synchronized 关键字可以修饰方法以及代码块 。  
2. volatile 关键字能保证数据的可见性，但不能保证数据的原子性。synchronized 关键字两者都能保证。  
3. volatile 关键字主要用于解决变量在多个线程之间的可见性，而 synchronized 关键字解决的是多个线程之间访问资源的同步性。  


















