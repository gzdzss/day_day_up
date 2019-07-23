# good good study, day day up











## Unsafe

[Unsafe](https://tech.meituan.com/2019/02/14/talk-about-java-magic-class-unsafe.html)是位于sun.misc包下的一个类，主要提供一些用于执行低级别、不安全操作的方法，如直接访问系统内存资源、自主管理内存资源等，这些方法在提升Java运行效率、增强Java语言底层资源操作能力方面起到了很大的作用。但由于Unsafe类使Java语言拥有了类似C语言指针一样操作内存空间的能力，这无疑也增加了程序发生相关指针问题的风险。在程序中过度、不正确使用Unsafe类会使得程序出错的概率变大，使得Java这种安全的语言变得不再“安全”，因此对Unsafe的使用一定要慎重。



Unsafe提供的API大致可分为内存操作、CAS、Class相关、对象操作、线程调度、系统信息获取、内存屏障、数组操作等几类

![unsafe1.png](https://github.com/gzdzss/day_day_up/blob/master/image/unsafe/unsafe1.png?raw=true)





## Atomiclnteger

[AtomicInteger](https://zhuanlan.zhihu.com/p/37302263) 是一个 Java concurrent 包提供的一个原子类，通过这个类可以对 Integer 进行一些原子操作。这个类的源码比较简单，主要是依赖于 sun.misc.Unsafe 提供的一些 native 方法保证操作的原子性。



### getAndAdd

1. 根据对象 var1 和内存偏移量 var2 来定位内存地址，获取当前地址值
2. 循环通过 CAS 操作更新当前地址值直到更新成功
3. 返回旧值





## AQS





###  [ReentrantLock](https://juejin.im/post/5aeb0a8b518825673a2066f0) 

**支持重入性，表示能够对共享资源能够重复加锁，即当前线程获取该锁再次获取不会被阻塞**



ReentrantLock支持两种锁：**公平锁**和**非公平锁**。**何谓公平性，是针对获取锁而言的，如果一个锁是公平的，那么锁的获取顺序就应该符合请求上的绝对时间顺序，满足FIFO**。





###  [Condition](https://zhuanlan.zhihu.com/p/39594030) 





# 数据结构

## Queue

> Queue： 基本上，一个队列就是一个先入先出（FIFO）的数据结构
>
> [《java队列——queue详细分析》](https://www.cnblogs.com/lemon-flm/p/7877898.html)

| 类型          | 说明                                                         | 例                                                        |
| ------------- | ------------------------------------------------------------ | --------------------------------------------------------- |
| Deque         | 双端队列                                                     | LinkedList  ,ArrayDeque                                   |
| AbstractQueue | 非阻塞队列                                                   | ConcurrentLinkedQueue，PriorityQueue（优先队列）          |
| BlockingQueue | [阻塞队列](https://www.cnblogs.com/jackyuj/archive/2010/11/24/1886553.html) | PriorityBlockingQueue（优先阻塞队列），ArrayBlockingQueue |



## Set

> Set:  不可重复的集合
>
> [Java Set集合的详解](https://blog.csdn.net/qq_33642117/article/details/52040345) ,
>
>  [HashSet,TreeSet和LinkedHashSet的区别](https://www.cnblogs.com/Terry-greener/archive/2011/12/02/2271707.html)

| 类型          | 说明                 |
| ------------- | -------------------- |
| HashSet       | 无序集合             |
| LinkedHashSet | 按插入顺序排列的集合 |
| TreeSet       | 有序的集合           |



## List

> List: 集合
>
> [java集合详解](https://blog.csdn.net/wz249863091/article/details/52853360)

| 类型       | 说明                      | 线程安全 | 相对优势     |
| ---------- | ------------------------- | -------- | ------------ |
| ArrayList  | 数组                      | 否       | 获取，修改   |
| LinkedList | 双向链表                  | 否       | 添加，删除， |
| Vector     | 数组                      | 是       | 过时         |
| Stack      | 数组，继承Vector,后进先出 | 是       | 过时         |



 

## Map

> Map: 映射
>
> [Java map 详解 - 用法、遍历、排序、常用API等](https://baike.xsoftlab.net/view/250.html) 
>
> [HashMap? ConcurrentHashMap?](https://crossoverjie.top/2018/07/23/java-senior/ConcurrentHashMap/)
>
> [HashMap在JDK7和JDK8中的区别](https://zhuanlan.zhihu.com/p/59250175)
>
> jdk7:基于链表+数组实现,底层维护一个Entry数组
>
> jdk8:基于位桶+链表/红黑树的方式实现,底层维护一个Node数组
>
> 







| 类型              | 说明                 | key可为null | 线程安全 |
| ----------------- | -------------------- | ----------- | -------- |
| HashMap           | 无序                 | 是          | 否       |
| Hashtable         | 无序                 | 否          | 是       |
| TreeMap           | 根据key排序          | 否          | 否       |
| LinkedHashMap     | 保存了记录的插入顺序 | 是          | 否       |
| ConcurrentHashMap | 无序                 | 是          | 是       |

>## 不同点
>
>**1.发生hash冲突时**
>
> **JDK7:**发生hash冲突时，新元素插入到链表头中，即新元素总是添加到数组中，就元素移动到链表中。 **JDK8：**发生hash冲突后，会优先判断该节点的数据结构式是红黑树还是链表，如果是红黑树，则在红黑树中插入数据；如果是链表，则将数据插入到链表的尾部并判断链表长度是否大于8，如果大于8要转成红黑树。
>
>**2.扩容时** **JDK7:**在扩容resize（）过程中，采用单链表的头插入方式，在将旧数组上的数据 转移到 新数组上时，转移操作 = 按旧链表的正序遍历链表、在新链表的头部依次插入，即在转移数据、扩容后，容易出现链表逆序的情况 。 多线程下resize()容易出现死循环。此时若（多线程）并发执行 put（）操作，一旦出现扩容情况，则 容易出现 环形链表，从而在获取数据、遍历链表时 形成死循环（Infinite Loop），即 死锁的状态 。
>
>**JDK8:**由于 JDK 1.8 转移数据操作 = 按旧链表的正序遍历链表、在新链表的尾部依次插入，所以不会出现链表 逆序、倒置的情况，故不容易出现环形链表的情况 ，但jdk1.8仍是线程不安全的，因为没有加同步锁保护。
>
>**建议:** 
>
>1.使用时设置初始值,避免多次扩容的性能消耗 
>
>2.使用自定义对象作为key时,需要重写hashCode和equals方法 
>
>3.多线程下,使用CurrentHashMap代替HashMap



>[jdk8与jdk7中hashMap的resize分析](https://www.cnblogs.com/luozhiyun/p/10616972.html)
>
>在jdk7中,在resize的时候首先阈值是用newCapacity * loadFactor 。然后一个个的遍历Entry数组，然后看看里面的元素是否已经是一条链表了，如果是链表的话，那么就重新计算在新的table中的槽值。
>
>在jdk8中使用的是2次幂的扩展(指长度扩为原来2倍)，所以，元素的位置要么是在原位置，要么是在原位置再移动2次幂的位置。
>因此，我们在扩充HashMap的时候，不需要像JDK1.7的实现那样重新计算hash，通过使用e.hash & oldCap来计算高位和低位的hash值，来把原来在一个槽位上面的链表拆分成两个链表即可
>有一点注意区别，JDK1.7中rehash的时候，旧链表迁移新链表的时候，如果在新表的数组索引位置相同，则链表元素会倒置，但是从上图可以看出，JDK1.8不会倒置。



# JVM

## 运行时数据区域

![img](https://raw.githubusercontent.com/gzdzss/day_day_up/master/image/jvm/jvm1.jpg)



  ![img](https://raw.githubusercontent.com/gzdzss/day_day_up/master/image/jvm/jvm2.jpg)



### 程序计数器（Program Counter Register）
程序计数器：是一块较小的内存空间，它可以看作是当前线程所执行的字节码的行号指示器。每个线程都有自己的独立的程序计数器。如果线程正在执行的是Java方法，那么这个计数器的值就是正在执行的虚拟机字节码指令的地址；如果正在执行的是Native方法，这个计数器值为空（undefined）。此内存区域是唯一一个在Java虚拟机规范中没有规定任何OutOfMemoryError情况的区域。



### java虚拟机栈(Java Virtual Machine Stacks)
线程私有的，它的生命周期与线程相同。虚拟机栈描述的是Java方法执行的内存模型：每个方法执行的同时都会创建一个栈帧用于存储局部变量表、操作数栈、动态链接、方法出口等信息。局部变量表存放了编译期可知的各种基本数据类型(boolean、byte、char、short、int、float、long、double)、对象引用和returnAddress类型（指向了一条字节码指令的地址）。其中64位长度的long和double类型的数据会占用2个局部变量空间（slot），其余的数据类型占1个。局部变量表所需的内存空间在编译期间分配完成，当进入一个方法时，这个方法需要在帧中分配多大的局部变量空间是完全确定的，在方法运行期间不会改变局部变量表的大小。如果线程请求栈的深度大于虚拟机所允许的深度，将抛出StackOverflowError异常；无法申请到内存抛出OutOfMemoryError异常。



### 本地方法栈(Native Method Stack)

本地方法栈与虚拟机栈所发挥的作用是非常相似的，它们之间的区别不过是虚拟机栈为虚拟机执行java方法，而本地栈则为虚拟机使用到的Native方法服务。

### java堆(Java Heap)
Java堆是线程共享的，在虚拟机启动时创建。此区域的唯一目的就是存放对象实例，几乎所有的对象实例都在这里分配内存。Java堆是垃圾收集器管理的主要区域，因此很多时候也被称作“GC堆”。由于现在收集器基本都采用分代收集算法，所以Java堆中还可以细分为：新生代和老年代；再细致一点的有Eden空间、From Survivor空间、To Survivor空间等。在实现时，既可以实现成固定大小的，也可以是可扩展的，不过当前主流的虚拟机都是按照可扩展来实现的（通过-Xmx和-Xms控制）

### 方法区(Method Area)

线程共享，用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。这区域的内存回收目标主要是针对常量池的回收和对类型的卸载！，无法满足内存分配需求时，将抛出OutOfMemoryError异常。

### 运行时常量池(Runtime Constant Pool)

他是方法区的一部分，Class文件中除了有类的版本、字段、方法、接口等描述信息外，还有一项信息就是常量池，用于存放编译期生成的各种字面量和符号引用，这部分内容将在类加载后进入方法区的运行时常量池中存放。

运行时常量池是方法区的一部分，自然收到方法区内存的限制，当常量池无法再申请内存时会抛出OutOfMemoryError异常

### 直接内存(Direct Memory)
直接内存不是虚拟机运行时数据区的一部分。但是这部分内存也被频繁地使用，而且也可能导致OutOfMemoryError异常出现。在JDK1.4中新加入了NIO类，引入了一种基于通道与缓存区（buffer）的I/O方式，它可以使用Native函数库直接分配堆外内存，然后通过一个存储在Java堆中的DirectByteBuffer对象作为这块内存的引用进行操作。这样能在一些场景中显著提高性能，因为避免了在Java堆和Native堆中来回复制数据。



## 参数

| **参数名称**                | **含义**                                                   | **默认值**           |                                                              |
| --------------------------- | ---------------------------------------------------------- | -------------------- | ------------------------------------------------------------ |
| -Xms                        | 初始堆大小                                                 | 物理内存的1/64(<1GB) | 默认(MinHeapFreeRatio参数可以调整)空余堆内存小于40%时，JVM就会增大堆直到-Xmx的最大限制. |
| -Xmx                        | 最大堆大小                                                 | 物理内存的1/4(<1GB)  | 默认(MaxHeapFreeRatio参数可以调整)空余堆内存大于70%时，JVM会减少堆直到 -Xms的最小限制 |
| -Xmn                        | 年轻代大小(1.4or lator)                                    |                      | **注意**：此处的大小是（eden+ 2 survivor space).与jmap -heap中显示的New gen是不同的。 整个堆大小=年轻代大小 + 年老代大小 + 持久代大小. 增大年轻代后,将会减小年老代大小.此值对系统性能影响较大,Sun官方推荐配置为整个堆的3/8 |
| -XX:NewSize                 | 设置年轻代大小(for 1.3/1.4)                                |                      |                                                              |
| -XX:MaxNewSize              | 年轻代最大值(for 1.3/1.4)                                  |                      |                                                              |
| -XX:PermSize                | 设置持久代(perm gen)初始值                                 | 物理内存的1/64       |                                                              |
| -XX:MaxPermSize             | 设置持久代最大值                                           | 物理内存的1/4        |                                                              |
| -Xss                        | 每个线程的堆栈大小                                         |                      | JDK5.0以后每个线程堆栈大小为1M,以前每个线程堆栈大小为256K.更具应用的线程所需内存大小进行 调整.在相同物理内存下,减小这个值能生成更多的线程.但是操作系统对一个进程内的线程数还是有限制的,不能无限生成,经验值在3000~5000左右 一般小的应用， 如果栈不是很深， 应该是128k够用的 大的应用建议使用256k。这个选项对性能影响比较大，需要严格的测试。（校长） 和threadstacksize选项解释很类似,官方文档似乎没有解释,在论坛中有这样一句话:"” -Xss is translated in a VM flag named ThreadStackSize” 一般设置这个值就可以了。 |
| -*XX:ThreadStackSize*       | Thread Stack Size                                          |                      | (0 means use default stack size) [Sparc: 512; Solaris x86: 320 (was 256 prior in 5.0 and earlier); Sparc 64 bit: 1024; Linux amd64: 1024 (was 0 in 5.0 and earlier); all others 0.] |
| -XX:NewRatio                | 年轻代(包括Eden和两个Survivor区)与年老代的比值(除去持久代) |                      | -XX:NewRatio=4表示年轻代与年老代所占比值为1:4,年轻代占整个堆栈的1/5 Xms=Xmx并且设置了Xmn的情况下，该参数不需要进行设置。 |
| -XX:SurvivorRatio           | Eden区与Survivor区的大小比值                               |                      | 设置为8,则两个Survivor区与一个Eden区的比值为2:8,一个Survivor区占整个年轻代的1/10 |
| -XX:LargePageSizeInBytes    | 内存页的大小不可设置过大， 会影响Perm的大小                |                      | =128m                                                        |
| -XX:+UseFastAccessorMethods | 原始类型的快速优化                                         |                      |                                                              |
| -XX:+DisableExplicitGC      | 关闭System.gc()                                            |                      | 这个参数需要严格的测试                                       |
| -XX:MaxTenuringThreshold    | 垃圾最大年龄                                               |                      | 如果设置为0的话,则年轻代对象不经过Survivor区,直接进入年老代. 对于年老代比较多的应用,可以提高效率.如果将此值设置为一个较大值,则年轻代对象会在Survivor区进行多次复制,这样可以增加对象再年轻代的存活 时间,增加在年轻代即被回收的概率 该参数只有在串行GC时才有效. |
| -XX:+AggressiveOpts         | 加快编译                                                   |                      |                                                              |
| -XX:+UseBiasedLocking       | 锁机制的性能改善                                           |                      |                                                              |
| -Xnoclassgc                 | 禁用垃圾回收                                               |                      |                                                              |
| -XX:SoftRefLRUPolicyMSPerMB | 每兆堆空闲空间中SoftReference的存活时间                    | 1s                   | softly reachable objects will remain alive for some amount of time after the last time they were referenced. The default value is one second of lifetime per free megabyte in the heap |
| -XX:PretenureSizeThreshold  | 对象超过多大是直接在旧生代分配                             | 0                    | 单位字节 新生代采用Parallel Scavenge GC时无效 另一种直接在旧生代分配的情况是大的数组对象,且数组中无外部引用对象. |
| -XX:TLABWasteTargetPercent  | TLAB占eden区的百分比                                       | 1%                   |                                                              |
| -XX:+*CollectGen0First*     | FullGC时是否先YGC                                          | false                |                                                              |

## GC

### 引用计数算法

为对象添加一个引用计数器，每当有一个地方引用它时，技术起值就加1，当引用失效时，计数器值就减1，任何时刻计数器为0的对象就是不可能再被使用的。



设置 VM options: -XX:+PrintGCDetails

```java
public class ReferenceCountingGC {


    public Object instance = null;
    //这个属性的作用是占点内存，方便看GC日志是否被回收
    private static final int _1MB = 1024 * 1024;
    private byte[] bigSize = new byte[2 * _1MB];

    public static void testGC() {
        ReferenceCountingGC objA = new ReferenceCountingGC();
        ReferenceCountingGC objB = new ReferenceCountingGC();
        objA.instance = objB;
        objB.instance = objA;
        objA = null;
        objB = null;

        //假设这里发生GC，  objA 和objB 是否能被回收

        System.gc();
    }

    public static void main(String[] args) {
        testGC();
    }

}
```

```java
[GC (System.gc()) [PSYoungGen: 7997K->808K(75776K)] 7997K->816K(249344K), 0.0009705 secs] [Times: user=0.00 sys=0.00, real=0.02 secs] 
[Full GC (System.gc()) [PSYoungGen: 808K->0K(75776K)] [ParOldGen: 8K->696K(173568K)] 816K->696K(249344K), [Metaspace: 3178K->3178K(1056768K)], 0.0037006 secs] [Times: user=0.00 sys=0.00, real=0.00 secs] 
Heap
 PSYoungGen      total 75776K, used 1951K [0x000000076b580000, 0x0000000770a00000, 0x00000007c0000000)
  eden space 65024K, 3% used [0x000000076b580000,0x000000076b767cb8,0x000000076f500000)
  from space 10752K, 0% used [0x000000076f500000,0x000000076f500000,0x000000076ff80000)
  to   space 10752K, 0% used [0x000000076ff80000,0x000000076ff80000,0x0000000770a00000)
 ParOldGen       total 173568K, used 696K [0x00000006c2000000, 0x00000006cc980000, 0x000000076b580000)
  object space 173568K, 0% used [0x00000006c2000000,0x00000006c20ae178,0x00000006cc980000)
 Metaspace       used 3192K, capacity 4496K, committed 4864K, reserved 1056768K
  class space    used 344K, capacity 388K, committed 512K, reserved 1048576K
```

从结果可知道 虚拟机并不是通过引用计数算法来判断对象是否存活



### 可达性分析算法（Reachability Analysis） 

在主流实现中，都是采用可达性分析算法判断对象是否存活。基本思路就是通过一系列称为“GC Roots”的对象作为起点，从这些节点开始向下搜索，搜索走过的路径为专用链（Reference CChain）当一个对象到GC Roots没有任何引用链相连时，证明此对象是不可用的。



java中GC Roots对象包括 下面几种

虚拟机栈 

方法区中的静态属性引用的对象

方法区中常量引用的对象

本地方法中的JNI（即Native方法）引用的对象



### 长期存活的对象进入老年代

虚拟机对每个对象定义了一个年龄（Age）的计数器。如果对象Eden出生并且经过第一次Minor GC后 任然存活，并且能被Survivor容纳的话，将被移动到Surivor空间中，并且年龄设为1.对象在Survior区中每“熬过”一次Minor GC. 年龄就增加1岁，当年龄加到一定的程度（默认15），对象晋升老年代，可以通过参数 -XX： MaxTenuringThreshold 设置 









### jdk命令工具



- jps 显示系统内所有虚拟机进程
- jstat  收集虚拟机各方面运行数据
- jinfo 显示虚拟机配置
- jmap 生产虚拟机内存转储快照（heapdump文件）
- jhat 虚拟机转快照分析工具
- jstack 显示虚拟机的线程快照
- jconsole java监控管理控制台
- VisualVM 多合一故障处理工具





#  线程



[thread类源码解读](https://segmentfault.com/a/1190000016058789)



## 线程创建



### 创建方式

- 继承Thread类，覆写run方法
- 实现Runnale接口，将它作为target参数传递给Thread类构造函数



### start运行

```java
System.out.println(Thread.currentThread().getName());
Thread thread = new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
});
thread.start();
```

```java
main
Thread-0
```



### run运行

```java
System.out.println(Thread.currentThread().getName());
Thread thread = new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }
});
thread.run();
```

```java
main
main
```



结论：启动一个线程一定要调用该线程的start方法，否则，并不会创建出新的线程来。



## 线程状态

- 新建（New): 创建后尚未启动的线程
- 运行（Runable）:就绪（ready）和运行中（running）两种状态笼统的称为“运行”。
线程对象创建后，其他线程(比如main线程）调用了该对象的start()方法。该状态的线程位于可运行线程池中，等待被线程调度选中，获取CPU的使用权，此时处于就绪状态（ready）。就绪状态的线程在获得CPU时间片后变为运行中状态（running）。
- 无限期等待（Waiting}:处于这种状态的线程，不会被分配CPU执行时间，它们要等待其他线程显示地唤醒。例：
  - 没有设置Timeout参数的 Object.wait()方法
  - 没有设置Timeout参数的Thread.join()方法
  - LockSupport.park()方法


- 限期等待（Timed Waiting）:处于这种状态的线程也不会被分配CPU执行时间，不过无须等待其他线程显示地唤醒，在一定时间后会由系统自动唤醒。例：

  - Thread.sleep()方法
  - 有设置Timeout参数的 Object.wait()方法
  - 有设置Timeout参数的Thread.join()方法
  - LockSupport.parkNanos()方法
  - LockSupport.parkUntil()方法
- 阻塞（Blocked）:线程被设置为阻塞，与等待的区别是：阻塞状态在等待着获取到一个排他锁，这个事件将在另外一个线程放弃这个锁的时候发生。


- 结束（Terminated）:已终止线程的线程状态，线程已经结束执行。

![img](https://raw.githubusercontent.com/gzdzss/day_day_up/master/image/thread/thread1.jpeg)



### join



demo1

```java
public class demo1 {

    private static void printWithThread(String content) {
        System.out.println("[" + Thread.currentThread().getName() + "线程]: " + content);
    }

    public static void main(String[] args) {
        printWithThread("开始执行main方法");
        Thread myThread = new Thread(() -> {
            printWithThread("我在自定义的线程的run方法里");
            printWithThread("我马上要休息1秒钟, 并让出CPU给别的线程使用.");
            try {
                Thread.sleep(1000);
                printWithThread("已经休息了1秒, 又重新获得了CPU");
                printWithThread("我休息好了, 马上就退出了");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        });
        try {
            myThread.start();
            printWithThread("我在main方法里面, 我要等下面这个线程执行完了才能继续往下执行.");
            myThread.join();
            printWithThread("我在main方法里面, 马上就要退出了.");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

```

```java
[main线程]: 开始执行main方法
[main线程]: 我在main方法里面, 我要等下面这个线程执行完了才能继续往下执行.
[Thread-0线程]: 我在自定义的线程的run方法里
[Thread-0线程]: 我马上要休息1秒钟, 并让出CPU给别的线程使用.
[Thread-0线程]: 已经休息了1秒, 又重新获得了CPU
[Thread-0线程]: 我休息好了, 马上就退出了
[main线程]: 我在main方法里面, 马上就要退出了.
```



demo2

```java
public class demo2 {

    private static void printWithThread(String content) {
        System.out.println("[" + Thread.currentThread().getName() + "线程]: " + content);
    }

    public static void main(String[] args) {
        printWithThread("开始执行main方法");
        Thread myThread = new Thread(() -> {
            printWithThread("我在自定义的线程的run方法里");
            printWithThread("我马上要休息1秒钟, 并让出CPU给别的线程使用.");
            try {
                Thread.sleep(1000);
                printWithThread("已经休息了1秒, 又重新获得了CPU");
                printWithThread("我休息好了, 马上就退出了");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        });
        try {
            myThread.start();
            printWithThread("我在main方法里面, 我要等下面这个线程执行完了才能继续往下执行.");
            myThread.join(500);
            printWithThread("我在main方法里面, 马上就要退出了.");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

```java
[main线程]: 开始执行main方法
[main线程]: 我在main方法里面, 我要等下面这个线程执行完了才能继续往下执行.
[Thread-0线程]: 我在自定义的线程的run方法里
[Thread-0线程]: 我马上要休息1秒钟, 并让出CPU给别的线程使用.
[main线程]: 我在main方法里面, 马上就要退出了.
[Thread-0线程]: 已经休息了1秒, 又重新获得了CPU
[Thread-0线程]: 我休息好了, 马上就退出了
```







 

## volatile



关键字 vlolatile 可以说是java虚拟机提供最轻量级的同步机制，。





volatile变量只能保证可见性，在不符合以下的两条规则的运算场景中，我们仍要通过加锁(synchronized 或者 java.util.conturrent中的原子类)来保证原子性

- 运算结果并不依赖变量的当前值，或者能够保证只有单一的线程修改变量的值
- 变量不需要与其他的变量共同参与必变约束





适合使用volatitle变量来控制并发，当shutdown()方法被调用时，能保证所有的线程中执行doWork()方法都立即停下来

```java 
volatile boolean shutdowonRequested;

public void shutdown() {
    shutdowonRequested = true;
}

public void doWork() {
    while (!shutdowonRequested) {
        // do stuff
    }
}
```



# 线程池





Java通过Executors提供四种线程池，分别为：

- newCachedThreadPool创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。
- newFixedThreadPool 创建一个定长线程池，可控制线程最大并发数，超出的线程会在队列中等待。
- newScheduledThreadPool 创建一个定长线程池，支持定时及周期性任务执行。
- newSingleThreadExecutor 创建一个单线程化的线程池，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行。
- newWorkStealingPool  (jdk1.8引入)会创建一个含有足够多线程的线程池，来维持相应的并行级别，它会通过工作窃取的方式，使得多核的 CPU 不会闲置，总会有活着的线程让 CPU 去运行。





## [ForkJoinPool](http://blog.dyngr.com/blog/2016/09/15/java-forkjoinpool-internals/)  

- `ForkJoinPool` 不是为了替代 `ExecutorService`，而是它的补充，在某些应用场景下性能比 `ExecutorService` 更好。（见 *Java Tip: When to use ForkJoinPool vs ExecutorService* ）
- **ForkJoinPool 主要用于实现“分而治之”的算法，特别是分治之后递归调用的函数**，例如 quick sort 等。
- `ForkJoinPool` 最适合的是计算密集型的任务，如果存在 I/O，线程间同步，`sleep()` 等会造成线程长时间阻塞的情况时，最好配合使用 `ManagedBlocker`。








# 分布式

## 分布式事务

[分布式事务](https://juejin.im/post/5b5a0bf9f265da0f6523913b)

ACID

2PC

TCC

本地消息表

mq

saga

