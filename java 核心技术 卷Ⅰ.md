# java 核心技术 卷Ⅰ

## 1.Java环境

+ JKD：java Development kit java开发工具包

+ JRE：java Runtime Environment 运行时环境
+ SE 标准版，EE企业版，ME 微型版

1.2-1.4的JKD 也叫SDK 

> 版本号:
>
> 在JAVA SE 9之前内部版本1.6.0就是 JAVA SE 6
>
> ![image-20220330101613010](D:\学习\截图\image-20220330101613010.png)

### 环境安装

省略，根据不同操作系统去安装即可

### JShell

java9引入了另一种使用java的方法—JShell，它提供了 `读取->计算->打印`的使用方法

>  窗口输入JShell即可打开，根据你的输入，计算后打印

![image-20220331141711927](D:\学习\截图\image-20220331141711927.png)

它的作用应该就是你不需要启动一个大的开发环境，不需要你的psvm,但我没有再深入了解了

## 2.程序设计结构

注释不可嵌套（fh）

### 数据类型

8种=4+2+1+1

### int short long byte

```java
public static void main(String[] args) {
    int a =010;
    long b =100000000000l;
    System.out.println(b);
    System.out.println(a);//8
    a=0x10;
    System.out.println(a);//16
    a=0b000010;
    System.out.println(a);//2
    a=0b0000_10;
    System.out.println(a);//2
   
}
```

### float double 

默认double 

```java
public static void main(String[] args) {
        double a =100;
        System.out.println(a/0);//输出：Infinity 正无穷
         a =0;
        System.out.println(a/0);//输出：NaN 不是一个数字

        if (Double.isNaN(a/0)){
            System.out.println("Y"); //可以通过这个方法判断，不可以使用 X==Double.NAN
        }
        int b=100;
        System.out.println(b / 0);//报错: / by zero
    -----------------------------------------
   	float a = 100.11f;//6
    double c = 100.11;
    //误差：
    System.out.println(2.0-1.1);0.8999999999999999
}
```

### char

一般不使用char, jkd5.0后 char类型实际是使用了UTF-16，并不是”经典Unicode“

### boolean

与整数型之间不能转化

### 其他：

> java 10后通过判断确定的类型 可以使用var

+ 变量声明尽量靠近第一次使用的位置

+ 常量使用全**大写和_**命名, final 只能被赋一次值，希望在多个方法中使用的使用 static final 如果是public 那么其他类也可以使用

+ 枚举类型  不可以在方法这写（起码JKD 11不行）

  ```java
  public class t25_2 {
      enum Size { SMAll,MEDIUM}
      public static void main(String[] args) {
          Size a =Size.MEDIUM;
          System.out.println(a);//MEDIUM
      }
  }
  ```

+ mian方法加strictfp ，main方法中使用严格的浮点运算（处理不同机器可能导致不一样的问题，可忽略）

  

### 字符串

是一个Unicode字符序列。java提供了求子串，拼接，repeat方法等等

是不可变长的，字符串都会放在一个公共的存储池，java的字符串不是c中的字符序列，类似char*指针

```java
public  static void main(String[] args) {
    String a ="hello";
    String b ="hello";
    String c = new String("hello");
    if (a==b){
        System.out.println("y"); //y
    }
    if (a==c){
        System.out.println("y"); //无
    }
}
```

> 设计者认为共享带来的高效比提前子串，拼接子串的低效更合适

空串长度是0 内容是空 null字符串表示没有对象和变量关联

+ length方法返回的是UTF-16的代码单元，一个代码单元可能是多个

```java
public static void main(String[] args) {
    //hi𝕆中的𝕆实际上是是一个辅助字符，它实际上占用了两个char来保存，这个字符串中总共为4个char（就是4个代码单元），3个代码点。

    String a = "\uD835\uDD46";//上下a其实是同一个
    a="𝕆";
    //这个是代码单元数量
    System.out.println(a.length());//2

    int count = a.codePointCount(0, a.length());
    //这个是码点数量
    System.out.println(count);//1
}
```

对码点的处理：

```java
String b = "hi𝕆";
char c = b.charAt(2);
System.out.println(c);//?
//得到第2个码点的
int i = b.offsetByCodePoints(0, 2);
int j = b.codePointAt(i);
System.out.println(j);//120134

int[] ints = b.codePoints().toArray(); //字符串到码点数组
String s = new String(ints, 0,ints.length); //码点数组到字符串
System.out.println(s);//hi𝕆
```

