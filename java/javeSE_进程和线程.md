# 进程和线程
## 一.实现多线程
### 1.  进程
 线程依赖于进程存在
 进程：正在运行的程序 。
+  是系统进行资源分配和调用的独立空间，
+  每一个进程都有自己的内存空间和系统资源
### 2. 线程
进程中的单个顺序控制流，是一条执行路径
+   单线程程序
+   多线程程序
### 3. 多线程实现方式
```java
public class MyThreadDemo {
    public static void main(String[] args) {
        MyThread myThread1 = new MyThread();
        MyThread myThread2 = new MyThread();

      //  myThread1.run(); //直接调run方法没有启动线程
      //  myThread2.run();
        //start() //导致此线程开始执行; Java虚拟机调用此线程的run方法。
        myThread1.start();
        myThread2.start();
    }
}
public class MyThread extends Thread {
    @Override
    public void run() {
        for (int i=0;i<1000;i++){
            System.out.println(i);
        }
    }
}
```
run 方法是封装被线程执行的代码的
start 此线程开始执行; Java虚拟机调用此线程的run方法。

### 

