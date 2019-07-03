# good good study, day day up



# 数据结构

## Queue

> Queue： 基本上，一个队列就是一个先入先出（FIFO）的数据结构
>
> [《java队列——queue详细分析》](https://www.cnblogs.com/lemon-flm/p/7877898.html)

| 类型          | 说明       | 例                                                        |
| ------------- | ---------- | --------------------------------------------------------- |
| Deque         | 双端队列   | LinkedList  ,ArrayDeque                                   |
| AbstractQueue | 非阻塞队列 | ConcurrentLinkedQueue，PriorityQueue（优先队列）          |
| BlockingQueue | 阻塞队列   | PriorityBlockingQueue（优先阻塞队列），ArrayBlockingQueue |



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





  

# 分布式

## 分布式事务

[分布式事务](https://juejin.im/post/5b5a0bf9f265da0f6523913b)

ACID

2PC

TCC

本地消息表

mq

saga

