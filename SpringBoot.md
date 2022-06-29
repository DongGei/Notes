

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

