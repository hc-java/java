# 前言

1.

<https://www.bilibili.com/video/av74761810/?spm_id_from=333.788.videocard.14>

2.**多线程基础**：P194-P232  -----听不懂

<https://www.bilibili.com/video/av59529105?p=194>

3.

<https://www.bilibili.com/video/av11076511/?spm_id_from=333.788.videocard.1>

代码链接:
https://github.com/EduMoral/edu/tree/master/concurrent/src/yxxy











synchronized实例：

~~~java

public class T implements Runnable {

    private int count = 10;

    public synchronized void run() {
        count--;
        System.out.println(Thread.currentThread().getName() + " count = " + count);
    }

    public static void main(String[] args) {
        T t = new T();
        for(int i=0; i<5; i++) {
            new Thread(t, "THREAD" + i).start();
        }
    }

}
~~~

总结：加上synchronized，输出的count值不会重复。因为当第一个线程进去的时候，锁住了run()，其它线程进不去。当第一个进去的线程出来时，把锁解开，其它线程才能进去运行run()里面的方法。





多线程：高可用、高性能、高并发

尽量使用Runnable接口，避免使用Thread产生的单继承的局限性。

# 多线程

## Runnable

### 实例1

~~~java

public class T implements Runnable {

 
    public  void run() {
        for (int i=0;i<20;i++){
            System.out.println("一边听歌");
        }
    }

    public static void main(String[] args) {
        //创建实现类对象
        T t = new T();
        //创建代理类对象
        Thread t1 = new Thread(t);
        //启动
        t1.start();
        for (int i=0;i<20;i++){
            System.out.println("一边敲代码");
        }
    }

}
~~~

总结：每次运行，结果都不一样。原因是cpu的调度不同。



~~~java
T t = new T();
Thread t1 = new Thread(t,"hc");//可以在创建代理类对象时，自己附上线程名
Thread t2 = new Thread(t,"ch");//可以在创建代理类对象时，自己附上线程名

System.out.println(Thread.currentThread().getName());//打印出当前线程的名称

Thread.sleep(1000); //线程睡眠1000毫秒
~~~

### 静态代理

含义：已经写好的，可以直接拿来用。**在程序运行前就已经存在代理类的字节码文件**，**代理类和委托类的关系在运行前就确定了**。

使用：记录日志、增强服务

~~~java


public class Test{
    public static void main(String[] args) {
        My my = new My();
        MarryCompany user_my = new MarryCompany(my);//代理角色引用真实角色。
        user_my.marry();                            //最后结婚的是自己，但可以使用代理角色提供的服务。 a
    }
}

interface Marry{           //结婚功能的接口
    public void marry();
}

class My implements Marry{ //my是真实角色，要实现结婚接口 d

    @Override
    public void marry() {
        System.out.println("my 结婚");
    }
}

class MarryCompany implements Marry{  //婚庆公司是代理角色也要实现结婚接口
    private Marry user;               //代理角色对真实角色的引用，首先在代理角色中创建真实角色。  c
    public MarryCompany(Marry user){
        this.user = user;
    }

    private void deadWork_1(){       //代理角色提供的一些服务（方法）。
        System.out.println("准备工作1");
    }

    private void deadWork_2(){
        System.out.println("准备工作2");
    }

    @Override
    public void marry() {            //代理角色中实现接口的方法。
        deadWork_1();            //代理角色提供的方法。
        deadWork_2();
        user.marry();            //真实角色结婚 b
    }
}
~~~



优点：业务类只需要关注业务逻辑本身，保证了业务类的重用性。这是代理的共有优点。

静态代理模式的缺点：

1、假设一个系统中有100个Service，则需要创建100个代理对象

2、如果一个Service中有很多方法需要事务（增强动作），发现代理对象的方法中还是有很多重复的代码

3、由第一点和第二点可以得出：静态代理的重用性不强



总结：

不使用代理角色：user_my.start(); 

使用代理角色：user_my.marry(); 

### lamda

参考地址：<https://blog.csdn.net/xxtnt/article/details/83381083>

对于只使用一次的，可以使用内部类加快运行速率，它的特点是，只有使用时才会进行编译，且内部类的引用更快。

