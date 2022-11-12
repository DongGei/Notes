

2021版配套笔记及源码：
          配套源码-雷神码云地址：https://gitee.com/leifengyang/springboot2
	  配套笔记-语雀地址：https://yuque.com/atguigu/springboot

评论区网友笔记：

https://blog.csdn.net/u011863024/article/details/113667634

https://blog.csdn.net/u011863024/article/details/113667946

# springboot

直接一个jar包就能跑，不需要tomcat环境

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

需要这个maven插件

![image-20220426230314582](https://s2.loli.net/2022/05/01/NFUkYufaql2hTWe.png)

![image-20220426231134833](https://s2.loli.net/2022/05/01/wryZd5Am9V3qaTg.png)

同一个configuration下，调用方法获取对象相当于从容器中获取bean

### 入口程序的作用，以及项目运行原理

入口程序的作用:配置和启动引导

```@SpringBootApplication```注解

1. 在入口程序的```@SpringBootApplication```注解中包括了3个注解

   ```@SpringBootConfiguration```

   ```@EnableAutoConfiguration```

   ```@ComponentScan```

2. ```
   @SpringBootConfiguration 就是Spring标准@Configuration注解，表明在入口程序中可以使用@Bean注入bean到容器中，与本次实验无关
   ```

3. ``` 
   @EnableAutoConfiguration 就是读取项目中的META-INF/spring.factories文件，然后选择性的交给Spring去加到上下文中，工程中一共有2个META-INF/spring.factories文件，一个在spring-boot包中，一个在spring-boot-autoconfigure包中。它们都是javaConfig配置类，都注入了一些Bean。包括常见组件的默认配置。这里利用了Spring条件注解的特性，通过设定一定的条件来实现不同场景下加载不同的配置，属性源加载器、运行监听器、应用上下文初始器以及应用监听器类型（包括数据库相关的，webservlet相关的等等）及相关实现类（整个J2EE的整体解决方案和自动配置都在springboot-autoConfigure的jar包中）
   ```

4. ```
   @ComponentScan 指定扫描哪些包中的Spring注解, 默认：主程序所在包及其下面的所有子包里面的组件都会被扫描进来。
   也就是package nuc.edu.cn.chapter01 下的所有Spring注解都会扫描,所有HelloController的@Controller
   @ResponseBody，能够被扫描到
   ```

SpringApplication.run分析

在执行SpringApplication的run方法时首先会创建一个SpringApplication类的对象，利用构造方法创建SpringApplication对象时会调用initialize方法。

SpringApplication.run一共做了两件事

    创建SpringApplication对象；在对象初始化时保存事件监听器，容器初始化类以及判断是否为web应用，保存包含main方法的主配置类。
    调用run方法；准备spring的上下文，完成容器的初始化，创建，加载等。会在不同的时机触发监听器的不同事件。


