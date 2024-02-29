# java集合

### 1.java集合框架的基础接口？

**Collection:集合接口**

1.list:是Collection的子接口，用来存储有序的数据集合，可重复。

-   ArrayList：底层采用动态数组存储，默认空数组。
-    LinkedList：底层采用双向链表存储，增删数据速度快
-    Vector:实现了可扩展的对象数组，大小可随时增大缩小。（线程安全）

2.Set:Collection的子接口，用来存储无序的数据集合，不可重复

- HashSet：底层采用哈希表（hashmap)存储，数据无序且唯一。
- TreeSet:采用有序二叉树存储数据，遵循自然排序（从小到大）

**Map:与Collection并列，用来存储具有映射关系的数据(key-value)**

- HashMap:哈希表结构存储结构，key值可以为null

- TreeMap：红黑树算法的实现

- HashTable:哈希表实现，key值不可以为null

  properties:hashtable的子类，键值对存储数据均为String类型，主要为配置文件。

  【特点】以key-value的形式存储数据，key值以set集合形式存储，唯一且无序。value是以collection形式存储，可以重复。
  
  **Queue**
  
  PriorityQueue : Object[] 数组来实现⼆叉堆
  
  ArrayQueue : Object[] 数组 + 双指针

### 2.Collection和Colletions有什么区别？

Collection:是一个集合接口，提供了对集合对象进行基本操作的通用接口，所有集合都是它的子类。

Collections:是一个包装类，包含很多静态方法，不能被实例化。ex:Collections.sort(list)排序

### 3.HashMap和Hashtable之间的区别

- 二者都是实现了map接口，使用键值对的形式存储和操作数据。
- hashmap运行key-value可以为null,但hashtable不允许。
- hashmap适合单线程，hashtable是同步的，适合多线程。
- hashmap提供对key的Set进行遍历，因此是fail-fast;hashtable提供对key的Enumeration进行遍历，不支持fail-fast。
- hashtable被认为是遗留类，在寻找迭代的时候修改map，应该用ConcurrentHashMap.

### 4.如何决定使用HashMap还是TreeMap?

对于在Map中插入、删除、定位一个元素，HashMap最好的选择。

对一个key集合进行有序的遍历，TreeMap是更好的选择。

【TreeMap】是基于红黑树的一种提供顺序访问的Map,它的get和put操作的时间复杂度都是0(long(n)).具体的顺序由指定的额Comparator来决定，或者根据键值对的顺序。

### 5.HashMap的底层数据结构存储

JDK1.7:数组+链表

JDK1.8:数组+链表+红黑树

红黑树是一种近似平衡的二叉查找树，其主要的优点就是“平衡“，即左右子树高度几乎一致，以此来防止树退化为链表，通过这种方式来保障查找的时间复杂度为 log(n)。

**红黑树：当hash表的单一链表长度超过8个，并且数组长度大于等于64的时候，链表转为红黑树。**

关于红黑树的内容，网上给出的内容非常多，主要有以下几个特性：
1、每个节点要么是红色，要么是黑色，但根节点永远是黑色的；

2、每个红色节点的两个子节点一定都是黑色；

3、红色节点不能连续（也即是，红色节点的孩子和父亲都不能是红色）；

4、从任一节点到其子树中每个叶子节点的路径都包含相同数量的黑色节点；

5、所有的叶节点都是是黑色的（注意这里说叶子节点其实是上图中的 NIL 节点）；
在树的结构发生改变时（插入或者删除操作），往往会破坏上述条件 3 或条件 4，需要通过调整使得查找树重新满足红黑树的条件。

**实现过程**：

- 调用HashMap的无参构造方法，加载因子loadFactor赋值为0.75，table数组为空。
- 当添加第一个元素时，创建长度为16的数组，threShold=12.
- 链表长度超过8个，并且数组长度大于等于64的时候，链表转为红黑树
- 当红黑树的节点少于6个时，转化为链表
- 当HashMap的容量超出阈值时，扩容为原来大小的2倍，减少元素的移动，提高效率。

