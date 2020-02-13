

java路线：<http://c.biancheng.net/java/>





# [Java面试宝典之----java基础(含答案)]

<https://www.cnblogs.com/talenter/p/9652976.html>

<https://blog.csdn.net/hao19980724/article/details/83792516>



<https://www.cnblogs.com/arsense/p/10156279.html>

<https://www.cnblogs.com/peke/p/7894685.html>

递归：

​		递归是重复调用函数自身实现循环

​		不断地深层调用函数，直到函数有返回才会逐层的返回

​		归涉及到运行时的堆栈开销

​		将问题交给计算机，以及将大问题分解为相同小问题从而解决大问题的动机

迭代：

​	迭代是函数内某段代码实现循环

​	**不断用变量的旧值递推新值的过程**

​	大部分时候需要人为的对问题进行剖析

​	迭代的效率高，但却不太容易理解



HashMap 

  	类没有分类或者排序。它允许一个 null 键和多个 null 值。

Hashtable 

​	 类似于 HashMap，但是不允许 null 键和 null 值。它也比 HashMap 慢，因为它是同步的。



## **sleep()**

**使线程停止一段时间的方法**，sleep方法的线程不会释放对象锁

在sleep 时间间隔期满后，线程不一定立即恢复执行。



wait()

wait()方法的线程会释放对象锁

**该线程会暂停执行，被调对象进入等待状态，直到被唤醒或等待时间到。** 

是Object类的方法，当一个线程执行到wait方法时，它就进入到一个和该对象相关的等待池，同时释放对象的机锁，使得其他线程能够访问，可以通过notify，notifyAll方法来唤醒等待的线程



接口、抽象类：

- 从使用上来看，一个类可以实现多个接口，但是不能继承多个抽象类。

- 接口的字段只能是 static 和 final 类型的，而抽象类的字段没有这种限制。

- 接口的成员只能是 public 的，而抽象类的成员可以有多种访问权限。

  

异常
Throwable 可以用来表示任何可以作为异常抛出的类，分为两种： Error 和 Exception。其中 Error 用来表示 JVM 无法处理的错误，Exception 分为两种：

受检异常 ：需要用 try...catch... 语句捕获并进行处理，并且可以从异常中恢复；
非受检异常 ：是程序运行时错误，例如除 0 会引发 Arithmetic Exception，此时程序崩溃并且无法恢复。



Error（错误）表示系统级的错误和程序不必处理的异常，是java运行环境中的内部错误或者硬件问题。比如：内存资源不足等。对于这种错误，程序基本无能为力，除了退出运行外别无选择，它是由Java虚拟机抛出的。
    Exception（违例）表示需要捕捉或者需要程序进行处理的异常，它处理的是因为程序设计的瑕疵而引起的问题或者在外的输入等引起的一般性问题，是程序必须处理的。
Exception又分为运行时异常，受检查异常。
       RuntimeException(运行时异常)，表示无法让程序恢复的异常，导致的原因通常是因为执行了错误的操作，建议终止程序，因此，编译器不检查这些异常。
       CheckedException(受检查异常)，是表示程序可以处理的异常，也即表示程序可以修复（由程序自己接受异常并且做出处理）， 所以称之为受检查异常。



length  数组的长度

length()   字符串的长度

size()  集合的元素个数



Arraylist：是基于动态数组的集合

随机访问数组元素的效率高，向数组尾部添加元素的效率高

删除数组中的数据以及向数组中间添加数据效率低

对于随机访问get和set，ArrayList优于LinkedList，因为LinkedList要移动指针。 



Linkedlist

基于链表的集合，数据添加删除效率高

访问数据的平均效率低，需要对链表进行遍历

对于新增和删除操作add和remove，LinedList比较占优势，因为ArrayList要移动数据。



 Hashtable继承自Dictionary类，而HashMap继承自AbstractMap类。但二者都实现了Map接口。

Hashtable  同步      key和value都不允许出现null值		在不指定容量的情况下的默认容量为11

HashMap  不同步   null可以作为键，这样的键只有一个；可以有一个或多个键所对应的值为null		在不指定容量的情况下的默认容量为16





 21.ArrayList:底层是数组结构，查询速度快，增加/删除速度较慢，是线程不安全的

　  Vector和ArrayList一样都实现了List接口，但是线程安全

　　LinkedList：底层是链表结构，查询速度慢，增加/删除速度快，是线程不安全的

22.HashMap：线程不安全，key和value可以是null值

　  HashTable：线程安全，key和value都不可以是null值，和HashMap一样都是Map的实现类

23.List：是存储单列数据的集合，存储的数据是有序并且是可以重复的 

　 Map：存储双列数据的集合，通过键值对存储数据，存储 的数据是无序的，Key值不能重复，value值可以重复

24.List/Set是、Map不是



HashSet是由一个hash表来实现的，因此，它的元素是无序的。add()，remove()，contains()方法的时间复杂度是O(1)。

另一方面，TreeSet是由一个树形的结构来实现的，它里面的元素是有序的。因此，add()，remove()，contains()方法的时间复杂度是O(logn)。





NullPointerException ：空指针异常 （出现条件：使用对象（字段/方法）值为null时）
       ArrayIndexOutOfBoundsException ：数组下标越界异常 （出现条件：使用超出数组下标范围的下标）
       NumberFormatException ：数字格式化异常  （出现条件：不符合转换格式的字符串被转换成数字时）
       ParseException : 解析异常 （出现条件：需要转换成Date的字符串内容，不符合SimpleDateFormat对象指定的格式）
       ClassCastException ：类型转换异常  （出现条件：将一个类型转换成另一个类型，两个类型没有继承关系）                   

  ArithmeticException : 数学运算异常 (出现条件： ex：1/0)

## **21.List、Set和Map的区别？**

List:是存储单列数据的集合，存储有顺序，允许重复。继承Collection接口。

Set: 是存储单列数据的集合。继承Collection接口。不允许重复。

Map:存储键和值这样的双列数据的集合，存储数据无顺序，键(key)不能重复，值(value)。可以重复。



# Sql常用查询操作

<https://blog.csdn.net/bruce_up/article/details/82750024>



SQL创建索引的目的如下：

1、通过唯一性索引（unique）可确保数据的唯一性；

2、加快数据的检索速度；

3、加快表之间的连接；

4、减少分组和排序时间；

5、使用优化隐藏器提高系统性能。