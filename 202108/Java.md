# Java基础  

## 对Java的理解  
- 简单易学  
- 面向对象  
- 平台无关性  
- 支持多线程  
- 可靠性、安全性  
- 支持网络编程并且很方便  
- 编译与解释并存  

## 对JVM的理解  
Java虚拟机是运行Java字节码的虚拟机，JVM有针对不同系统的特定实现，目的是使用相同的字节码，他们都会给出相同的结果。  
“一次编译，随处可以运行”  

## 面向对象  
### 封装  
封装，是指把一个对象的状态信息（也就是属性）隐藏在对象内部，不允许外部对象直接访问对象的内部信息。  
主要任务是对属性、数据、敏感行为实现隐藏，使对象关系变得简单，降低耦合。  
### 继承  
继承用来扩展类，子类可继承父类的部分属性和行为，使模块具有复用性。  
继承，是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。  
通过使用继承，可以快速地创建新的类，可以提高代码的重用，程序的可维护性，节省大量创建新类的时间 ，提高我们的开发效率。  
### 多态  
多态以封装和继承为基础，根据运行时对象实际类型使同一行为具有不同表现形式。  
多态，表示一个对象具有多种的状态。具体表现为父类的引用指向子类的实例。  
多态指在编译层面无法确定最终调用的方法体，在运行期由 JVM 动态绑定，调用合适的重写方法。  
由于重载属于静态绑定，本质上重载结果是完全不同的方法，因此多态一般专指重写。  

## final  
最终的，不可修改的，用来修饰类、方法、变量：  
- final修饰的类不能被继承，final类中成员方法隐式指定为final方法  
- final修饰的方法不能被重写  
- final修饰的变量是常量，基本数据类型不可修改，引用类型不可指向另一个对象  

## static  
- 修饰成员变量和成员方法：属于类，不属于类的某个对象，被类的所有对象共享，可以用类名直接调用，静态变量存放在方法区  
- 修饰代码块：静态代码块在非静态代码块之前执行，只执行一次  
- 修饰内部类：它的创建不需要依赖外部类，不能使用外部类的非静态成员变量和方法  
- 静态导包：指定导入某个类的静态资源，可以直接使用类中静态成员变量和成员方法  





this、super 不能用在 static 方法中  
### 静态变量  
静态变量 存放在 Java 内存区域的方法区  
### 静态代码块  
静态代码块—>非静态代码块—>构造方法  
静态代码块只执行一次  

### 静态内部类  
静态内部类与非静态内部类之间存在一个最大的区别:  
**非静态内部类在编译完成之后会隐含地保存着一个引用，该引用是指向创建它的外围类，但是静态内部类却没有。**  
没有这个引用就意味着：
**1. 它的创建是不需要依赖外围类的创建。**   
**2. 它不能使用任何外围类的非 static 成员变量和方法。**  

当 Singleton 类加载时，静态内部类 SingletonHolder 没有被加载进内存。只有当调用 getUniqueInstance()方法从而触发 SingletonHolder.INSTANCE 时 SingletonHolder 才会被加载，此时初始化 INSTANCE 实例，并且 JVM 能确保 INSTANCE 只被实例化一次。

这种方式不仅具有延迟初始化的好处，而且由 JVM 提供了对线程安全的支持。


### 静态方法  
静态方法在访问本类的成员时，只允许访问静态成员（即静态成员变量和静态方法），而不允许访问实例成员变量和实例方法  

### 存在继承的情况下，初始化顺序为：  
- 父类（静态变量、静态语句块）  
- 子类（静态变量、静态语句块）  
- 父类（实例变量、普通语句块）  
- 父类（构造函数）  
- 子类（实例变量、普通语句块）  
- 子类（构造函数）  

## 重载与重写  
### 重载  
发生在同一个类中（或者父类和子类之间），方法名必须相同，参数类型不同、个数不同、顺序不同，方法返回值和访问修饰符可以不同。  
重载就是同一个类中多个同名方法根据不同的传参来执行不同的逻辑处理。  

### 重写  
重写发生在运行期，是子类对父类的允许访问的方法的实现过程进行重新编写。  
重写就是子类对父类方法的重新改造，外部样子不能改变，内部逻辑可以改变  
- “两同”即方法名相同、形参列表相同；  
- “两小”指的是子类方法返回值类型应比父类方法返回值类型更小或相等，子类方法声明抛出的异常类应比父类方法声明抛出的异常类更小或相等；  
- “一大”指的是子类方法的访问权限应比父类方法的访问权限更大或相等。  