### 6.Hashset的实现原理

HashSet是基于HashMap实现的，HashSet底层使用HashMap来保存所有的元素，因此HashSet的实现比较简单，相关HashSet的操作，基本上都是直接调用底层HashMap的相关方法来完成的HashSet不允许重复的值。

### 7.ArrayList 和 LinkedList的区别

- 数据结构实现：ArrayList是动态数组的数据结构实现，LinkedList是双向链表的数据结构。
- 随机访问效率：ArrayList比LinkedList效率高，因为后者是线性数据结构，需要移动指针。
- 增加、删除效率：ArrayList比LinkedList效率低，因为前者会影响组内下标数据。

**总结**：二者线程都不安全，在需要频繁读取集合时，推荐ArrayList;在插入和删除操作较多时，推荐LinkedList。并且ArrayList可以进行扩容，扩容后是之前的1.5倍。

### 8.ArrayList和Vector的区别

相同点：

- 都是基于索引，内部有一个数组支持
- 维护插入的顺序，可以根据插入元素的顺序来获取元素
- 二者的迭代器实现都是fail-fast的
- 两者允许null值，也可以用索引值进行随机访问

不同点：

- Vector是同步的，线程安全，速度慢
- ArrayList可以进行扩容，扩容后是之前的1.5倍，而Vector默认情况下，扩容是之前的2倍
- vector可以设置增量，ArrayList不可以。

### 9.ArrayList和Array的区别

- Array可以存储基本数据类型和对象，ArrayList只能存储对象。
- Array是指定固定大小，ArrayList是可扩展的大小。
- Array内置方法没有ArrayList多，ex:addAll、removeAll 、iteration等方法只要ArrayList里有。

### 10.在Queue中poll()和remove()的区别

相同点：都是返回第一个元素，并在队列中删除返回的对象。

不同点：如果没有元素poll()会返回null，而remove（）会返回NoSuchElementException异常

![image-20240228200446517](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240228200446517.png)

### 11.LinkedHashMap的特点

是HashMap的一个子类，保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录先插入，也可以在构造时带参数，按照访问次序排序。

### 12.HashMap的底层实现原理

HashMap中的put()和get()的实现原理：
1）map.put(k,v)实现原理
（1）首先将k,v封装到Node对象当中（节点）。
（2）然后它的底层会调用K的hashCode()方法得出hash值。
（3）通过哈希表函数/哈希算法，将hash值转换成数组的下标，下标位置上如果没有任何元素，就把Node添加到这个位置上。如果说下标对应的位置上有链表。此时，就会拿着k和链表上每个节点的k进行equal。如果所有的equals方法返回都是false，那么这个新的节点将被添加到链表的末尾。如其中有一个equals返回了true，那么这个节点的value将会被覆盖。
2、map.get(k)实现原理
(1)先调用k的hashCode()方法得出哈希值，并通过哈希算法转换成数组的下标。
(2)通过上一步哈希算法转换成数组的下标之后，在通过数组下标快速定位到某个位置上。如果这个位置上什么都没有，则返回null。如果这个位置上有单向链表，那么它就会拿着K和单向链表上的每一个节点的K进行equals，如果所有equals方法都返回false，则get方法返回null。如果其中一个节点的K和参数K进行equals返回true，那么此时该节点的value就是我们要找的value了，get方法最终返回这个要找的value。

### 13.HashMap并发安全的问题

在Java中，HashMap本身并不是线程安全的，也就是说在多线程环境下对HashMap的操作可能会导致不可预测的结果。这是因为HashMap不是同步的，多个线程同时对其进行读写操作可能会导致数据不一致性。

如果需要在多线程环境下使用HashMap，并且要求线程安全，可以考虑使用ConcurrentHashMap。ConcurrentHashMap是Java提供的线程安全的HashMap的替代实现，它通过使用锁分段技术来确保线程安全，在多线程环境下性能表现较好。

另外，如果不想使用ConcurrentHashMap，也可以通过Collections工具类的静态方法synchronizedMap()来创建一个线程安全的HashMap，该方法返回的Map会对所有方法添加同步关键字来保证线程安全，但相对性能会有所损失。