### 线程的状态

参考网址：<https://blog.csdn.net/tongxuexie/article/details/80145663>

创建状态（New）、就绪状态（Runnable）、运行状态（Running）、阻塞状态（Blocked）、死亡状态（Dead）

![1580910848474](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1580910848474.png)

### 线程的终止

~~~java

public class TerminateThread implements Runnable{

    //加入标识，线程是否可以运行	
    private boolean flag=true;
    
    @Override
    public void run() {
		//关联标识
        while(flag){//当flag为true，死循环输出下面语句
            System.out.println("study thread");
        }
    }
	//对外提供方法改变标识
    public void terminate(){
        this.flag=false;
    }

    public static void main(String[] args) {
        TerminateThread terminateThread=new TerminateThread();
        Thread thread=new Thread(terminateThread);
        thread.start();

        for (int i=0;i<999;i++){
            if (i == 888){
                terminateThread.terminate();//通过改变标识，使线程停止
                System.out.println("study stop");
            }
            System.out.println("study-->"+i);
        }
    }
}

~~~

总结：不建议使用stop()强制终止线程。

​		常用 外部方法，终止线程。

### sleep

~~~java
 		
		//写在线程的run()方法里
		try {            	
                Thread.sleep(1000); //要放在try catch 捕获异常中
            }catch (InterruptedException e){
                e.printStackTrace();
            }
~~~

因为sleep()是静态方法，所以最好的调用方法就是 Thread.sleep()。

sleep不释放锁，睡眠时间到了之后，重新进入就绪状态，而不是进入运行状态。

### yield

礼让线程：让当前正在执行的线程暂停（让出cpu的调度），直接进入就绪状态，不是阻塞状态（相当于大家一起重新竞争cpu）。

避免线程占用资源过久

~~~java
Thread.yield();//礼让线程。可能礼让成功、也可以礼让不成功
~~~

### join

合并线程（插队线程）：join方法其实就是阻塞当前调用它的线程，等待join执行完毕，当前线程继续执行。

join是成员方法：对象名.jion(毫秒)；

join()写在某个线程体中，某个线程就被阻塞了。必须等join()执行后，才能执行该线程后面的代码。

join方法是等待子线程全部执行完后，主线程才往下执行

### 观察状态

~~~java

Stage stage=t.getState();
System.out.println(stage);//输出当前线程的状态	
~~~



## 

### 优先级

优先级高的线程更容易被cpu调度运行。

设置优先级要在线程启动之前

等级：
MAX_PRIORITY:10
MIN_PRIORITY:1
NORM_PRIORITY:5

方法：
getPriority():返回线程优先级
setPriority(int newPriority):改变线程的优先级



~~~java
System.out.println(Thread.currentThread().getPriority());//输出当前线程的优先级，默认都是5
~~~



两段相同的代码，优先级高的运行的快。

### 守护线程

守护线程：是为了用户线程服务的，jvm停止不用等待守护线程执行完毕。

默认：用户线程jvm等待用户线程执行完毕才会停止	



线程对象名.setDaemon(true);//默认为false；当为true时，表示该线程为守护线程



Daemon的作用是为其他线程的运行提供便利服务，守护线程最典型的应用就是 GC (垃圾回收器)

在Daemon线程中产生的新线程也是Daemon的。



```java
System.out.println(Thread.currentThread().isAlive());//判断当前线程是否结束
```





### 线程同步

线程安全：在并发时保证数据的正确性、效率尽可能高

synchronized: 锁的是对象

- 同步方法

   - 同步块



要锁对 对象





## Runnable

### 可重入锁

也叫递归锁，表示一个被锁的函数(1)里面 调用另外一个被锁的函数(2)。那么这个另外一个被锁函数(2) 可以获得第一个被锁函数(1)的锁。



synchronized:

​	在执行过程中，如果出现异常，锁是会被释放的。

