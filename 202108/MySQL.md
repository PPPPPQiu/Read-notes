
# 主键
主键用于唯一标识一个元组，不能有重复，不允许为空。一个表只能有一个主键。
外键用来和其他表建立联系用，外键是另一表的主键，外键是可以有重复的，可以是空值。一个表可以有多个外键。  
不要使用更新频繁的列作为主键，不适用多列主键（相当于联合索引）  
不要使用 UUID,MD5,HASH,字符串列作为主键（无法保证数据的顺序增长）  
主键建议使用自增 ID 值  


# 树  
平衡二叉树的常用实现方法有 红黑树、AVL 树、替罪羊树、加权平衡树、伸展树 等  

## B树  
多路平衡查找树  
## B+树  
B 树& B+树两者有何异同  
- B 树的所有节点既存放键也存放数据，而 B+树只有叶子节点存放 key 和 data，其他内节点只存放 key。  
- B 树的叶子节点都是独立的;B+树的叶子节点有一条引用链指向与它相邻的叶子节点。  
- B 树的检索的过程相当于对范围内的每个节点的关键字做二分查找，可能还没有到达叶子节点，检索就结束了。而 B+树的检索效率就很稳定了，任何查找都是从根节点到叶子节点的过程，叶子节点的顺序检索很明显。  


## 红黑树  
二叉查找树（二叉搜索树）：左子树节点小于等于根节点，右子树节点大于等于根节点，左右子树也是二分查找树。  
缺点：极端情况下接近线性查找。  
红黑树是一个棵自平衡的二叉查找树，五条重要的性质：  
每个节点要么是黑色，要么是红色  
根节点是黑色  
每个叶子节点（NIL）都是黑色  
每个红色节点的两个子节点都一定是黑色（从叶子到根的所有路径不能存在连续的红节点）  
任意一节点到叶子节到每个叶子节点的路径都包含数量相同的黑色节点  


## Hash表

# MySQL  
关系型数据库  

## 数据库事务  
数据库事务可以保证多个对数据库的操作（也就是 SQL 语句）构成一个逻辑上的整体。构成这个逻辑上的整体的这些数据库操作遵循：要么全部执行成功,要么全部不执行 。  
原子性： 事务是最小的执行单位，不允许分割。事务的原子性确保动作要么全部完成，要么完全不起作用；  
一致性： 执行事务前后，数据保持一致，例如转账业务中，无论事务是否成功，转账者和收款人的总额应该是不变的；  
隔离性： 并发访问数据库时，一个用户的事务不被其他事务所干扰，各并发事务之间数据库是独立的；  
持久性： 一个事务被提交之后。它对数据库中数据的改变是持久的，即使数据库发生故障也不应该对其有任何影响。  

MySQL InnoDB 引擎使用 redo log(重做日志) 保证事务的持久性，使用 undo log(回滚日志) 来保证事务的原子性。

MySQL InnoDB 引擎通过 锁机制、MVCC 等手段来保证事务的隔离性  

保证了事务的持久性、原子性、隔离性之后，一致性才能得到保障  

## 分布式事务
分布式事务指的是允许多个独立的事务资源参与到一个全局的事务中。  
事务资源通常是关系型数据库系统，但也可以是其他类型的资源。   
全局事务要求在其中的所有参与的事务要么都提交，要么都回滚，这对于事务原有的 ACID 要求又有了提高。  
另外，在使用分布式事务时，InnoDB 存储引擎的事务隔离级别必须设置为可串行化。  

分布式事务就是指事务的参与者、支持事务的服务器、资源服务器以及事务管理器分别位于不同的分布式系统的不同节点之上。  
就是一次大的操作由不同的小操作组成，这些小的操作分布在不同的服务器上，且属于不同的应用，分布式事务需要保证这些小操作要么全部成功，要么全部失败。本质上来说，分布式事务就是为了保证不同数据库的数据一致性。  

### 数据库三大范式  
第一范式：每个列都不可以再拆分。  
第二范式：在第一范式的基础上，非主键列完全依赖于主键，而不能是依赖于主键的一部分。  
第三范式：在第二范式的基础上，非主键列只依赖于主键，不依赖于其他非主键。  

### MySQL存储引擎  
**Innodb引擎**：Innodb引擎提供了对数据库ACID事务的支持。并且还提供了行级锁和外键的约束。它的设计的目标就是处理大数据容量的数据库系统。  
**MyIASM引擎**：不提供事务的支持，也不支持行级锁和外键。  
**MEMORY引擎**：所有的数据都在内存中，数据的处理速度快，但是安全性不高。  

