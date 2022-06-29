# Spring

**尚硅谷课堂笔记：**

链接: https://pan.baidu.com/s/1BPdI_vDWW2M-1A0okF3Pww 提取码: 2333 
视频的代码笔记和资料。

老师笔记中的某些内容不再赘叙

**此笔记  针对以上笔记为基础** **添加补充**

 主要目的为针对自己复习学习

## 入门案例

```java
public class TestSpring5 {

    @Test
    public void testAdd(){

        //1.加载spring配置文件
        /*
        在src下 ，写下面这个（类路径）
        new FileSystemXmlApplicationContext()不同
        写绝对路径 D:\Mozilla Firefox\aa......
         */
        ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
        //2.获取配置创建的对象
        User user1 = context.getBean("user1", User.class);
        user1.add();
    }
}
```

```java
public class User {
    public void add(){
        System.out.println("add....");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- 配置User对象创建-->
    <bean id="user1" class="com.company.Spring5.User"></bean>
</beans>
```

## 一.IOC

![image-20220308183759166](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220308183759166.png)

调用过程 也就是在其他类里 去new 一个类，

虽然省去了new, 但是要到配置文件读取，配置文件中也得说明

目前还看不出来它的强大

### 1.IOC原理理解

![image-20220308185159566](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220308185159566.png)

问题：

![image-20220308193444373](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220308193444373.png)

dao路径变化了 方法变了 Service 都要变，耦合度高   

这不满足 迪米特原则（最少知道原则）

目的：耦合度减低到最低

解决方法一（最终方法是IOC）：工厂模式

![image-20220308200809070](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220308200809070.png)

这样解耦 还没有到达最低

解决方法二：IOC(xml+反射)

![image-20220308202634815](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220308202634815.png)

这样只需要改xml的class 就可以改动类，类似于定义变量，修改不用一个一个改

### 2.IOC接口

![image-20220309210903236](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220309210903236.png)

使用第二种 因为服务器启动时 可以慢，而且第一种是内部使用的接口

![image-20220309212426404](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220309212426404.png)

### 3.IOC —Bean管理操作

![image-20220309213400767](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220309213400767.png)

![image-20220309220544081](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220309220544081.png)

## 以下是xml Bean管理操作

![image-20220309222246439](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220309222246439.png)

```java
User user1 = context.getBean("user1", User.class);
```

上面的代码 如果没有无参构造会报错

![image-20220309222645785](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220309222645785.png)

DI ：Dependency Injection

DI 是IOC的一种具体实现。需要在创建对象的基础上完成

**第一种注入方式：使用** **set** **方法进行注入**

+ （1）创建类，定义属性和对应的 set 方法
+ （2）在 spring 配置文件配置对象创建，配置属性注入

**第二种注入方式：使用有参数构造进行注入**

+ （1）创建类，定义属性，创建属性对应有参数构造方法

+ （2）在 spring 配置文件中进行配置

第三种注入方式：p名称空间

> 就是 标签的 p属性
>
> 其实底层还是第一种方法



### 4. 注入null和特殊值

![image-20220310113736533](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220310113736533.png)

![image-20220310142707384](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220310142707384.png)

xml语法那部分讲过的，cdata纯文本区域，不需要xml解析，自然不会错误识别了

### 5.注入属性—外部bean

解答：创建实例很费时间，通过配置文件的方式可以在服务器运行过程中提高速度，同时好管理好维护，

```xml
<bean id="UserService" class="com.company.Spring5.service.UserService">
    <property name="userDao" ref="userDaoImpl"></property>
</bean>

<bean id="userDaoImpl" class="com.company.Spring5.dao.UserDaoImpl"></bean>
```

```java
public class UserDaoImpl implements UserDao {

    @Override
    public void update() {
        System.out.println("dao.update");
    }
}
```

```java
public class UserService {
    private UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void add() {
        System.out.println("service.add");
    }
}
```

### 6.内部bean和级联赋值

外部bean可以通过ioc获取，内部的获取不到