~~~java
public class T {
	int count = 0;
	synchronized void m() {
		System.out.println(Thread.currentThread().getName() + " start");
		while(true) {
			count ++;
			System.out.println(Thread.currentThread().getName() + " count = " + count);
			try {
				TimeUnit.SECONDS.sleep(1);
				
			} catch (InterruptedException e) {
				e.printStackTrace();
			}
			
			if(count == 5) {
				int i = 1/0; //此处抛出异常，锁将被释放，要想不被释放，可以在这里进行catch，然后让循环继续
				System.out.println(i);
			}
		}
	}
	
	public static void main(String[] args) {
		T t = new T();
		Runnable r = new Runnable() {

			@Override
			public void run() {
				t.m();
			}
			
		};
		new Thread(r, "t1").start();
		
		try {
			TimeUnit.SECONDS.sleep(3);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		
		new Thread(r, "t2").start();
	}
	
}

~~~

总结：如果t1没有释放锁，t2无法执行



## 脏数据

多线程对同一变量进行读写操作不同步产生。

例子：

有一个共享的变量，结果两个线程输出打印出来的变量一样，该现象就是我们通常称为的脏数据。

解决办法：

在访问变量方法中增加synchronized关键字



## **pipeStream**

<https://www.jianshu.com/p/1bd6d6b1b31a>

两个线程不借助文本，使用管道【**输出管道-写数据，输入管道-读数据**】方式(单向通信)直接进行数据传输。

 管道通讯两种方式：
  1)`PipedInputStream`和`PipedOutputStream`
  2)`PipedReader`和`PipedWriter`
  管道通讯使用注意点：
  1)管道通讯必须建立连接。src.connect(snk)/snk.connect(src)【src:未连接的管道输出流，snk：未链接的管道输入流】。
  2)PipedInputStream和PipedOutputStream不能在同一个线程使用。【会出现死锁现象】

实例：<https://www.cnblogs.com/wangbin2188/p/9293173.html>





## wait、notify、notifyAll

这三个都是属于Object基础类

wait()导致当前的线程等待（会释放自己的锁），直到其他线程调用此对象的notify( ) 方法或 notifyAll( ) 方法

notify( )方法只会通知等待队列中的第一个相关线程，不会让出锁

notifyAll( )通知所有等待该竞争资源的线程





# java多线程编程核心技术

## 第一章

### 多个线程访问一个变量

~~~java

public class ShareData {
    public static void main(String[] args) {

        MyThread myThread = new MyThread();
        Thread thread1 = new Thread(myThread,"A");
        Thread thread2 = new Thread(myThread,"B");
        Thread thread3 = new Thread(myThread,"C");
        Thread thread4 = new Thread(myThread,"D");
        Thread thread5 = new Thread(myThread,"E");
        thread1.start();
        thread2.start();
        thread3.start();
        thread4.start();
        thread5.start();
    }
    public static class MyThread extends Thread{
        private int count=5;
        
         public MyThread() {
            //System.out.println("这是构造函数，构造函数是由main函数调用的"+this.currentThread().getName());
        }

        @Override
        synchronized public void run() {
            super.run();
            count--;
            //这里输出的线程名称是 A/B/....
            System.out.println("由"+this.currentThread().getName() +"计算count="+count);
        }
    }
}
//未加synchronized，出现 “非线程安全”
/*由A计算count=4
由C计算count=2
由B计算count=2
由D计算count=1
由E计算count=0*/

//加了synchronized，可以让这些线程将变量按顺序输出
/*由A计算count=4
由C计算count=3
由B计算count=2
由E计算count=1
由D计算count=0*/
~~~

### 非线程安全

非线程安全：主要指多个线程多同一个对象中的同一个实例变量进行操作时出现值被更改、值不同步的情况，进而影响程序的执行流程。



### isAlive()

~~~java
System.out.println("begin---"+myThread.isAlive());
~~~

判断当前叫做myThread的线程是否存活

### getId()

~~~java
System.out.println("begin2---"+myThread.getId());
~~~

取得线程的唯一标识



### 停止线程

有3种方法可以终止正在运行的线程：

- run()运行完，程序正常退出，线程终止。
- stop()强行终止线程，已被弃用
- interrupt() 中断线程

### interrupt()

调用interrupt()方法仅仅是在当前线程中打了一个停止的标记，并不是真的停止线程。

判断线程是否停止：

