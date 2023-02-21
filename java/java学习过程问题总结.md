# 问题总结

#### 1.增强型for循环

增强型for循环只能用来取值，却不能用来修改数组里的值

#### 2.Scanner.nextLine()读取回车问题

解决如何处理多出来的换行符
方法一

既然多出了一个换行符，那我们可以在读取字符串之前将换行符读掉：



```Java
Scanner sc=new Scanner(System.in);
int n=sc.nextInt();
sc.nextLine();//将换行符读掉
String s=sc.nextLine();
```

方法二

重新创建Scanner对象：

```java
Scanner sc1=new Scanner(System.in);
int n=sc1.nextInt();
Scanner sc2=new Scanner(System.in);
String s=sc2.nextLine();
```

方法三 

有时候可以改成next

```Java
num1 = sc.nextDouble();
op = sc.next().charAt(0);
num2 = sc.nextDouble();
```

#### 3.构造方法

构造方法的名字必须与定义他的类名完全相同，没有返回类型，甚至连void也没有

#### 4.重载

必须改变参数列表(参数个数或类型不一样)
可以改变.返回类型.访问修饰符.
#### 5.重写
参数列表不能改
返回值不改 或者是被重写方法的子类
#### 6.instanceof 
其中 obj 为一个对象，Class 表示一个类或者一个接口，当 obj 为 Class 的对象，或者是其直接或间接子类 结果result 都返回 true，否则返回false

#### 7.子类父类同名成员变量

父类和子类的变量是同时存在的，即使是同名。
 子类中看到的是子类的变量，父类中看到的是父类中的变量。
 它们互相隐藏，而同名的方法则是实实在在的覆盖（重写）

**java中，向上造型呈现的多态性仅仅针对成员函数，成员属性不具有多态性**

```java
Animals d1 = new Dog();
d1.enjoy();//方法是执行dog重写后的
System.out.println(d1.age);//成员变量是调的Animal的
Dog s = (Dogg) d1;
System.out.println(s.age);//成员变量是调的Dog的
	
```

**总之：成员变量 编译看左边 运行看左边**

​			**成员方法 编译看左边 运行看右边**

#### 8.接口

接口中的方法会被隐式的指定为 **public abstract**

接口中的变量会被隐式的指定为 **public static final** 

接口中所有的方法必须是抽象方法

+ ​	使用Java 8,接口可以有静态方法

 			接口中可以使用 default 关键字修饰的非抽象方法

#### 9.





## 小注意点

1. java中 1 与true 不兼容

2. 同时实现**接口**和继承**重写抽象类**，不冲突

   ![image-20211113214055766](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20211113214055766.png)

3. 使用**throw**抛出异常对象后，程序将终止执行

4. **try—catch**
   第二个catch块永远无法到达。如果不同的类有继承扩展关系，则子类的catch块必须放在前面。

5. 抽象类实现接口 也需要重写 但是可以只实现单个方法https://www.cnblogs.com/java-demo/p/9095038.html

6. 如果一个类的所有构造方法的访问权限都是private的，意味着这个类不能有子类，理由是：一个类的private方法不能在其他类中被使用，但子类的构造方法中一定会调用父类的某个构造方法）。
7. 如果在子类的构造方法中，没有显示地写出super关键字来调用父类的某个构造方法，那么编译器默认地有：super();调用父类的无参数的构造方法，如果父类没有这样的构造方法，代码将出现编译错误。

8. 静态方法可以重载，不可以被重写。

```
	其实，在Java中，如果父类中含有一个静态方法，且在子类中也含有一个返回类型、方法名、参数列表均与之相同的静态方法，那么该子类实际上只是将父类中的该同名方法进行了隐藏，而非重写。换句话说，父类和子类中含有的其实是两个没有关系的方法，它们的行为也并不具有多态性。正如同《Java编程思想》中所说：“一旦你了解了多态机制，可能就会认为所有事物都可以多态地发生。然而，只有普通方法的调用可以是多态的。如果你直接访问某个域，（不管是否是静态static），这个访问就将在编译期间进行解析。”这也很好地理解了，为什么在Java中，static方法和final方法（private方法属于final方法）是前期绑定，而其他所有的方法都是后期绑定了。
```