## String  
### String StringBuffer 和 StringBuilder 的区别
- String：不可变，线程安全  
- StringBuilder：可变，非线程安全  
- StringBuffer：可变，线程安全  


## 反射  
反射是在运行状态中，  
对于任意一个类，都能够知道这个类的所有属性和方法；  
对于任意一个对象，都能够调用它的任意一个方法和属性；  
这种动态获取的信息以及动态调用对象的方法的功能称为反射机制。  

反射赋予了我们在运行时分析类以及执行类中方法的能力。  
通过反射你可以获取任意一个类的所有属性和方法，你还可以调用这些方法和属性。  
优点：灵活，便利  
确定：增加安全问题，性能差  

### 反射创建对象  
- 方法1：通过类对象调用newInstance()方法，例如：String.class.newInstance()  
- 方法2：通过类对象的getConstructor()或getDeclaredConstructor()方法获得构造器（Constructor）对象并调用其newInstance()方法创建对象  
- 例如：String.class.getConstructor(String.class).newInstance("Hello");  

## 创建对象的方式  
- new  
- 反射  
- 反序列化  
- clone()函数  

## 抽象类和接口
抽象类是对类的抽象，接口是对行为的抽象  
抽象类是对整个类的整体进行抽象，包括属性、行为，接口却是对类局部（行为）进行抽象  

抽象类不一定含有抽象方法，接口中的所有方法必须是抽象方法    
接口的字段只能是static final字段，接口的成员只能是public  
抽象类有构造方法，接口没有构造方法  
一个类可以实现多个接口，但是不能继承多个抽象类  


继承抽象类的关键字为extends，接口的为implements  

## 异常  

## IO流  
- 字节操作：InputStream 和 OutputStream  
- 字符操作：Reader 和 Writer  

### 装饰者模式  

### 序列化  
Java 对象在 JVM 退出时会全部销毁，如果需要将对象持久化就要通过序列化实现，将内存中的对象保存在二进制流中，需要时再将二进制流反序列化为对象。  
对象序列化保存的是对象的状态，属于类属性的静态变量不会被序列化。  