MyISAM索引与InnoDB索引的区别？  

- InnoDB 是事务型的，可以使用 Commit 和 Rollback 语句。  
- InnoDB 支持行级锁，而MyISAM 只支持表级锁。  
- InnoDB 支持外键和在线热备份，奔溃概率小恢复快。   
- MyISAM 支持压缩表和空间数据索引。  
- InnoDB 适合频繁修改以及涉及到安全性较高的应用，MyISAM 适合查询以及插入为主的应用。
![UBP$)GIF%C446P3F46FVB%8](https://user-images.githubusercontent.com/87803098/130386469-1ef5f154-2fad-4956-8c81-1c28eba22943.png)

## 索引

### 什么是索引  
索引是一种用于快速查询和检索数据的数据结构。常见的索引结构有: B 树， B+树和 Hash。  
优点：  
可以大大加快数据的检索速度。  
通过创建唯一性索引，可以保证数据库表中每一行数据的唯一性。   
缺点：  
时间方面：创建索引和维护索引要耗费时间，具体地，当对表中的数据进行增加、删除和修改的时候，索引也要动态的维护，会降低增/改/删的执行效率；  
空间方面：索引需要占物理空间。  

索引的使用场景：where，order by，join，索引覆盖  
> 索引覆盖，如果要查询的字段都建立过索引，那么引擎会直接在索引表中查询而不会访问原始数据  

### 索引有哪几种类型  
主键索引：数据表的主键列使用的就是主键索引，数据列不允许重复，不允许为NULL，一个表只能有一个主键。  
二级索引：二级索引的叶子节点存储的数据是主键，包括唯一索引、普通索引、前缀索引
> 唯一索引：唯一索引的属性列不能出现重复的数据，允许为NULL值，一个表允许多个列创建唯一索引。  
> 普通索引：普通索引的唯一作用就是为了快速查询数据，一张表允许创建多个普通索引，并允许数据重复和 NULL。    
> 前缀索引：只适用于字符串类型的数据，前缀索引是对文本的前几个字符创建索引，相比普通索引建立的数据更小。  
> 全文索引：要是为了检索大文本数据中的关键字的信息，是目前搜索引擎使用的一种关键技术。  

覆盖索引：包含了所有查询字段的索引（where、select、order by、group by）,就是把要查询出的列和索引是对应的，不做回表操作    
> 好处：避免InnoDB表进行索引的二次查询；可以把随机IO变成顺序IO加快查询效率。  

聚簇索引：引结构和数据一起存放的索引。主键索引属于聚集索引。  
> 优点：查询速度快  
> 缺点：依赖有序的数据；更新代价大； 
非聚簇索引：索引结构和数据分开存放的索引，二级索引属于非聚集索引。  
> 优点：更新代价小  
> 缺点：依赖有序数据，可能会二次查询  

### 索引的底层数据结构  

#### 哈希索引  
哈希表是键值对的集合，通过键(key)即可快速取出对应的值(value)，因此哈希表可以快速检索数据   
解决冲突：链地址法  
在绝大多数需求为单条记录查询的时候，可以选择哈希索引
**缺点**：hash冲突；hash索引不支持顺序和范围查询  

#### B+树索引（默认）  

在B树中，你可以将键和值存放在内部节点和叶子节点；但在B+树中，内部节点都是键，没有值，叶子节点同时存放键和值。  
B+树的叶子节点有一条链相连，而B树的叶子节点各自独立。  

- 为什么使用B+树  
B树只适合随机检索，而B+树同时支持随机检索和顺序检索；  
B+树空间利用率更高，可减少I/O次数，磁盘读写代价更低。  
B+树的查询效率更加稳定。  
B+树的叶子节点使用指针顺序连接在一起，只要遍历叶子节点就可以实现整棵树的遍历。
增删文件（节点）时，效率更高。  




### 事务  
事务是一个不可分割的数据库操作序列，也是数据库并发控制的基本单位，其执行的结果必须使数据库从一种一致性状态变到另一种一致性状态。事务是逻辑上的一组操作，要么都执行，要么都不执行。  

原子性： 事务是最小的执行单位，不允许分割。事务的原子性确保动作要么全部完成，要么完全不起作用；  
一致性： 执行事务前后，数据保持一致，多个事务对同一个数据读取的结果是相同的；  
隔离性： 并发访问数据库时，一个用户的事务不被其他事务所干扰，各并发事务之间数据库是独立的；  
持久性： 一个事务被提交之后。它对数据库中数据的改变是持久的，即使数据库发生故障也不应该对其有任何影响。  

丢失修改，读脏数据，不可重复读，幻读  
未提交读，提交读，可重复读，可串行化   

事务隔离机制的实现基于锁机制和并发调度。  
按照锁的粒度把数据库锁分为行级锁(INNODB引擎)、表级锁(MYISAM引擎)和页级锁(BDB引擎)。  

数据库锁的类型：  
锁的粒度：行级锁、表级锁  
锁的类别：读锁、写锁  

行级锁，为共享锁和排他锁。   
特点：开销大，加锁慢；会出现死锁；锁定粒度最小，发生锁冲突的概率最低，并发度也最高。  

表级锁，分为表共享读锁（共享锁）与表独占写锁（排他锁）。  
特点：开销小，加锁快；不会出现死锁；锁定粒度大，发出锁冲突的概率最高，并发度最低。  

页级锁   
特点：开销和加锁时间界于表锁和行锁之间；会出现死锁；锁定粒度界于表锁和行锁之间，并发度一般  

InnoDB是基于索引来完成行锁  

#### InnoDB存储引擎的锁的算法有三种  
Record lock：对索引项加锁    
Gap lock：间隙锁，对索引之间的“间隙”、第一条记录前的“间隙”或最后一条后的间隙加锁。    
Next-key lock：record+gap 前两种放入组合，对记录及前面的间隙加锁。  

死锁是指两个或多个事务在同一资源上相互占用，并请求锁定对方的资源，从而导致恶性循环的现象。  


### 悲观锁  乐观锁   
悲观锁：假定会发生并发冲突，屏蔽一切可能违反数据完整性的操作。在查询完数据的时候就把事务锁起来，直到提交事务。  
实现方式：使用数据库中的锁机制    
传统关系型数据库中的锁机制：行锁、表锁、读锁、写锁  
Java里的同步synchronized关键字的实现  
乐观锁：假设不会发生并发冲突，只在提交操作时检查是否违反数据完整性。在修改数据的时候把事务锁起来，通过version的方式来进行锁定。   
实现方式：乐观锁一般会使用版本号机制或CAS算法实现。  
CAS实现，版本号控制  

### MVCC  
多版本并发控制利用了多版本的思想，写操作更新最新的版本快照，而读操作去读旧版本快照，没有互斥关系。  
在 MVCC 中事务的修改操作（DELETE、INSERT、UPDATE）会为数据行新增一个版本快照。  

#### Undo日志  
MVCC 的多版本指的是多个版本的快照，快照存储在 Undo 日志中，该日志通过回滚指针 ROLL_PTR 把一个数据行的所有快照连接起来。  
快照中除了记录事务版本号 TRX_ID 和操作之外，还记录了一个 bit 的 DEL 字段，用于标记是否被删除。  
INSERT、UPDATE、DELETE 操作会创建一个日志，并将事务版本号 TRX_ID 写入。DELETE 可以看成是一个特殊的 UPDATE，还会额外将 DEL 字段设置为 1。  

#### ReadView  
MVCC 维护了一个 ReadView 结构，主要包含了当前系统未提交的事务列表 TRX_IDs {TRX_ID_1, TRX_ID_2, ...}，还有该列表的最小值 TRX_ID_MIN 和 TRX_ID_MAX。  
在进行 SELECT 操作时，根据数据行快照的 TRX_ID 与 TRX_ID_MIN 和 TRX_ID_MAX 之间的关系，从而判断数据行快照是否可以使用：  
TRX_ID < TRX_ID_MIN，表示该数据行快照时在当前所有未提交事务之前进行更改的，因此可以使用。  
TRX_ID > TRX_ID_MAX，表示该数据行快照是在事务启动之后被更改的，因此不可使用。  
TRX_ID_MIN <= TRX_ID <= TRX_ID_MAX，需要根据隔离级别再进行判断：  
提交读：如果 TRX_ID 在 TRX_IDs 列表中，表示该数据行快照对应的事务还未提交，则该快照不可使用。否则表示已经提交，可以使用。  
可重复读：都不可以使用。因为如果可以使用的话，那么其它事务也可以读到这个数据行快照并进行修改，那么当前事务再去读这个数据行得到的值就会发生改变，也就是出现了不可重复读问题。  
在数据行快照不可使用的情况下，需要沿着 Undo Log 的回滚指针 ROLL_PTR 找到下一个快照，再进行上面的判断。  

#### 快照读与当前读  
快照读：MVCC 的 SELECT 操作是快照中的数据，不需要进行加锁操作。  
当前读：MVCC 其它会对数据库进行修改的操作（INSERT、UPDATE、DELETE）需要进行加锁操作，从而读取最新的数据。可以看到 MVCC 并不是完全不用加锁，而只是避免了 SELECT 的加锁操作。  

#### Next-Key Locks  
在可重复读（REPEATABLE READ）隔离级别下，使用 MVCC + Next-Key Locks 可以解决幻读问题。  
Next-Key Locks 是 MySQL 的 InnoDB 存储引擎的一种锁实现。  

- Record Locks
  锁定一个记录上的索引，而不是记录本身。  
  如果表没有设置索引，InnoDB 会自动在主键上创建隐藏的聚簇索引，因此 Record Locks 依然可以使用。  
- Gap Locks
  锁定索引之间的间隙，但是不包含索引本身。例如当一个事务执行以下语句，其它事务就不能在 t.c 中插入 15。  
- Next-Key Locks
  它是 Record Locks 和 Gap Locks 的结合，不仅锁定一个记录上的索引，也锁定索引之间的间隙。它锁定一个前开后闭区间，例如一个索引包含以下值：10, 11, 13, and 20，那么就需要锁定以下区间：  
  
  


## SQL语句执行  
MySQL 主要分为 Server 层和存储引擎层  
Server 层主要包括连接器、查询缓存、分析器、优化器、执行器，同时还有一个日志模块（binlog），这个日志模块所有执行引擎都可以共用  
存储引擎层 ，主要负责数据的存储和读取，redolog 只有 InnoDB 有。  

### 查询语句的执行流程如下：权限校验（如果命中缓存）--->查询缓存--->分析器--->优化器--->权限校验--->执行器--->引擎
### 更新语句执行流程如下：分析器---->权限校验---->执行器--->引擎---redo log(prepare 状态)--->binlog--->redo log(commit状态)

查询语句的执行顺序：  
- from  
- join  
- on  
- where  
- group by  
- sum,avg  
- having  
- select  
- order by  
- limit  




### 视图  
视图，本质上是一种虚拟表，在物理上是不存在的，其内容与真实的表相似，包含一系列带有名称的列和行数据。  
视图使开发者只关心感兴趣的某些特定数据和所负责的特定任务，只能看到视图中所定义的数据，而不是视图所引用表中的数据，从而提高了数据库中数据的安全性。  

视图使用场景：重用SQL语句，减缓复杂的SQL操作，使用表的组成部分而不是整个表，保护数据，更改数据格式和表示。  
优点：查询简单化，数据安全性，逻辑数据独立性  
缺点：性能，修改限制  

### 游标  
游标是系统为用户开设的一个数据缓冲区，存放SQL语句的执行结果，每个游标区都有一个名字。  
用户可以通过游标逐一获取记录并赋给主变量，交由主语言进一步处理。  

### 存储过程  
存储过程是一个预编译的SQL语句，优点是允许模块化的设计，就是说只需要创建一次，以后在该程序中就可以调用多次。  
如果某次操作需要执行多次SQL，使用存储过程比单纯SQL语句执行要快。  

### 触发器  
触发器是用户定义在关系表上的一类由事件驱动的特殊的存储过程。  
触发器是指一段代码，当触发某个事件时，自动执行这些代码。  


## SQL语句  

### EXPLAIN  
使用EXPLAIN关键字可以模拟优化器执行SQL查询语句，从而知道MySQL如何处理SQL语句，分析查询语句或表结构的性能瓶颈  
通过EXPLAIN，可以分析出以下结果：  
- 表的读取顺序  
- 数据读取操作的操作类型  
- 哪些索引可以使用  
- 哪些索引被实际使用  
- 表之间的引用  
- 每张表有多少行被优化器查询  

### drop delete truncate  
drop(丢弃数据): drop table 表名 ，直接将表都删除掉，在删除表的时候使用。  
truncate (清空数据) : truncate table 表名 ，只删除表中的数据，再插入数据的时候自增长 id 又从 1 开始，在清空表中数据的时候使用。  
delete（删除数据） : delete from 表名 where 列名=值，删除某一列的数据，如果不加 where 子句和truncate table 表名作用类似。  

truncate 和 delete 只删除数据不删除表的结构(定义)，执行 drop 语句，此表的结构也会删除，也就是执行 drop 之后对应的表不复存在。  

## 数据库规范  

### 数据库设计的基本步骤  
1. 需求分析：分析用户需求，包括数据、功能、性能需求    
2. 概念结构设计：主要采用ER模型  
3. 逻辑结构设计：ER模型到关系模型的转换    
4. 物理结构设计：为设计的数据选择合适的存储结构和存取路径    
5. 数据库实施：包括编程、测试和试运行    
6. 数据库的运行与维护：系统的运行与日常维护  



### 数据库基本设计规范  
1. 所有表必须使用 Innodb 存储引擎  
2. 数据库和表的字符集统一使用 UTF8  
3. 所有表和字段都需要添加注释   
4. 尽量控制单表数据量的大小,建议控制在 500 万以内。  
5. 谨慎使用 MySQL 分区表  
6.尽量做到冷热数据分离,减小表的宽度   
7. 禁止在表中建立预留字段  
8. 禁止在数据库中存储图片,文件等大的二进制数据  
9. 禁止在线上做数据库压力测试  
10. 禁止从开发环境,测试环境直接连接生成环境数据库  


### 索引设计规范  
1. 限制每张表上的索引数量,建议单张表索引不超过 5 个  
2. 禁止给表中的每一列都建立单独的索引  
3. 每个 Innodb 表必须有个主键  
4. 常见索引列建议  
5. 如何选择索引列的顺序  
6. 避免建立冗余索引和重复索引（增加了查询优化器生成执行计划的时间）  
7. 对于频繁的查询优先考虑使用覆盖索引  
8.索引 SET 规范  

### 创建索引的注意事项  
- 选择合适的字段创建索引：不为NULL，被频繁查询，被作为条件查询，频繁需要排序，被经常频繁用于连接的字段  
- 被频繁更新的字段应该慎重建立索引  
- 尽可能的考虑建立联合索引而不是单列索引  
- 注意避免冗余索引  
- 考虑在字符串类型的字段上使用前缀索引代替普通索引  

### 索引失效的情况  
- like以%开头  
- where中索引列使用了函数  
- or语句前后没有同时使用索引  
- 组合索引，不是使用第一列索引，索引失效  
- 数据类型出现隐式转化。如varchar不加单引号的话可能会自动转换为int型，使索引无效，产生全表扫描。  
- 在索引列上使用 IS NULL 或 IS NOT NULL操作。  
- 在索引字段上使用not，<>，!=。  
- 对索引字段进行计算操作、字段上使用函数。  
- 当全表扫描速度比索引速度快时，mysql会使用全表扫描，此时索引失效。  

## 日志  
进制日志 binlog（归档日志）和事务日志 redo log（重做日志）和 undo log（回滚日志）。  

### redo log  
redo log（重做日志）是InnoDB存储引擎独有的，它让MySQL拥有了崩溃恢复能力。
MySQL 实例挂了或宕机了，重启时，InnoDB存储引擎会使用redo log恢复数据，保证数据的持久性与完整性。  
每条 redo 记录由“表空间号+数据页号+偏移量+修改数据长度+具体修改的数据”组成  


### binlog  
binlog 是逻辑日志，记录内容是语句的原始逻辑，类似于“给 ID=2 这一行的 c 字段加 1”，属于MySQL Server 层。  
redo log 它是物理日志，记录内容是“在某个数据页上做了什么修改”，属于 InnoDB 存储引擎。  
MySQL数据库的数据备份、主备、主主、主从都离不开binlog，需要依靠binlog来同步数据，保证数据一致性。  

redo log（重做日志）让InnoDB存储引擎拥有了崩溃恢复能力。  
 
binlog（归档日志）保证了MySQL集群架构的数据一致性。  

为了解决两份日志之间的逻辑一致问题，InnoDB存储引擎使用两阶段提交方案。  
将redo log的写入拆成了两个步骤prepare和commit，这就是两阶段提交。  

使用两阶段提交后，写入binlog时发生异常也不会有影响，因为MySQL根据redo log日志恢复数据时，发现redo log还处于prepare阶段，并且没有对应binlog日志，就会回滚该事务。  
### undo log
在 MySQL 中，恢复机制是通过 回滚日志（undo log） 实现的，所有事务进行的修改都会先先记录到这个回滚日志中，然后再执行相关的操作。  

MySQL InnoDB 引擎使用 redo log(重做日志) 保证事务的持久性，使用 undo log(回滚日志) 来保证事务的原子性。

MySQL数据库的数据备份、主备、主主、主从都离不开binlog，需要依靠binlog来同步数据，保证数据一致性。













> https://www.nowcoder.com/discuss/637486?source_id=profile_create_nctrack&channel=-1  
