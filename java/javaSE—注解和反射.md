# javaSE—注解和反射



## 一.注解
### 1.什么是注解
Annotation是从JDK5.0开始引入的概念

eg：@override  重写的注解
+ 1.不是程序本身 给程序作出解释（）
+ 2.**可以被其他程序读取**
+ 非必须
+ 3.格式：@注释+注释名，还可以加一些参数值
+ 4.有检查和约束的作用
+ 可以放在方法或者类等等上面

.....注释是给人看的,注解是给人和机器看的

### 2.内置注解
@override 指示方法声明旨在覆盖超类型中的方法声明。
@Deprecated 已过时 不推荐使用的代码
@SuppressWarnings  镇压警告（可以放入参数）
### 3.元注解
作用：负责注解其他注解 
4个标准的meta-annotation类型：

+ @Target 	被描述的注解可以用在什么地方
```java
@Target(value =ElementType.METHOD)  说明作用域在方法上 
@Target(ElementType.FIELD) 作用域在属性上（成员变量）
@Target(value ={ElementType.METHOD,ElementType.TYPE})  说明作用域在方法和类上 

```
+ @Retention	表示需要在什么级别保存注释信息（一般都写RUNTIME）
  表示我们的注释在什么地方还有效
```java
@Retention(value =RetentionPolicy.RUNTIME)
```
+ @Documented  是否我们的注解生成在JAVAdoc中
+ @Inherited    表示子类能继承父类该注解
### 4.自定义注解
+ 使用@interface自定义注解时，自动继承注解接口
+ 写在注解里的参数格式：参数类型 + 参数名（）；
+ default 设默认值
注解显示赋值，如果没有默认值，我们必须赋值
 ```java
@interface MyAnnotion{
string name() default ""
int id() default -1   //一般负一代表不存在
}

@interface MyAnnotion2{
string value()
}

@MyAnnotion2("abc")  //只有一个且名字是value时 这样 等价于 (value="'abc'")
public viod test(){
}
 ```
## 二.反射
### 1.概念
Java（静态语言）被视为准动态语言的关键，反射机制允许程序在**执行期**借助于Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性及方法。   private 里的也可以操作

加载完类之后，在堆内存的方法区中就产生了一个Class类型的对象(一个类只有一个Class对象) ，这个对象就包含了完整的类的结构信息。我们可以通过这个对象看到类的结构。这个对象就像一面镜子， 透过这个镜子看到类的结构，所以，我们形象的称之为:反射

正常方式: 引入需要的"包类"名称一> 通过new实例化—>取得实例化对象
反射方式:实例化对象一> getClass方法—>得到完整的“包类”名称

### 2.class类的创建方式
```java
public class test03 {
    public static void main(String[] args) throws ClassNotFoundException {
        Person person =new Student();
        System.out.println("这个人是："+person.name);
        //通过对象获得
        Class c1 = person.getClass();
        //forname获得
        Class c2 = Class.forName("lesson05.Student");
        //通过类名.class获得
        Class c3 = Student.class;
        
        //获得父类类型
        Class c5 = c1.getSuperclass();
    }

    
    
}

class Person{
   public String name;

    public Person() {
    }

    public Person(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                '}';
    }
}

class Student extends Person{
    public Student() {
       this.name="学生";
    }
}

class Teacher extends Person{
    public Teacher() {
        this.name="老师";
    }
}
```
### 3.那些类型有Class对象
```java
public class test04 {
    public static void main(String[] args) {
        Class objectClass = Object.class;//类
        Class comparableClass = Comparable.class;//接口
        Class aClass = String[].class;//一维数组
        Class aClass1 = int[][].class; //二维数组
        Class overrideClass = Override.class;//注解
        Class elementTypeClass = ElementType.class; //枚举
        Class  integerClass = Integer.class;//基本数据类型
        Class voidClass = void.class;//void
        Class classClass = Class.class;//Class
    }
}
```
### 4.分析类初始化
​       (1.) 类的主动引用(一定会发生类的初始化)
+ 当虚拟机启动，先初始化main方法所在的类

+ new 一个类的对象

+ 调用类的静态成员(除了final常量)和静态方法

+ 使用java.lang.reflect包的方法对类进行反射调用

+ 当初始化一个类,如果其父类没有被初始化，则先会初始化它的父类
  (2.)类的被动引用(不会发生类的初始化)

+ 当访问一个静态域时，只有真正声明这个域的类才会被初始化。如:当通过子类引用父类的静态变量,不会导致子类初始化

+ 通过数组定义类引用，不会触发此类的初始化

+ 引用常量（final）不会触发此类的初始化(常量在链接阶段就存入调用类的常量池中了)

  ```java
  public class test05 {
      public static void main(String[] args) throws ClassNotFoundException {
          System.out.println("main调用");
  
          //(1.) 类的主动引用(一定会发生类的初始化)
          ///zi son = new zi();
  
          //System.out.println(Fu.b);
  
  
        // Class.forName("lesson05.zi");
          //Class.forName("lesson05.Fu");
  
          //(2.)类的被动引用(不会发生类的初始化)
  
          //System.out.println(zi.b);
  
          //zi[] zis = new zi[5];
  
  
          //System.out.println(zi.m);
  
  
      }
  
  }
  class Fu {
      static int b=2;
      static {
          System.out.println("父类被调用");
      }
  }
  class zi extends Fu{
     final static int m=0;
      static {
          System.out.println("子类被调用");
      }
  }
  ```