## IO  
### BIO 同步阻塞IO模型  
应用程序发起 read 调用后，会一直阻塞，直到内核把数据拷贝到用户空间。  
### NIO  I/O 多路复用模型  同步非阻塞 IO 模型  
同步非阻塞 IO 模型中，应用程序会一直发起 read 调用，等待数据从内核空间拷贝到用户空间的这段时间里，线程依然是阻塞的，直到在内核把数据拷贝到用户空间。  
通过轮询操作，避免了一直阻塞。  
问题：应用程序不断进行 I/O 系统调用轮询数据是否已经准备好的过程是十分消耗 CPU 资源的。  
IO 多路复用模型中，线程首先发起 select 调用，询问内核数据是否准备就绪，等内核把数据准备好了，用户线程再发起 read 调用。read 调用的过程（数据从内核空间->用户空间）还是阻塞的。  
### AIO 异步IO模型  
异步 IO 是基于事件和回调机制实现的，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。  
![image](https://user-images.githubusercontent.com/87803098/132950915-dccaaebe-eaff-4e13-b08d-85967d78ce5e.png)




# Java容器  

## Java集合  
Collection：List，Set，Queue  
Map：SortedMap（TreeMap），HashMap，HashTable  
  
细分：  
List：Vector（Stack），ArrayList，LinkedList  
> 有序，可重复  

Set：SortedSet（TreeSet），HashSet，LinkedHashSet  
> 无序，不可重复  

Queue：Deque（ArrayDeque，LinkedList），PriorityQueue  
> 有序，可重复  
> 按特定的排队规则来确定先后顺序  


### 线程安全否？  
安全：
- Vector  
- CopyOnWriteArrayList：读多写少的场景  
- ConcurrentHashMap  
- HashTable  
- BlockingQueue：一个接口，JDK内部通过链表、数组等方式实现这个接口，表示阻塞队列，非常适合用作数据共享的通道。  
- ConcurrentLinkedQueue  
> 高效的并发队列，链表实现，线程安全的LinkedList，非阻塞队列  

- ConcurrentSkipListMap  
> 跳表实现，一个Map，使用跳表的数据结构进行快速查找  
> 跳表的本质是同时维护多个链表，并且链表是分层的  
> 跳表是一种利用空间换时间的算法  


不安全：
- ArrayList   
- LinkedList  
- HashSet  
- TreeSet  
- HashMap  
- TreeMap  


## List
### ArrayList  
数组，线程不安全    
#### 扩容：新容量是旧容量的1.5倍  

### Vector  
数组，线程安全    
#### 扩容：新容量是旧容量的2倍  

### CopyOnWriteArrayList  
写操作在一个复制的数组上进行，读操作还是在原始数组中进行，读写分离，互不影响。

写操作需要加锁，防止并发写入时导致写入数据丢失。

写操作结束之后需要把原始数组指向新的复制数组。


### LinkedList  
双向链表  


## Set  
comparable接口：compareTo()方法  
comparator接口：compare()方法  
### HashSet  
基于 HashMap 实现的  
> 无序，不可重复  


### LinkedHashSet  
通过 LinkedHashMap 来实现

### TreeSet  
红黑树
> 有序，不可重复  



## Queue  
### ArrayQueue  
数组 + 双指针  

### PriorityQueue  
基于堆结构实现    
线程不安全  
- 利用了二叉堆的数据结构来实现的，底层使用可变长的数组来存储数据  
- 通过堆元素的上浮和下沉，实现了在 O(logn) 的时间复杂度内插入元素和删除堆顶元素  

### ArrayDeque和LinkedList区别  
- ArrayDeque 和 LinkedList 都实现了 Deque 接口，两者都具有队列的功能，但两者有什么区别呢？

- ArrayDeque 是基于可变长的数组和双指针来实现，而 LinkedList 则通过链表来实现。

- ArrayDeque 不支持存储 NULL 数据，但 LinkedList 支持。

- ArrayDeque 插入时可能存在扩容过程, 不过均摊后的插入操作依然为 O(1)。虽然 LinkedList 不需要扩容，但是每次插入数据时均需要申请新的堆空间，均摊性能相比更慢。

- 从性能的角度上，选用 ArrayDeque 来实现队列要比 LinkedList 更好。此外，ArrayDeque 也可以用于实现栈。

## Map

### TreeMap  
红黑树（自平衡的排序二叉树）  
TreeMap它还实现了NavigableMap接口和SortedMap 接口。  
实现 NavigableMap 接口让 TreeMap 有了对集合内元素的搜索的能力。  

实现SortMap接口让 TreeMap 有了对集合中的元素根据键排序的能力。  

### HashMap  
基于哈希表实现。  
线程不安全  
数组+链表 || 数组+红黑树  

#### 扩容：动态扩容，主要参数有：capacity、size、threshold 和 load_factor  
#### 扩容：新容量是旧容量的2倍  
取余(%)操作中如果除数是 2 的幂次则等价于与其除数减一的与(&)操作  
采用二进制位操作 &，相对于%能够提高运算效率  

### ConcurrentHashMap  
ConcurrentHashMap 在执行 size 操作时先尝试不加锁，如果尝试的次数超过 3 次，就需要对每个 Segment 加锁。  
#### JDK 1.7 ：分段的数组+链表 ，使用分段锁机制来实现并发更新操作。  
> 采用了分段锁（Segment），每个分段锁维护着几个桶（HashEntry），多个线程可以同时访问不同分段锁上的桶，从而使其并发度更高。  
> 并发度就是 Segment 的个数   
> Segment 继承自 ReentrantLock。  
#### JDK 1.8 ：数组+链表/红黑二叉树，并发控制使用 synchronized 和 CAS 来操作  
> 使用了 CAS 操作来支持更高的并发度，在 CAS 操作失败时使用内置锁 synchronized  
synchronized 只锁定当前链表或红黑二叉树的首节点，这样只要 hash 不冲突，就不会产生并发，效率又提升 N 倍。  
### LinkedHashMap  

数组+链表 || 数组+红黑树 的基础上增加了一条双向链表  
使用双向链表来维护元素的顺序  

### HashTable  
线程安全，使用 synchronized 来进行同步    
数组+链表  
#### 扩容：新容量是旧容量的2n + 1倍  






