总的来说，在多线程环境下，为了保证HashMap的安全性，推荐使用ConcurrentHashMap或者通过Collections工具类来创建线程安全的HashMap。

### 14.jdk1.8和jdk1.7的性能对比

jdk1.8:

- 使用数组+链表+红黑树存储
- 将头插法改为尾插法
- 当hash表的单一链表长度超过8个，并且数组长度大于等于64的时候，链表转为红黑树；当红黑树的节点少于6个时，转化为链表
- 扩容时，不会重新计算hash值，而是原先位置的key的hash值与旧数组长度进行与运算：1）为1，迁移到当前位置+原数组长度的位置 2）为0，位置不变。

jdk1.7:

使用数组+链表存储

扩容时，在准备好新数组时，会遍历数组每一个节点，重新计算其hash值，使用头插法将其插入新数组（无限的链表反转），在多线程的情况下，会导致CPU彪高，出现死循环。

### 15.HashMap操作注意事项以及优化

- 扩容是一个特别耗性能的操作，因此在使用HashMap的时候，估算map的大小，初始化的时候给一个大致的值，避免进行重复扩容。
- 负载因子是可以修改的，也可以大于1，但是一般情况不建议修改。
- HashMap是线程不安全的，不要在并发的环境中同时操作HashMap,建议使用ConcurrentHashMap
- jdk1.8引入红黑树大程度优化了HashMap的性能。

### 16.List、Map、Set的区别

![image-20240228205512710](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240228205512710.png)

### 17.TreeSet如何保证对象的有序性及元素的唯一性

TreeSet的特点：数据会自然排序、不可存储null值、数据不可重复、线程不安全

其底层依赖于TreeMap，TreeMap的数据结构是红黑树，由其底层结构决定了TreeSet中的元素有序且唯一。在使用TreeSet时，如果要添加对象，就必须实现Comparable或Comparator接口。

实现自然排序Comparable，重写comparaTo(T t)方法

实现比较器排序Comparator，重写compare(T  t1, T t2)方法

### 18.HashMap的扩容机制

**扩容(resize)\*就是重新计算容量，向HashMap对象里不停的添加元素，而HashMap对象内部的数组无法装载更多的元素时，对象就需要扩大数组的长度，以便能装入更多的元素\**

当向容器添加元素的时候，会判断当前容器的元素个数，如果大于等于阈值—即**当前数组的长度乘以加载因子的值的时候，就要自动扩容啦**。

\*HashMap1.7\**

**在JDK1.7的扩容机制相对简单，有以下特质：**

- **空参数的构造函数：以默认容量、默认负载因子、默认阈值初始化数组。内部数组是空数组。**
- **有参构造函数：根据参数确定容量、负载因子、阈值等。**
- **第一次put时会初始化数组，其容量变为不小于指定容量的2的幂数。然后根据负载因子确定阈值。**
- **如果不是第一次扩容，则 新容量=旧容量X2  新阈值=新容量X负载因子**

***\*HashMap 1.8\****

**HashMap的容量变化通常存在以下几种情况：**

- **空参数的构造函数：实例化的HashMap默认内部数组是null，即没有实例化。第一次调用put方法时，则会开始第一次初始化扩容，长度为16。**
- **有参构造函数：用于指定容量。会根据指定的正整数找到不小于指定容量的2的幂数，将这个数设置赋值给阈值（threshold）。第一次调用put方法时，会将阈值赋值给容量，然后让 阈值=容量X负载因子****（因此并不是我们手动指定了容量就一定不会触发扩容，超过阈值后一样会扩容！！）**
- **如果不是第一次扩容，则容量变为原来的2倍，阈值也变为原来的2倍。**

**此外还有几个细节需要注意：**

