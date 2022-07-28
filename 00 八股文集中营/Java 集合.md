![[Pasted image 20220728210307.png]]
# Set

## TreeSet

基于红黑树实现，支持有序性操作，例如根据一个范围查找元素的操作。但是查找效率不如 HashSet，HashSet 查找的时间复杂度为 O(1)，TreeSet 则为 O(logN)。

## HashSet

基于哈希表实现，支持快速查找，但不支持有序性操作。并且失去了元素的插入顺序信息，也就是说使用 Iterator 遍历 HashSet 得到的结果是不确定的。

## LinkedHashSet

具有 HashSet 的查找效率，且内部使用双向链表维护元素的插入顺序。

# List

## ArrayList

基于动态数组实现，支持随机访问。

## Vector

和 ArrayList 类似，但它是线程安全的。

## LinkedList

基于双向链表实现，只能顺序访问，但是可以快速地在链表中间插入和删除元素。不仅如此，LinkedList 还可以用作栈、队列和双向队列。

# Queue

## LinkedList

可以用它来实现双向队列。

## PriorityQueue

基于堆结构实现，可以用它来实现优先队列。

# Map

## TreeMap

基于红黑树实现。

## HashMap

基于哈希表实现。

## HashTable

和 HashMap 类似，但它是线程安全的，这意味着同一时刻多个线程可以同时写入 HashTable 并且不会导致数据不一致。它是遗留类，不应该去使用它。现在可以使用 ConcurrentHashMap 来支持线程安全，并且 ConcurrentHashMap 的效率会更高，因为 ConcurrentHashMap 引入了分段锁。

## LinkedHashMap

使用双向链表来维护元素的顺序，顺序为插入顺序或者最近最少使用(LRU)顺序。

