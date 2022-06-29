# SpringMVC

[视频链接](https://www.bilibili.com/video/BV1Ry4y1574R)

[文档链接](https://mowangblog.github.io/SpringMVC-Demo/#/?id=%e4%b8%80%e3%80%81springmvc%e7%ae%80%e4%bb%8b)

[我的代码](https://gitee.com/dong2645981073/springmvc_demo.git)

[我的demo](https://gitee.com/dong2645981073/springmvc_demo)

iea: IDEA

构建工具：maven 

视图渲染技术：thymeleaf 

核心技术：SpringMVC5.3.1

软件架构：RESTful

## 一介绍

### 1.什么是MVC



![image-20220320184323954](D:\学习\截图\image-20220320184323954.png)

Javabean：项目中所有处理数据的类

### 2.什么是SpringMVC

![image-20220320183423341](D:\学习\截图\image-20220320183423341.png)

是基于servlet的，封装了处理请求等等的过程

#### 三层架构和MVC

![image-20220320183824049](D:\学习\截图\image-20220320183824049.png)

表示层(web层) :包括 servlet 和展示页面

业务逻辑层(service层)、数据访问层(dao层)

![image-20220320184035674](D:\学习\截图\image-20220320184035674.png)

### 3.SpringMVC的特点

![image-20220320185928135](D:\学习\截图\image-20220320185928135.png)

## 二.入门案例

1.创建空maven项目 不选择idea 中的maven模板

复制粘贴 pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.donggei.mvc</groupId>
    <artifactId>springMVC-demo1</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>
    <dependencies>
        <!-- SpringMVC -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.1</version>
        </dependency>
        <!-- 日志 -->
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.2.3</version>
        </dependency>
        <!-- ServletAPI -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
               <!-- Tomcat 里包括了servlet，设置provided后 这个依赖不会打包
                因为这个api本身就由服务器提供，程序也没有提供的必要了。
                -->
        </dependency>
        <!-- Spring5和Thymeleaf整合包 -->
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring5</artifactId>
            <version>3.0.12.RELEASE</version>
        </dependency>
    </dependencies>
</project>
```

provided---只在maven编译和测试阶段生效，不在打包，安装，部署中生效

2.main 下创建webapp目录

3.在webapp下创建web.xml （是web工程的入口和配置文件）

> 以后可以用 注解  配置类 代替web.xml和SpringMCV的配置文件以及Spring的配置文件

![image-20220320222340416](D:\学习\截图\image-20220320222340416.png)

如果选择了webapp模板 其实2，3步就省略了

4.写web.xml

 配置前端控制器 

```xml
<!-- 配置SpringMVC 前端控制器 对浏览器所有请求统一管理-->
<servlet>
    <servlet-name>SpringMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>SpringMVC</servlet-name>
    <url-pattern>/</url-pattern>
    <!-- / 表示的是除了.jsp请求外的所有请求-->
</servlet-mapping>
```

<url-pattern>/</url-pattern>
 **表示的是除了.jsp请求外的所有请求**（一般也不会用到jsp） , 因为jsp 本质是一个servlet, 一般是做特定的处理，不需要被前端控制器统一处理

如果被前端控制器统一处理，请求这个jsp 就找不到 你要访问的jsp啦

> /* 表示的是包括.jsp的所有请求 注意区别

![image-20220321110527339](C:\Users\DONG\AppData\Roaming\Typora\typora-user-images\image-20220321110527339.png)



（一般不使用 使用的是下面这种）

不使用上面这种是因为和maven的打包有关

> 前端控制器是一个Servlet 所以要在web.xml配置，前端控制器有它的初始化初始化参数指向了spingMVC.xml

![image-20220321143352515](D:\学习\截图\image-20220321143352515.png)

![image-20220321142216380](D:\学习\截图\image-20220321142216380.png)

web.xml:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!-- 配置SpringMVC 前端控制器 对浏览器所有请求统一管理-->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--配置SpringMVC配置文件的位置和名称-->
        <init-param>
            <!--上下文配置路径-->
            <param-name>contextConfigLocation</param-name>
            <!--配置文件的位置和名称  classpath 表示类路径 就是java和resources-->
            <param-value>classpath:SpringMVC.xml</param-value>
        </init-param>
        <!--把前端控制器DispatcherServlet
        的初始化提前到启动服务器时，默认是在第一次访问时-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <url-pattern>/</url-pattern>
        <!-- / 表示的是除了.jsp请求外的所有请求-->
    </servlet-mapping>
</web-app>
```

初始化参数，是要在init方法中完成，那么servlet的创建是在第一次访问的时候创建，所以初始化参数会在第一次访问时加载

但是初始化参数的操作如果过多，那么影响的就是客户体验。所以一般都会提前创建servlet。使用<load-on-startup>标签在服务器启动的时候就加载，那么耗时操作就是在服务器开启。

![image-20220321145912467](D:\学习\截图\image-20220321145912467.png)

注解+扫描 使用IOC  	(可以用xml配置文件也可以用配置类,这里是xml配置)

```java
@Controller
public class HelloController {

}
```

springMVC.xml:

```xml
<context:component-scan base-package="com.donggei.mvc.controller"></context:component-scan>
```

![image-20220321150909487](D:\学习\截图\image-20220321150909487.png)

这个图标表示 当前对象已经交给IOC啦

然后配置Thymeleaf视图解析器 它负责页面跳转

```xml
<context:component-scan base-package="com.donggei.mvc.controller"></context:component-scan>

<!-- 配置Thymeleaf视图解析器 -->
<bean id="viewResolver" class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
    <property name="order" value="1"/>
    <property name="characterEncoding" value="UTF-8"/>
    <property name="templateEngine">
        <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
            <property name="templateResolver">
                <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">

                    <!-- 视图前缀 -->
                    <property name="prefix" value="/WEB-INF/templates/"/>

                    <!-- 视图后缀 -->
                    <property name="suffix" value=".html"/>
                    <property name="templateMode" value="HTML5"/>
                    <property name="characterEncoding" value="UTF-8" />
                </bean>
            </property>
        </bean>
    </property>
</bean>
```

在对应目录下写一个index.html

![image-20220321155336305](D:\学习\截图\image-20220321155336305.png)

写HelloController

```java
@Controller
public class HelloController {

    // "/" -->/WEB- INF/templates/index.html
    //请求映射的注解
    @RequestMapping("/")
    public String index() {
    // 返回的是最后要跳转的页面  也就是视图名称（视图名称: 去掉前后缀）
        return "index"; //相当于请求跳转，因为重定向不能访问到web-inf目录

    }
}
```

![image-20220321210211808](D:\学习\截图\image-20220321210211808.png)

浏览器访问=》转到分发器=》读取配置文件=》组件扫描到对应控制器=》RequestMapping映射

## 三.@RequestMapping注解

1.

> 如果在两个类都写了 映射同一个请求地址  那请求这个这个地址会500，也会报错。其实就是两个方法的访问路径相同，会报错
解决的方法就是在类上使用@RequestMappin注解 加了一层路径

属性都是数组

+ ![image-20220322100204524](D:\学习\截图\image-20220322100204524.png)

### value属性

404

### method属性

405

不设置都接收 get/post

![image-20220322162202312](D:\学习\截图\image-20220322162202312.png)

查询用户信息用get，新增用户信息post，修改用put，删除用delete

### params属性

400

与上面两个不一样  parames 设置的条件必须全部满足

```java
params = {"username","password !=123456"}
```

上面两个条件必须同时满足

```
 params ={"username"} //必须携带这些参数
```

```
params ={"!username"} //一定不能有username 这个参数
```

```
params ={"username=admin"} //必须参数username 值必须是admin
```

```
params ={"username!=admin"} //必须参数username 值一定不能是admin
```

### Headers属性

404

与params类型 匹配的是请求报文中的**请求头**

![image-20220322175709103](D:\学习\截图\image-20220322175709103.png)

测试代码

```html
<body>
<h1>首页</h1>
<a th:href="@{/hello/t}"> RequestMapping注解测试</a><br>
<a th:href="@{/test1}"> RequestMapping注解的value属性测试</a><br>
<a th:href="@{/test2}"> RequestMapping注解的value属性测试</a><br>
<form th:action="@{/test1}" method="get">
    <input type="submit" value="method属性get提交表单">
</form>

<form th:action="@{/test1}" method="post">
    <input type="submit" value="method属性post提交表单">
</form>

<a th:href="@{/getMapping}" > GetMapping注解的测试</a><br><!--超链接就是get请求-->

<form th:action="@{/testPut}" method="put">
    <input type="submit" value="测试method属性put或delete提交表单 （不行）">
</form>

<a th:href="@{/testParams(username='admin',password=123456)}">测试params属性</a><br>


</body>
```

```java
@Controller
//@RequestMapping("/hello")
public class RequestMappingController {

    @RequestMapping(
            value = {"/test1","/test2","/test3"},
            method = {RequestMethod.GET,RequestMethod.POST}, //限制了请求方式
            params ={"username"} //必须携带这些参数
            //params ={"!username"} //一定不能有username 这个参数
            //params ={"username=admin"} //必须参数username 值必须是admin
            //params ={"username!=admin"} //必须参数username 值一定不能是admin
    )
    public String success(){
        return "success";
    }

    @GetMapping("/getMapping")
    public String testMapping(){
        return "success";
    }

    @RequestMapping(value = "/testPut" , method = RequestMethod.PUT)
    public String testPut(){
        return "success";
    }
    @RequestMapping(value = "/testParams", params = {"username"})
    public String testParams(){
        return "success";
    }
}
```

```java
@Controller
public class TestController {
    @RequestMapping("/")
    public String index(){
        return "index";
    }
}
```

### SpringMVC支持ant风格路径![image-20220322201259120](C:\Users\DONG\AppData\Roaming\Typora\typora-user-images/image-20220322201259120.png)

?： 必须有且只有一个字符 可以数字 字母 +-冒号 等等（  /和？ 不行）

 空格也能匹配？![image-20220322202448186](D:\学习\截图\image-20220322202448186.png)

** 前后不能有内容 比如下面的是错的（两颗星真的是两颗星，这样就相当于上面的那个）

```java
@RequestMapping(value = "/a**a/testAnt")
```

正确的使用方式：

```java
@RequestMapping(value = "/**/testAnt")
```

![image-20220322211915244](D:\学习\截图\image-20220322211915244.png)

![image-20220322211959302](D:\学习\截图\image-20220322211959302.png)

![image-20220322212051361](D:\学习\截图\image-20220322212051361.png)

### SpringMVC支持路径占位符

在restful风格里原来的请求参数 变成了请求路径 的方式去传输

![image-20220322212547601](D:\学习\截图\image-20220322212547601.png)

感觉是在服务器端规定了每个/分隔符中间表示的value的含义，这样传输的时候必须根据这个格式来，比如第一个为id，第二个为name，反过来都不行，不过的确前端不需要name了

所以说支持路径中的占位符是在服务器端设置一个占位符，请求发送后会将对应的值赋给占位符

```java
										//占位符的id自动赋值给形参id
                                            //如果前端请求/testPath 没有占位直接404
 @RequestMapping(value = "/testPath/{id}")  //testPath/1/ad 这样也是404 多了少了都会报错
public String testPath(@PathVariable("id") Integer id){
    System.out.println(id);                 //但是貌似不写@PathVariable 也行
    return "success";
}

@RequestMapping(value = "/testPath/{id}/{username}")
public String testPath2(@PathVariable("id") Integer id,@PathVariable("username") String username){
    System.out.println(id);
    System.out.println(username);
    return "success";
}
```

## 四.获取请求参数

### 原生servlet获取参数

请求 **被RequestMapping注解匹配，匹配成功由控制器方法来处理请求** ，（前面这些部分是由dispatchServlet来完成的）

```Java
@Controller
public class TestController {
    @RequestMapping("/")
    public String index(){//这个就叫控制器方法
        return "index";
    }
}  
```

dispatchServlet 会根据这个方法的形参 来注入不一样的值

（dispatchServlet会获得请求的内容）

```java
@RequestMapping("/testServletAPI")
public String testServletAPI(HttpServletRequest request){

}
```

使用restful风格就不能使用这种了，在springMVC这些常数其实我们已经获取过了 我们基本不可能用这种方法

###  [通过控制器方法的形参获取请求参数](https://mowangblog.github.io/SpringMVC-Demo/#/?id=_2、通过控制器方法的形参获取请求参数)

```xml
<form th:action="@{/testParam}" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="text" name="password"><br>
    爱好：<input type="checkbox" name="hobby" value="a">a<br>
        <input type="checkbox" name="hobby" value="b">b<br>
        <input type="checkbox" name="hobby" value="c">c<br>
    <input type="submit" value="测试使用控制器形参 同名参数请求">
</form>
```

```java
@RequestMapping("/testParam")
public String testParam(String username,String password,String[] hobby){
    System.out.println(username+ password);
    System.out.println(hobby);//参数位置是：(String hobby)// a,b,c
                                //参数位置是：(String[] hobby)//[Ljava.lang.String;@a9f7fac
    System.out.println(Arrays.toString(hobby));//[a, b, c]
    return "success";
}
```

上面的是表单name属性和控制器请求参数相应的情况

###  @RequestParam

一共3个参数

+ 如果不对应使用 @RequestParam

```java
public String testParam(
        @RequestParam("user_name") String username,
        String password,
        String[] hobby){
```

> 插曲：

> ### 在xml注入属性装配时和@Autowired的区别

> ![image-20220327222634821](D:\学习\截图\image-20220327222634821.png)

> xml注入的时候 能注入就注入 没有的话 就不注入，Autowired required默认值是true 意思就是必须在IOC中有对应的注入 ，没有对应的注入进去 就报错

RequestParam中的这个参数也是相同的意思，默认是参数必须获取到，可以设置它为false,请求表示可以没有当前要的这个参数

![image-20220327223814562](D:\学习\截图\image-20220327223814562.png)

+ 默认是true 如果没有传这个参数，（并且也没有设置默认值defaultValue属性）会直接报错400

+ 设置为false 不是必须传，如果没有传，当前值为null

```java
@RequestMapping("/testParam")
public String testParam(
        @RequestParam(value = "user_name",required = false) String username,
        												String password,
        												String[] hobby){
    System.out.println(username+ password);
    System.out.println(hobby);//参数位置是：(String hobby)// a,b,c
                                //参数位置是：(String[] hobby)//[Ljava.lang.String;@a9f7fac
    System.out.println(Arrays.toString(hobby));//[a, b, c]
    return "success";
}
```

设置如果传参数时的默认值 注意：1.如果是上面那种没传会是默认值，2.如果文本框是空的，传过来的是空字符串""也是使用默认值

```java
@RequestParam(value = "user_name",required = false,defaultValue = "hehe")
```

### @RequestHeader

获取请求头中的值 是必须加这个的，如果是请求参数可以同名不用注解，这个就必须要用注解啦

其中的属性和RequestParam类似

### @CookieValue

```java
@RequestMapping("/testParam")
public String testParam(
        @RequestParam(value = "user_name",required = false,defaultValue = "hehe") String username,
        String password,
        String[] hobby,
        @RequestHeader("Host") String host,
        @CookieValue("JSESSIONID") String jsessionid){
    System.out.println(username+ password);
    System.out.println(hobby);//参数位置是：(String hobby)// a,b,c
                                //参数位置是：(String[] hobby)//[Ljava.lang.String;@a9f7fac
    System.out.println(Arrays.toString(hobby));//[a, b, c]

    System.out.println("Host:"+host);//[a, b, c]

    System.out.println(jsessionid);
    return "success";
}
```

### 通过POJO获取请求参数

**保证表单中的参数名 和 实体类中的属性名一致就是可以使用**

实体类设置有参和无参构造

**要有无参构造因为大部分框架中反射使用的是无参构造**

```java
@RequestMapping("/testpojo")
public String testPOJO(User user){
    System.out.println(user);
    return "success";
}
```

两个都会注入，类型符合的都会

![image-20220328221404433](D:\学习\截图\image-20220328221404433.png)

> 请求乱码也分  get和post
>
> get乱码是tomcat造成的，sever....配置文件中配置
>
> ![image-20220328224510532](D:\学习\截图\image-20220328224510532.png)
>
> post 乱码
>
> ```java
> // 解决post请求中文乱码问题
> // 一定要在获取请求参数之前调用才有效
> req.setCharacterEncoding("UTF-8");
> ```

> 查看原码小技巧 如果参数有filterchain 大概这个方法就是执行过滤的方的的方法（doFilter ）
>
> ![image-20220328231821152](D:\学习\截图\image-20220328231821152.png)

在目前来说在控制器方法解决乱码已经来不及了

启动时间：监听器->过滤器->servlet

servletcontext监听器启动最早 

使用SpringMVC提供的编码过滤器 解决post乱码：

/*所有请求，/是不包括.jsp的请求

```xml
<!--配置springMVC的编码过滤器-->
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <!--查看源码可知设置请求的编码是下面的和-->
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <init-param>
        <!--响应想改编码，encoding和forceResponseEncoding 都要设置-->
        <param-name>forceResponseEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

## 五.域对象共享数据

>  复习：4个域对象
>
> page jsp页面内
>
> request 一次请求内
>
> session 浏览器开启到浏览器关闭(就算服务器关闭了，也会钝化和活化)
>
> application（servletContext） 服务器开启到关闭

原生servlet方法与获取请求参数部分类似。

debug看源码发现原生servlet不会用到Model

### 使用ModelAndView向request 域共享数据(一般使用这个)

+ Model：模型 向域对象共享数据的功能
+ View：视图  我们设置的视图名称经过视图解析器解析跳转到我们页面的过程

> 下面共享数据的方法都会把Model和View封装到ModelAndView对象里

```java
@Controller
public class ScopeController {
    @RequestMapping("/testModelAndView")
    public ModelAndView  testModelAndView(){
        //返回值必须是这个才能被前端控制器解析到
        ModelAndView mav =new ModelAndView();
        //处理模型数据，即向请求域中共享数据
        mav.addObject("testRequestScope","hello.ModelAndView");
        //设置视图名称 相当于之前的return String
        mav.setViewName("success");
        return mav;
    }
}
```

### 使用Model向request 域共享数据

```java
@RequestMapping("/testModel")
public String testModel(Model model) {
    model.addAttribute("testRequestScope", "hello.Model");
    return "success";
}
```

### 使用Map向request 域共享数据

```java
@RequestMapping("/testMap")
public String testMap(Map<String,Object> map) {
    map.put("testRequestScope","hello.map");
    return "success";
}
```

### 使用MapModel向request 域共享数据

```java
@RequestMapping("/testModelMap")
public String testModelMap(ModelMap modelMap) {
    modelMap.addAttribute("testRequestScope","hello.modelMap");
    return "success";
}
```

### Map，ModelMap,ModelAndView的关系

就是一个类实现了这些接口，虽然说的是很多种方法，但是本质上实现类是同一个

使用了同一个对象org.springfarmework.validation.support.DindingAwareModelMap

分析源码和debug发现本质都是最后封装成了一个ModelAndView

### 向session域共享数据

### 向application域共享数据

这两种一般都使用Servlet原生API 在参数位置设置

```java
@RequestMapping("/testSession")
public String testSession(HttpSession session) {
    session.setAttribute("testSessionScope","hello.session");
    return "success";
}
@RequestMapping("/testApplication")
public String testApplication(HttpSession session) {
    ServletContext servletContext = session.getServletContext();
    servletContext.setAttribute("testApplicationScope","hello.Application");
    return "success";
}
```

提醒：session.setAttribute 会先get的 也就是如果没有会创建session.setAttribute 会先getsession的 也就是如果没有会创建

## 六.SpringMVC的视图

### thymeleafView

没有前缀就会被配置的thymeleafView解析器解析

debug 看源码发现：你创建什么样的视图，只和你视图名称有关系

> 使用了thymeleaf后 html只能通过转发去访问

![image-20220331211609611](D:\学习\截图\image-20220331211609611.png)

> html本来是静态页面，通过thymeleaf渲染实际上就是数据的动态变化，静态到动态的改变。以前的html可没办法从服务器端获取数据并且显示，咱们用的EL和JSTL输出数据都是基于jsp页面进行的.
>
> 因为Html页面中有th标签，必须由Thymeleaf在服务器端解析后才能获取结果使用，所以必须要转发.

```java
@RequestMapping("/testThymeleafView")
public String testThymeleafView(){
    return "success";
}
```

### InternalResourceView

内部资源视图(如果使用了Thymelea，这个使用的不多)

只能转发到一个html的请求

> 如果使用的jsp视图解析。配置的是这种的话，就算是没有前缀，使用的也是InternalResourceView。只不过现在配置了thymeleafView。

```java
@RequestMapping("/testForward")
public String testForward(){
    //解析时前缀（forward:）去掉，后面的部分使用请求转发跳转
    return "forward:/testThymeleafView";
}
```

```java
@RequestMapping("/testForward")
public String testForward(){
    //解析时前缀（forward:）去掉，后面的部分使用请求转发跳转
    return "forward:https://www.baidu.com";//错的。No mapping for GET /springMVC_demo3_war_exploded/https://www.baidu.com",
}
```

### 	RedirectView

当一个事务完成后如：登入，添加，删除，一般使用重定向

解析时前缀（redirect:）去掉，后面的部分使用重定向跳转

转发只能用内部资源，无法跨域；重定向可以

### 视图控制器

```java
@RequestMapping("/")
public String index(){
    return "index";
}
```

像上面这种，请求映射对应的控制器方法中，没有其他请求过程的处理，只需要设置一个视图名称时使用视图控制器。

```xml
<!--    只写了这个之后 控制器类中的方法会全部失效！！！-->
<mvc:view-controller path="/" view-name="index"></mvc:view-controller>
<!--    开启MVC注解驱动  加上这个恢复-->
    <mvc:annotation-driven></mvc:annotation-driven>
```

### 使用jsp的视图

springMVC.xml:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <context:component-scan base-package="com.dong.mvc.controller"> </context:component-scan>
<!--    没有前缀的视图名称和forward开头的都会创建InternalResourceView  Redirect开头的会创建Redirect视图-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/templates/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>
</beans>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--配置springMVC的编码过滤器-->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <!--查看源码可知请求和响应都想改编码，encoding和forceResponseEncoding 都要设置-->
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceResponseEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>



    <!-- 配置SpringMVC 前端控制器 对浏览器所有请求统一管理-->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--配置SpringMVC配置文件的位置和名称-->
        <init-param>
            <!--上下文配置路径-->
            <param-name>contextConfigLocation</param-name>
            <!--配置文件的位置和名称  classpath 表示类路径 就是java和resources-->
            <param-value>classpath:SpringMVC.xml</param-value>
        </init-param>
        <!--把前端控制器DispatcherServlet
        的初始化提前到启动服务器时，默认是在第一次访问时-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <url-pattern>/</url-pattern>
        <!-- / 表示的是除了.jsp请求外的所有请求-->
    </servlet-mapping>
</web-app>
```

![image-20220401160848634](D:\学习\截图\image-20220401160848634.png)

## 七.Restful

[以下内容补充](https://mowangblog.github.io/SpringMVC-Demo/#/?id=_1%e3%80%81restful%e7%ae%80%e4%bb%8b)

核心：请求地址URL不变，根据请求方式的不同，对操作资源的方式进行区分

+ get	 查询
+ post   添加
+ delect 删除
+ put    修改

![image-20220401172421992](D:\学习\截图\image-20220401172421992.png)

处理put和delete方法

### HiddenHttpMethodFilte

配置HiddenHttpMethodFilter：

```xml
<!--配置HiddenHttpMethodFilter-->
<filter>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

+ 写post请求的表单
+ 在表单中写_method属性提交

> 查看源码：

> 其实绕这么一大串，就是对你传过来的请求方式名和过滤器里的允许的请求方式名匹配

> 以POST方式发送请求，然后在request域中携带参数_method = “实际的请求方法（DELETE or PUT ）”，包装器类封装新的请求，然后filterChain.doFilter(新包装请求，原响应),把post替换成了其他请求

就是将请求方式作为参数传给后端，过滤器会将参数指定的请求方式作为请求的真正方式

```java
@Controller
public class UserController {
    /**
     * 使用Restful风格实现用户资源的增删改查
     * /user
     */
    @RequestMapping(value = "/user",method = RequestMethod.GET)
    public String getAllUser(){
        System.out.println("查询所有用户信息");
        return "success";
    }
    @RequestMapping(value = "/user/{id}",method = RequestMethod.GET)
    public String getUserById(@PathVariable("id") Integer id){
        System.out.println("查询用户："+id);
        return "success" ;
    }
    @RequestMapping(value = "/user",method = RequestMethod.POST)
    public String addUser(String username,String password){
        System.out.println("添加用户信息："+username+password);
        return "success";
    }

    @RequestMapping(value = "/user",method = RequestMethod.PUT)
    public String modifyUser(String username,String password){
        System.out.println("修改用户信息："+username+password);
        return "success";
    }
}
```

```html
<a th:href="@{/user}">查询所有用户信息</a><br>
<a th:href="@{/user/1}">根据id查询用户信息</a><br><!--超链接的请求方式就是get-->
            <!--表单的method如果不写post或者get.也会默认为get-->
<form th:action="@{/user}" method="post">
    用户名：<input type="text" name="username">
    密码<input type="password" name="password">
    <input type="submit" value="添加">
</form>

<form th:action="@{/user}" method="post">
    <input  type="hidden" name="_method" value="put">
    用户名：<input type="text" name="username">
    密码<input type="password" name="password">
    <input type="submit" value="添加">
</form>
```

delete 如果要使用超链接，要绑定到一个表单，利用表单提交post，隐藏域delete。

> 注意！！！
>
> springMVC的编码过滤器  过滤前不能获取参数，否则失效。
>
> 如果HiddenHttpMethodFilter写在前面，获取了_method的常数，那么编码过滤器就会失效
>
> 解决方法：在xml中springMVC的编码过滤器配置在前面即可
>
> 

### Restful使用案例

[原链接](https://mowangblog.github.io/SpringMVC-Demo/#/?id=%e5%85%ab%e3%80%81restful%e6%a1%88%e4%be%8b)

#### delete请求（包含get请求）
```html
<!--原理-->
<form th:action="@{/employee/1001}" method="post">
    <!-- HiddenHttpMethodFilter要求：必须传输_method请求参数，并且值为最终的请求方式 -->
    <input type="hidden" name="_method" value="delete"/>
    <input type="submit" value="删除第一个">
</form>
```
```java
//@RequestMapping(value = "/employee/{id}", method = RequestMethod.DELETE)
    @RequestMapping(value = "/employee/{id}", method = RequestMethod.DELETE)
    public String deleteEmployee(@PathVariable("id") Integer id) {
        employeeDao.delete(id);
        return "redirect:/employee";//相当于地址栏输入，就是get请求
    }
```

删除功能时 使用了本地的vue要：1.重新打包一下项目  2.因为所有请求都经过了前端控制器，所以要在SpringMVC中配置一下（添加下面的代码）。

原请求前端控制器找不到对应的控制器，开放了默认的servlet就可以了

> 访问静态资源的问题：
>
> 意思就是说，这个Handler接收了前端控制器处理不了的请求，并在内部将请求交给了Tomcat的DefaultServlet去处理！所以真正处理请求的是Tomcat的DefaultServlet

```xml
 	<!--开启MVC注解驱动-->
    <mvc:annotation-driven></mvc:annotation-driven>
	
    <!--开放对静态资源的访问-->
    <!--如果在控制器找不到相当于的请求映射，就交给默认的servlet去找-->
    <mvc:default-servlet-handler/>
```

![image-20220404131518110](D:\学习\截图\image-20220404131518110.png)

绑定了超链接的实现：

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>员工信息</title>
</head>
<body>
<table id="dataTable" border="1" cellspacing="0" cellpadding="0" style="text-align: center">
    <tr>
        <th colspan="5">员工信息</th>
    </tr>
    <tr>
        <th>id</th>
        <th>lastName</th>
        <th>email</th>
        <th>gender</th>
        <th>操作</th>
    </tr>
    <tr th:each="employee:${employeeList}">
        <td th:text="${employee.id}"></td>
        <td th:text="${employee.lastName}"></td>
        <td th:text="${employee.email}"></td>
        <td th:text="${employee.gender}"></td>
        <td>
            <!--            th:href="@{/emloyee/}+${employee.id}" 也可以-->
            <a @click="deleteEmployee" th:href="@{'/employee/'+${employee.id}}">delete</a>
            <a>update</a>
        </td>
    </tr>
</table>
<!-- 作用：通过超链接控制表单的提交，将post请求转换为delete请求 -->
<form id="delete_form" method="post">
    <!-- HiddenHttpMethodFilter要求：必须传输_method请求参数，并且值为最终的请求方式 -->
    <input  type="hidden" name="_method" value="delete"/>
</form>
<script type="text/javascript" th:src="@{/static/js/vue.js}"></script>
<!--<script src="https://cdn.staticfile.org/vue/2.2.2/vue.min.js"></script> 使用这个就不用弄springMVC注解搞了-->
<script type="text/javascript">
    var vue = new Vue({
        el:"#dataTable",
        methods:{
            deleteEmployee:function (event){
                var delete_form = document.getElementById("delete_form");
                //点击事件的href属性 赋值给表单的action
                delete_form.action=event.target.href;
                //提交表单
                delete_form.submit();
                //取消超链接的默认行为（原get提交）
                event.preventDefault();
            }
        }
    });

</script>

<!--原理-->
<form th:action="@{/employee/1001}" method="post">
    <!-- HiddenHttpMethodFilter要求：必须传输_method请求参数，并且值为最终的请求方式 -->
    <input type="hidden" name="_method" value="delete"/>
    <input type="submit" value="删除第一个">
</form>

</body>
</html>
```

```java
@Controller
public class EmployeeController {
    @Autowired
    private EmployeeDao employeeDao;

    @GetMapping("/employee")
    public String getAllEmployee(Model model) {
        Collection<Employee> employeeList = employeeDao.getAll();
        model.addAttribute("employeeList", employeeList);
        return "employee_list";
    }
//@RequestMapping(value = "/employee/{id}", method = RequestMethod.DELETE)
    @RequestMapping(value = "/employee/{id}", method = RequestMethod.DELETE)
    public String deleteEmployee(@PathVariable("id") Integer id) {
        employeeDao.delete(id);
        return "redirect:/employee";//相当于地址栏输入，就是get请求
    }
}
```

#### post请求：

```html
<body>
<form th:action="@{/employee}" method="post">
    lastName:<input type="text" name="lastName"><br>
    email:<input type="text" name="email"><br>
    gender:<input type="radio" name="gender" value="1">male<br>
            <input type="radio" name="gender" value="0">female<br>
    <input type="submit" value="提交添加"><br>
</form>
</body>
```

```java
@PostMapping("/employee")
public String addEmployee(Employee employee){
    employeeDao.save(employee);
    return "redirect:/employee";
}
```

#### put请求（修改）

首先使用了get信息**回显**：

> field单选框回显

```html
<form th:action="@{/employee}" method="post">
    <input type="hidden" name="_method" value="put">
    <input type="hidden" name="id" th:value="${employee.id}">
    lastName:<input type="text" name="lastName" th:value="${employee.lastName}"><br>
    email:<input type="text" name="email" th:value="${employee.email}"><br>
    gender:
            <input type="radio" name="gender" value="1" th:field="${employee.gender}">male<br>
            <input type="radio" name="gender" value="0" th:field="${employee.gender}">female<br>
    <input type="submit" value="提交修改"><br>
</form>
```

```java
@GetMapping("/employee/{id}") //这是回显信息
public String getEmployee(@PathVariable("id") Integer id,Model model) {
    Employee employee = employeeDao.get(id);
    model.addAttribute("employee", employee);
    return "employee_updata";
}
```

```java
@PutMapping("/employee") //表单提交后的修改
public String updateEmployee(Employee employee){
    employeeDao.save(employee);
    return "redirect:/employee";
}
```

## 八.HTTPMessageConverter

报文信息转换器

> 复习：
>
> post请求才有请求体
>
> json:大括号是对象 方括号是数组
>
> json里map转换为json对象，list转换为json数组
>
> json两种存在形式：json对象指的是JavaScript，json字符串是浏览器接受的，或者说java接收的

+ @RequestBody可以获取请求体，需要在控制器方法设置一个形参，使用@RequestBody进行标识，当前请求的请求体就会为当前注解所标识的形参赋值

+ RequestEntity封装请求报文的一种类型，需要在控制器方法的形参中设置该类型的形参，当前请求的请求报文就会赋值给该形参，可以通过getHeaders()获取请求头信息，通过getBody()获取请求体信息

+ @ResponseBody用于标识一个控制器方法，可以将该方法的返回值直接作为响应报文的响应体响应到浏览器

+ ResponseEntity用于控制器方法的返回值类型，该控制器方法的返回值就是响应到浏览器的响应报文

> [@RestController注解](https://mowangblog.github.io/SpringMVC-Demo/#/?id=_6、restcontroller注解)
>
> @RestController注解是springMVC提供的一个复合注解，标识在控制器的类上，就相当于为类添加了@Controller注解，并且为其中的每个方法添加了@ResponseBody注解

```java
@Controller
public class HttpController {

    @RequestMapping("/testRequestBody")
    public String testRequestBody(@RequestBody String requestBody){
        System.out.println("requestBody"+requestBody);
        return "success";
    }
    @RequestMapping("/testRequestEntity")
    public String testRequestBody( RequestEntity<String> entity){
        //RequestEntity是整个请求报文的信息
        System.out.println(entity.getHeaders());
        System.out.println(entity.getBody());
        return "success";
    }
    @RequestMapping("/testResponseBody")
    @ResponseBody
    public String testResponseBody(){
        return "success,ResponseBody";
    }

    @RequestMapping("/testResponseUser")
    @ResponseBody
    public User testResponseUser(){
        //User没有写toString
        //会直接报错，所以要用json
        return new User(1001,"admin","123465",18,"男");
        //然后 加入 jackson——databind依赖 直接可用！！放回JSon
    }
}
```

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.1</version>
</dependency>
```

### 使用ResponseEntity实现文件下载           和上传

```java
@Controller
public class FileUpAndDownController {

    @RequestMapping("/testDown")
    public ResponseEntity<byte[]> testResponseEntity(HttpSession session) throws IOException {
        //获取ServletContext对象
        ServletContext servletContext = session.getServletContext();
        //获取服务器中文件的真实路径
        String realPath = servletContext.getRealPath("/static/img/test.png");
        //创建输入流
        InputStream is = new FileInputStream(realPath);
        //创建字节数组
        byte[] bytes = new byte[is.available()];
        //将流读到字节数组中
        is.read(bytes);
        //创建HttpHeaders对象设置响应头信息
        MultiValueMap<String, String> headers = new HttpHeaders();
        //设置要下载方式以及下载文件的名字
        headers.add("Content-Disposition", "attachment;filename=test.png");
        //设置响应状态码
        HttpStatus statusCode = HttpStatus.OK;
        //创建ResponseEntity对象
        ResponseEntity<byte[]> responseEntity = new ResponseEntity<>(bytes, headers, statusCode);
        //关闭输入流
        is.close();
        return responseEntity;
    }

    @RequestMapping("/testUp")
    public String testUp(MultipartFile photo,HttpSession session) throws IOException {
        //System.out.println(photo.getName());//表单name属性
        //System.out.println(photo.getOriginalFilename());//上传的文件名
        String filename = photo.getOriginalFilename();
        ServletContext servletContext = session.getServletContext();
        String realPath = servletContext.getRealPath("/photo");
        //动态创建文件夹
        File file = new File(realPath);
        //判断文件是否存在
        if (!file.exists()) {
            file.mkdir();
        }
        //上传最终路径 File.separator就是分隔符 / or \ 根据系统来判断分隔符
        String finalFile = realPath+File.separator+filename;
        photo.transferTo(new File(finalFile));
        return "success";
    }
}
```

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>文件上传和下载</title>
</head>
<body>
<a th:href="@{/testDown}">testDown</a>
<!--分成多段以流的形式传输:multipart/form-data-->
<form th:action="@{/testUp}" method="post" enctype="multipart/form-data">
    图片<input type="file" name="photo">
    <input type="submit" value="上传">
</form>
</body>
</html>
```

文件上传需要的：

依赖：

```xml
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.1</version>
</dependency>
```

配置：

```xml
<!--必须通过文件解析器的解析才能将文件转换为MultipartFile对象-->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver"></bean>
```

防止上传文件后被覆盖的方法（上面的方法会覆盖）：

```java
@RequestMapping("/testUp")
public String testUp(MultipartFile photo,HttpSession session) throws IOException {
    //System.out.println(photo.getName());//表单name属性
    //System.out.println(photo.getOriginalFilename());//上传的文件名
    String fileName = photo.getOriginalFilename();

    //防止覆盖
    //从最后一个点开始
    //(包括点)substring包前不包后eg: .jpg
    String suffixName = fileName.substring(fileName.lastIndexOf("."));
    //UUID作为文件名一部分
    String uuid = UUID.randomUUID().toString();
    //拼接 替换原来的文件名
     fileName =uuid+suffixName;
    ServletContext servletContext = session.getServletContext();
    String realPath = servletContext.getRealPath("/photo");
    //动态创建文件夹
    File file = new File(realPath);
    //判断文件是否存在
    if (!file.exists()) {
        file.mkdir();
    }
    //上传最终路径 File.separator就是分隔符 / or \ 根据系统来判断分隔符
    String finalFile = realPath+File.separator+fileName;
    photo.transferTo(new File(finalFile));
    return "success";
}
```

## 九.拦截器

过滤器--->DispatcherServlet---->Controller方法

拦截器对Controller方法 进行拦截

filter是servlet之前拦截，interceptor是触发请求方法拦截

>  拦截器中的3个抽象方法
>
> 控制器方法，也叫处理器方法，Handerle
>
> preHanderle 就是在控制器方法前执行
>
> postHanderle控制器方法后执行
>
> afterComplerion 渲染视图后执行

![image-20220407185535342](D:\学习\截图\image-20220407185535342.png)

### 使用案例

1.

```java
//extends HandlerInterceptorAdapter 也可以 官方说过时了
public class FirstInterceptor implements HandlerInterceptor 
```

2.配置（注册）拦截器

方法一：

```java
<mvc:interceptors>
    <!--当前的拦截器就配置完成了-->
    <bean class="com.dong.mvc.Interceptor.FirstInterceptor"></bean>
</mvc:interceptors>
```

方法二：扫描这个bean,然后在类上加上@Component，然后再写下面的配置。

```java
<mvc:interceptors>
        <!--当前的拦截器就配置完成了-->
        <ref bean="firstInterceptor"></ref>
    </mvc:interceptors>
```

+ 方法一和二会对所有的前端控制器处理的请求进行拦截

方法三：

这个就可以自定义拦截的访问路径

```xml
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <mvc:exclude-mapping path="/"/>
 <!-- 当前拦截所有前端控制器处理的请求 除了主页面/ -->
            <bean class="com.dong.mvc.Interceptor.FirstInterceptor"></bean>
        </mvc:interceptor>
    </mvc:interceptors>
```

> ```xml
>  <mvc:mapping path="/*"/> 这个/* 表示拦截web目录下一层的请求，不会拦截访问下下层的请求，/** 才是所有的请求
> ```
>
> 和dispatcherServlet里的规则不一样

3.

```java
public class FirstInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("FirstInterceptor--->preHandle");
        //HandlerInterceptor.super.preHandle(request, response, handler) 默认的这个应该是true;
        //返回f 是拦截
        return true;
    }
```

### 多个时的执行顺序

两个拦截器时：

```java
<bean class="com.dong.mvc.Interceptor.FirstInterceptor"></bean>
<bean class="com.dong.mvc.Interceptor.SecondInterceptor"></bean>
```

顺序：输出结果：

> FirstInterceptor--->preHandle
> SecondInterceptor--->preHandle
> SecondInterceptor--->postHandle
> FirstInterceptor--->postHandle
> SecondInterceptor--->afterCompletion
> FirstInterceptor--->afterCompletion

+ preHandle:配置的顺序

+ afterCompletion，postHandle：配置的反序

源码分析：

```java
boolean applyPreHandle(HttpServletRequest request, HttpServletResponse response) throws Exception {
   for (int i = 0; i < this.interceptorList.size(); i++) {
      HandlerInterceptor interceptor = this.interceptorList.get(i);
      if (!interceptor.preHandle(request, response, this.handler)) {
         triggerAfterCompletion(request, response, null);
         return false;
      }
      this.interceptorIndex = i;
   }
   return true;
}
```

interceptorList是根据配置文件中的顺序加进去的拦截器

所以PreHandle这里是顺序执行的

> interceptorindex可以理解为最后一个成功的拦截器

```java
void applyPostHandle(HttpServletRequest request, HttpServletResponse response, @Nullable ModelAndView mv)
      throws Exception {

   for (int i = this.interceptorList.size() - 1; i >= 0; i--) {
      HandlerInterceptor interceptor = this.interceptorList.get(i);
      interceptor.postHandle(request, response, this.handler, mv);
   }
}
```

i--  反序执行的postHandle

```java
void triggerAfterCompletion(HttpServletRequest request, HttpServletResponse response, @Nullable Exception ex) {
   for (int i = this.interceptorIndex; i >= 0; i--) {
      HandlerInterceptor interceptor = this.interceptorList.get(i);
      try {
         interceptor.afterCompletion(request, response, this.handler, ex);
      }
      catch (Throwable ex2) {
         logger.error("HandlerInterceptor.afterCompletion threw exception", ex2);
      }
   }
}
```

interceptorIndex 代表的是interceptor的preHandle返回true的最后一个

<u>也就是 已经放行了的拦截器会执行afterCompletion，而applyPostHandle是不管放行了没有都会执行的</u>

> 通过第一段的源码 我们应该发现 一旦有一个拦截后所有的PostHandle都不执行，而afterCompletion会执行前面已经放行了的对应的afterCompletion

## 十.异常处理器

配置文件写法

```xml
<!--配置异常处理-->
    <bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
        <property name="exceptionMappings">
            <props>
<!--                发生的异常和对应的（调整）请求页面-->
                <prop key="java.lang.ArithmeticException">error</prop>
            </props>
        </property>
<!--设置键，存储我们的异常信息 默认存储在我们的请求域中 value就是请求域的键K 当前的异常发生的异常自动设为值V（K-V）-->
        <property name="exceptionAttribute" value="ex">
        </property>
    </bean>
```

注解写法

```java
@ControllerAdvice
public class ExceptionController {
    /**
     *
     * @param ex 设置了一个参数 用来获取当前的异常
     * @param model 使用model向请求域放东西（也可以是其他的参数
     * @return
     */
    @ExceptionHandler({java.lang.ArithmeticException.class,NullPointerException.class})
    public String testException(Exception ex, Model model){
        System.out.println(ex);//java.lang.ArithmeticException: / by zero
        model.addAttribute("ex",ex);
        return "error";
    }

}
```

> 总结就是:设置好对应的异常，一旦发生异常后，就执行异常处理器来跳转和发送参数到页面上

## 十一.注解代替配置文件

> springboot框架里大量使用，我们配置还是以配置文件和扫描加注解为主

### 1.创建初始化类，代替web.xml

![image-20220410211807392](D:\学习\截图\image-20220410211807392.png)

写好依赖 直接在main创建webapp和下面的WEB—INF

新建类继承AbstractAnnotationConfigDispatcherServletInitializer

```java
/**
 * @className: WebInit
 * @description: 代替web.xml web工程的初始化类
 * @author: Dong
 * @date: 2022/4/10
 **/
public class WebInit extends AbstractAnnotationConfigDispatcherServletInitializer {
    /**
     * 指定spring的配置类
     * @return
     */
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }
    /**
     * 指定SpringMVC的配置类
     * @return
     */
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{WebConfig.class};
    }
    /**
     * 指定DispatcherServlet的映射规则，即url-pattern
     * @return
     */
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
    /**
     * 添加过滤器
     * @return
     */
    @Override
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter characterEncodingFilter = new CharacterEncodingFilter();
        characterEncodingFilter.setEncoding("UTF-8");
        characterEncodingFilter.setForceRequestEncoding(true);
        HiddenHttpMethodFilter hiddenHttpMethodFilter = new HiddenHttpMethodFilter();//不设置，默认全拦截
        return new Filter[]{characterEncodingFilter,hiddenHttpMethodFilter};
    }
}
```

```java
@Configuration
public class SpringConfig {
	//ssm整合之后，spring的配置信息写在此类中
}
```

```java
/**
 * @className: webConfig
 * @description: 代替之前的SpringMVC.xml
 * 1.扫描注解 2.视图解析器 3.view-controller 4.default-servlet-handler
 * 5.annotation-driven注解驱动 6.文件上传解析器 7.异常处理  8.拦截器
 * @author: Dong
 * @date: 2022/4/10
 **/
//标识当前类为一个配置类
@Configuration
//1.扫描组件
@ComponentScan("com.dong.mvc")
//5.注解驱动
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {
    //4.使用默认的servlet处理静态资源
    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }

    //6.配置文件上传解析器
    @Bean
    public CommonsMultipartResolver multipartResolver(){
        return new CommonsMultipartResolver();
    }
    //7.异常处理
    @Override
    public void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
        Properties properties = new Properties();
        properties.setProperty("java.lang.ArithmeticException","error");
        SimpleMappingExceptionResolver exceptionResolver = new SimpleMappingExceptionResolver();
        exceptionResolver.setExceptionMappings(properties);
        exceptionResolver.setExceptionAttribute("ex");
        resolvers.add(exceptionResolver);
    }

    //8.配置拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        FirstInterceptor firstInterceptor = new FirstInterceptor();
        registry.addInterceptor(firstInterceptor).addPathPatterns("/**");
        // registry.addInterceptor(firstInterceptor).excludePathPatterns("/**");
    }

    //3.view-controller
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/hello").setViewName("hello");
    }

    //2.视图解析
    //配置生成模板解析器
    @Bean
    public ITemplateResolver templateResolver() {
        WebApplicationContext webApplicationContext = ContextLoader.getCurrentWebApplicationContext();
        // ServletContextTemplateResolver需要一个ServletContext作为构造参数，可通过WebApplicationContext 的方法获得
        ServletContextTemplateResolver templateResolver = new ServletContextTemplateResolver(
                webApplicationContext.getServletContext());
        templateResolver.setPrefix("/WEB-INF/templates/");
        templateResolver.setSuffix(".html");
        templateResolver.setCharacterEncoding("UTF-8");
        templateResolver.setTemplateMode(TemplateMode.HTML);
        return templateResolver;
    }

    //生成模板引擎并为模板引擎注入模板解析器
    @Bean
    public SpringTemplateEngine templateEngine(ITemplateResolver templateResolver) {
        SpringTemplateEngine templateEngine = new SpringTemplateEngine();
        templateEngine.setTemplateResolver(templateResolver);
        return templateEngine;
    }

    //生成视图解析器并未解析器注入模板引擎
    @Bean
    public ViewResolver viewResolver(SpringTemplateEngine templateEngine) {
        ThymeleafViewResolver viewResolver = new ThymeleafViewResolver();
        viewResolver.setCharacterEncoding("UTF-8");
        viewResolver.setTemplateEngine(templateEngine);
        return viewResolver;
    }
}
```

> 感觉比配置文件还麻烦
>
> （尽量掌握webConfig的配置方式，后面不使用springboot里面默认的配置的类，你就需要用这种方式来去写一个类去覆盖默认的配置类）

复习：

```xml
<!--    只写了这个之后 控制器类中的方法会全部失效！！！-->
<mvc:view-controller path="/" view-name="index"></mvc:view-controller>
<!--    开启MVC注解驱动  加上这个恢复-->
    <mvc:annotation-driven></mvc:annotation-driven>
```



springMVC的IOC容器进行初始化的

![image-20220411151334312](D:\学习\截图\image-20220411151334312.png)

监听器创建的容器是父容器，存放dao层、service层等对象。dispatcherServlet创建的是子容器，存放controller层的对象，子容器持有父容器，



就是利用contextloaderListener ，在tomcat工程启动，servletcontext初始化完后，先加载初始化spring的配置。

然后dipatcherServlet初始化时，在这里从servletcontext获取父容器，设置给springmvc的容器

```
一般spring和springMVC的配置各管各的 需要整合（可写一起 但是因为配置加在一起太多 所以不会）
wac.setParent(parent);
//就是把	springMVC容器 设置为了spring容器的子容器 
```

```java
protected void initStrategies(ApplicationContext context) {
   initMultipartResolver(context); //文件上传
   initLocaleResolver(context);
   initThemeResolver(context);
   initHandlerMappings(context); //找请求映射
   initHandlerAdapters(context); //调用对应的控制器方法
   initHandlerExceptionResolvers(context); //异常处理器
   initRequestToViewNameTranslator(context);
   initViewResolvers(context); //视图解析器
   initFlashMapManager(context);
}
```

## DispatcherServlet 初始化过程源码分析

DispatcherServlet 本质上是一个 Servlet，所以天然的遵循 Servlet 的生命周期。所以宏观上是 Servlet 生命周期来进行调度。

> 复习：
>
> ![image-20220411224712471](D:\学习\截图\image-20220411224712471.png)

继承关系：

![image-20220411224357397](D:\学习\截图\image-20220411224357397.png)

第一步Servlet构造器方法 4个抽象类都是空，DispatcherServlet的构造器中写了一个：

```java
public DispatcherServlet() {
   super();
   setDispatchOptionsRequest(true); //大概意思就是开始就是 我们应该向 doService 发送一个 HTTP OPTIONS 请求吗 设置了True
}
```

这个暂不讨论 。

接下来按Servlet生命周期应该是调用init（）方法 也是我们本篇讨论的重点

init()调用了许多方法 调用流程分析 结论 在下面两图中

![image-20220411225531723](D:\学习\截图\image-20220411225531723.png)

![扫描全能王 2022-04-11 22.58](D:\学习\截图\扫描全能王 2022-04-11 22.58.jpg)

> 对上图的说明：圈 1 2 3都在是在 initWebApplicationContext()中执行的

一切的开始是在``` init(ServletConfig config) ``` GenericServlet重写了init(ServletConfig config) 又在init(ServletConfig config)中调用了init()

HttpServletBean 又重新写了init()并调用了initServletBean...............................如上图

最后就是在创建了wac后刷新了wac 又把它共享到了ServletContext

+ 总结就是初始化WebApplicationContext后 以K-V的形式放在了ServletContext

>  扩展：这系列工程比较复杂所以我们之前在web.xml中配置DispatcherServlet设置了（防止第一次访问的时间太长
>
> ```xml
> <!--把前端控制器DispatcherServlet
> 的初始化提前到启动服务器时，默认是在第一次访问时-->
> <load-on-startup>1</load-on-startup>
> ```

> 补充：
>
> ServletContext是一个域对象。ServletContext是在web工程部署启动的时候创建。在web工程停止的时候销毁。Servlet上下文 一个 Web工程只有一个。

```java
//DispatcherServlet中的源码
protected void onRefresh(ApplicationContext context) {
   initStrategies(context);
}
protected void initStrategies(ApplicationContext context) {
   initMultipartResolver(context); //文件上传
   initLocaleResolver(context);
   initThemeResolver(context);
   initHandlerMappings(context); //找请求映射
   initHandlerAdapters(context); //调用对应的控制器方法
   initHandlerExceptionResolvers(context); //异常处理器
   initRequestToViewNameTranslator(context);
   initViewResolvers(context); //视图解析器
   initFlashMapManager(context);
}
```

补充：

ServletContext：Servlet容器（Tomcat、Jboss等）需要给项目初始化一个ServletContext作为公共环境容器存放公共信息，而ServletContext中的信息都是由容器提供的。

WebApplicationContext：是继承于ApplicationContext的一个接口。扩展了ApplicationContext，是专门为Web应用准备的，它允许从相对于Web根目录的路径中装载配置文件完成初始化。
    
ApplicationContext：是 Spring Bean 的上下文环境，它的创建理论上与 ServletContext 无关，但在 SpringMVC 中，这个创建过程自动化了（具体是由 org.springframework.web.servlet.DispatcherServlet 触发的），并自动绑定到 ServletContext 下。



[引用：](https://www.cnblogs.com/jiangtao1218/p/9739540.html)

spring的启动过程：

   - 首先，对于一个web应用，其部署在web容器中，web容器提供其一个全局的上下文环境，这个上下文就是ServletContext，其为后面的spring IoC容器提供宿主环境； 
   - 其次，在web.xml中会提供有contextLoaderListener。在web容器启动时，会触发容器初始化事件，此时contextLoaderListener会监听到这个事件，其contextInitialized方法会被调用，在这个方法中，spring会初始化一个启动上下文，这个上下文被称为根上下文，即WebApplicationContext，这是一个接口类，确切的说，其实际的实现类是XmlWebApplicationContext。这个就是spring的IoC容器，其对应的Bean定义的配置由web.xml中的context-param标签指定。在这个IoC容器初始化完毕后，spring以WebApplicationContext.ROOT*WEB*APPLICATION*CONTEXT*ATTRIBUTE为属性Key，将其存储到ServletContext中，便于获取； 
   - 再次，contextLoaderListener监听器初始化完毕后，开始初始化web.xml中配置的Servlet，这个servlet可以配置多个，以最常见的DispatcherServlet为例，这个servlet实际上是一个标准的前端控制器，用以转发、匹配、处理每个servlet请求。DispatcherServlet上下文在初始化的时候会建立自己的IoC上下文，用以持有spring mvc相关的bean。在建立DispatcherServlet自己的IoC上下文时，会利用WebApplicationContext.ROOT*WEB*APPLICATION*CONTEXT*ATTRIBUTE先从ServletContext中获取之前的根上下文(即WebApplicationContext)作为自己上下文的parent上下文。有了这个parent上下文之后，再初始化自己持有的上下文。这个DispatcherServlet初始化自己上下文的工作在其initStrategies方法中可以看到，大概的工作就是初始化处理器映射、视图解析等。这个servlet自己持有的上下文默认实现类也是mlWebApplicationContext。初始化完毕后，spring以与servlet的名字相关(此处不是简单的以servlet名为Key，而是通过一些转换，具体可自行查看源码)的属性为属性Key，也将其存到ServletContext中，以便后续使用。这样每个servlet就持有自己的上下文，即拥有自己独立的bean空间，同时各个servlet共享相同的bean。

## DispatcherServlet 服务过程



此处的源码设计是模板方法设计模式 

springMVC调用组件处理请求的整个过程

+ 从Servlet接口的public void service(ServletRequest req, ServletResponse res)方法开始看

+ 在HttpServlet中重写了service(ServletRequest req, ServletResponse res)并且调用了重载的service(HttpServletRequest req, HttpServletResponse resp)，在这个重载的方法中获得请求的方法（req.getMethod();），去调用对应的方法（doGet(req, resp);等等）

  ~~此处用哈夫曼树思想，最常见就是get，放前面，后面就越来越少用，程序优化点滴做起~~

+ 接下来FrameworkServlet中重写了service(HttpServletRequest request, HttpServletResponse response)

  也就是说frameworkServlet其实就算对请求方式patch和为null作出了特殊处理processRequest

  如果不是patch和为null调 super.service(request, response); 

+ 最后发现 常见的4个请求方式在do里又调了processRequest 

  那么直接关注FrameworkServlet的processRequest方法就行了

+ 在processRequest 中调了doService,doService在DispatcherServlet中得到了实现，在里面一堆代码里执行了doDispatch(request, response);



然后我们分析一下doDispatch：

>  下面的分析都按代码执行顺序介绍

+ 先处理了 HandlerExecutionChain mappedHandler（处理程序执行链）

![image-20220412132801567](D:\学习\截图\image-20220412132801567.png)

![image-20220412132841659](D:\学习\截图\image-20220412132841659.png)

它里面第一个是匹配的控制器方法，第二个是拦截器 第三个是使用后的拦截器索引



+ 然后是HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());

  处理器适配器

  这里是获得对象没有调用方法，真正调用方法是在ha.handle   （1061 行

  这里获得的其实就是那个控制器对象，具体调用还得看下面那个方法，相当于这里反射已经获得了对象，准备调用控制器方法

+ 接下来执行拦截器的postHandle：

```java
if (!mappedHandler.applyPreHandle(processedRequest, response)) {
   return;
}
```

+ 调用控制器方法的：

  在里面进行了形参的赋值 请求报文，cookie信息，session信息，数据转型(例如：String类型的转Integer，获取数组)等等都在这里处理的

```java
mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
```

+ 调用拦截器PostHandle

```java
mappedHandler.applyPostHandle(processedRequest, response, mv);
```

+ 后续的处理  处理ModelAndView：

```java
processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
```

> processDispatchResult里面的：
>
> 1.如果发生错误后的处理
>
> 2.渲染视图的：	
>
> 比如 模型数据放在请求域，根据视图名称创建对应的视图
>
> ```java
> render(mv, request, response);
> ```
>
> 3.最后 调用拦截器里的AfterCompletion方法
>
> ```java
> mappedHandler.triggerAfterCompletion(request, response, null);
> ```

## SpringMVC执行流程源码分析



  