- this.interrupted(): 测试当前线程是否已经是中断状态，执行后将 **状态标记** 清除为false
- this.isInterrupted():测试线程Thread对象是否已经是中断状态，但不清除 **状态标记**



interrupt 与wait或者join遇到，会报错

#### 异常法



~~~java
public class Interrupt {
    public static void main(String[] args) {

        try {
            MyThread myThread = new MyThread();
            Thread thread = new Thread(myThread);
            thread.start();

            Thread.sleep(1000);
            thread.interrupt();
        } catch (InterruptedException e) {
            System.out.println("main catch");
            e.printStackTrace();
        }


    }
    public static class MyThread extends Thread{

        @Override
        public void run() {
            for (int i = 0; i < 100000; i++) {
                if (this.interrupted()){
                    System.out.println("停止。。。interrupted退出");
                    break;
                }
                System.out.println("i="+i);
            }
            
            System.out.println("上面的for停止，但是下面的代码还可以继续执行。");
        }

    }
}

~~~



修改后

~~~java

    public static class MyThread extends Thread{


        @Override
        public void run() {
            try {
                for (int i = 0; i < 500000; i++) {
                    if (this.interrupted()){
                        System.out.println("停止。。。interrupted退出");
                        throw new InterruptedException();
                    }
                    System.out.println("i="+i);
                }
                System.out.println("上面的for停止，下面的代码因为抛出异常 无法执行");

            }catch (InterruptedException e) {
                e.printStackTrace();
            }


        }

    }


~~~

#### 在沉睡中停止



#### 使用return停止线程

~~~java
  public void run() {
           
                for (int i = 0; i < 500000; i++) {
                    if (this.interrupted()){
                        System.out.println("停止。。。interrupted退出");
                        return ；
                    }
                    System.out.println("i="+i);
                }
                System.out.println("上面的for停止，下面的代码因为return 无法执行");           

        }

~~~

### 暂停线程

#### suspend()

使线程暂停

#### resume()

使线程恢复执行

#### 缺点

- 独占：使用不当，极易造成公共的同步对象的独占，使其它线程无法访问公共同步对象
- 不同步： 容易出现线程的暂停而造成数据不同步的情况

## 第二章

多个对象多个锁

出现异常，锁自动释放

同步不具有继承性

不在synchronized块中就是异步执行，在synchronized块中就是同步执行。

synchronized 锁的是当前对象

synchronized  加在static上，是对class加锁；加在非static上，是对 对象加锁。



同步synchronized代码块都不使用String作为锁对象，因为String常量池会带来问题





### synchronized 方法的弊端

两个多线程，一个运行时间很久的线程占用锁太久。

关键字synchronized可以使多个线程访问同一个资源具有同步性，而且他还具有将线程工作内存中私有变量与公共内存中的变量同步的功能





### synchronized(非this对象)



### 多线程死锁

监测死锁：

cmd显示控制台

进入jdk安装文件的bin目录下

执行jps，显示正在运行的线程的id

jstack -l 线程ID   ---查看结果：是否有死锁



### volatile

作用：使变量在多个线程间可见

强制从公共堆栈中取得变量的值，而不是从线程私有数据栈中取得变量的值。

volatile缺点：

不支持原子性

## 第三章

### wait、notify

调用wait()方法，是该线程进入等待状态，并释放锁

notify()方法，唤醒一个因wait操作而处于阻塞状态的线程，使其成为就绪状态

被从新唤醒的线程会试图重新获得锁，并执行wait之后的代码



### wait(long)

等待某一段时间内，是否有线程对锁进行唤醒，如果超出这个时间 则自动唤醒。



### join(long)

内部使用wait(long)来实现的



假死：所有线程进入waiting状态

### 管道通信

- 字节流 PipedInputStream   PipedOutputStream
- 字符流 PipedReader    PipedWriter



字节流与字符流的区别：

字节流：读取单个字节，用来处理二进制文件，是给机器看的

字符流：读取单个字符，用来处理文本文件，是给人看的

### 类ThreadLocal的使用

使每个线程绑定自己的值。

可以将ThreadLocal类比喻成 全局存放数据的盒子，盒子中可以存储每个线程的私有数据