补充：

1.static方法

　　static方法一般称作静态方法，由于静态方法不依赖于任何对象就可以进行访问，因此对于静态方法来说，是没有this的，因为它不依附于任何对象，既然都没有对象，就谈不上this了。并且由于这个特性，在静态方法中不能访问类的非静态成员变量和非静态成员方法，因为非静态成员方法/变量都是必须依赖具体的对象才能够被调用。

　　但是要注意的是，虽然在静态方法中不能访问非静态成员方法和非静态成员变量，但是在非静态成员方法中是可以访问静态成员方法/变量的。

2.static变量

　　static变量也称作静态变量，静态变量和非静态变量的区别是：静态变量被所有的对象所共享，在内存中只有一个副本，它当且仅当在类初次加载时会被初始化。而非静态变量是对象所拥有的，在创建对象的时候被初始化，存在多个副本，各个对象拥有的副本互不影响。

　　static成员变量的初始化顺序按照定义的顺序进行初始化。

3.static代码块

　　static关键字还有一个比较关键的作用就是 用来形成静态代码块以优化程序性能。static块可以置于类中的任何地方，类中可以有多个static块。在类初次被加载的时候，会按照static块的顺序来执行每个static块，并且只会执行一次。

4.static作用于成员变量用来表示只保存一份副本，final的作用是用来保证变量不可变

### 5.类加载内存分析