- **首次put时，先会触发扩容（算是初始化），然后存入数据，然后判断是否需要扩容；**
- **不是首次put，则不再初始化，直接存入数据，然后判断是否需要扩容；**
- **1.7 是大于阈值（threshold = factor \* capacity ）且没有空位时才扩容，而 1.8 是大于阈值就扩容**
- **1.7是先扩容再插入数据，1.8是先插入数据再扩容**
- **扩容是一个特别耗性能的操作，所以当程序员在使用HashMap的时候，估算map的大小，初始化的时候给一个大致的数值，避免map进行频繁的扩容**
- **HashMap的容量达到2的30次方，就不会再进行扩容了**

### 19.HashMap链表与红黑树的转换时机

当hash表的单一链表长度超过8个，并且数组长度大于等于64的时候，链表转为红黑树；当红黑树的节点少于6个时，转化为链表。

### 20.Hash碰撞以及解决办法

    hash冲突
    当两个key通过hashCod计算相同时（其实hashCode是随机产生的，是有可能hashCode相同）,则发生了hash冲突，开放定址法、再哈希法、链地址法、建立公共溢出区
HashMap解决hash冲突的方式是用链表。当发生hash冲突时，则将存放在数组中的Entry设置为新值的next，说白就是比如A和B都hash后都映射到下标i中，之前已经有A了，当map.put(B)时，将B放到下标i中，A则为B的next，所以新值存放在数组中，旧值在新值的链表上

**开放定址法**：当关键字key的哈希地址p=H（key）出现冲突时，以p为基础，产生另一个哈希地址p1，如果p1仍然冲突，再以p为基础，产生另一个哈希地址p2，…，直到找出一个不冲突的哈希地址pi ，将相应元素存入其中
**再哈希法**：同时构造多个不同的哈希函数，当哈希地址Hi=RH1（key）发生冲突时，再计算Hi=RH2（key）……，直到冲突不再产生。
**链地址法**：这种方法的基本思想是将所有哈希地址为i的元素构成一个称为同义词链的单链表，并将单链表的头指针存在哈希表的第i个单元中，因而查找、插入和删除主要在同义词链中进行。链地址法适用于经常进行插入和删除的情况。
**建立公共溢出区**：将哈希表分为基本表和溢出表两部分，凡是和基本表发生冲突的元素，一律填入溢出表。

### 21.如何保证集合是安全的

- 使用juc包下的ConcurrentHashMap，线程安全且速度快。
- 选用Synchronize或者Lock加锁解决。

### 22.迭代器Iterator是什么，如何使用

Iterator接口提供遍历任何Collection的接口，从一个Collection的一个接口中使用迭代器方法来获取迭代器实例。迭代器取代了java集合框架中的Enumeration，迭代器允许调用者在调用过程中移除元素。特点是更加安全，当遍历的集合元素被改变时，就会抛出ConcurrentHashMap异常。

代码如下：

```
List <String> list = new ArrayList<>();
Iterator<String> it = list.iterator();
while(it.hasNext()){
String obj = it.next();
System.out.println(ob);
}
```

### 23.Iterator和ListIterator的区别

- lterator可以遍历set和list集合，而listiterator只能遍历List.
- iterator只能单向遍历，listiterator可以双向遍历。
- listiterator从iterator接口继承来，然后添加了一些额外功能。

### 24.简述并发修改异常的原因及解决办法

ConcurrentModificationException：在迭代器迭代的过程中，集合的元素个数发生了改变，此时导致修改异常。

解决方法：1）通过列表迭代器自带的方法完成元素的增删2）通过for循环遍历集合，使用集合中的方法完成元素增删操作。

### 25.聊聊juc包下的并发工具

1.ReentrantLock是一种可重入的独占锁，它允许同一个线程多次获取同一个锁而不会被阻塞。

它的功能类似于synchronized是一种互斥锁，可以保证线程安全，相对于synchronized，

ReentrantLock具备如下特点：

    可中断
    可以设置超时时间
    可以设置为公平锁
    支持多个条件变量
    与synchronized一样，都支持可重入

