# spring boot项目案例学习

小白 ***自顶向下*** 的学习

### 1.框架中各层理解

+ Controller层（响应用户请求）

  控制层，负责前后端交互，接收前端发送的请求，然后调用service层，service层再返回数据给它，它再返回给前端。

+ Service层（接口实现类）

  业务层，负责业务逻辑的处理，调用dao层操作数据库，再对返回的数据进行各种业务上的处理，再返回给控制层。

+ DAO层

  数据持久层，也叫mapper层，主要是操作数据库，完成增删改查功能，把数据返回给service层

+ Model层

  + 数据库实体层，存放实体类，实现get、set方法。属性要和数据库的一样。

  springboot的流程：
  前端发送请求，controller控制层接收请求信息，然后调用service层的接口以及接口实现类，实现类再调用dao层去操作数据库，dao层把数据返回给service层，然后再在service层进行业务处理，再接着把数据返回给controller控制层。

### 2.MVC开发模式

    M：Model（模型） —》 例如：JavaBean
    作用：完成集体的逻辑业务操作，如：查询数据库、封装对象……
    V：View（视图） —》 例如：JSP
    作用：展示数据
    C：Controller（控制器） —》 例如：Servlet
    作用：
    1、获取用户的参数请求
    2、调用模型处理请求
    3、将结果交给视图进行响应、展示

三层架构：

    界面层（Web层）：用户看到的界面，用户可以通过界面上的组件和服务器进行互动
    功能：接收用户的参数，封装数据，调用业务逻辑层完成处理，转发JSP页面完成显示
    包名：cn.公司名.项目名.web
    学习框架：SpringMVC框架
    业务逻辑层（Service层）：处理业务逻辑
    功能：组合DAO层中的简单方法，实现复杂的业务逻辑
    包名：cn.公司名.项目名.service
    学习框架：Spring框架
    数据访问层（DAO层）：操作数据存储文件
    功能：定义了对于数据库最基本的CRUD操作
    包名：cn.公司名.项目名.dao
    学习框架：MyBatis框架



### 3.常用注解

https://www.php.cn/faq/417146.html

### 4.表单提交及处理

在html中，编写元素这个元素必须有name属性，表单提交时必须有name属性才能正常提交。

在form标签中增加action属性，这个属性用来设置表单提交的地址，这个地址用于接收并处理表单提交过来的数据，也是必须的。



### 5.本次结构 流程

![image-20210918094938970](C:\Users\DONG\AppData\Roaming\Typora\typora-user-images\image-20210918094938970.png)

整体的开发流程从 `entity` -> `dao` -> `service` -> `controller`



### 6.thymeleaf

https://www.jianshu.com/p/d1370aeb0881

### Spring MVC 中 Model 的用法

### post get @GetMapping @PostMapping

1. 哪一些情况下，浏览器会发送get请求

 a. 直接在浏览器地址栏输入某个地址

 b. 点击链接

 c. 表单默认的提交方式

  

2. ​	哪一些情况下，浏览器会发送post请求？

 a. 设置表单method = "post"

  

3. get请求的特点

 a. 请求参数会添加到请求资源路劲的后面，只能添加少量参数（因为请求行只有一行，大约只能存放2K左右的数据）（2K左右的数据，看起来也不少。。。）

 b. 请求参数会显示在浏览器地址栏，路由器会记录请求地址

  

4. post请求的特点

 a. 请求参数添加到实体内容里面，可以添加大量的参数（也解释了为什么浏览器地址栏不能发送post请求，在地址栏里我们只能填写URL，并不能进入到Http包的实体当中）

 b. 相对安全，但是，post请求不会对请求参数进行加密处理（可以使用https协议来保证数据安全）。

## 问题总结

### (1).Controller与RestController

```java
@RestController
public class IndexController {
    @RequestMapping("/test")

    public String test(Model module){
        module.addAttribute("msg","hello,springboot");
        return "test";
    }
}
```

![image-20210909175359044](C:\Users\DONG\AppData\Roaming\Typora\typora-user-images\image-20210909175359044.png)

```java
@Controller
public class IndexController {
    @RequestMapping("/test")

    public String test(Model module){
        module.addAttribute("msg","hello,springboot");
        return "test";
    }
}
```

![image-20210909175504418](C:\Users\DONG\AppData\Roaming\Typora\typora-user-images\image-20210909175504418.png)

### (2).导包注意点

使用 @Value("${name}") 时候是
import org.springframework.beans.factory.annotation.Value;
不是import lombok.Value;

### (3).接口作为成员变量 的使用

其中接口作为成员变量，在这个主体类的成员方法中调用了这个接口的抽象方法，会自动找到这个这个接口实现类的覆盖重写的方法，避免多个实现类不同的覆盖重写，<u>所以如果用实现类类实现的话都是直接传参该实现类就行</u>；

下划线的解释：

```java
 public void setSkill(Skill skill) {
        this.skill = skill;
    }

hero.setSkill(new SkillImpl());

public class SkillImpl implements Skill {
    @Override
    public void use() {
        System.out.println("Biu~biu~biu~");
    }
}
```

### (4)Mapper 形式的 开发

 普通的一个单表 User ,需要写UserDao,UserDaoImpl, 与User.xml 三个文件并且这三个文件之间并没有很明显的关联关系， 比较麻烦，

所以MyBatis 推荐使用Mapper 的形式进行开发， 只需要创建 XxxMapper.java **接口**和XxxMapper.xml **配置文件**即可

在利用Mapper 方式进行处理的时候，需要遵守以下的几个原则:

    1.接口名称与类配置文件的名称保持一致。 一个是UserMapper.java, 一个是UserMapper.xml , 最好的命名方式为 类实体+Mapper 的形式。 （非必须）  
    UserDAO和UserDAOMapper.xml 也行
    2.xml 文件的命名空间为 接口的全限定名称, 这样可以方便的找到用的是哪一个接口。
    <mapper namespace="com.yusael.dao.UserDAO">
    3.接口中的方法名称要与 xml配置文件中的id 保持一致。
    如方法名称是 getById, 那么select 语句中的id 就是getById, 方法名称是insertUser, 那么insert 语句中的id 就是insertUser 。 可用于方法重载的情况
    4.参数类型一致. 接口中的方法的参数类型要与 statement 中的parameterType 的类型保持一致。
    5.返回结果一致。 接口中的方法的返回值，要与statement 中的resultType 的类型保持一致，但并非一模一样。 接口的返回类型为List, resultType 的类型只是对象类型即可。

### (5).

总结：

如果用到了Controller（控制层），需要在Controller类上添加@Controller注解；

如果用到了Service（业务层）的话，需要在Service接口类上添加@Service注解；

如果用到了ServiceImpl（业务层实现类），则需要在实现类上添加@Component注解；

如果用到了MapperImpl/DaoImpl（dao层实现类），则需要在实现类上添加@Repository注解，但如果在dao层接口类上添加了@Mapper注解的话，其实可以不需要dao层实现类了。
