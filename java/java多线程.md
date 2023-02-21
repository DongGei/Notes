JavaThread

## 概述



Thread，英文单词，名词、动词，作名词时意思是“线；螺纹；思路；衣服；线状物；玻璃纤维；路线”，java多线程。

进程是系统运行程序的基本单位，线程是比进程更小、更轻量级的执行单位，每个进程都拥有自己的一块内存空间和变量资源等，然而同一个进程下的多个线程则共享数据和资源，所以不管线程的创建和销毁工作，还是在线程之间切换工作，都要比进程更加轻量级、消耗系统资源更少。

多线程在多个cpu的机器上可以实现。如果只是单cpu，只是让我们看起来是多线程的。实际上运行的是靠cup的调度。而不是同时去执行的。

在jvm中 每一个线程都有自己的java虚拟机栈，本地方法栈，程序计数器。而对应的所有线程共享堆内存和元空间（以前版本也叫方法区）。与上一段文字进程和线程的关系对应。

## 创建多线程

+ 声明一个Thread类的子类，子类中重写Thread类的run方法。Thread也实现了runnable接口

+ 声明一个实现Runnable接口的类，类中实现run方法。

+ 声明一个实现Callable接口的类（在初级阶段不需要太了解）

  > 推荐使用runnable接口
  >
  >  我认为最主要的原因是 使用runnable接口 方便的对同一个资源进行操作。**Runnable可以实现多个相同的程序代码的线程去共享同一个资源 Thread类也可以，只是不适合（相当于没做）。当以Thread方式去实现资源共享时，实际上源码内部是将thread向下转型为了Runnable，实际上内部依然是以Runnable形式去实现的资源共享** [参考](https://www.jianshu.com/p/333ce4b3d5b8)
  >
  > 1. Thread oop(面向对象)的单继承局限性，不建议使用。
  >
  >    Runnable接口，避免单继承局限性，灵活方便，方便同一个对象被多个线程使用。
  >
  > 2. 该类继承了Thread类，就不能继承其他类了，不满足实际的需求
  >
  > 3. 法一把线程和任务合并在了一起，法二分开了
  >
  > 4. Runnable更容易与线程池等高级API配合，建议操作任务对象，不建议直接操作线程对象。
  >
  > 5. Runnable让任务脱离了Thread继承体系，更灵活。Java中，组合优于继承，Runnable和Thread的关系是聚合关系(has-a)，而法一是继承关系（is-a）。
  >    tip：聚合关系就是A类的属性中有B类

重写run方法 调用start方法启动线程，但不是立即执行的。等待cpu调度。

```java
public class ThreadTest1 extends Thread {
    public static void main(String[] args) {
        ThreadTest1 t = new ThreadTest1();
        t.start();
    }

    @Override
    public void run() {
        System.out.println("线程run");
    }
}
```

```java
public class ThreadTest2 implements Runnable{
    public static void main(String[] args) {
        //Runable是函数式接口，可以用Lambda表达式
        //实现类 放到了Thread中
        Thread thread = new Thread(new ThreadTest2());
        thread.start();
    }

    @Override
    public void run() {
        System.out.println("线程run");
    }
}
```

 Callable的好处：

1. 可以定义返回值
2. 可以抛出一个异常

```java
public class CallableTest implements Callable<Boolean> {

    @Override
    public Boolean call() throws Exception {
        System.out.println("call方法正在执行");
        //写死了
        return true;
    }

    public static void main(String[] args) throws ExecutionException, InterruptedException {
        CallableTest c = new CallableTest();
        //创建执行服务 创建了一个池子
        ExecutorService executorService = Executors.newFixedThreadPool(1);
        //提交执行
        Future<Boolean> result = executorService.submit(c);
        //获取结果
        Boolean aBoolean = result.get();
        System.out.println(aBoolean);
        //关闭服务
        executorService.shutdown();


    }
}
```

## 静态代理

![image-20220924201259595](http://bijioss.donggei.top/image-20220924201259595.png)

![image-20220924201126111](http://bijioss.donggei.top/image-20220924201126111.png)

![image-20220924210105903](http://bijioss.donggei.top/image-20220924210105903.png)

代理类和目标类实现同一个接口,然后在代理类里面注入目标类,在代理类重写的方法里面通过注入的目标类调用目标类的同名方法,在这个方法执行前后添加一些逻辑处理。

目标类只需要专注于自己的事情就可以了

```java
Thread thread = new Thread(() -> System.out.println("线程run"));
thread.start();//线程run
```

说了这么多， Thread对Runnable target 就是进行了一种静态代理 

## 线程状态和常用方法

![http://static.cyblogs.com/20181120173640764.jpg](http://bijioss.donggei.top/20181120173640764.jpg)

源码：

```java
public enum State {
NEW,
RUNNABLE,
BLOCKED,
WAITING,
TIMED_WAITING,
TERMINATED;
}
```

[参考地址](https://cloud.tencent.com/developer/article/1897194#:~:text=Java%E4%B8%AD%E7%BA%BF%E7%A8%8B%E7%9A%84%E7%8A%B6%E6%80%81%E5%88%86%E4%B8%BA6%E7%A7%8D%E3%80%82%20%E5%88%9D%E5%A7%8B%20%28NEW%29%EF%BC%9A%E6%96%B0%E5%88%9B%E5%BB%BA%E4%BA%86%E4%B8%80%E4%B8%AA%E7%BA%BF%E7%A8%8B%E5%AF%B9%E8%B1%A1%EF%BC%8C%E4%BD%86%E8%BF%98%E6%B2%A1%E6%9C%89%E8%B0%83%E7%94%A8start%20%28%29%E6%96%B9%E6%B3%95%E3%80%82,%E8%BF%90%E8%A1%8C%20%28RUNNABLE%29%EF%BC%9AJava%E7%BA%BF%E7%A8%8B%E4%B8%AD%E5%B0%86%E5%B0%B1%E7%BB%AA%EF%BC%88ready%EF%BC%89%E5%92%8C%E8%BF%90%E8%A1%8C%E4%B8%AD%EF%BC%88running%EF%BC%89%E4%B8%A4%E7%A7%8D%E7%8A%B6%E6%80%81%E7%AC%BC%E7%BB%9F%E7%9A%84%E7%A7%B0%E4%B8%BA%E2%80%9C%E8%BF%90%E8%A1%8C%E2%80%9D%E3%80%82%20%E7%BA%BF%E7%A8%8B%E5%AF%B9%E8%B1%A1%E5%88%9B%E5%BB%BA%E5%90%8E%EF%BC%8C%E5%85%B6%E4%BB%96%E7%BA%BF%E7%A8%8B%20%28%E6%AF%94%E5%A6%82main%E7%BA%BF%E7%A8%8B%EF%BC%89%E8%B0%83%E7%94%A8%E4%BA%86%E8%AF%A5%E5%AF%B9%E8%B1%A1%E7%9A%84start%20%28%29%E6%96%B9%E6%B3%95%E3%80%82)

[参考地址](https://blog.csdn.net/pange1991/article/details/53860651)

**几个方法的比较**

1、Thread.sleep(long millis)，线程休眠。一定是当前线程调用此方法，当前线程进入TIMED_WAITING状态，但==不释放对象锁==，millis后线程自动苏醒进入就绪状态。作用：给其它线程执行机会的最佳方式。

2、Thread.yield()，线程礼让。一定是当前线程调用此方法，当前线程放弃获取的CPU时间片，但==不释放锁资源==，由运行状态变为就绪状态，让OS再次选择线程。作用：让相同优先级的线程轮流执行，但并不保证一定会轮流执行。==实际中无法保证yield()达到让步目的，因为让步的线程还有可能被线程调度程序再次选中==。Thread.yield()不会导致阻塞。该方法与sleep()类似，只是不能由用户指定暂停多长时间，也不一定会让其他线程先跑，看cpu调度的心情。就是回去重新竞争。 也可能是礼让没有成功但是时间片用完了

3、thread.join()/thread.join(long millis)， 作用就是让其他线程变为等待，直到调用join()方法的线程执行完毕。==当前线程里调用其它线程t的wait()方法==，当前线程进入WAITING/TIMED_WAITING状态，当前线程t不会释放已经持有的对象锁。线程t执行完毕或者millis时间到，当前线程t一般情况下进入RUNNABLE状态，也有可能进入BLOCKED状态（因为join是基于wait实现的）。

4、obj.wait()，当前线程调用对象 的wait()方法，==当前线程释放对象锁==进入等待队列。依靠notify()/notifyAll()唤醒或者wait(long timeout) timeout时间到自动唤醒。

5、obj.notify()唤醒在此对象监视器上等待的单个线程，选择是任意性的。notifyAll()唤醒在此对象监视器上等待的所有线程。

6、LockSupport.park()/LockSupport.parkNanos(long nanos),LockSupport.parkUntil(long deadlines), 当前线程进入WAITING/TIMED_WAITING状态。对比wait方法,不需要获得锁就可以让线程进入WAITING/TIMED_WAITING状态，需要通过LockSupport.unpark(Thread thread)唤醒。

- 等待队列里许许多多的线程都wait()在一个对象上，此时某一线程调用了对象的notify()方法，那唤醒的到底是哪个线程？随机？队列FIFO？or sth else？Java文档就简单的写了句：选择是任意性的（The choice is arbitrary and occurs at the discretion of the implementation）。

![image-20220924221357148](http://bijioss.donggei.top/image-20220924221357148.png)

```java
public class StateTest {
    //查看线程的状态
    public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println("ooooo"+i);
            }
        });
        Thread.State state = thread.getState();
        System.out.println(state); //new

        thread.start();

        state = thread.getState();
        System.out.println(state);//run

        //只要线程没终止 就输出它的状态
        while (state!=Thread.State.TERMINATED){
            Thread.sleep(100);
            state = thread.getState();
            System.out.println(state);
        }

    }

}
```

## 线程的优先级

thread.setPriority() 方法 从低到高 1到10

先设置优先级再启动

所有线程默认优先级是5，我测试守护线程的默认优先级也是5

优先级并不是一定的，也要看cpu具体的调度 

## 守护线程

线程分为**用户线程**和**守护线程**
虚拟机必须确保用户线程执行完毕。当然主线程也是用户线程
虚拟机不用等待守护线程执行完毕如，后台记录操作日志，监控内存垃圾回收等待..
当进程中不存在非守护线程了,则守护线程自动销毁

例：人生不过三万天，且行且珍惜。

```java
public class TestDaemon {
    public static void main(String[] args) {
        You you = new You();
        God god = new God();

        Thread godThread = new Thread(god);
        godThread.setDaemon(true); //默认是false
        godThread.start();

        Thread youThread = new Thread(you);
        youThread.start();
    }

}
class God implements Runnable{

    @Override
    public void run() {
        while (true){
            System.out.println("上帝保佑你");
        }
    }
}
class You implements Runnable{

    @Override
    public void run() {
        for (int i = 0; i < 36500; i++) {
            System.out.println("你开心的第"+i+"天");
        }
        System.out.println("====goodbye! world!===");
    }
}
```

## 线程同步

同一个资源,多个人都想使用。属于并发	并发指的是多个任务交替进行，而并行则是指真正意义上的“同时进行”。并发程序之间有相互制约的关系。直接制约体现为一个程序需要另一个程	序的计算结果；间接体现为多个程序竞争共享资源，如处理器、缓冲区等。

处理多线程问题时,多个线程访问同一个对象,并且某些线程还想修改这个对象.这时候我们就需要线程同步.线程同步其实就是一种等待机制,多个需要同时访问此对象的线程进入这个对象的**等待池形成队列**，等待前面线程使用完毕，下一个线程再使用



为了保证数据在方法中被访问时的正确性,在访问时加入锁机制
synchronized，当一个线程获得对象的排它锁,独占资源,其他线程必须等待，使用后释放锁即可.存在以下问题:

+ 一个线程持有锁会导致其他所有需要此锁的线程挂起;
+ 在多线程竞争下,加锁,释放锁会导致比较多的上下文切换和调度延时，引起性能问题;
+ 如果一个优先级高的线程等待一个优先级低的线程释放锁会导致优先级倒置,引起性能问题.

方法里面需要修改的内容才需要锁，
锁的太多，浪费资源

[参考](https://www.cnblogs.com/pinghuimolu/p/15113413.html#41-%E7%BA%BF%E7%A8%8B%E4%B8%8D%E5%AE%89%E5%85%A8%E4%B8%BE%E4%BE%8B)

## 常见问题

> Runnable为什么不可以直接run
>
> start（）方法来启动线程，真正实现了多线程运行。这时无需等待run方法体代码执行完毕，可以直接继续执行下面的代码；通过调用Thread类的start()方法来启动一个线程， 这时此线程是处于就绪状态， 并没有运行。 然后通过此Thread类调用方法run()来完成其运行操作的， 这里方法run()称为线程体，它包含了要执行的这个线程的内容， Run方法运行结束， 此线程终止。然后CPU再调度其它线程
>
> 2.run（）方法当作普通方法的方式调用。程序还是要顺序执行，要等待run方法体执行完毕后，才可继续执行下面的代码； 程序中只有主线程——这一个线程， 其程序执行路径还是只有一条， 这样就没有达到写线程的目的  相当于没用多线程，在jvm中还是同一个方法栈

# JUC

JUC指的是：Java里的一个包
 java.util.concurrent 并发
 java.util.concurrent.atomic：原子性
 java.util.concurrent.locks：lock锁

![image-20220925163032504](http://bijioss.donggei.top/image-20220925163032504.png)

业务:普通的线程代码Thread

**Runnable**没有返回值、效率相比**Callable**   较低!

所以企业中一般更多使用**Callable** 去执行一些任务

## 1.概述

> 进程和线程
>
> **进程是操作系统分配资源的最小单元，而线程是cpu调度的最小单元。**
>
> 程序执行的一次过程，一个进程包含一个或多个线程。进程是资源分配的单位。
>
> 可以指程序执行过程中，负责实现某个功能的单位。线程是CPU调度和执行的单位。
>
> 一个进程至少包含一个线程
>
> java默认有两个线程：主线程和gc守护线程



>  java 真的可以开启线程吗？
>
> 不能！！！
>
> 在Thread.start中调用了 本地方法，底层的C++。Java无法直接操作硬件。
>
> 从源码来看， 一个线程如果已经调用过 start 方法，那么再次调用 start 方法的时候会抛出 IllegalThreadStateException 异常， 涉及的技术点为线程的状态切换。
>
> ```java
> public synchronized void start() {
>     /**
>      * This method is not invoked for the main method thread or "system"
>      * group threads created/set up by the VM. Any new functionality added
>      * to this method in the future may have to also be added to the VM.
>      *
>      * A zero status value corresponds to state "NEW".
>      */
>     if (threadStatus != 0)
>         throw new IllegalThreadStateException();
> 
>     /* Notify the group that this thread is about to be started
>      * so that it can be added to the group's list of threads
>      * and the group's unstarted count can be decremented. */
>     group.add(this);
> 
>     boolean started = false;
>     try {
>         start0();
>         started = true;
>     } finally {
>         try {
>             if (!started) {
>                 group.threadStartFailed(this);
>             }
>         } catch (Throwable ignore) {
>             /* do nothing. If start0 threw a Throwable then
>               it will be passed up the call stack */
>         }
>     }
> }
> 
> private native void start0();
> ```



> 并发和并行  
>
> 并发：同一时刻，多个线程交替执行。（一个CPU交替执行线程）
>
>  并行：同一时刻，多个线程同时执行。（多个CPU同时执行多个线程）

```java
public class TestProcessors {
    public static void main(String[] args) {
        //获取cpu核数
        //cpu密集型 io密集型
        System.out.println(Runtime.getRuntime().availableProcessors());
    }
}
多核 CPU 内部集成了多个计算核心（Core），每个核心相当于一个简单的 CPU，多核 CPU 的每个核心都可以独立地执行一个任务，而且多个核心之间不会相互干扰。在不同核心上执行的多个任务，是真正地同时运行，这种状态就叫做并行。
    多核心技术是将多个一样的CPU放置于一个封装内（或直接将两个CPU做成一个芯片），而英特尔的HT技术（超线程技术）是在CPU内部仅复制必要的资源、让一个核模拟成两个线程；也就是一个实体核心，两个逻辑线程，在一单位时间内处理两个线程的工作，模拟实体双核心、双线程运作。
    多核CPU，可以并行执行多进程、多线程
```

所以说 并发编程可以充分利用cpu的资源

### wait/sleep的区别
Thread.sleep(long millis)，线程休眠。一定是当前线程调用此方法，当前线程进入TIMED_WAITING状态，但不释放对象锁，millis后线程自动苏醒进入就绪状态。作用：给其它线程执行机会的最佳方式。
obj.wait()，当前线程调用对象的wait()方法，当前线程释放对象锁进入等待队列。依靠notify()/notifyAll()唤醒或者wait(long timeout) timeout时间到自动唤醒。

1、来自不同的类

wait => Object  

sleep => Thread 

一般情况企业中使用休眠是：

    import java.util.concurrent.TimeUnit; 
    TimeUnit.DAYS.sleep(1); //休眠1天
    TimeUnit.SECONDS.sleep(1); //休眠1s

2、关于锁的释放

wait：会释放锁；

sleep：睡觉了，不会释放锁；

3、使用的范围是不同的

wait 必须在同步代码块（synchronized）中；如果不放在同步代码块或方法中，运行时会报错

sleep 可以在任何地方睡；

4.

Wait 通常被用于线程间交互/通信，sleep 通常被用于暂停执行。

5、是否需要捕获异常

wait不需要捕获异常；(目前也需要了)

sleep必须要捕获异常；

## 2.传统的synchronized

在开发中，我们在javaThread中的new Thread() 放一个rannable 已经不用了。

我们应该把线程看作一个单独的资源类，没有任何的附属操作！

```java
// 资源类 OOP 属性、方法
class Ticket {
    //属性
  private int number = 30;

  //卖票的方式
  public synchronized void sale() {
    if (number > 0) {
      System.out.println(Thread.currentThread().getName() + "卖出了第" + (number--) + "张票剩余" + number + "张票");
    }
  } 
}
```

```java
 public static void main(String[] args) {
     //并发 多线程操作同一个资源类
    final Ticket ticket = new Ticket();
	//把资源类丢入线程
    new Thread(()->{
      for (int i = 0; i < 40; i++) {
        ticket.sale();
      }
    },"A").start();
    new Thread(()->{
      for (int i = 0; i < 40; i++) {
        ticket.sale();
      }
    },"B").start();
    new Thread(()->{
      for (int i = 0; i < 40; i++) {
        ticket.sale();
      }
    },"C").start();
  }
}
```

synchronized **方法**控制 “对象” 的访问，每个对象对应一把锁，每个 synchronized 方法都必须获得调用该方法的对象的锁才能执行，否则线程会阻塞，方法一旦执行，就独占该锁，直到该方法返回才释放，后面被阻塞的线程才能获得这个锁，继续执行。

synchronized的方法加上static变成静态方法 那么锁的就是这个类文件

缺陷：若将一个大的方法申明为 synchronized 将会影响效率。

## 3.LOCK锁

![image-20220926084240585](http://bijioss.donggei.top/image-20220926084240585.png)

![image-20220926082243785](http://bijioss.donggei.top/image-20220926082243785.png)

 new ReentrantLock

无参构造是非公平锁，传入参数，如果是true 是公平锁。

源码：

```java

    /**
     * Creates an instance of {@code ReentrantLock}.
     * This is equivalent to using {@code ReentrantLock(false)}.
     */
    public ReentrantLock() {
        sync = new NonfairSync();
    }

    /**
     * Creates an instance of {@code ReentrantLock} with the
     * given fairness policy.
     *
     * @param fair {@code true} if this lock should use a fair ordering policy
     */
    public ReentrantLock(boolean fair) {
        sync = fair ? new FairSync() : new NonfairSync();
    }
```

####  公平锁和非公平锁

公平锁是指**多个线程按照申请锁的顺序来获取锁**。
优点：等待锁的线程不会饿死。
缺点：整体吞吐效率相对非公平锁要低，等待队列中除第一个线程以外的所有线程都会阻塞，CPU唤醒阻塞线程的开销比非公平锁大。
作用：严格按照线程启动的顺序来执行的，不允许其他线程插队执行

非公平锁是指多个线程获取锁的顺序并不是按照申请锁的顺序，有可能后申请的线程比先申请的线程优先获取锁。有可能，会造成优先级反转或者饥饿现象。非公平锁是按照cup调度的
优点：可以减少唤起线程的开销，吞吐量比公平锁大。，因为线程有几率不阻塞直接获得锁，CPU不必唤醒所有线程。
缺点：处于等待队列中的线程可能会饿死，或者等很久才会获得锁。
作用：线程启动允许插队的。

默认是非公平锁，是为了公平，比如一个线程要3s，另一个线程要3h,难道一定要让3h的锁先来就先执行吗

对于Synchronized而言，也是一种非公平锁。由于其并不像ReentrantLock是通过AQS的来实现线程调度，所以并没有任何办法使其变成公平锁。

使用方法：

+ new
+ 加锁
+ 业务代码
+ 解锁

```java
{
    private int number = 30;


    Lock lock = new ReentrantLock();


    //卖票的方式
    public void sale() {
        //加锁
        lock.lock();
        try {
            //业务代码
            if (number > 0) {
                System.out.println(Thread.currentThread().getName() + "卖出了第" + (number--) + "张票剩余" + number + "张票");
            }
        } catch (Exception e) {
            e.printStackTrace();
        }finally{
            //解锁
            lock.unlock();
        }

    }
}
```

### Synchronized 与Lock 的区别 

手动挡和自动挡的区别

1、Synchronized 内置的Java关键字，Lock是一个Java类

2、Synchronized 无法判断获取锁的状态，Lock可以判断

3、Synchronized 会自动释放锁，lock必须要手动加锁和手动释放锁！可能会遇到死锁

4、Synchronized 线程1(获得锁->阻塞)、线程2(等待)；lock就不一定会一直等待下去，lock会有一个trylock去尝试获取锁，不会造成长久的等待。

假设线程 A 获得锁，B 线程等待。如果 A 发生阻塞，那么 B 会一直等待。在 Lock 中，会分情况而定，Lock 中有尝试获取锁的方法，如果尝试获取到锁，则不用一直等待

5、Synchronized 是可重入锁，不可以中断的，非公平的；Lock，可重入的，可以判断锁，可以自己设置公平锁和非公平锁；

6、Synchronized 适合锁少量的代码同步问题，Lock适合锁大量的同步代码；

#### 什么是可重入锁

又叫递归锁

    就是一个线程不用释放，可以重复的获取一个锁n次，只是在释放的时候，也需要相应的释放n次。（简单来说：A线程在某上下文中或得了某锁，当A线程想要在次获取该锁时，不会应为锁已经被自己占用，而需要先等到锁的释放）假使A线程即获得了锁，又在等待锁的释放，就会造成死锁。
    指的是以线程为单位，当一个线程获取对象锁之后，这个线程可以再次获取本对象上的锁，而其他的线程是不可以的。
    synchronized 和 ReentrantLock 都是可重入锁。
```java
public class Demo01 {
    public static void main(String[] args) {
        Phone phone = new Phone();
        new Thread(()->{
            phone.sms();
        },"A").start();
        new Thread(()->{
            phone.sms();
        },"B").start();
    }

}

class Phone{
    public synchronized void sms(){
        System.out.println(Thread.currentThread().getName()+"=> sms");
        call();//这里也有一把锁
    }
    public synchronized void call(){
        System.out.println(Thread.currentThread().getName()+"=> call");
    }
}
```

```java
//lock
public class Demo02 {

    public static void main(String[] args) {
        Phone2 phone = new Phone2();
        new Thread(()->{
            phone.sms();
        },"A").start();
        new Thread(()->{
            phone.sms();
        },"B").start();
    }

}
class Phone2{

    Lock lock=new ReentrantLock();

    public void sms(){
        lock.lock(); //细节：这个是两把锁，两个钥匙
        //lock锁必须配对，否则就会死锁在里面
        try {
            System.out.println(Thread.currentThread().getName()+"=> sms");
            call();//这里也有一把锁
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
    public void call(){
        lock.lock();
        try {
            System.out.println(Thread.currentThread().getName() + "=> call");
        }catch (Exception e){
            e.printStackTrace();
        }
        finally {
            lock.unlock();
        }
    }
}
```



#### 案例：Synchronzied 版本生产者和消费者的问题

```java
/*
线程之间的通信问题：生产者和消费者问题
线程交替执行 A B操作同一个变量 num=0 A让num+1，B让num-1;
 **/
public class ConsumeAndProduct {
  public static void main(String[] args) {
    Data data = new Data();

    new Thread(() -> {
      for (int i = 0; i < 10; i++) {
        try {
          data.increment();
        } catch (InterruptedException e) {
          e.printStackTrace();
        }
      }
    }, "A").start();
    new Thread(() -> {
      for (int i = 0; i < 10; i++) {
        try {
          data.decrement();
        } catch (InterruptedException e) {
          e.printStackTrace();
        }
      }
    }, "B").start();
  }
}

class Data {
  private int num = 0;

  // +1
  public synchronized void increment() throws InterruptedException {
    // 判断等待
    if (num != 0) {//while
      this.wait();
    }
    num++;
    System.out.println(Thread.currentThread().getName() + "=>" + num);
    // 通知其他线程 +1 执行完毕
    this.notifyAll();
  }

  // -1
  public synchronized void decrement() throws InterruptedException {
    // 判断等待
    if (num == 0) { //while
      this.wait();
    }
    num--;
    System.out.println(Thread.currentThread().getName() + "=>" + num);
    // 通知其他线程 -1 执行完毕
    this.notifyAll();
  }
}


```

存在的问题（虚假唤醒）

如果上面的代码是4个线程，if判断时 可能进去2个，其他线程通知时，两个又同时唤醒，可能原本是0，+1后 通知了两-1，最后变成了负一。

![image-20220926092234091](../../tools/Typora/upload/image-20220926092234091.png)

```java
public synchronized void increment() throws InterruptedException {
  // 判断等待
  while (num != 0) {//while
    this.wait();

  }
  num++;
  System.out.println(Thread.currentThread().getName() + "=>" + num);
  // 通知其他线程 +1 执行完毕
  this.notifyAll();
}

// -1
public synchronized void decrement() throws InterruptedException {
  // 判断等待
  while (num == 0) { //while
    this.wait();
    //把if改成while后 被唤醒不会马上去num--; 会再进行一次判断，防止了一下子唤醒了多个 多个线程的-1或者+1
  }
  num--;
  System.out.println(Thread.currentThread().getName() + "=>" + num);
  // 通知其他线程 -1 执行完毕
  this.notifyAll();
}
```

#### 案例：LOCK版本生产者和消费者的问题

![image-20220926094334561](http://bijioss.donggei.top/image-20220926094334561.png)

```java
public class LockCAP {
  public static void main(String[] args) {
    Data2 data = new Data2();

    new Thread(() -> {
      for (int i = 0; i < 10; i++) {

        try {
          data.increment();
        } catch (InterruptedException e) {
          e.printStackTrace();
        }

      }
    }, "A").start();
    new Thread(() -> {
      for (int i = 0; i < 10; i++) {
        try {
          data.decrement();
        } catch (InterruptedException e) {
          e.printStackTrace();
        }
      }
    }, "B").start();
    new Thread(() -> {
      for (int i = 0; i < 10; i++) {
        try {
          data.increment();
        } catch (InterruptedException e) {
          e.printStackTrace();
        }
      }
    }, "C").start();
    new Thread(() -> {
      for (int i = 0; i < 10; i++) {
        try {
          data.decrement();
        } catch (InterruptedException e) {
          e.printStackTrace();
        }
      }
    }, "D").start();
  }
}

class Data2 {
  private int num = 0;
  Lock lock = new ReentrantLock();
  Condition condition = lock.newCondition();
  // +1
  public  void increment() throws InterruptedException {
    lock.lock();
    try {
      // 判断等待
      while (num != 0) {
        condition.await();
      }
      num++;
      System.out.println(Thread.currentThread().getName() + "=>" + num);
      // 通知其他线程 +1 执行完毕
      condition.signalAll();
    }finally {
      lock.unlock();
    }

  }

  // -1
  public  void decrement() throws InterruptedException {
    lock.lock();
    try {
      // 判断等待
      while (num == 0) {
        condition.await();
      }
      num--;
      System.out.println(Thread.currentThread().getName() + "=>" + num);
      // 通知其他线程 +1 执行完毕
      condition.signalAll();
    }finally {
      lock.unlock();
    }

  }
}

```

精准的通知和唤醒的线程

任何一个新的技术，绝对不是仅仅只是覆盖了原来的技术，优势和补充

这个例子不太好，这个是通过状态控制的，不知道为啥要用三个condition  一个也是可以的，因为这里while条件已经限制了顺序

```java
public class C {
  public static void main(String[] args) {
    Data3 data3 = new Data3();

    new Thread(()  -> {
      for (int i = 0; i < 10; i++) {
        data3.printA();
      }
    },"A").start();
    new Thread(() -> {
      for (int i = 0; i < 10; i++) {
        data3.printB();
      }
    },"B").start();
    new Thread(() -> {
      for (int i = 0; i < 10; i++) {
        data3.printC();
      }
    },"C").start();
  }

}
class Data3 {
  private Lock lock = new ReentrantLock();
  private Condition condition1 = lock.newCondition();
  private Condition condition2 = lock.newCondition();
  private Condition condition3 = lock.newCondition();
  private int num = 1; // 1A 2B 3C

  public void printA() {
    lock.lock();
    try {
      // 业务代码 判断 -> 执行 -> 通知
      while (num != 1) {
        condition1.await();
      }
      System.out.println(Thread.currentThread().getName() + "==> AAAA" );
      num = 2;
      condition2.signal();
    }catch (Exception e) {
      e.printStackTrace();
    }finally {
      lock.unlock();
    }
  }
  public void printB() {
    lock.lock();
    try {
      // 业务代码 判断 -> 执行 -> 通知
      while (num != 2) {
        condition2.await();
      }
      System.out.println(Thread.currentThread().getName() + "==> BBBB" );
      num = 3;
      condition3.signal();
    }catch (Exception e) {
      e.printStackTrace();
    }finally {
      lock.unlock();
    }
  }
  public void printC() {
    lock.lock();
    try {
      // 业务代码 判断 -> 执行 -> 通知
      while (num != 3) {
        condition3.await();
      }
      System.out.println(Thread.currentThread().getName() + "==> CCCC" );
      num = 1;
      condition1.signal();
    }catch (Exception e) {
      e.printStackTrace();
    }finally {
      lock.unlock();
    }
  }
}
/*
A==> AAAA
B==> BBBB
C==> CCCC
A==> AAAA
B==> BBBB
C==> CCCC
...
*/
```

## 4.八个关于锁的问题

1.

```java
public class dome01 {
    public static void main(String[] args) throws InterruptedException {
        Phone phone = new Phone();

        new Thread(() -> {
            try {
                phone.sendMs();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }).start();
        TimeUnit.SECONDS.sleep(1);
        new Thread(() -> { phone.call(); }).start();
    }
}

class Phone {
    public synchronized void sendMs() throws InterruptedException {
        TimeUnit.SECONDS.sleep(4);
        System.out.println("发短信");
    }
    public synchronized void call() {
        System.out.println("打电话");
    }
}

输出结果为

发短信

打电话
```

原因：并不是顺序执行，而是synchronized 锁住的对象是方法的调用！对于两个方法用的是同一个锁，谁先拿到谁先执行，另外一个等待

2.

```java
public class dome01 {
    public static void main(String[] args) throws InterruptedException {
        Phone phone = new Phone();

        new Thread(() -> {
            try {
                phone.sendMs();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }).start();
        TimeUnit.SECONDS.sleep(1);
        new Thread(() -> { phone.hello(); }).start();
    }
}

class Phone {
    public synchronized void sendMs() throws InterruptedException {
        TimeUnit.SECONDS.sleep(4);
        System.out.println("发短信");
    }
    public synchronized void call() {
        System.out.println("打电话");
    }
    public void hello() {
        System.out.println("hello");
    }
}
输出结果为
hello

```

hello是一个普通方法，不受synchronized锁的影响，不用等待锁的释放

sendMs在里面等了4秒

3.加一个普通方法

```java
public class dome01 {
    public static void main(String[] args) throws InterruptedException {
        Phone phone = new Phone();

        new Thread(() -> {
            try {
                phone.sendMs();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }).start();
        TimeUnit.SECONDS.sleep(1);
        new Thread(() -> { phone.hello(); }).start();
    }
}

class Phone {
    public synchronized void sendMs() throws InterruptedException {
        TimeUnit.SECONDS.sleep(4);
        System.out.println("发短信");
    }
    public synchronized void call() {
        System.out.println("打电话");
    }
    public void hello() {
        System.out.println("hello");
    }
}
输出结果
hello
发短信
```

原因：hello是一个普通方法，不受synchronized锁的影响，不用等待锁的释放

4.如果我们使用的是两个对象，一个调用发短信，一个调用打电话，那么整个顺序是怎么样的呢？

```java
public class dome01 {
    public static void main(String[] args) throws InterruptedException {
        Phone phone1 = new Phone();
        Phone phone2 = new Phone();

        new Thread(() -> {
            try {
                phone1.sendMs();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }).start();
        TimeUnit.SECONDS.sleep(1);
        new Thread(() -> { phone2.call(); }).start();
    }
}

class Phone {
    public synchronized void sendMs() throws InterruptedException {
        TimeUnit.SECONDS.sleep(4);
        System.out.println("发短信");
    }
    public synchronized void call() {
        System.out.println("打电话");
    }
    public void hello() {
        System.out.println("hello");
    }
}
输出结果
打电话
发短信
```

原因：两个对象两把锁，不会出现等待的情况，发短信睡了4s,所以先执行打电话

5和6.如果我们把synchronized的方法加上static变成静态方法！那么顺序又是怎么样的呢？

（1）我们先来使用一个对象调用两个方法！

答案是：先发短信,后打电话

（2）如果我们使用两个对象调用两个方法！

答案是：还是先发短信，后打电话

原因是：对于static静态方法来说，对于整个类Class来说只有一份，对于不同的对象使用的是同一份方法，相当于这个方法是属于这个类的，如果静态static方法使用synchronized锁定，那么这个synchronized锁会锁住整个类！不管多少个对象，对于静态的锁都只有一把锁，谁先拿到这个锁就先执行，其他的进程都需要等待！


7.

```java
public class dome01 {
    public static void main(String[] args) throws InterruptedException {
        Phone phone1 = new Phone();
//        Phone phone2 = new Phone();

        new Thread(() -> { 
            try {
                phone1.sendMs();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }).start();
        TimeUnit.SECONDS.sleep(1);
        new Thread(() -> { phone1.call(); }).start();
    }
}

class Phone {
    public static synchronized void sendMs() throws InterruptedException {
        TimeUnit.SECONDS.sleep(4);
        System.out.println("发短信");
    }
    public synchronized void call() {
        System.out.println("打电话");
    }
    public void hello() {
        System.out.println("hello");
    }
}
输出结果
打电话
发短信
```

原因：因为一个锁的是Class类的模板，一个锁的是对象的调用者。所以不存在等待，直接运行。

8.

如果我们使用一个静态同步方法、一个同步方法、**两个**对象调用顺序是什么？

```java
public class dome01 {
    public static void main(String[] args) throws InterruptedException {
        Phone phone1 = new Phone();
        Phone phone2 = new Phone();

        new Thread(() -> {
            try {
                phone1.sendMs();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }).start();
        TimeUnit.SECONDS.sleep(1);
        new Thread(() -> { phone2.call(); }).start();
    }
}

class Phone {
    public static synchronized void sendMs() throws InterruptedException {
        TimeUnit.SECONDS.sleep(4);
        System.out.println("发短信");
    }
    public synchronized void call() {
        System.out.println("打电话");
    }
    public void hello() {
        System.out.println("hello");
    }
}
输出结果
打电话
发短信
```

原因：两把锁锁的不是同一个东西,其实和上面一样。

**new** 出来的 this 是具体的一个对象

**static Class** 是唯一的一个模板

## 5.集合的不安全操作

### 1）List不安全

```java
//java.util.ConcurrentModificationException 并发修改异常！
public class ListTest {
    public static void main(String[] args) {
		//并发下Arraylist是不安全的
        List<Object> arrayList = new ArrayList<>();

        for(int i=1;i<=10;i++){
            new Thread(()->{
                arrayList.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(arrayList);
            },String.valueOf(i)).start();
        }

    }
}
```

解决方案：

```
1. List<String> arrayList = new Vector<>();
2. List<String> arrayList = Collections.synchronizedList(new ArrayList<>());
3. List<String> arrayList = new CopyOnWriteArrayList<>();
```

> 	源码里Vector1.0 就出来的
> 	Vector Since:1.0
> 	ArrayList Since:1.2

CopyOnWriteArrayList：写入时复制！ COW 计算机程序设计领域的一种优化策略

核心思想是，如果有多个调用者（Callers）同时要求相同的资源（如内存或者是磁盘上的数据存储），他们会共同获取相同的指针指向相同的资源，直到某个调用者视图修改资源内容时，系统才会真正复制一份专用副本（private copy）给该调用者，而其他调用者所见到的最初的资源仍然保持不变。这过程对其他的调用者都是透明的（transparently）。此做法主要的优点是如果调用者没有修改资源，就不会有副本（private copy）被创建，因此多个调用者只是读取操作时可以共享同一份资源。

> **CopyOnWriteArrayList**比**Vector**厉害在哪里？
>
> 不是锁的问题。是读写性能的问题，vector所有的操作都有锁，但是copyonwrite读操作是没有锁的！在开发中读比写更频繁，所以说cow好在这里，另外还有一个有点是数组扩容问题。cow不需要扩容.
>
> 在资源竞争不是很激烈的情况下，Synchronized的性能要优于ReetrantLock，但是在资源竞争很激烈的情况下，Synchronized的性能会下降几十倍，但是ReetrantLock的性能能维持常态；
>
> 下面的在jdk8中适用。在jdk11中CopyOnWriteArrayList也使用的是**synchronized**锁
>
> **Vector**底层是使用**synchronized**关键字来实现的。
>
> ![image-20200811144549151](http://bijioss.donggei.top/ec0193ec03b4e67874350644b0f7b6ec.png)
>
> **CopyOnWriteArrayList**使用的是Lock锁
>
> ![image-20200811144447781](http://bijioss.donggei.top/65d81d2d9f14913e79f91dcf23f8eea7.png)

### 2）set 不安全

**Set和List同理可得:** 多线程情况下，普通的Set集合是线程不安全的；

解决方案还是两种：

- 使用Collections工具类的**synchronized**包装的Set类
- 使用CopyOnWriteArraySet 写入复制的**JUC**解决方案

```java
public class SetTest {
    public static void main(String[] args) {
        /**
         * 1. Set<String> set = Collections.synchronizedSet(new HashSet<>());
         * 2. Set<String> set = new CopyOnWriteArraySet<>();
         */
//        Set<String> set = new HashSet<>();
        Set<String> set = new CopyOnWriteArraySet<>();

        for (int i = 1; i <= 30; i++) {
            new Thread(() -> {
                set.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(set);
            },String.valueOf(i)).start();
        }
    }
}
```

**HashSet底层是什么？**

hashSet底层就是一个**HashMap**；

map 是KV格式，set是无重复元素，无索引对迭代顺序不作保证的，

```java
源码:
public HashSet() {
    map = new HashMap<>();
 }
就p放入了map的key,map的K本来就唯一的，V是一个常量
public boolean add(E e) {
    return map.put(e, PRESENT)==null;
 }
```

### 3）Map不安全

```java
public class MapTest {
    //map 是这样用的吗？ 
    //默认等价什么？ new HashMap<>(16,0.75);    //初始化容量 加载因子
    //0.75是空间复杂度和时间复杂度的平衡
    Map<String, String> map = new HashMap<>();
}
```

```java
public class MapTest {
    public static void main(String[] args) {
        //map 是这样用的吗？  不是，工作中不使用这个
        //默认等价什么？ new HashMap<>(16,0.75);
        /**
         * 解决方案
         * 1. Map<String, String> map = Collections.synchronizedMap(new HashMap<>());
         * 2.Map<String, String> map = new ConcurrentHashMap<>();
         */
        Map<String, String> map = new ConcurrentHashMap<>();
        //加载因子、初始化容量
        for (int i = 1; i < 100; i++) {
            new Thread(()->{
                map.put(Thread.currentThread().getName(), UUID.randomUUID().toString().substring(0,5));
                System.out.println(map);
            },String.valueOf(i)).start();
        }
    }
}
```

## 6.callable

![image-20220926133257339](http://bijioss.donggei.top/image-20220926133257339.png)

![image-20220926141326865](http://bijioss.donggei.top/image-20220926141326865.png)

不太好的一个例子

因为Thread的构造参数中有rannable

![image-20220926141424770](../../tools/Typora/upload/image-20220926141424770.png)

```
用futureTask去调用callable
```

```java
public class CallableTest {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        for (int i = 1; i < 10; i++) {
            MyThread1 myThread1 = new MyThread1();

            FutureTask<Integer> futureTask = new FutureTask<>(myThread1);
            // 放入Thread中使用，结果会被缓存
            new Thread(futureTask,String.valueOf(i)).start();
            // 这个get方法可能会被阻塞，如果在call方法中是一个耗时的方法，所以一般情况我们会把这个放在最后，或者使用异步通信
            int a = futureTask.get();
            System.out.println("返回值:" + s);
        }

    }

}
class MyThread1 implements Callable<Integer> {

    @Override
    public Integer call() throws Exception {
        System.out.println("call()");
        return 1024;
    }
}


```

## 7.常用辅助类

### countDownLatch

![image-20220926205321753](http://bijioss.donggei.top/image-20220926205321753.png)

数量到零 线程才执行下面的

```java
public class CountDownLatchDemo {
    public static void main(String[] args) throws InterruptedException {
        // 总数是6
        CountDownLatch countDownLatch = new CountDownLatch(6);

        for (int i = 1; i <= 6; i++) {
            new Thread(() -> {
                System.out.println(Thread.currentThread().getName() + "==> Go Out");
                countDownLatch.countDown(); // 每个线程都数量 -1
            },String.valueOf(i)).start();
        }
        countDownLatch.await(); // 等待计数器归零 然后向下执行
        System.out.println("close door");
    }
}
```

主要方法：

- countDown 减一操作；
- await 等待计数器归零;

await 等待计数器归零，就唤醒，再继续向下运行

### cyclicBarrier

![image-20200811202603352](http://bijioss.donggei.top/dee6ef3d75096d41547b6729fcce3037.png)

集齐数量才触发线程执行后面的

```java
public class CyclicBarrierDemo {
    public static void main(String[] args) {
        // 主线程  
        // 下面的代码解释：等够了7个就发车 触发线程，召唤神龙
        CyclicBarrier cyclicBarrier = new CyclicBarrier(7,() -> {
            System.out.println("召唤神龙");
        });

        for (int i = 1; i <= 7; i++) {
            // 子线程
            int finalI = i;
            new Thread(() -> {
                System.out.println(Thread.currentThread().getName() + "收集了第" + finalI + "颗龙珠");
                try {
                    cyclicBarrier.await(); // 加法计数+1 等待
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } catch (BrokenBarrierException e) {
                    e.printStackTrace();
                }
            }).start();
        }
    }
}
```

### Semaphore

![image-20220926212133277](http://bijioss.donggei.top/image-20220926212133277.png)

```java
public class SemaphoreDemo {
    public static void main(String[] args) {

        // 线程数量，停车位，限流
        Semaphore semaphore = new Semaphore(3);
        for (int i = 0; i <= 6; i++) {
            new Thread(() -> {
                // acquire() 得到
                try {
                    semaphore.acquire(); //他就会努力去抢到线程
                    System.out.println(Thread.currentThread().getName() + "抢到车位");
                    TimeUnit.SECONDS.sleep(2);
                    System.out.println(Thread.currentThread().getName() + "离开车位");
                }catch (Exception e) {
                    e.printStackTrace();
                }finally {
                    semaphore.release(); // release() 释放
                }
            }).start();
        }
    }
}
```

原理：

**semaphore.acquire()获得资源，如果资源已经使用完了，就等待资源释放后再进行使用！**

**semaphore.release()释放，会将当前的信号量释放，然后唤醒等待的线程！**

作用： 多个共享资源互斥的使用！ 并发限流，控制最大的线程数！

## 8.读写锁

![image-20220926212658657](http://bijioss.donggei.top/image-20220926212658657.png)

**ReadWriteLock**也是一个接口，提供了readLock和writeLock两种锁的操作机制，一个资源可以被多个线程同时读，或者被一个线程写，但是不能同时存在读和写线程。

**独占锁** 写锁。 **共享锁** 读锁。

适合读多写少的场景

```java
public class ReadWriteLockDemo {
    public static void main(String[] args) {
        MyCache myCache = new MyCache();
        int num = 6;
        for (int i = 1; i <= num; i++) {
            int finalI = i;
            new Thread(() -> {

                myCache.write(String.valueOf(finalI), String.valueOf(finalI));

            },String.valueOf(i)).start();
        }

        for (int i = 1; i <= num; i++) {
            int finalI = i;
            new Thread(() -> {

                myCache.read(String.valueOf(finalI));

            },String.valueOf(i)).start();
        }
    }
}

/**
 *  方法未加锁，导致写的时候被插队
 所以如果我们不加锁的情况，多线程的读写会造成数据不可靠的问题。

我们也可以采用synchronized这种重量锁和轻量锁 lock去保证数据的可靠。

但是这次我们采用更细粒度的锁：ReadWriteLock 读写锁来保证
 */
class MyCache {
    private volatile Map<String, String> map = new HashMap<>();

    public void write(String key, String value) {
        System.out.println(Thread.currentThread().getName() + "线程开始写入");
        map.put(key, value);
        System.out.println(Thread.currentThread().getName() + "线程写入ok");
    }

    public void read(String key) {
        System.out.println(Thread.currentThread().getName() + "线程开始读取");
        map.get(key);
        System.out.println(Thread.currentThread().getName() + "线程写读取ok");
    }
}

class MyCache2 {
    private volatile Map<String, String> map = new HashMap<>();
    private ReadWriteLock lock = new ReentrantReadWriteLock();

    public void write(String key, String value) {
        lock.writeLock().lock(); // 写锁
        try {
            System.out.println(Thread.currentThread().getName() + "线程开始写入");
            map.put(key, value);
            System.out.println(Thread.currentThread().getName() + "线程写入ok");

        }finally {
            lock.writeLock().unlock(); // 释放写锁
        }
    }

    public void read(String key) {
        lock.readLock().lock(); // 读锁
        try {
            System.out.println(Thread.currentThread().getName() + "线程开始读取");
            map.get(key);
            System.out.println(Thread.currentThread().getName() + "线程写读取ok");
        }finally {
            lock.readLock().unlock(); // 释放读锁
        }
    }
}

```

## 9.阻塞队列BlockingQueue

![img](http://bijioss.donggei.top/afc6f8af8b8ac6696d341aebe33ffdc6.jpeg)

![image-20220927101703742](http://bijioss.donggei.top/image-20220927101703742.png)

什么时候会使用堵塞队列：多线程并发处理，线程池

### 四组API

| 方式         | 抛出异常  | 有返回值（T or F），不抛出异常 | 阻塞，一直等待 | 阻塞，超时等待 |
| ------------ | --------- | ------------------------------ | -------------- | -------------- |
| 添加         | add()     | offer()                        | put()          | offer(，，)    |
| 移除         | remove()  | pull()                         | take()         | pull(，)       |
| 检测队首元素 | element() | peek()                         | -              | -              |

```java
public class TestBlockingQueue {
    public static void main(String[] args) throws InterruptedException {
        test4();
    }
    // 抛出异常：java.lang.IllegalStateException: Queue full
    public static void test1(){
        // 队列的大小为3
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);
        // add()方法返回boolean值
        boolean flag1 = blockingQueue.add("a");
        boolean flag2 = blockingQueue.add("b");
        boolean flag3 = blockingQueue.add("c");
        // !!! add添加元素超过队列的长度会抛出异常java.lang.IllegalStateException: Queue full
        boolean flag4 = blockingQueue.add("d");
        System.out.println(blockingQueue.element());// 获得队首元素
        System.out.println("=========");
        // remove()返回本次移除的元素
        Object e1 = blockingQueue.remove();//a
        Object e2 = blockingQueue.remove();//b
        Object e3 = blockingQueue.remove();//c
        //!!!  队列中没有元素仍继续移除元素会抛出异常java.util.NoSuchElementException
        Object e4 = blockingQueue.remove();
    }
    // 有返回值，不抛出异常
    public static void test2(){
        // 队列的大小为3
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);
        // offer返回boolean值
        boolean flag1 = blockingQueue.offer("a");
        boolean flag2 = blockingQueue.offer("b");
        boolean flag3 = blockingQueue.offer("c");

        //boolean flag4 = blockingQueue.offer("d");// offer添加元素超过队列的长度会返回false
        System.out.println(blockingQueue.peek());// 获得队首元素
        System.out.println("=========");
        // poll()返回本次移除的元素
        Object poll1 = blockingQueue.poll();
        Object poll2 = blockingQueue.poll();
        Object poll3 = blockingQueue.poll();
        Object poll4 = blockingQueue.poll();// 队列中没有元素仍继续移除元素会打印出null
    }
    // 阻塞，一直等待
    public static void test3() throws InterruptedException {
        // 队列的大小为3
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);
        // put没有返回值
        blockingQueue.put("a");
        blockingQueue.put("b");
        blockingQueue.put("c");
        //blockingQueue.put("d");// put添加元素超过队列的长度会一直等待
        System.out.println("=========");
        // take()返回本次移除的元素
        Object take1 = blockingQueue.take();
        Object take2 = blockingQueue.take();
        Object take3 = blockingQueue.take();
        Object take4 = blockingQueue.take();// 队列中没有元素仍继续移除元素会一直等待
    }
    // 阻塞，超时等待
    public static void test4() throws InterruptedException {
        // 队列的大小为3
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);
        // offer返回boolean值
        boolean flag1 = blockingQueue.offer("a");
        boolean flag2 = blockingQueue.offer("b");
        boolean flag3 = blockingQueue.offer("c");
        // offer添加元素超过队列的长度会返回false；并且等待指定时间后推出，向下执行
        boolean flag4 = blockingQueue.offer("d", 2, TimeUnit.SECONDS);
        System.out.println("=========");
        // poll()返回本次移除的元素
        Object poll1 = blockingQueue.poll();
        Object poll2 = blockingQueue.poll();
        Object poll3 = blockingQueue.poll();
        // 队列中没有元素仍继续移除元素会打印出null，等待指定之间后退出。
        Object poll4 = blockingQueue.poll(2,TimeUnit.SECONDS);
    }
}
```

### SynchronousQueue同步队列

SynchronousQueue是BlockingQueue的一个实现类

没有容量。

进去**一个**元素，必须等待取出这个元素后，才能放下**一个**元素。put()、take()

## 10.线程池

**线程池:三大方法,7大参数,4种拒绝策略**

> 创建，销毁浪费资源
>
> 池化技术 优化资源的使用
>
> 事先准备好一些资源，有人用就来这里拿，用完后，还给我。

好处：

+ 降低资源的消耗
+ 提高响应速度
+ 方便管理

**线程可以复用,可以控制最大并发量,管理线程**

### 三大方法

Executors：执行人

线程池不允许使用Executors去创建，而是通过ThreadPoolExecutor的方式，这样的处理方式让写的同学更加明确线程池的运行规则，规避资源耗尽的风险。 说明：Executors各个方法的弊端：
1）newFixedThreadPool和newSingleThreadExecutor:
  主要问题是堆积的请求处理队列可能会耗费非常大的内存，甚至OOM。
2）newCachedThreadPool和newScheduledThreadPool:
  主要问题是线程数最大数是Integer.MAX_VALUE(约为21亿)，可能会创建数量非常多的线程，甚至OOM。

```java
public interface Executor {
    void execute(Runnable command); //也是Runnable接口
```

```java
public class demo1 {
    public static void main(String[] args) {

        //Executors工具类,三大方法
        // ExecutorService threadPool = Executors.newSingleThreadExecutor();//单个线程
        // ExecutorService threadPool = Executors.newFixedThreadPool(5);//创建一个固定大小的线程池
        //ExecutorService threadPool = Executors.newCachedThreadPool();//可伸缩，线程数可变

        ExecutorService threadPool = Executors.newSingleThreadExecutor();//单个线程
        for (int i = 0; i < 10; i++) {
            //使用了线程池之后,使用线程池来创建线程
            threadPool.execute(() -> {
                System.out.println(Thread.currentThread().getName() + "ok");
            }); 
        }
        //线程池用完,程序结束,关闭线程池
        try {
            threadPool.shutdown();
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
        }
    }

}
```

每一个的执行结果：

```java
 Executors.newSingleThreadExecutor();//单个线程      
pool-1-thread-1ok
pool-1-thread-1ok
pool-1-thread-1ok
pool-1-thread-1ok
pool-1-thread-1ok
pool-1-thread-1ok
pool-1-thread-1ok
pool-1-thread-1ok
pool-1-thread-1ok
pool-1-thread-1ok
 Executors.newFixedThreadPool(5);//创建一个固定大小得线程池
pool-1-thread-3ok
pool-1-thread-2ok
pool-1-thread-5ok
pool-1-thread-4ok
pool-1-thread-1ok
pool-1-thread-4ok
pool-1-thread-5ok
pool-1-thread-3ok
pool-1-thread-4ok
pool-1-thread-1ok
 ExecutorService threadPool = Executors.newCachedThreadPool();//可伸缩，线程数可变
pool-1-thread-2ok
pool-1-thread-1ok
pool-1-thread-3ok
pool-1-thread-10ok
pool-1-thread-8ok
pool-1-thread-9ok
pool-1-thread-5ok
pool-1-thread-6ok
pool-1-thread-4ok
pool-1-thread-7ok
```

### 七大参数



```java
public static ExecutorService newSingleThreadExecutor() {
        return new FinalizableDelegatedExecutorService
            (new ThreadPoolExecutor(1, 1,
                                    0L, TimeUnit.MILLISECONDS,
                                    new LinkedBlockingQueue<Runnable>()));
    }
public static ExecutorService newFixedThreadPool(int nThreads) {
        return new ThreadPoolExecutor(nThreads, nThreads,
                                      0L, TimeUnit.MILLISECONDS,
                                      new LinkedBlockingQueue<Runnable>());
    }
public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
    }
三大方法底层都调用得是ThreadPoolExecutor!!
    
```

不推荐使用的原因：

![image-20220927171545335](http://bijioss.donggei.top/image-20220927171545335.png)

```java
//七个参数
    public ThreadPoolExecutor(int corePoolSize,//核心线程池大小
                              int maximumPoolSize,//线程池中允许的最大线程数
                              long keepAliveTime,//超时了,没人用就会释放
                              TimeUnit unit,//超时单位
                              BlockingQueue<Runnable> workQueue,//阻塞队列
                              ThreadFactory threadFactory,//线程工厂，创建线程，一般不用动
                              RejectedExecutionHandler handler//拒绝策略) {
        if (corePoolSize < 0 ||
            maximumPoolSize <= 0 ||
            maximumPoolSize < corePoolSize ||
            keepAliveTime < 0)
            throw new IllegalArgumentException();
        if (workQueue == null || threadFactory == null || handler == null)
            throw new NullPointerException();
        this.acc = System.getSecurityManager() == null ?
                null :
                AccessController.getContext();
        this.corePoolSize = corePoolSize;
        this.maximumPoolSize = maximumPoolSize;
        this.workQueue = workQueue;
        this.keepAliveTime = unit.toNanos(keepAliveTime);
        this.threadFactory = threadFactory;
        this.handler = handler;
    }
```



+ corePoolSize,//核心线程池大小 就是最小线程池大小

+ keepAliveTime：线程的最大生命周期，这里的生命周期有两个约束条件

  一：该参数针对的是超过corePoolSize数量的线程

  二：处于非运行状态的线程。即非核心线程数存活时间

+ 一般情况就是核心线程去处理

  **当线程过来 阻塞队列等待（候客区）的都满了 才会去开启非核心线程**（测过）

  如下图就是6个及以上 才会去创建非核心线程
  
+ 最大的承载就是阻塞队列+最大线程数   

  如下图就是8个


+ 注意还有一个`largestPoolSize`，记录了曾经出现的最大线程个数。因为`setMaximumPoolSize()`可以改变最大线程数。

+ keepAliveTime 超时等待：下图 345是非核心线程 ，keepAliveTime 时间内没人使用就自动关闭了

  

![image-20220927172328093](http://bijioss.donggei.top/image-20220927172328093.png)

```java
new ThreadPoolExecutor(2,
        5,
        3,
        TimeUnit.SECONDS,
        new LinkedBlockingDeque<>(3),
        Executors.defaultThreadFactory(),
        new ThreadPoolExecutor.AbortPolicy());
```

![image-20220927173820754](http://bijioss.donggei.top/image-20220927173820754.png)

与上图对应的手动创建的 线程池

### 四种拒绝策略

```java
new ThreadPoolExecutor.AbortPolicy(); // 抛出异常
new ThreadPoolExecutor.CallerRunsPolicy();// 哪来的去哪（主线程来的，就回去让主线程执行,）
new ThreadPoolExecutor.DiscardPolicy();// 丢掉任务，不抛出异常
new ThreadPoolExecutor.DiscardOldestPolicy();// 尝试和最早的竞争，竞争失败了也丢掉任务，也不抛出异常
```

```java
for (int i = 0; i < 10; i++) {
    //使用了线程池之后,使用线程池来创建线程
    threadPool.execute(() -> {  CallerRunsPolicy 让主线程去跑里面的run方法，让主线程去代办 线程池不管了
        System.out.println(Thread.currentThread().getName() + "ok");
    });
}
```
最大线程应该如何设置？		

```java
CPU密集型：最大线程数，CPU几核的就是几，可以保持CPU效率最高。

IO密集型：判断程序中十分耗IO的线程数量，大于这个数，一般是这个数的两倍。


System.out.println(Runtime.getRuntime().availableProcessors());//获得cpu的核心数
```

## 11.ForkJoin

翻译：分支合并

ForkJoin在JDk1.7,并行执行任务!提高效率,数据量大!
 大数据中的一个概念: Map Reduce，把大任务拆分为小任务.

![image-20220928141624530](http://bijioss.donggei.top/image-20220928141624530.png)

ForkJoin的 特点: 工作窃取

这个里面维护的都是双端队列

![image-20220928141808344](http://bijioss.donggei.top/image-20220928141808344.png)

b线程执行完了 去帮a执行一个任务

具体使用：

![image-20220928143142384](http://bijioss.donggei.top/image-20220928143142384.png)

![image-20220928144515633](http://bijioss.donggei.top/image-20220928144515633.png)



## 12.异步执行任务

[CompletableFuture 使用详解](https://www.jianshu.com/p/6bac52527ca4)

在用CompletableFuture编写多线程时，如果需要处理异常，可以用exceptionally，它的作用相当于catch。

exceptionally的特点：

    当出现异常时，会触发回调方法exceptionally
    exceptionally中可指定默认返回结果，如果出现异常，则返回默认的返回结果
```java
public class Demo02 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // completableFuture.get(); // 获取阻塞执行结果
        // 有返回值的 supplyAsync 异步回调
        // ajax，成功和失败的回调
        // 失败返回的是错误信息；

        //给一个生产型的接口实现
        CompletableFuture<Integer> completableFuture = CompletableFuture.supplyAsync(() -> {
            System.out.println(Thread.currentThread().getName() + "supplyAsync=>Integer");
         //  int i = 10 / 0;
            return 200;
        });


        //给个消费类接口  当CompletableFuture的计算结果完成，或者抛出异常的时候，可以执行特定的Action
        Integer integer = completableFuture.whenComplete((t, u) -> {
            System.out.println(Thread.currentThread().getName() + "whenComplete");
            System.out.println("t=>" + t);// 正常的返回结果 200
            System.out.println("u=>" + u);// 错误信息 输出：java.util.concurrent.CompletionException: java.lang.ArithmeticException: / by zero
			//需要这样链式编程
        }).exceptionally(new Function<Throwable, Integer>() {
            @Override
            public Integer apply(Throwable e) {
                System.out.println(Thread.currentThread().getName() + "exceptionally");
                System.out.println(e.getMessage());
                return 500;// 可以获取到错误的返回结果
            }
        }).get();

        System.out.println(integer);

    }
    /**
     * ForkJoinPool.commonPool-worker-19supplyAsync=>Integer
     * ForkJoinPool.commonPool-worker-19whenComplete
     * t=>200
     * u=>null
     * 200
     *
     *
     *
     *
     *   int i = 10 / 0;
     * ForkJoinPool.commonPool-worker-19supplyAsync=>Integer
     * ForkJoinPool.commonPool-worker-19whenComplete
     * t=>null
     * u=>java.util.concurrent.CompletionException: java.lang.ArithmeticException: / by zero
     * ForkJoinPool.commonPool-worker-19exceptionally
     * java.lang.ArithmeticException: / by zero
     * 500
     *
     * Process finished with exit code 0
     */
}
```

## 13.JMM和volatile

线程的工作内存对应虚拟机栈中的栈帧的局部变量表，主存相当于堆

volatile的JVM提供的轻量级的同步机制。

    可见性。（可见性和JMM挂钩）
    不保证原子性。
    禁止指令重排。

什么是JMM？

Java的内存模型，是一个概念，不存在。
JMM同步约定：

    线程解锁前，必须把共享变量立即刷回主内存。
    线程枷锁前，必须读取主内存中最新值到工作内存中。
    加锁和解锁是同一把锁。

Java内存模型定义了8种操作来完成，虚拟机实现必须保证每一种操作都是原子的、不可再拆分的（double和long类型例外）。

    lock（锁定）：作用于主内存的变量，它把一个变量标识为一条线程独占的状态。
    unlock（解锁）：作用于主内存的变量，它把一个处于锁定状态的变量释放出来，释放后的变量才可以被其他线程锁定。
    read（读取）：作用于主内存的变量，它把一个变量的值从主内存传输到线程的工作内存中，以便随后的load动作使用。
    load（载入）：作用于工作内存的变量，它把read操作从主内存中得到的变量值放入工作内存的变量副本中。
    use（使用）：作用于工作内存的变量，它把工作内存中一个变量的值传递给执行引擎，每当虚拟机遇到一个需要使用到变量的值的字节码指令时将会执行这个操作。
    assign（赋值）：作用于工作内存的变量，它把一个从执行引擎接收到的值赋给工作内存的变量，每当虚拟机遇到一个给变量赋值的字节码指令时执行这个操作。
    store（存储）：作用于工作内存的变量，它把工作内存中一个变量的值传送到主内存中，以便随后的write操作使用。
    write（写入）：作用于主内存的变量，它把store操作从工作内存中得到的变量的值放入主内存的变量中。

8种操作必须满足的规则：

    不允许read和load、store和write操作之一单独出现。即不允许一个变量从主内存读取了但工作内存不接受；或者从工作内存发起回写了但主内存不接受的情况出现。
    不允许一个线程丢弃它的最近的assign操作。即变量在工作内存中改变了之后必须把该变化同步回主内存。
    不允许一个线程无原因地（没有发生过任何assign操作）把数据从线程的工作内存同步回主内存。
    一个新的变量只能在主内存中“诞生”，不允许在工作内存中直接使用一个未被初始化（load或assign）的变量，换句话说，就是对一个变量实施use、store操作之前，必须先执行过了assign和load操作。
    一个变量在同一时刻只允许一条线程对其进行lock操作，但lock操作可以被同一条线程重复执行多次，多次执行lock后，只有执行相同次数的unlock操作，变量才会被解锁。
    如果对一个变量执行lock操作，那将会清空工作内存中此变量的值，在执行引擎使用这个变量前，需要重新执行load或assign操作初始化变量的值。
    如果一个变量事先没有被lock操作锁定，那就不允许对它执行unlock操作，也不允许去unlock一个被其他线程锁定住的变量。
    对一个变量执行unlock操作之前，必须先把此变量同步回主内存中（执行store、write操作）。

![img](http://bijioss.donggei.top/33f7e9c7ab1327cc04761de874857761.jpeg)
指令重排

你写的程序，计算机并不一定你写的去执行。
源代码->编译器优化的重排->指令并行也可能会重排->内存系统也会重排->执行
处理器在进行指令重排时，考虑数据之间的依赖性。

volatile避免指令重排：
内存屏障（CPU的指令）。

    保证特定操作的执行顺序。
    可以保证某些变量的内存可见性。
#### 可见性

```java

public class JMMDemo {
    private static int num=0;
    public static void main(String[] args) throws InterruptedException {
        new Thread(()->{
            while (num==0){
                //不能加sout，因为 printIn() 方法使用了 synchronized 同步代码块，可以保证原子性与可见性，它是 PrintStream 类的方法
            }
        }).start();
        TimeUnit.SECONDS.sleep(1);
        num=1;

        System.out.println(num);
    }

}
// 发现JVM 不会停止
```

![image-20220929111824517](http://bijioss.donggei.top/image-20220929111824517.png)

```java
private volatile static int num=0;
```

改了这一行后 程序就退出了  volatile保证了可见性（可见性和JMM挂钩）

#### 不保证原子性

> 拓展：ACID:原子性、一致性、隔离性、持久性
>
> 原子性：不可分割性
>
> 线程A在执行任务的时候，不能被打扰，也不能被分割。要么同时成功，要么同时失败

```java
public class volatileDemo2 {
    //下面加了volatile 没效果 不保证原子性
    private volatile static int num =0;
    //下面加一个synchronized 肯定能解决
    public   static void  add(){
        num++;
    }
    public static void main(String[] args) {
        //理论值为2w
        for (int i = 0; i < 20; i++) {
            new Thread(()->{
                for (int j = 0; j < 1000; j++) {
                    add();
                }
            }).start();
        }
        while (Thread.activeCount()>2){
            //其他线程没完 主线程先等等 礼让
            Thread.yield();
        }
        System.out.println(num);
    }
}
```

上面例子体现的实际就是可见性，它保证的是在多个线程之间，一个线程对volatile 变量的修改对另一个线程可见， 不能保证原子性，仅用在一个写线程，多个读线程的情况。

**如果不加lock和synchronized , 怎么样保证原子性**



线程1加一值为1但是线程二拿到的时候是0，加一还是1。所以两次操作值还是1，肯定就少了

![image-20220929122930755](http://bijioss.donggei.top/image-20220929122930755.png)

使用原子类解决

AtomicInteger的value 是volatile的   同时保证了原子性和可见性  

```java
public class volatileDemo2 {
    //这里volatile不用加了
    private  static AtomicInteger num =new AtomicInteger();
    
    public   static void  add(){
        num.getAndIncrement(); //CAS 调用底层操作系统的 在内存中修改值 比 加锁效率高
    }
    public static void main(String[] args) {
        //理论值为2w
        for (int i = 0; i < 20; i++) {
            new Thread(()->{
                for (int j = 0; j < 1000; j++) {
                    add();
                }
            }).start();
        }
        while (Thread.activeCount()>2){
            //其他线程没完 主线程先等等 礼让
            Thread.yield();
        }
        System.out.println(num);
    }


}
```

#### 禁止指令重排



> 什么是指令重排
>
> 指令重排简单来说可以，在程序结果不受影响的前提下，可以调整指令语句执行顺序。多线程下指令重排会影响正确性。
>
> **指令重排序**是指编译器或CPU为了优化程序的执行性能而对指令进行重新排序的一种手段，重排序会带来可见性问题，所以在多线程开发中必须要关注并规避重排序。
>
> 从源代码到最终运行的指令，会经过如下两个阶段的重排序。
>
> **第一阶段**，编译器重排序，就是在编译过程中，编译器根据上下文分析对指令进行重排序，目的是减少CPU和内存的交互，重排序之后尽可能保证CPU从寄存器或缓存行中读取数据。
>
> 在前面分析[JIT](https://www.cnblogs.com/zwtblog/p/15618879.html)优化中提到的循环表达式外提（Loop Expression Hoisting）就是编译器层面的重排序，从CPU层面来说，避免了处理器每次都去内存中加载stop，减少了处理器和内存的交互开销。
>
> **第二阶段**，处理器重排序，处理器重排序分为两个部分。
>
> - 并行指令集重排序，这是处理器优化的一种，处理器可以改变指令的执行顺序。
> - 内存系统重排序，这是处理器引入Store Buffer缓冲区延时写入产生的指令执行顺序不一致的问题。

例子：

```java
int num = 0;

// volatile 修饰的变量，可以禁用指令重排 volatile boolean ready = false; 可以防止变量之前的代码被重排序
boolean ready = false; //加volatile
// 线程1 执行此方法
public void actor1(I_Result r) {
    //volatile 保证了读ready时后面的读的num 都是主内存里最新的
 if(ready) {
 	r.r1 = num + num;
 } 
 else {
 	r.r1 = 1;
 }
}
// 线程2 执行此方法
public void actor2(I_Result r) {
 num = 2;
 ready = true;  //volatile保证了ready写时 前面的已经写了到主内存中
}
在多线程环境下，以上的代码 r1 的值有三种情况：
第一种：线程 2 先执行，然后线程 1 后执行，r1 的结果为 4
第二种：线程 1 先执行，然后线程 2 后执行，r1 的结果为 1
第三种：线程 2 先执行，但是发送了指令重排，num = 2 与 ready = true 这两行代码语序发生装换，
原文链接：https://blog.csdn.net/weixin_50280576/article/details/113532093
```

![image-20220929124853911](http://bijioss.donggei.top/image-20220929124853911.png)

![image-20220929125747263](http://bijioss.donggei.top/image-20220929125747263.png)

[参考](https://www.cnblogs.com/zwtblog/p/15619334.html#%E4%BB%80%E4%B9%88%E6%98%AF%E6%8C%87%E4%BB%A4%E9%87%8D%E6%8E%92)

经过重排序之后，如果t1和t2线程同时运行，这个结果从人的视角来看，就有点**类似于**t1线程中a=1的修改结果对t2线程**不可见**，同样t2线程中b=1的执行结果对t1线程不可见。

![image-20220929130423949](http://bijioss.donggei.top/image-20220929130423949.png)

volatile 的底层实现原理是**内存屏障**，Memory Barrier（Memory Fence）
 对 volatile 变量的写指令**后会加入写屏障**
 对 volatile 变量的读指令**前会加入读屏障**

写屏障（sfence）保证在该屏障之前的，对共享变量的改动，都同步到主存当中

而读屏障（lfence）保证在该屏障之后，对共享变量的读取，加载的是主存中最新数据

![image-20220929130911005](http://bijioss.donggei.top/image-20220929130911005.png)



####  volatile在单例模式的应用





```java
// 懒汉式: 用的时候创建
public class LazyMan {
    private LazyMan() {
    }
	
	// volatile保证lazyMan创建过程中不进行指令重排
    private volatile static LazyMan lazyMan;



    // 双重检验锁 DCL懒汉（Double Check Lock）
    public static LazyMan getLazyMan() {
        但是因为用到了synchronized，会导致很大的性能开销，并且加锁其实只需要在第一次初始化的时候用到，之后的调用都没必要再进行加锁。先判断对象是否已经被初始化，再决定要不要加锁。第二个lazyMan == null: 如果都进到了拿锁那一行，拿到锁再判断一下是不是null
        if (lazyMan == null) {
        	// 获取LazyMan这个类的锁
            synchronized (LazyMan.class) {
                if (lazyMan == null) {
                    try {
                        TimeUnit.SECONDS.sleep(1);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    lazyMan = new LazyMan();
                }
            }
        }
        return lazyMan;
    }


    // 模拟多线程
    public static void main(String[] args) {
        for (int i = 0; i < 10; i++) {
            new Thread(() -> {
                System.out.println(Thread.currentThread().getName() + "lazyMan : " + getLazyMan());
            }, "" + i).start();
        }
    }
}

```

实例化对象的那行代码

实际上可以分解成以下三个步骤：

1. 分配内存空间
2. 初始化对象
3. 将对象指向刚分配的内存空间

但是有些编译器为了性能的原因，可能会将第二步和第三步进行**重排序**，顺序就成了：

1. 分配内存空间
2. 将对象指向刚分配的内存空间
3. 初始化对象

![image-20220929201159352](http://bijioss.donggei.top/image-20220929201159352.png)





扩展：玩转单例类

> ```java
> /**
>  * 饿汉式单例模式，一加载就创建单例对象
>  */
> public class Hungry {
>     private Hungry(){
>     }
>     private static final Hungry hungry = new Hungry();
>     public static Hungry getInstance(){
>         return hungry;
>     }
> }
> /**
>  * 懒汉式单例模式
>  */
> public class LazyMan {
>     private LazyMan() {
>         System.out.println(Thread.currentThread().getName());
>     }
>     private static LazyMan lazyMan;
>     public static LazyMan getInstance(){
>         if (lazyMan==null){
>             lazyMan = new LazyMan();
>         }
>         return lazyMan;
>     }
>     // 多线程并发创建对象存在问题。
>     public static void main(String[] args) {
>         for (int i = 0; i < 10; i++) {
>             new Thread(()->{
>                 LazyMan.getInstance();
>             }).start();
>         }
>     }
> }
> /**
>  * 懒汉式单例模式
>  */
> public class LazyMan01 {
>     private LazyMan01(){
>         System.out.println(Thread.currentThread().getName());
>     }
>     // 加上volatile关键字禁止指令重排
>     private volatile static LazyMan01 lazyMan01;
>     /**
>      * 双重检测锁模式，DCL懒汉式
>      * @return
>      */
>     public static LazyMan01 getInstance(){
>         if (lazyMan01==null){
>             synchronized(LazyMan01.class){
>                 if (lazyMan01==null){
>                     lazyMan01 =  new LazyMan01();
>                 }
>             }
>         }
>         return lazyMan01;
>     }
>     public static void main(String[] args) {
>         for (int i = 0; i < 10; i++) {
>             new Thread(()->{
>                 LazyMan01.getInstance();
>             }).start();
>         }
>     }
> }
> 对于上面的两种单例模式，使用反射可以简单破解单例
>     private Hungry(){
>     }
>     private static final Hungry hungry = new Hungry();
>     public static Hungry getInstance(){
>         return hungry;
>     }
> 
>     public static void main(String[] args) throws InvocationTargetException, InstantiationException, IllegalAccessException {
>         Constructor<?> declaredConstructor = Hungry.class.getDeclaredConstructors()[0];
>         declaredConstructor.setAccessible(true);
>         Object o = declaredConstructor.newInstance();
>         Hungry instance = Hungry.getInstance();
>         System.out.println(o);
>         System.out.println(instance);
> 
>     }
> 
> //解决一:把构造函数改成这样
> private Hungry(){
>         synchronized (Hungry.class){
>             if (hungry!=null){
>                 throw new RuntimeException("不要试图使用反射破坏");
>             }
>         }
>     }
> // 还能破坏:
>  public static void main(String[] args) throws InvocationTargetException, InstantiationException, IllegalAccessException {
> //        LazyMan01 instance = LazyMan01.getInstance();
> //        System.out.println(instance);
> 
>         Constructor<?> declaredConstructor = LazyMan01.class.getDeclaredConstructors()[0];
>         declaredConstructor.setAccessible(true);
>         Object o1 = declaredConstructor.newInstance();
>         Object o2 = declaredConstructor.newInstance();
> 
>         System.out.println(o1);
>         System.out.println(o2);
>     }
> 
> 
> ```
>
> 这样又解决了：但是如果知道你这个属性的名字，知道你使用的这种方法， 也可以通过反射破解
>
> ![image-20220929215527774](http://bijioss.donggei.top/image-20220929215527774.png)
>
> 在反射的源码里：定义了反射不能创建枚举   可以使用枚举解决![image-20220929221656346](http://bijioss.donggei.top/image-20220929221656346.png)
>
> ![image-20220929222706152](http://bijioss.donggei.top/image-20220929222706152.png)
>
> 

## 14.CAS

[例子](https://www.cnblogs.com/zengcongcong/p/12751761.html#:~:text=ABA%E9%97%AE%E9%A2%98,%E5%B0%B1%E6%98%AF%E4%B8%80%E4%B8%AA%E5%80%BC%E7%94%B1A%E5%8F%98%E4%B8%BAB%EF%BC%8C%E5%9C%A8%E7%94%B1B%E5%8F%98%E4%B8%BAA%EF%BC%8C%E4%BD%BF%E7%94%A8CAS%E6%93%8D%E4%BD%9C%E6%97%A0%E6%B3%95%E6%84%9F%E7%9F%A5%E5%88%B0%E8%AF%A5%E7%A7%8D%E6%83%85%E5%86%B5%E4%B8%8B%E5%87%BA%E7%8E%B0%E7%9A%84%E5%8F%98%E5%8C%96%EF%BC%8C%E5%B8%A6%E6%9D%A5%E7%9A%84%E5%90%8E%E6%9E%9C%E5%BE%88%E4%B8%A5%E9%87%8D%EF%BC%8C%E6%AF%94%E5%A6%82%E9%93%B6%E8%A1%8C%E5%86%85%E9%83%A8%E5%91%98%E5%B7%A5%EF%BC%8C%E4%BB%8E%E7%B3%BB%E7%BB%9F%E6%8C%AA%E8%B5%B0%E4%B8%80%E7%99%BE%E4%B8%87%EF%BC%8C%E4%B9%8B%E5%90%8E%E8%BF%98%E4%BA%86%E5%9B%9E%E6%9D%A5%EF%BC%8C%E7%B3%BB%E7%BB%9F%E6%84%9F%E7%9F%A5%E4%B8%8D%E5%88%B0%E5%B2%82%E4%B8%8D%E6%98%AF%E8%A6%81%E5%87%BA%E4%BA%8B%E3%80%82%E6%A8%A1%E6%8B%9F%E4%B8%8B%E5%87%BA%E7%8E%B0ABA%E9%97%AE%E9%A2%98%3A)

## 15. 自旋锁

在上面的cas中底层调用的getAndAddInt就是自旋锁

```java
public final int getAndAddInt(Object var1, long var2, int var4) {
    int var5;
    do {
        var5 = this.getIntVolatile(var1, var2);
    } while(!this.compareAndSwapInt(var1, var2, var5, var5 + var4));

    return var5;
}
```

自己写的自旋锁

```java
public class SpinlockDemo {
	原子引用想 锁的是一个线程
    // 默认值：  
    // int 0
    //thread null
    AtomicReference<Thread> atomicReference=new AtomicReference<>();

    //加锁
    public void myLock(){
        Thread thread = Thread.currentThread();
        
        System.out.println(thread.currentThread().getName()+"===> mylock");

        //自旋锁 如果是null就把这个atomicReference赋值这个锁，如果没锁就加锁
       //如果不是null 就一直等着  (其他线程进来这个方法之后，就一直等着)
        while (!atomicReference.compareAndSet(null,thread)){
            System.out.println(Thread.currentThread().getName()+" ==> 自旋中~");
        }
    }


    //解锁
    public void myUnlock(){
        Thread thread=Thread.currentThread();
        System.out.println(thread.currentThread().getName()+"===> myUnlock");
        //如果是我期望的这个线程那就把这个线程改成null
        atomicReference.compareAndSet(thread,null);
    }

}


```

```java
public class TestSpinLock {
    public static void main(String[] args) throws InterruptedException {
        //ReentrantLock reentrantLock = new ReentrantLock();
        //reentrantLock.lock();
        //reentrantLock.unlock();


        //使用CAS实现自旋锁
        SpinlockDemo spinlockDemo=new SpinlockDemo();
        new Thread(()->{
            spinlockDemo.myLock();
            try {
                TimeUnit.SECONDS.sleep(3);
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                spinlockDemo.myUnlock();
            }
        },"t1").start();

        TimeUnit.SECONDS.sleep(1);


        new Thread(()->{
            spinlockDemo.myLock();
            try {
                TimeUnit.SECONDS.sleep(3);
            } catch (Exception e) {
                e.printStackTrace();
            } finally {
                spinlockDemo.myUnlock();
            }
        },"t2").start();
    }
}
```

**t2进程必须等待t1进程Unlock后，才能Unlock，在这之前进行自旋等待**

## 16.死锁

产生死锁必须同时满足以下四个条件，只要其中任一条件不成立，死锁就不会发生。

（1）互斥条件：进程要求对所分配的资源（如打印机）进行排他性控制，即在一段时间内某资源仅为一个进程所占有。此时若有其他进程请求该资源，则请求进程只能等待。

（2）不剥夺条件：进程所获得的资源在未使用完毕之前，不能被其他进程强行夺走，即只能由获得该资源的进程自己来释放（只能是主动释放)。

（3）请求和保持条件：进程已经保持了至少一个资源，但又提出了新的资源请求，而该资源已被其他进程占有，此时请求进程被阻塞，但对自己已获得的资源保持不放。

（4）循环等待条件：存在一种进程资源的循环等待链，链中每一个进程已获得的资源同时被链中下一个进程所请求。即存在一个处于等待状态的进程集合{Pl, P2, ..., pn}，其中Pi等 待的资源被P(i+1)占有（i=0, 1, ..., n-1)，Pn等待的资源被P0占有，

![image-20221001124501982](http://bijioss.donggei.top/image-20221001124501982.png)

```java
// 例子不好理解，其实是对"lockA"和"lockB" 这两个常量进行加锁和解锁
这里的锁对象是上面的定义的两个字符串，T1线程的A锁是字符串A，T2线程的A锁是字符串B，这样两个线程获取字符串AB的锁就卡死了
public class DeadLock {
    public static void main(String[] args) {
        String lockA= "lockA";
        String lockB= "lockB";

        new Thread(new MyThread(lockA,lockB),"t1").start();
        new Thread(new MyThread(lockB,lockA),"t2").start();
    }
}

class MyThread implements Runnable{

    private String lockA;
    private String lockB;

    public MyThread(String lockA, String lockB) {
        this.lockA = lockA;
        this.lockB = lockB;
    }

    @Override
    public void run() {
        synchronized (lockA){
            System.out.println(Thread.currentThread().getName()+" lock"+lockA+"===>get"+lockB);
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            synchronized (lockB){
                System.out.println(Thread.currentThread().getName()+" lock"+lockB+"===>get"+lockA);
            }
        }
    }
}
```

> 解决死锁
>
> 1 看日志
>
> 2 jstack 堆栈信息

java bin 目录下有很多工具 有一个工具叫jps



![image-20221001125937539](http://bijioss.donggei.top/image-20221001125937539.png)





[全文参考](https://www.bilibili.com/video/BV1B7411L7tE/?p=39&spm_id_from=333.1007.top_right_bar_window_history.content.click&vd_source=945c5b6f7f6d338aa576bbf6e812a336)

[黑马的参考](https://blog.csdn.net/weixin_50280576/article/details/113727645)

## 线程池在springboot中的应用

背景：

> 在一个项目中  有一个操作是更新文章的阅读数量，如果没有使用redis，他会访问我们数据库的文章表，这个更新操作，更新时加写锁，阻塞其他的读操作，性能就会差 。
>
> 增加了耗时，如果和查询文章详细信息在同一个service中，一旦发生错误，就会影响文章的查看

相关知识点：[ThreadPoolTaskExecutor和ThreadPoolExecutor区别](https://blog.csdn.net/weixin_43168010/article/details/97613895)

解决方法：

>  线程池 可以把更新操作扔到线程池中去执行，和主线程就不相关了		
>
> ![image-20220928101621007](http://bijioss.donggei.top/image-20220928101621007.png)



写配置类

```java
@Configuration
@EnableAsync //开启多线程
public class ThreadPoolOConfig {

    @Bean("taskExecutor")
    public Executor asyncServiceExecutor(){
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(5);
        executor.setMaxPoolSize(20);
        executor.setQueueCapacity(Integer.MAX_VALUE);
        executor.setThreadGroupName("donggei.top");
        //设置所有任务结束后关闭线程池
        executor.setWaitForTasksToCompleteOnShutdown(true);
        //执行初始化
        executor.initialize();
        return executor;
    }

}
```

模拟service

```java
@Component
public class ThreadService {

    //使用这个线程池去执行下面的任务,不会影响主线程
    @Async("taskExecutor")
    public void updateCount(ArticleMapper articleMapper, Article article){
        try {
            Thread.sleep(500); //更新操作
            System.out.println("更新完成了");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

一个有返回值 并回调的方法：[参考](https://blog.csdn.net/QYHuiiQ/article/details/85014983#:~:text=%E5%BC%82%E6%AD%A5%E5%9B%9E%E8%B0%83%E5%B0%B1%E6%98%AF%E8%AE%A9%E6%AF%8F%E4%B8%AA%E8%A2%AB%E8%B0%83%E7%94%A8%E7%9A%84%E6%96%B9%E6%B3%95%E8%BF%94%E5%9B%9E%E4%B8%80%E4%B8%AAFuture%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%80%BC%EF%BC%8C%E8%80%8CSpring%E6%8F%90%E4%BE%9B%E4%BA%86%E4%B8%80%E4%B8%AAFuture%E6%8E%A5%E5%8F%A3%E7%9A%84%E5%AD%90%E7%B1%BB%EF%BC%9AAsyncResult%EF%BC%8C%E6%89%80%E4%BB%A5%E6%88%91%E4%BB%AC%E5%8F%AF%E4%BB%A5%E8%BF%94%E5%9B%9E%E7%9A%84%E6%97%B6%E5%80%99new%E4%B8%80%E4%B8%AAAsyncResult%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%80%BC%E3%80%82%20isDone%20%28%29%E8%BF%94%E5%9B%9EBoolean%E7%B1%BB%E5%9E%8B%E5%80%BC%EF%BC%8C%E7%94%A8%E6%9D%A5%E5%88%A4%E6%96%AD%E8%AF%A5%E5%BC%82%E6%AD%A5%E4%BB%BB%E5%8A%A1%E6%98%AF%E5%90%A6%E6%89%A7%E8%A1%8C%E5%AE%8C%E6%88%90%EF%BC%8C%E5%A6%82%E6%9E%9C%E6%89%A7%E8%A1%8C%E5%AE%8C%E6%88%90%EF%BC%8C%E5%88%99%E8%BF%94%E5%9B%9Etrue%EF%BC%8C%E5%A6%82%E6%9E%9C%E6%9C%AA%E6%89%A7%E8%A1%8C%E5%AE%8C%E6%88%90%EF%BC%8C%E5%88%99%E8%BF%94%E5%9B%9Efalse.,cancel%20%28boolean%20mayInterruptRunning%29%E8%BF%94%E5%9B%9Eboolean%E7%B1%BB%E5%9E%8B%E5%80%BC%EF%BC%8C%E5%8F%82%E6%95%B0%E4%B9%9F%E6%98%AF%E4%B8%80%E4%B8%AAboolean%E7%B1%BB%E5%9E%8B%E7%9A%84%E5%80%BC%EF%BC%8C%E7%94%A8%E6%9D%A5%E4%BC%A0%E5%85%A5%E6%98%AF%E5%90%A6%E5%8F%AF%E4%BB%A5%E6%89%93%E6%96%AD%E5%BD%93%E5%89%8D%E6%AD%A3%E5%9C%A8%E6%89%A7%E8%A1%8C%E7%9A%84%E4%BB%BB%E5%8A%A1%E3%80%82)

项目中线程池一般不需要关闭，跟随项目启动而启动，跟随项目关闭而关闭，如果是局本使用了，也可以关闭。