它的主要应用场景是在多线程环境下对共享资源进行独占式访问，以保证数据的一致性和安全性
2.Semaphore (信号量) 是一种用于多线程编程的同步工具，用于控制同时访问某个资源的线程数量。Semaphore维护了一个计数器，线程可以通过调用acquire()方法来获取Semaphore中的许可证，当计数器为0时，调用acquire()线程将被阻塞，直到有其他线程释放许可证，线程可以通过调用release()方法来释放Semaphore中的许可证，这会使Semaphore中的计数器增加，从而允许更多的线程访问共享资源。

3.CountDownLatch （闭锁）是一个同步协助类，允许一个或多个线程等待，直到其他线程完成操作集。CountDownLatch使用给定的计数值(count) 初始化。await方法会阻塞直到当前的计数值(count) ,由于countDown方法的调用达到0，count为0之后所有等待的线程都会被释放，并且随后对await方法的调用都会立即返回。这是一个一次性现象，-----count 不会被重置
4.CyclicBarrier（回环栅栏或循环屏障），是Java并发库中的一个同步工具，通过它可以实现让一组线程等待只某个状态（屏障点）之后再全部同时执行，叫做回环是因为当所有等待线程都被释放后，CyclicBarrier可以被重用。

### 26.ConcurrentHashMap如何保证线程安全

![image-20240228213346435](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240228213346435.png)

### 27.ConcurrentLinkedQueue和LinkedBlockingQueue有什么区别

![img](https://img2018.cnblogs.com/blog/1771684/201908/1771684-20190822150335865-780841351.png)

两者的区别在于

- **ConcurrentLinkedQueue基于CAS的无锁技术，不需要在每个操作时使用锁，所以扩展性表现要更加优异，在常见的多线程访问场景，一般可以提供较高吞吐量。**

- **LinkedBlockingQueue内部则是基于锁，并提供了BlockingQueue的等待性方法**

  ![image-20240228213714212](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240228213714212.png)

- 单生产者，单消费者 用 LinkedBlockingqueue
- 多生产者，单消费者 用 LinkedBlockingqueue
- 单生产者 ，多消费者 用 ConcurrentLinkedQueue
- 多生产者 ，多消费者 用 ConcurrentLinkedQueue

### 28.hashmap在jdk1.7和1.8中的区别

JDK1.7:数组+链表 采用segment保证安全

JDK1.8:数组+链表+红黑树 采用CAS+synchronize保证安全

### 29.ConcurrentHashMap的实现原理

![image-20240228212936374](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240228212936374.png)

![image-20240228213127978](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240228213127978.png)

![image-20240228213148194](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240228213148194.png)

### 30.Colletions.sort排序内部原理

Collections.sort 排序通过泛化实现对所有类型的排序，对于基础类型，如int，string按照字符表，数字大小排序，对于自定义类型，通过实现Comparable接口，重写compareto函数自定义比较大小的方式，接受对象类型 extends Comparable<? super T>或者Comparator外比较器,Comparable接口的方式比实现Comparator接口耦合性要强。

Collections.sort 内部内部调用的是Arrays.sort方法，对于Arrays类有两个sort方法，一个是sort（object），一个是sort（int），前者用的是归并排序，后者是快排

源码中优化：
1.短数组使用插入排序快
2.混乱度判断（通过找出所有的递增递减子数组，判断）

### 31.Queue和Deque的区别

Queue 是单端队列，只能从⼀端插⼊元素，另⼀端删除元素，实现上⼀般遵循 **先进先出（****FIFO****）**

规则。

Queue 扩展了 Collection 的接⼝，根据 **因为容量问题⽽导致操作失败后处理⽅式的不同** 可以分为

两类⽅法: ⼀种在操作失败后会抛出异常，另⼀种则会返回特殊值。

![image-20240229203208838](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240229203208838.png)

Deque 是双端队列，在队列的两端均可以插⼊或删除元素。

Deque 扩展了 Queue 的接⼝, 增加了在队⾸和队尾进⾏插⼊和删除的⽅法，同样根据失败后处理

⽅式的不同分为两类：

![image-20240229203240850](C:\Users\Luckylemon\AppData\Roaming\Typora\typora-user-images\image-20240229203240850.png)