![20211201120121](https://gitee.com/dong2645981073/picture-summary/raw/master//image/20211201120121.png)

测试代码：

![20211201125030](https://gitee.com/dong2645981073/picture-summary/raw/master//image/20211201125030.png)

运行结果：

![20211201125036](https://gitee.com/dong2645981073/picture-summary/raw/master//image/20211201125036.png)

分析：![20211201120508](https://gitee.com/dong2645981073/picture-summary/raw/master//image/20211201120508.png)

![20211201124815](https://gitee.com/dong2645981073/picture-summary/raw/master//image/20211201124815.png)

**注意：静态代码块和 静态变量执行的先后顺序取决于代码编写的顺序** 在<clinit>时完成



![image-20211201130716568](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211201130716568.png)

![image-20211201132233921](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211201132233921.png)



### 6.类加载器

+ 引导类加载器 

```
引导类加载器:用C++编写的， 是JVM自带的类
加载器，负责Java平台核心库（rt.jar），用来装载核心类
库。该加载器无法直接获取
```



+ 扩展类加载器

```
扩展类加载器:负责jre/ib/ext目录下的jar包或-
D java.ext.dirs指定目录下的jar包装入工作库
```



+ 系统类加载器

```
系统类加载器:负责java - classpath或-D
java.class. path所指的目录下的类与jar包装入工
作库，是最常用的加载器
```



![](https://gitee.com/dong2645981073/picture-summary/raw/master//image/Screenshot_2021-08-07-09-06-43-674_tv.danmaku.bil.jpg)



![ ](https://gitee.com/dong2645981073/picture-summary/raw/master//image/Screenshot_2021-08-07-09-08-42-040_tv.danmaku.bil.jpg)

![](https://gitee.com/dong2645981073/picture-summary/raw/master//image/Screenshot_2021-08-07-09-10-42-338_tv.danmaku.bil.jpg)





### 7.获取运行时类的完整结构

```java
import java.lang.reflect.Constructor;
import java.lang.reflect.Method;

//获取类的信息
public class test06 {
    public static void main(String[] args) throws NoSuchFieldException, NoSuchMethodException {
        USer uSer = new USer();
        Class c1 = uSer.getClass();
        //获取类的名字
        System.out.println(c1.getName());

//        //获得类的属性
//        System.out.println("===========================================");
//        Field[] fields = c1.getFields(); //只能找到public属性
//        for (Field field : fields) {
//            System.out.println(field);
//        }
//        System.out.println("===========================================");
//        Field[] fields1 = c1.getDeclaredFields();//找到全部属性
//        for (Field field : fields1) {
//            System.out.println(field);

        //获得指定属性
            System.out.println("===========================================");
            System.out.println(c1.getDeclaredField("name"));

        //获得类的方法
            System.out.println("===========================================");
            Method[] methods = c1.getMethods();  //本类及父类所有方法
        
            for (Method method : methods) {
                System.out.println("正常的："+method);
            }
            System.out.println("===========================================");
            Method[] declaredMethods = c1.getDeclaredMethods();//本类的所有方法
        
            for (Method declaredMethod : declaredMethods) {
                System.out.println("getDeclaredMethods的："+declaredMethod);
        }
        //获得指定方法
        //可能会发生重载 所以要传入参数 丢进去一个类型就可以了
            System.out.println("===========================================");
            Method getName = c1.getMethod("getName", null);
            Method setName = c1.getMethod("setName", String.class);
            System.out.println(getName);
            System.out.println(setName);
        
        //构造方法
            System.out.println("===========================================");
            Constructor[] constructors = c1.getConstructors();//本类public构造方法
        
            for (Constructor constructor : constructors) {
                System.out.println(constructor);
            }

            System.out.println("===========================================");
            Constructor[] constructors1 = c1.getDeclaredConstructors(); //本类全部构造方法
        
            for (Constructor constructor1 : constructors1) {
                System.out.println(constructor1);
            }

        //获得指定构造器
            System.out.println("===========================================");
            System.out.println(c1.getConstructor(String.class, int.class, int.class));


    }
}
```
### 7. 动态创建对象执行方法
其中没有参数的给null
```java
//通过反射，动态创建对象
public class test07 {
    public static void main(String[] args) throws Exception {
        //获得class对象
        Class c1 = Class.forName("lesson05.USer");

        //构造对象
        USer user = (USer) c1.newInstance();//本质调用了无参构造器
        System.out.println(user);

        //通过构造器创建对象
        Constructor constructor = c1.getConstructor(String.class, int.class, int.class);
        USer user2  = (USer) constructor.newInstance("dongGEi", 01, 18);
        System.out.println(user2);

        //通过反射调用普通方法
        USer user3 = (USer) c1.newInstance();
        //通过反射获得方法
        Method setName = c1.getMethod("setName", String.class);
        setName.invoke(user3,"abcd"); //invoke 激活，给user3这个对象 参数是abcd
        System.out.println(user3);
        //通过反射操作属性
        USer user4 = (USer) c1.newInstance();
        Field name = c1.getDeclaredField("name");
            //不能直接操作私有属性，需要先关闭安全检测，属性和方法都是setAccessible(true)
        name.setAccessible(true);//设置权限
        name.set(user4,"abcd222");
        System.out.println(user4);

    }
}
```

### 8.性能对比
+ 普通方法 new User 出来的进行set get等等操作
+ 反射方法 uesr.getClass 然后获得方法 然后invoke 
+ 关闭权限检测的反射调用 加一个setAccessible（true）

速度  普通>关闭权限检测的反射>反射方法

需要反射时可以关闭权限检测 提高速度

### 9.反射获取泛型信息

![image-20211201151837962](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211201151837962.png)

解释上面代码：test01 是一个参数为Map和List的方法，

反射拿到这个方法 然后获得参数列表

 遍历参数列表 然后参数类型是参数化类型（加<>的；泛型 ）的强转成参数化类型

再遍历 拿到真真实参数  

（口语化 有点绕=.=）



### 10. 反射获取注解信息

##### ORM对象关系映射

◆类和表结构对应

◆属性和字段对应

◆对象和记录对应

要求:利用注解和反射完成类和表结构的映射关系

![image-20211202082253348](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211202082253348.png)

先看下面 再看psvm

```java
package test;

import java.lang.annotation.*;
import java.lang.reflect.Field;

public class Test {

    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException {
        Class<?> c1 = Class.forName("test.Student");
        //通过反射获得注解
        Annotation[] annotations = c1.getAnnotations();
        for (Annotation annotation : annotations) {
            System.out.println(annotation);
        }
        //获得注解value的值
        //获得指定注解 传进注解的class
        TableDong tableDong = c1.getAnnotation(TableDong.class);
        String value = tableDong.value();
        System.out.println(value);

        //获得指定属性 （成员变量）
        Field f = c1.getDeclaredField("name");
        //获得属性上的 指定注解 取出注解的值
        FieldDong fieldDong = f.getAnnotation(FieldDong.class);
        System.out.println(fieldDong.columName());
        System.out.println(fieldDong.length());
        System.out.println(fieldDong.type());
        System.out.println("---------------");

        f = c1.getDeclaredField("id");
        //获得指定注解
        fieldDong = f.getAnnotation(FieldDong.class);
        System.out.println(fieldDong.columName());
        System.out.println(fieldDong.length());
        System.out.println(fieldDong.type());
    }
}
@TableDong("db_student")
class Student{
    @FieldDong(columName = "db_id",type = "varchar",length = 10)
    private String id;
    @FieldDong(columName = "db_age",type = "int",length = 3)
    private int age;
    @FieldDong(columName = "db_name",type = "varchar",length = 10)
    private String name;

    public Student() {
    }

    public Student(String id, int age, String name) {
        this.id = id;
        this.age = age;
        this.name = name;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Student{" +
                "id='" + id + '\'' +
                ", age=" + age +
                ", name='" + name + '\'' +
                '}';
    }
}

//类名的注解
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface TableDong{
    String value();
}

//属性的注解
@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@interface FieldDong{
    String columName(); //列名（不是学生名）
    String type();
    int length();
}
```

![image-20211202084133761](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211202084133761.png)



（本篇 JVM深入部分 还是不太懂 以后再深入了解吧- . -）