##  简述JAVA的List
List是一个有序队列，在JAVA中有两种实现方式:
ArrayList 使用数组实现，是容量可变的非线程安全列表，随机访问快，集合扩容时会创建更大的数组，
把原有数组复制到新数组。
LinkedList 本质是双向链表，与 ArrayList 相比插入和删除速度更快，但随机访问元素很慢。
## Java中线程安全的基本数据结构有哪些
HashTable: 哈希表的线程安全版，效率低
ConcurrentHashMap：哈希表的线程安全版，效率高，用于替代HashTable
Vector：线程安全版Arraylist
Stack：线程安全版栈
BlockingQueue及其子类：线程安全版队列
## 简述JAVA的Set
Set 即集合，该数据结构==不允许元素重复且无序==。JAVA对Set有三种实现方式：
HashSet 通过 HashMap 实现，HashMap 的 Key 即 HashSet 存储的元素，Value系统自定义一个名为
PRESENT 的 Object 类型常量。判断元素是否相同时，先比较hashCode，相同后再利用equals比较，
查询O(1)
LinkedHashSet 继承自 HashSet，通过 LinkedHashMap 实现，使用双向链表维护元素插入顺序。
TreeSet 通过 TreeMap 实现的，底层数据结构是红黑树，添加元素到集合时按照比较规则将其插入合适
的位置，保证插入后的集合仍然有序。查询O(logn)
## 简述JAVA的HashMap
JDK8 之前底层实现是数组 + 链表，JDK8 改为数组 + 链表/红黑树。主要成员变量包括存储数据的
table 数组、元素数量 size、加载因子 loadFactor。
HashMap 中数据以键值对的形式存在，键对应的 hash 值用来计算数组下标，如果两个元素 key 的
hash 值一样，就会发生哈希冲突，被放到同一个链表上。
table 数组记录 HashMap 的数据，每个下标对应一条链表，所有哈希冲突的数据都会被存放到同一条链
表，Node/Entry 节点包含四个成员变量：key、value、next 指针和 hash 值。在JDK8后链表超过8会转
化为红黑树。
若当前数据/总数据容量>负载因子，Hashmap将执行扩容操作。
默认初始化容量为 16，扩容容量必须是 2 的幂次方、最大容量为 1<< 30 、默认加载因子为 0.75。
## 为何HashMap线程不安全
在JDK1.7中，HashMap采用头插法插入元素，因此并发情况下会导致环形链表，产生死循环。
虽然JDK1.8采用了尾插法解决了这个问题，但是并发下的put操作也会使前一个key被后一个key覆盖。
由于HashMap有扩容机制存在，也存在A线程进行扩容后，B线程执行get方法出现失误的情况。
## 简述java的TreeMap
TreeMap是底层利用红黑树实现的Map结构，底层实现是一棵平衡的排序二叉树，由于红黑树的插入、
删除、遍历时间复杂度都为O(logN)，所以性能上低于哈希表。但是哈希表无法提供键值对的有序输
出，红黑树可以按照键的值的大小有序输出。
## Collection和Collections有什么区别？
1. Collection是一个集合接口，它提供了对集合对象进行基本操作的通用接口方法，所有集合都是它
的子类，比如List、Set等。
2. Collections是一个包装类，包含了很多静态方法、不能被实例化，而是作为工具类使用，比如提供
的排序方法：Collections.sort(list);提供的反转方法：Collections.reverse(list)。
## ArrayList、Vector和LinkedList有什么共同点与区别？
1. ArrayList、Vector和LinkedList都是可伸缩的数组，即可以动态改变长度的数组。
2. ArrayList和Vector都是基于存储元素的Object[] array来实现的，它们会在内存中开辟一块连续的空
间来存储，支持下标、索引访问。但在涉及插入元素时可能需要移动容器中的元素，插入效率较低。当存储元素超过容器的初始化容量大小，ArrayList与Vector均会进行扩容。
3. Vector是线程安全的，其大部分方法是直接或间接同步的。ArrayList不是线程安全的，其方法不具
有同步性质。LinkedList也不是线程安全的。
4. LinkedList采用双向列表实现，对数据索引需要从头开始遍历，因此随机访问效率较低，但在插入
元素的时候不需要对数据进行移动，插入效率较高。
## HashMap和Hashtable有什么区别？
1. HashMap是Hashtable的轻量级实现，HashMap允许key和value为null，但最多允许一条记录的key
为null.而HashTable不允许。
2. HashTable中的方法是线程安全的，而HashMap不是。在多线程访问HashMap需要提供额外的同步
机制。
3. Hashtable使用Enumeration进行遍历，HashMap使用Iterator进行遍历。
## 如何决定使用HashMap还是TreeMap?
如果对Map进行插入、删除或定位一个元素的操作更频繁，HashMap是更好的选择。如果需要对key集
合进行有序的遍历，TreeMap是更好的选择。
## fail-fast和fail-safe迭代器的区别是什么？
1. fail-fast直接在容器上进行，在遍历过程中，一旦发现容器中的数据被修改，就会立刻抛出ConcurrentModificationException异常从而导致遍历失败。常见的使用fail-fast方式的容器有HashMap和ArrayList等。
2. fail-safe这种遍历基于容器的一个克隆。因此对容器中的内容修改不影响遍历。常见的使用fail-safe
方式遍历的容器有ConcurrentHashMap和CopyOnWriteArrayList。
## HashSet中，equals与hashCode之间的关系？
equals和hashCode这两个方法都是从object类中继承过来的,equals主要用于判断对象的内存地址引用
是否是同一个地址；hashCode根据定义的哈希规则将对象的内存地址转换为一个哈希码。HashSet中
存储的元素是不能重复的，主要通过hashCode与equals两个方法来判断存储的对象是否相同：
1. 如果两个对象的hashCode值不同，说明两个对象不相同。
2. 如果两个对象的hashCode值相同，接着会调用对象的equals方法，如果equlas方法的返回结果为
true，那么说明两个对象相同，否则不相同。

## HashMap 与 ConcurrentHashMap 的实现原理是怎样的？ConcurrentHashMap 是如何保证线程安全的？


## 简述 ArrayList 与 LinkedList 的底层实现以及常见操作的时间复杂度