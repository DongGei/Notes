java基础面

#### 1.数组和链表结构简单对比？（ArrayList和linkedList）

数组是一段连续的空间。  大小固定 可能大小不够用或者有浪费 

数组查询比较方便，根据下标就可以直接找到元素，时间复杂度O(1)；增加和删除比较复杂，需要移动操作数所在位置后的所有数据，时间复杂度为O(N)

链表是使用是一种物理存储单元上非连续，非顺序的存储结构

插入、删除数据比较方便,时间复杂度O(1)；查询必须从头开始找起，十分麻烦，时间复杂度O(N)

```
LinkedList 插入到中间位置并不快 比ArrayList还慢 因为要先遍历
除了头部插入 LinkedList快 其他都不如ArrayList
```

![image-20221222201335858](http://bijioss.donggei.top/image-20221222201335858.png)

因为一个node里面有前后指针 所以占用内存多

#### 2.类的初始化 对象实例化

**类的初始化：**

是完成程序执行前的准备工作。在这个阶段，**静态的**（变量，方法，代码块）会被执行。同时在会开辟一块存储空间用来存放静态的数据。**类初始化只在类加载的时候执行一次**

**类的实例化（实例化对象）：**

是指创建一个对象的过程。这个过程中会在堆中开辟内存，将一些**非静态**的方法，变量存放在里面。在程序执行的过程中，可以创建多个对象，既多次实例化。每次实例化都会开辟一块新的内存。（就是调用构造函数）

一.什么时候类初始化？

1.第一次实例化之前会先类初始化

**main方法所在的类需要先加载和初始化**

2.子类初始化会先让父类初始化

3.一个类初始化就是执行<clinit>()方法

>  clinit 方法由**静态类变量显示赋值代码**和**静态代码块**组成
>
> 从上到下顺序执行

![image-20221107172006076](http://bijioss.donggei.top/image-20221107172006076.png)

二.什么时候实例化？

实例化就是执行<init>()方法

<init>()方法 方法可能重载有多个，有几个构造器就有几个<init>()方法方法

<init>()方法方法由**非静态实例变量显示赋值代码和非静态代码块，对应构造器代码**组成

 非静态实例变量显示赋值代码和非静态代码块从上到下顺序执行，而对应构造器的代码最后执行、



```java
Son son1 = new Son(); (9)(3)(2)(9)(8)(7) 
```

==注意==！：上图中的 4 没有输出，重写的方法两个类中都调用的是子类的

解释：

![image-20221107182354065](http://bijioss.donggei.top/image-20221107182354065.png)

```java
public static void main(String[] args) {
    Son son = new Son();
    System.out.println();
    Son son1 = new Son();
}
(5)(1)(10)(6)(9)(3)(2)(9)(8)(7)

(9)(3)(2)(9)(8)(7)
```

#### 3.重写的要求是什么？

●方法名  				   必须一样
●形参列表			        必须一样  参数个数、种类、排列顺序相同
●返回值类型  			返回类型可以相同也可以是原类型的子类型
●修饰符        			 子类比父类权限更大或者相等。   父类方法是包访问权限，子类的重写方法是public访问权限
●抛出的异常列表  	  重写方法不能抛出新的异常或者。重写方法不能抛出比被重写方法更高层次的被检查异常

一大 俩小

#### 重载的要求是什么？

方法名称必须相同

参数列表必须不同（个数不同、或类型不同、参数类型排列顺序不同等）

对访问控制修饰符、返回值类型、异常类型 未作要求

#### 4.方法值传递

![image-20221107184416237](http://bijioss.donggei.top/image-20221107184416237.png)

#### 5.变量

1、声明的位置

 局部变量：方法体{}中，形参，代码块{}中

 成员变量：类方法外

 类变量(静态变量)： 有static修饰

 实例变量：没有static修饰

2、修饰符

 局部变量：final

 成员变量：public protected,private,final ,static volatile,transient

3、值存储位置

 局部变量：栈

 实例变量：堆

 类变量（静态变量）：方法区。 1.7后静态变量放在堆中了

4、生命周期

局部变量：每一个线程，每一次调用执行都是新的生命周期

实例变量：随着对象的创建而初始化，随着对象的被回收而消亡，每一个对象的实例变量都是独立的

类变量：随着类的初始化而初始化，随着类的卸载而消亡，该类的所有对象的类变量是共享的

如何区分：

1、局部变量与实例变量重名

     在成员便令前面加 this

2、局部变量与类变量重名

     在类变量前面加 类名
```java
/**
 * @author gcq
 * @Create 2020-09-29
 */
public class Exam5 {
    静态变量在方法区 所有对象共享 所有方法都可以访问  (扩展：静态方法只能访问静态成员
    static int s;//成员变量  类变量 方法区
    int i; //成员变量  	实例变量 	堆
    int j;//成员变量 	实例变量  	堆
    {
        int i = 1; //局部变量
        i++; // 就近原则 上面的这个i
        j++;
        s++;
    }
    public  void test(int j) {
        j++; // 就近原则 21  局部变量
        i++; //成员变量i
        s++;
    }
    public static void main(String[] args){
        Exam5 obj1 = new Exam5();
        Exam5 obj2 = new Exam5();
        obj1.test(10);
        obj1.test(20);
        obj2.test(30);
        // 2 1 5
        System.out.println(obj1.i + "," + obj1.j + "," + obj1.s);
        // 1 1 5
        System.out.println(obj2.i + "," + obj2.j + "," + obj2.s);
    }
}
```

![image-20221107200220215](../../../tools/Typora/upload/image-20221107200220215.png)

![image-20221107200422636](http://bijioss.donggei.top/image-20221107200422636.png)

#### 6.String作为hashmap的键为什么好？

![image-20221112210212234](http://bijioss.donggei.top/image-20221112210212234.png)

调用时放到成员变量了 效率会高一点

#### 7.finally和return之间的纠缠

```java
public class Test{	
    public int add(int a,int b){	
         try {	
             return a+b;		
         } 
        catch (Exception e) {	
            System.out.println("catch语句块");	
         }	
         finally{	
             System.out.println("finally语句块");	
         }	
         return 0;	
    } 
     public static void main(String argv[]){ 
         Test test =new Test(); 
         System.out.println("和是："+test.add(9, 34)); 
     }
}
```

链接：https://www.nowcoder.com/questionTerminal/f041115dd21148c981803a203cf13be7
来源：牛客网

先来看一段代码：

```
public abstract class Test {
    public static void main(String[] args) {
        System.out.println(beforeFinally());
    }
    
    public static int beforeFinally(){
        int a = 0;
        try{
            a = 1;
            return a;
        }finally{
            a = 2;
        }
    }
}
/**output:
1
*/
```

 从结果上看，貌似`finally` 里的语句是在`return` 之后执行的，其实不然，实际上`finally` 里的语句是在在`return` 之前执行的。那么问题来了，既然是在之前执行，那为什么`a` 的值没有被覆盖了？
 实际过程是这样的：当程序执行到try{}语句中的return方法时，它会干这么一件事，将要返回的结果存储到一个临时栈中，然后程序不会立即返回，而是去执行finally{}中的程序，  **在执行`a = 2`时，程序仅仅是覆盖了a的值，但不会去更新临时栈中的那个要返回的值** 。执行完之后，就会通知主程序“finally的程序执行完毕，可以请求返回了”，这时，就会将临时栈中的值取出来返回。这下应该清楚了，要返回的值是保存至临时栈中的。
 再来看一个例子,稍微改下上面的程序：

```
public abstract class Test {
    public static void main(String[] args) {
        System.out.println(beforeFinally());
    }
    
    public static int beforeFinally(){
        int a = 0;
        try{
            a = 1;
            return a;
        }finally{
            a = 2;
            return a;
        }
    }
}
/**output:
2
*/
```

 在这里，finally{}里也有一个return，那么在执行这个return时，就会更新临时栈中的值。同样，在执行完finally之后，就会通知主程序请求返回了，即将临时栈中的值取出来返回。故返回值是2.

总结：==可以把return时看成一个临时储存了  然后去执行finally，finally过程中新的return可以覆盖旧的。==

注意：finally块中的return语句会阻止异常的栈调用传输，使caller认为该方法已经正常返回，让catch中抛的异常失效。

finally块中的throw语句会覆盖try和catch语句中的异常。

#### 8.不会初始化子类的3种情况

1. 调用的是父类的static方法或者字段 

   static是在类构造方法执行初始化的。**会触发子类的加载、父类的初始化，不会导致子类初始化**

  2.调用的是父类的final方法或者字段  **==子类和父类都不会初始化！==**

- final+static修饰：使用ConstantValue属性赋值。

  ConstantValue是字节码指令，final修饰的String和基本数据类型都在编译时就初始了

- 仅仅使用static修饰：在类构造方法中赋值。

![image-20221211223530708](http://bijioss.donggei.top/image-20221211223530708.png)

类构造方法在初始化阶段，final修饰的String和基本数据在这些之前的编译阶段

final修饰，也就是在编译器把结果放入常量池中

  3. 通过数组来引用 

```java
 SuperClass[] superClasses = new SuperClass[10];
```

#### 9.String s = new String(" a ") 到底产生几个对象？

1或者2个

"a"在字符串常量池存在了 就是1个   不存在就是2个

#### 10.String s="java"+"and"+360 到底产生几个对象？;                                        

1个

通过反编译。

​    因为字符串字面量拼接操作是在Java编译器编译期间就执行了，也就是说编译器编译时，直接把"java"、"and"和"360 "这三个字面量进行"+"操作得到一个"javaand360 " 常量，并且直接将这个常量放入字符串池中，这样做实际上是一种优化

这是因为在编译期间，应用了编译器优化中一种被称为**常量折叠**(Constant Folding)的技术，会将**编译期常量**的加减乘除的运算过程在编译过程中折叠。编译器通过语法分析，会将常量表达式计算求值，并用求出的值来替换表达式，而不必等到运行期间再进行运算处理，从而在运行期间节省处理器资源。

完整满足下面的要求 会常量折叠

- 被声明为`final`
- 基本类型或者字符串类型
- 声明时就已经初始化
- 使用常量表达式进行初始化

**String s = "java"**

如果是 String s = "java"，如果原来字符串常量池有“java”，就直接将s指向字符串常量池的“java”，不会创建新的对象。如果原来字符串常量池没有“java”，则会在字符串常量池创建一个新的“java”对象；

#### 11.二分查找的次数

![image-20221213114829016](http://bijioss.donggei.top/image-20221213114829016.png)

奇数取  中间那一个作为中值

偶数个取 中间靠左

然后不断模拟这个算法

![image-20221213160632427](http://bijioss.donggei.top/image-20221213160632427.png)

查找的最多次数：n个元素里最多查找log二N

个元素Log2 128   = 7                                                 

#### 12.equals和hashCode 

java.lang.Object类中有两个非常重要的方法：

```java
public boolean equals(Object obj)
public int hashCode()
```

`equals()`方法是用来判断其他的对象是否和该对象相等

默认在object的实现是``` return (this == obj); ```

但是我们知道，String 、Math、Integer、Double等这些封装类在使用equals()方法时，已经覆盖了object类的equals()方法。从而进行的是内容的比较。

**需要注意的是当`equals()`方法被override时，`hashCode()`也要被override。按照一般`hashCode()`方法的实现来说，相等的对象，它们的hash code一定相等。**



**1、相等（相同）的对象必须具有相等的哈希码（或者散列码）。**

**2、如果两个对象的hashCode相同，它们并不一定相同。**

==hashMap相关的：==

当集合要添加新的元素时，先调用这个元素的hashCode方法，如果这个位置上没有元素，它就可以直接存储在这个位置上，不用再进行任何比较了；如果这个位置上已经有元素了，就调用它的equals方法与新元素进行比较，相同的话就不存了，不相同就散列其它的地址。所以这里存在一个冲突解决的问题。这样一来实际调用equals方法的次数就大大降低了，几乎只需要一两次。  

[参考](https://www.cnblogs.com/Qian123/p/5703507.html)

#### java异常体系

![img](http://bijioss.donggei.top/20150107221255554)

两个子类：Exception（异常）和Error（错误）

Error（错误）：是程序中无法处理的错误，表示运行应用程序中出现了严重的错误。此类错误一般表示代码运行时JVM出现问题

Exception（异常）：程序本身可以捕获并且可以处理的异常。

 	Exception异常又分为两类：运行时异常和编译异常。

​	==运行时异常==(不受检异常)此类异常属于不可查异常，一般是由程序逻辑错误引起的，在程序中**可以选择捕获处理，也可以不处理**。

​	==编译异常==(受检异常)：Exception中除RuntimeException极其子类之外的异常。如果程序中出现此类异常，比如说IOException，**必须对该异常进行处理，否则编译不通过**。在程序中，通常不会自定义该类异常，而是直接使用系统提供的异常类。



除RuntimeException及其子类外，其他的Exception异常都属于可查异常。编译器会检查此类异常，也就是说当编译器检查到应用中的某处可能会此类异常时，将会提示你处理本异常——要么使用try-catch捕获，要么使用throws语句抛出，否则编译不通过。

在开发中用的是？to do

#### 13.jre 判断程序是否执行结束的标准是所有的前台线程执行完毕

后台线程就是守护线程，前台线程就是用户线程。

#### 14. mysql主键为啥自增主键比uuid好

`MySQL`默认的索引结构是`B+Tree`，代表着索引节点的数据是有序的。

不过一张表中只能存在一个聚簇索引，一般都会选用主键作为聚簇索引

聚簇索引中，索引数据和表数据在磁盘中的位置是一起的

如果使用`UUID`作为主键，那么每当插入一条新数据，都有可能破坏原本的树结构。但使用自增`ID`就不会有这个问题，所有新插入的数据都会放到最后。

#### 15.什么是跨域？

1. 协议不同：如 http 和 https；
2. 域名不同
3. 端口不同

> springboot 的解决方法
>
> 1. **使用 @CrossOrigin 注解实现跨域 **修饰类，也可以修饰方法
>
> 2. **通过配置文件实现跨域；**
>
>    添加 @Configuration 注解，实现 WebMvcConfigurer 接口；
>
>    重写 addCorsMappings 方法，设置允许跨域的代码。
>
> 3. **通过 CorsFilter 对象实现跨域；**
>
>    和上一种实现方式类似，**它也可以实现全局跨域**
>
>    @Configuration // 一定不能忽略此注解 
>
>    public class MyCorsFilter {     
>
>    @Bean
>
>    public CorsFilter corsFilter() {    
>
>       // 1.创建 CORS 配置对象     
>
>    ​    CorsConfiguration config = new CorsConfiguration();
>
> 4. **通过 Response 对象实现跨域；**
>
>     response.setHeader("Access-Control-Allow-Origin", "*");        return new HashMap<String, Object>() {{            put("state", 200);            put("data", "success");            put("msg", "");        }};
>
> 5. **通过实现 ResponseBodyAdvice 实现跨域。**(过滤器来实现跨域)
>
> 是对返回结果进行处理  切面编程！
>
> 
>
> 

#### 16.mysql 索引失效的情况

https://juejin.cn/post/7161964571853815822

![图片.png](http://bijioss.donggei.top/10bddf61252f42c7919e2f7be2b410e5~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.image)

### mysql索引

#### 17.为什么要建立索引？

每次查找就是一次磁盘IO 加上局部性原理  由高速缓冲区的缓存行大小决定。

在`InnoDB`引擎中，一次默认会读取`16KB`数据到内存。

假设表中一条数据大小为`512Bit`，一次磁盘`IO`也只能读`32`条。**全表扫描需要很多次的磁盘IO** 

全表扫描由于走的是线性查询，因此数据越多，开销越大，

#### 18.为什么选择B+树

因为B+的非叶子结点相当于 索引表 保存了主键字段值+指针，3层的高度就可以到底千万级的数据量储存（四千多万条数据）。

还在基本是B+树进行了改进 叶子节点之间是互存指针的，所有叶子节点是一个双向链表结构。

#### 19.`InnoDB`和`MyISAM`

- `InnoDB`：因为有聚簇索引存在，所以非聚簇索引在与行数据建立关联时，存放的是主键/聚簇索引的字段值。
- `MyISAM`：由于表数据在单独的`.MYD`文件中，因此可以直接以指针的形式关联起来。

`MyISAM`每个非聚簇索引都能直接获取到行数据的地址，可以直接根据指针获取数据，从整体而言，`MyISAM`检索数据的效率会比`InnoDB`快上不少。



[参考](https://juejin.cn/post/7151275584218202143#heading-5)

### 回收机制

java提供了一个守护线程，即垃圾回收器线程。用来对每一个分配出去的内存空间进行跟踪。当JVM空闲时，自动回收每块可能被回收的内存，GC是完全自动的，不能被强制执行。程序员最多只能用System.gc()来建议执行垃圾回收器回收内存，但是具体的回收时间，是不可知的。当对象的引用变量被赋值为null，可能被当成垃圾。 

1.

```
HashMap实现Map接口，它允许任何类型的键和值对象，并允许将null用作键或值
```

2.

static方法不能被子类覆写，在子类中定义了和父类完全相同的static方法，则父类的static方法被隐藏，Son.staticmethod()或new Son().staticmethod()都是调用的子类的static方法，如果是Father.staticmethod()或者Father f = new Son(); f.staticmethod()调用的都是父类的static方法。 

3.

nginx 502 (Bad Gateway) 状态代码表示服务器在充当网关或代理时，在尝试满足请求时从它访问的入站服务器接收到无效响应。

这里的无效响应，**一般**是指TCP的`RST`报文或四次挥手的`FIN`报文。

发出RST报文，一般有两个常见原因

**第一个**是，**服务端设置的超时时间过短**。

**第二个**原因，也是造成502状态码**最常见的原因**，就是**服务端应用进程崩了（crash）。**

（CPU或内存的监控图是否出现过**断崖式的突然下跌**。）

#### 大流量秒杀解决方案

这篇文章很全面，覆盖面很广 缓存 库存 mq 限流

1. 页面静态化
2. CDN加速
3. 缓存
4. mq异步处理
5. 限流
6. 分布式锁

为什么要分库

数据库实例分配的磁盘容量是固定的，数据库的连接数却是有限的，有效分摊数据库读写压力，也提高了系统容错性。

为什么分表？

唯独数据量大是MySQL无法通过自身优化解决的，慢的根本原因是`InnoDB`存储引擎，聚簇索引结构的 B+tree 层级变高，磁盘IO变多查询性能变慢

分页、排序、联合查询，这些看似普通，开发中使用频率较高的操作，在分库分表后却是让人非常头疼的问题。把分散在不同库中表的数据查询出来，再将所有结果进行汇总合并整理后提供给用户。

#### 缓存

对于一些**变更频率比较高**的数据，采用`集中式缓存`，这样可以确保数据变更之后所有节点都可以实时感知到，确保数据一致；

对于一些**极少变更的数据**（比如一些系统配置项）或者是一些**对短期一致性要求不高**的数据（比如用户昵称、签名等）则采用`本地缓存`，大大减少对远端集中式缓存的网络IO次数。

写的相当不错！！， 从本地缓存 到 远程集中缓存 之后是各种遇到过的缓存，ARP缓存表，CPU中的缓存，MyBatis的多级缓存。

缓存身份鉴权信息，操作习惯，本地设置

旁路型缓存 （最多使用的），穿透型缓存，异步型缓存

---

final类型的变量一定要**初始化**（必须显性赋值，否则编译错误），因为final的变量不可更改。

 final  定义的变量，可以在**不是必须要在定义的同时完成初始化，也可以在构造方法中完成初始化**。

---



----