```java
<bean id="emp1" class="com.company.Spring5.bean.Emp">
    <property name="empName" value="ZS"></property>
    <property name="gender" value="man"></property>
    <property name="dept">
    <!--就是属性也是个对象，这个对象有自己的小属性-->
        <bean id="dept1" class="com.company.Spring5.bean.Dept">
            <property name="deptName" value="KF"></property>
        </bean>
    </property>
</bean>
```

```java
public class Emp {
    private String empName;
    private String gender;
    //部门
    private Dept dept;

    public void setDept(Dept dept) {
        this.dept = dept;
    }

    public void setEmpName(String empName) {
        this.empName = empName;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }
}
```

以上的效果和上面的外部bean 一样

![image-20220310215954566](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220310215954566.png)

![image-20220310220931712](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220310220931712.png)

方法二需要生成get方法

![image-20220310220123574](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220310220123574.png)

### 7.注入集合类属性

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--集合类型属性-->
    <bean id="stu" class="com.company.Spring5.bean.Stu">
        <!--数组类型注入-->
        <property name="courses" >
            <array>
                <value>java课程</value>
                <value>数据库课程</value>
            </array>
        </property>

        <!--list类型注入-->
        <property name="list">
            <list>
                <value>张三</value>
                <value>法外狂徒</value>
            </list>
        </property>
        <!--map类型注入-->
        <property name="map">
            <map>
                <entry key="skill1" value="java"></entry>
                <entry key="skill2" value="php"></entry>
            </map>
        </property>
        <!--set类型注入-->
        <property name="set">
            <set>
                <value>Mysql</value>
                <value>Redis</value>
            </set>
        </property>
    </bean>
</beans>
```

```java
public class Stu {
    private String[] courses;
    private List<String> list;
    private Map<String, String> map;
    private Set<String> set;

    public void setList(List<String> list) {
        this.list = list;
    }

    public void setMap(Map<String, String> map) {
        this.map = map;
    }

    public void setCourses(String[] courses) {
        this.courses = courses;
    }

    public void setSet(Set<String> set) {
        this.set = set;
    }
}
```

注入值是对象的情况

```java
<!--创建多个 course 对象--> 
<bean id="course1"class="com.atguigu.spring5.collectiontype.Course">
 	<property name="cname" value="Spring5 框架"></property>
</bean>
<bean id="course2" class="com.atguigu.spring5.collectiontype.Course">
 	<property name="cname" value="MyBatis 框架"></property>
</bean>
<!--注入 list 集合类型，值是对象-->
  <property name="courseList">
 	<list>
 		 <ref bean="course1"></ref>
		 <ref bean="course2"></ref>
 </list>
</property>
```

集合注入部分提取

![image-20220311214439416](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220311214439416.png)

好像  一个公共类里面的公共方法

![image-20220311215249831](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220311215249831.png)

### 8.FactoryBean

FactoryBean 和BeanFactory 没有区别

![image-20220312122605435](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220312122605435.png)

工厂bean 就是xml 定义的类型 和 读取xml 后实际创建出来的bean类型不一样

### 9.Bean的作用域

1、在Spring里面，设置创建bean实例是单实例还是多实例。

**默认情况下是单实例**：

```java
@Test
public void test2(){

    ApplicationContext context = new ClassPathXmlApplicationContext("bean6.xml");
    //2.获取配置创建的对象
    Book t1 = context.getBean("t1", Book.class);
    System.out.println(t1);
    Book t11 = context.getBean("t1", Book.class);
    System.out.println(t11);
    //com.company.Spring5.Book@72e5a8e
    //com.company.Spring5.Book@72e5a8e
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="t1" class="com.company.Spring5.Book"></bean>
    
</beans>
```

设置是单实例还是多实例：

![image-20220312135527392](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220312135527392.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="t1" class="com.company.Spring5.Book" scope="prototype"></bean>

</beans>
```

```java
public void test2(){

    ApplicationContext context = new ClassPathXmlApplicationContext("bean6.xml");
    //2.获取配置创建的对象
    Book t1 = context.getBean("t1", Book.class);
    System.out.println(t1);
    Book t11 = context.getBean("t1", Book.class);
    System.out.println(t11);
    //com.company.Spring5.Book@29215f06
    //com.company.Spring5.Book@59505b48
}
```

![image-20220312154433465](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220312154433465.png)

### 10.Bean的生命周期

![image-20220312155833994](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220312155833994.png)

 1，2 步上面已经演示过了

第三步 是自己根据需要配置的，

![image-20220312160903205](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220312160903205.png)

![image-20220312160931519](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220312160931519.png)

![image-20220312161604259](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220312161604259.png)

同样是自己配置的

![image-20220312161746776](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220312161746776.png)

但是 销毁需要手动使用 .close() 才会去调用你配置的销毁方法。

![image-20220312162053311](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220312162053311.png)



**bean的后置处理器，bean生命周期有七步**
> 在实现BeanPostProcessor接口后 还有两步，可以在 **初始化前** 和**初始化后** 调用

1）通过构造器创建 bean 实例（无参数构造）

2）为 bean 的属性设置值和对其他 bean 引用（调用 set 方法）

3）把 bean 实例传递 bean 后置处理器的方法 postProcessBeforeInitialization 

4）调用 bean 的初始化的方法（需要进行配置初始化的方法）把bean实例传递bean后置处理器的方法postProcessAfterInitialization

6）bean 可以使用了（对象获取到了）

7）当容器关闭时候，调用 bean 的销毁的方法（需要进行配置销毁的方法

实际开发的时候可以在项目启动时就获取到Bean对象，比如希望做一个过滤器链，可以存放到Map中在调用的时候直接从Map中获取，就不用在使用getBean方法了

![image-20220312173456032](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220312173456032.png)

配置之后所以的bean 都添加了后置处理器（好像是类似过滤器的那种）

![image-20220312173515506](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220312173515506.png)

### 11.XML管理方式（自动装配）（实际开发直接用注解方式 自动装配）

上面使用   <property name="A" value="B"></property> 或者外部bean的方法属于手动装配

```xml
<!--
  以前的外部bean方式的属性注入
<bean id="emp" class="com.company.Spring5.Autowire.Emp2">
    <property name="Dept2" ref="dept" ></property>
</bean>
<bean id="dept" class="com.company.Spring5.Autowire.Dept2"></bean>
-->
```

byName根据属性名称: 就是根据set方法注入

```xml
 <!--实现自动装配
 bean标签属性autowire,配置自动装配
 autowire属性常用两个值:
 byName根据属性名称注入  注入值bean的id值和类属性名称一样 我类的属性名字是dept2 下面那个标签的id 也是dept2
 byType根据属性类型注入-->
<bean id="emp2" class="com.company.Spring5.Autowire.Emp2" autowire="byName">
</bean>
<bean id="dept2" class="com.company.Spring5.Autowire.Dept2"></bean>
```

![image-20220312185145102](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220312185145102.png)

byType根据属性类型注入：

```xml
<bean id="emp2" class="com.company.Spring5.Autowire.Emp2" autowire="byType">
</bean>
<bean id="dept2" class="com.company.Spring5.Autowire.Dept2"></bean>
```

下面这种就会报错 ，有两个Dept2类型的bean

```xml
<bean id="emp2" class="com.company.Spring5.Autowire.Emp2" autowire="byType">
</bean>
<bean id="dept2" class="com.company.Spring5.Autowire.Dept2"></bean>
<bean id="dept3" class="com.company.Spring5.Autowire.Dept2"></bean>
```

### 12.引入外部属性文件

引入namespace(命名空间）其实就是为了解析那个标签，spring默认引入的只有4个默认的标签

这个是使用schema技术对这个xml文件进行约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"   <!--引入的命名空间-->
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <!--
        直接配置连接池
    <bean id=" dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
        <property name="url" value="jdbc:mysql://localhost:3306/userDb"></property>
        <property name="username" value="root"></property>
        <property name="password" value="123456"></property>
    </bean>
    -->
    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>

    <bean id=" dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${prop.driverClassName}"></property>
        <property name="url" value="${prop.url}"></property>
        <property name="username" value="${prop.username}"></property>
        <property name="password" value="${prop.password}"></property>
    </bean>
</beans>
```

## 以下是注解 Bean管理操作

![image-20220313120753017](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220313120753017.png)

### 13.创建对象

![image-20220313121220504](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220313121220504.png)

4种功能没有区别 都可以创建对象 命名不同 开发更清晰而已（约定大于配置）

  ![image-20220313162723596](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220313162723596.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <!-- 开启组件扫描-->
    <!--多个逗号分开-->
    <context:component-scan base-package="com.company.Spring5.dao,com.company.Spring5.service"></context:component-scan>

    <!--或者写上层路径-->
    <context:component-scan base-package="com.company.Spring5"></context:component-scan>


</beans>
```

```java
//括号中的值可以不写默认首字母小写，就是userService
//等价于bean标签的Id ，上面的其他3种效果也一样
@Component(value = "userService")
public class UserService {
    public void add() {
        System.out.println("service.add");
    }
}
```

```java
public class TestBean {
    @Test
    public void testService() {
    ApplicationContext context =new ClassPathXmlApplicationContext("bean1.xml");
        UserService userService = context.getBean("userService", UserService.class);
        userService.add();

    }

}
```

### 14组件扫描细节

例1：

在设置的包中只扫描Controller注解

![image-20220313165336192](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220313165336192.png)

user-default-filters=“fasle” 时候 不写 includ-filter  就不会扫描包

例2：

在设置的包中除了Controller 其他的都扫描

![image-20220313165621172](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220313165621172.png)

### 15.属性注入

![image-20220314205036572](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220314205036572.png)

autowired根据bytype  quilife根据byname  reources可以根据byname也可以根据bytype

![image-20220314205112044](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220314205112044.png)

+ ![image-20220314210151309](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220314210151309.png)

```java
@Component(value = "userService")//等价于bean标签的Id
public class UserService {
    @Autowired
    private UserDao userDao;
    public void add() {
        System.out.println("service.add");
    }
}
```

```Java
@Repository
public class UserImpl implements UserDao{
    @Override
    public void add() {
        System.out.println("UserDao.add");
    }
}
```

如果多个实现类可能就要指定名称 上面的例子中只有一个实现类

+ ![image-20220314215515525](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220314215515525.png)

```java
@Component(value = "userService")//等价于bean标签的Id
public class UserService {
    @Autowired
    @Qualifier(value = "userImpl1")
   	private UserDao userDao;

    public void add() {
        System.out.println("service.add");
        userDao.add();
    }
}
```

```java
@Repository(value = "userImpl1")
public class UserImpl implements UserDao{

    @Override
    public void add() {
        System.out.println("UserDao.add");
    }
}
```

+ @Resource注解是javax.annotacion包下的，属于java的扩展包，在标准jdk中没有 。需要导入
+ 就是上面两种 这个都行

![image-20220314215705003](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220314215705003.png)

```java
import javax.annotation.Resource;
```

+ ![image-20220314224119841](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220314224119841.png)

  ```Java
  @Value(value = "aaa")
  private String name;
  ```

### 16.纯注解开发

了解，实际中使用Springboot开发

（1）创建配置类 替代xml(中开启组件扫描)

```java
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration //当前类作为配置类
@ComponentScan(basePackages = {"com.company"})
public class SpringConfig {

}
```

（2）测试类

```Java
@Test
public void testSpringConfig() {
    ApplicationContext context =new AnnotationConfigApplicationContext(SpringConfig.class);
    UserService userService = context.getBean("userService", UserService.class);
    userService.add();
}
```

## 二.AOP

![image-20220315103119533](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220315103119533.png)

![image-20220315095850559](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220315095850559.png)

### 1.AOP底层原理

底层使用了   动态代理

> 知识补充:     静态代理，动态代理

代理类和被代理类实现同一个接口

静态代理

+ 代理类 和 被代理类 编译期间已经确定下来啦

  ```java
  interface ClothFactory{
      void produceCloth();
  }
  //代理类
  class ProxyClothFactory implements ClothFactory{
      private ClothFactory factory;//用被代理类进行实例化
  
      public ProxyClothFactory(ClothFactory factory) {
          this.factory = factory;
      }
  
      @Override
      public void produceCloth() {
          System.out.println("代理工厂准备工作代码");
          factory.produceCloth();
          System.out.println("代理工厂后续结尾代码");
      }
  }
  //被代理类对象
  class NikeClothFactory implements ClothFactory{
  
      @Override
      public void produceCloth() {
          System.out.println("Nike工厂生产衣服");
      }
  }
  public class StaticProxyTest {
      public static void main(String[] args) {
          NikeClothFactory nikeCloth = new NikeClothFactory();
          ProxyClothFactory proxyClothFactory = new ProxyClothFactory(nikeCloth);
          proxyClothFactory.produceCloth();
          //代理工厂准备工作代码
          //Nike工厂生产衣服
          //代理工厂后续结尾代码
      }
  }
  ```

  动态代理

  ![image-20220315170604584](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220315170604584.png)

```java
interface  Human{
    String getBelief();
    void eat(String food);
}
//被代理类
class SuperMan implements Human{

    @Override
    public String getBelief() {
        return "I believe I can fly!";
    }

    @Override
    public void eat(String food) {
        System.out.println("我喜欢吃"+food);
    }
}

class proxyFactory{
    // obj 就是创建的被代理的对象
    //调用这个方法解决问题1
    public static Object getProxyInstance(Object obj){
        MyInvocationHandler handler = new MyInvocationHandler();
        handler.bind(obj);
        return Proxy.newProxyInstance(obj.getClass().getClassLoader(), obj.getClass().getInterfaces(),handler);
    }
}
//匿名内部类也可以
class MyInvocationHandler implements InvocationHandler{
    private Object obj;//需要使用被代理类的对象进行赋值

    public void bind(Object obj){
        this.obj =obj;
    }
    //当我们通过代理类的对象，调用方法a时，就会自动的调用如下的方法: invoke()
    //里面已经封装好了 比如调eat方法时 会调用下面这个invoke方法
    //这步很关键，那个参数是接口，接口无法实例化，通过它的实现类来实例化，实现类的对象“替代”了这个参数，多态的思想
    /**
     *
     * @param proxy 就是上面那个getProxyInstance返回的对象
     * @param method 代理类调的哪个方法 就是哪个方法（应该是被代理类的方法）
     * @param args
     * @return
     * @throws Throwable
     */
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //method:即为代理类对象调用的方法，此方法也就作为了被代理类对象要调用的方法
        Object returnValue = method.invoke(obj, args);
        //上述方法的返回值
        return returnValue;
    }
}

public class ProxyTest {
    public static void main(String[] args) {
        SuperMan superMan = new SuperMan();
        Human proxyInstance = (Human) ProxyFactory.getProxyInstance(superMan);
        System.out.println(proxyInstance.getBelief());
        proxyInstance.eat("麻辣烫");
        System.out.println("-------------------------");
		//动态性
        ClothFactory proxyInstance1 = (ClothFactory) ProxyFactory.getProxyInstance(new NikeClothFactory());
        proxyInstance1.produceCloth();

    }
}
```

![image-20220315175717274](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220315175717274.png)

![image](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220315201040382.png)

> 知识补充完毕



![image-20220315111053965](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220315111053965.png)

#### 有接口的情况

![image-20220315201952897](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220315201952897.png)

![image-20220315203922864](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220315203922864.png)

代码参考上面知识补充的即可

你调的是什么方法 就会增强什么方法

#### 没有接口的情况

原始方法，利用子类继承发展功能

![image-20220315202836857](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220315202836857.png)

![image-20220315203120575](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220315203120575.png)

### 2.AOP操作的术语

+ 连接点：可以被增强的方法；
+ 切入点：实际被增强的方法
+ 通知：增强的功能
+ 切面： 是一个动作 

![image-20220315212925448](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220315212925448.png)

### 3.AOP操作—准备

![image-20220315213829611](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220315213829611.png)

![image-20220315213922831](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220315213922831.png)

AOP相关依赖：

![image-20220315214525279](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220315214525279.png)

 maven :  spring-boot-starter-aop

### 4.切入点表达式

![image-20220315220511902](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220315220511902.png)

![image-20220315220801264](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220315220801264.png)

![image-20220315221245882](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220315221245882.png)

[语法结构链接点击](https://blog.csdn.net/qq_32659773/article/details/105679502)

修饰符可以以省略，返回值不能省

*代表 任意返回类型

### 5.注解操作

![image-20220316093625566](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220316093625566.png)

![image-20220316094114709](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220316094114709.png)

![image-20220316164727805](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220316164727805.png)

对应的注解 在配置类上写 @EnableAspectJAutoProxy 

```java
@Configuration //当前类作为配置类
@ComponentScan(basePackages = {"com.company"})
//@EnableAspectJAutoProxy注解等同于在xml中配置aspectj-autoproxy，表示开启spring对注解AOP的支持
//ture表示使用cglib代理 默认false表示用jdk代理 没接口应该使用cglib但是我的类测试false也行 可能高版本自己选择了
@EnableAspectJAutoProxy(proxyTargetClass = true) 
public class SpringConfig {

}
```

![image-20220316094846288](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220316094846288.png)

```java
@Component
@Aspect
@EnableAspectJAutoProxy
public class UserProxy {

    @Before(value = "execution(* com.company.Spring5.AOP.Anno.User.add(..))")
    public void before(){
        System.out.println("Before...");
    }
    // 最终通知 不管有没有异常
    @After("execution(* com.company.Spring5.AOP.Anno.User.add(..))")
    public void after(){
        System.out.println("After...");
    }
    //返回通知（后置通知）  afterreturning只有正常返回才会执行， 发生异常不执行
    @AfterReturning("execution(* com.company.Spring5.AOP.Anno.User.add(..))")
    public void afterReturning(){
        System.out.println("afterReturning...");
    }
    //异常通知 发生异常后执行
    @AfterThrowing("execution(* com.company.Spring5.AOP.Anno.User.add(..))")
    public void afterThrowing(){
        System.out.println("AfterThrowing...");
    }
    //环绕通知
    @Around("execution(* com.company.Spring5.AOP.Anno.User.add(..))")
    public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {

        System.out.println("Around前...");
        proceedingJoinPoint.proceed();
        //发生异常后不执行  Around后
        System.out.println("Around后...");
    }
}
```

```java
//被增强的类
@Component
public class User {
    public void add(){

        System.out.println("add...");
    }

}
```

Around前...
Before...
add...
Around后...
After...
afterReturning...

> 这里方法里面的..代表适配所有方法，实际上参数是用来区分重载方法的

### 6.公共切入点提取

```java
//相同切入点抽取
@Pointcut(value = "execution(* com.company.Spring5.AOP.Anno.User.add(..))")
public  void pointDemo() {

}

@Before(value = "pointDemo")
public void before(){
    System.out.println("Before...");
}
```

### 7.多个增强类对同一个类进行了增强 设置优先级

![image-20220316164349203](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220316164349203.png)

0开始

Before 永远在After前面

### 8.xml操作（了解）

![image-20220317202039473](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220317202039473.png)

## 三.JDBCTemplate

![image-20220317204034218](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220317204034218.png)

![image-20220317205236837](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220317205236837.png)

小型项目还是能用一下 ，小项目比较合适，但是这我直接看老师ppt  就不听了（：

## 四.Spring事务

![image-20220317222609617](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220317222609617.png)

atomicity consistency isolation durability

>+ 原子性
>
>不可分割  要么都成功 要么都失败 
>
>+ 一致性
>
>事务提交前后的数据完整性和状态保持一致
>
>总量守恒
>
>+ 隔离性
>
>多事务操作之间不影响
>
>不考虑隔离性 产生的问题：脏读 幻读 不可重复读
>
>+ 持久性
>
>提交之后 表中的数据真正发生变化

#### 脏读 幻读 不可重复读 的理解

脏读：一个未提交事务读取到另一个未提交事务的数据。

> 假设事务 A 更新了一行数据的值为 A 值，此时事务 B 去查询了一下这行数据的值，看到的值是 A 值
>
> 事务 B 拿着刚才查询到的 A 值做各种业务处理。但是,事务 A 突然回滚了事务，导致它刚才功能的 A 值没了，此时那行数据的值回滚为 NULL 值。然后事务 B 紧接着此时再次查询那行数据的值，看到的居然是 NULL 值
>
> 说明隔离级别“READ-UNCOMMITTED” 但这个隔离级别太低了，一般数据库不会采用

不可重复读: 一个未提交事务读取到另一个提交事务修改数据。

>前提：必须要事务 B 提交之后，它修改的值才能被事务 A 读取到，其实这种情况下，就是我们首先避免了脏读的发生
>
>假设缓存页里一条数据原来的值是 A 值，此时事务 A 开启之后，第一次查询这条数据，读取到的就是 A 值
>
>接着事务 B 更新了那行数据的值为 B 值，同时事务 B 立马提交了，然后事务 A 此时还没提交。所以事务 A 是可以读到B
>
>紧接着事务 C 再次更新数据为 C 值，并且提交事务了，此时事务 A 在还没提交的情况下，第三次查询数据，查到的值为 C 值
>
>

幻读: 一个未提交事务读取到另一个提交事务添加数据。

> 幻读就是你一个事务用一样的 SQL 多次查询，结果每次查询都会发现查到一些之前没看到过的数据
>
> 如 `SELECT * FROM table WHERE id > 10`
>
> 它一开始查询出来了 n 条数据。接着这个时候，别的事务 B往表里插了几条数据，而且事务 B 还提交了，此时多了几行数据。事务 A 此时第二次查询,这次它查询出来了 n+m条数据

​		幻读,不可重复读 都是现象，   脏读是问题

Spring中 解决通过设置事务隔离级别，解决3个问题。在下面（（ioslation:事务隔离级别））

### 1.注解使用流程

![image-20220318113659525](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220318113659525.png)

Spring支持声明式事务管理，一般使用注解

>  **在Spring进行声明式事务管理，底层使用AOP原理**

![image-20220318115338539](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220318115338539.png)

platfromTransactionManager

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                        http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!-- 组件扫描 -->
    <context:component-scan base-package="com.atguigu"></context:component-scan>

    <!-- 数据库连接池 -->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"
          destroy-method="close">
        <property name="url" value="jdbc:mysql:///user_DB?useUnicode=true&amp;characterEncoding=utf8" />
        <property name="username" value="root" />
        <property name="password" value="123456" />
        <property name="driverClassName" value="com.mysql.jdbc.Driver" />
    </bean>

    <!-- JdbcTemplate对象 -->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <!--注入dataSource-->
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <!--创建事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!--注入数据源,对那个数据库进行事务管理-->
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <!--开启事务注解  就是认识事务的注解-->
    <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
</beans>
```

1、在spring配置文件配置事务管理器 

2、在spring配置文件，开启事务注解

3、在service类上面（或者service类里面方法上面）添加事务注解

> 1）@Transactional，这个注解添加到类上面，也可以添加方法上面

> 2）如果把这个注解添加类上面，这个类里面所有的方法都添加事务

> 3）如果把这个注解添加方法上面，为这个方法添加事务

```java
@Service
@Transactional
public class UserService {
```

### 2.@Transactional 的属性

> tip:     idea 中ctrl + p 查看属性

![image-20220320103605424](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220320103605424.png)

+ 1、propagation: 事务传播行为

  ```java
  @Transactional(propagation = Propagation.REQUIRED)
  ```

  举例

  首先这个传播行为是针对于被调用的方法而言的，也就是update方法上设置传播行为

  注意：目前的前提都是update有事务

  ![image-20220320104741579](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220320104741579.png)

  ![image-20220320105138746](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220320105138746.png)

  ![image-20220320104538988](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220320104538988.png)

+ 2、ioslation:事务隔离级别

  ```java
  @Transactional(isolation = Isolation.REPEATABLE_READ) //MySQL默认就是这个
  ```

  ![image-20220320114605305](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220320114605305.png)

+ 3、timeout: 超时时间

  ```java
  @Transactional(timeout = 5) //@Transactional(timeout = -1)
  ```

  ![image-20220320132236518](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220320132236518.png)

+ 4、readOnly: 是否只读

  ![image-20220320132352235](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220320132352235.png)

  

+ 5、rollbackFor: 回滚

  设置出现哪些异常进行事务回滚。

+ 6、noRollbackFor: 不回滚

  设置出现哪些异常不进行事务回滚。

### 3.纯注解开发

```java
//  @Configuration //配置类
//组件扫描  @ComponentScan(basePackages = "com.atguigu")
//  @EnableTransactionManagement 开启事务
public class TxConfig {

    //创建数据库连接池
    //@Component：表示这是一个组件。IOC会托管这个类的对象。@bean：方法返回的对象交给IOC管理。
    //@bean：表示方法返回的是对象，然后对象交给IOC管理。在配置文件中配置对象要写上注解@Bean
    @Bean
    public DruidDataSource getDruidDataSource() {
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql:///user_db");
        dataSource.setUsername("root");
        dataSource.setPassword("root");
        return dataSource;
    }

    //创建JdbcTemplate对象
    @Bean
    public JdbcTemplate getJdbcTemplate(DataSource dataSource) {
        //到ioc容器中根据类型找到dataSource （好比@@Autowired 根据类型做注入）
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        //注入dataSource
        jdbcTemplate.setDataSource(dataSource);
        return jdbcTemplate;
    }

    //创建事务管理器
    @Bean
    public DataSourceTransactionManager getDataSourceTransactionManager(DataSource dataSource) {
        DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
        transactionManager.setDataSource(dataSource);
        return transactionManager;
    }
}
```

springboot 又进行了进一步封装

## 五.Spring5新特性

整个框架基于JKD8 兼容JDK9

Spring5框架自带了通用的日志封装,但是它可以整合其他的第三方日志工具 比如log4j

![image-20220320161305974](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220320161305974.png)

### 1.整合LOg4j2日志框架

第一步：导入jar包

![image-20220320161633928](https://gitee.com/dong2645981073/picture-summary/raw/master//image/image-20220320161633928.png)

第二步：创建log4j.xml 配置文件（名字是固定的log4j2）

内容：比较固定

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--日志级别以及优先级排序: OFF > FATAL > ERROR > WARN > INFO > DEBUG > TRACE > ALL -->
<!--Configuration后面的status用于设置log4j2自身内部的信息输出，可以不设置，当设置成trace时，可以看到log4j2内部各种详细输出-->
<configuration status="INFO">
    <!--先定义所有的appender-->
    <appenders>
        <!--输出日志信息到控制台-->
        <console name="Console" target="SYSTEM_OUT">
            <!--控制日志输出的格式-->
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
    </console>
    </appenders>
    <!--然后定义logger，只有定义了logger并引入的appender，appender才会生效-->
    <!--root：用于指定项目的根日志，如果没有单独指定Logger，则会使用root作为默认的日志输出-->
    <loggers>
        <root level="info">
            <appender-ref ref="Console"/>
        </root>
    </loggers>
</configuration>
```

### 2.Spring5框架核心容器支持@Nullable注解

