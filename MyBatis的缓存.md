# MyBatis的缓存

## MyBatis的一级缓存

一级缓存默认开启，缓存一般是针对{增删改查}的查



```java
public interface CacheMapper {
    Emp getEmpByEid(@Param("eid") Integer eid);
}
```

```xml
<mapper namespace="com.dong.mybatis.mapper.CacheMapper">
    <!--Emp getEmpByEid(@Param("eid") Integer eid);-->
    <select id="getEmpByEid" resultType="Emp">
        select * from t_emp where eid=#{eid}
    </select>

</mapper>
```

```java
@Test
    public void testCache(){
        SqlSession sqlSession = SqlSessionUtils.getSqlSession();
        CacheMapper mapper = sqlSession.getMapper(CacheMapper.class);
        Emp empByEid = mapper.getEmpByEid(1);
        System.out.println(empByEid);
    }
```
输出：

![](D:\学习\截图\image-20220430220734584.png)

```java
public void testCache(){
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    CacheMapper mapper = sqlSession.getMapper(CacheMapper.class);
    Emp empByEid = mapper.getEmpByEid(1);
    System.out.println(empByEid);
    Emp empByEid2 = mapper.getEmpByEid(1);
    System.out.println(empByEid2);
}
```

更改test后的输出：

只执行了一次sql,第二次从缓存中取出

![image-20220430220935594](D:\学习\截图\image-20220430220935594.png)

**一级缓存：从同一个sqlSession查询同一条Sql语句，不会查第二次**

验证：

+ 同一个sqlSession 换一个mapper

```java
@Test
public void testCache(){
    SqlSession sqlSession = SqlSessionUtils.getSqlSession();
    CacheMapper mapper1 = sqlSession.getMapper(CacheMapper.class);
    CacheMapper mapper2 = sqlSession.getMapper(CacheMapper.class);
    Emp empByEid = mapper1.getEmpByEid(1);
    System.out.println(empByEid);
    Emp empByEid2 = mapper2.getEmpByEid(1);
    System.out.println(empByEid2);
}
//DEBUG 04-30 22:19:10,111 ==>  Preparing: select * from t_emp where eid=? (BaseJdbcLogger.java:137) 
//DEBUG 04-30 22:19:10,144 ==> Parameters: 1(Integer) (BaseJdbcLogger.java:137) 
//DEBUG 04-30 22:19:10,171 <==      Total: 1 (BaseJdbcLogger.java:137) 
//Emp{eid=1, empName='张三', age=20, sex='男', email='26@qq.com'}
//Emp{eid=1, empName='张三', age=20, sex='男', email='26@qq.com'}
```

+ 不同sqlsession

```java
@Test
public void testCache2(){
    SqlSession sqlSession1 = SqlSessionUtils.getSqlSession();
    SqlSession sqlSession2 = SqlSessionUtils.getSqlSession();
    CacheMapper mapper1 = sqlSession1.getMapper(CacheMapper.class);
    CacheMapper mapper2 = sqlSession2.getMapper(CacheMapper.class);
    Emp empByEid = mapper1.getEmpByEid(1);
    System.out.println(empByEid);
    Emp empByEid2 = mapper2.getEmpByEid(1);
    System.out.println(empByEid2);
}
//DEBUG 04-30 22:30:08,856 ==>  Preparing: select * from t_emp where eid=? (BaseJdbcLogger.java:137)
//DEBUG 04-30 22:30:08,887 ==> Parameters: 1(Integer) (BaseJdbcLogger.java:137)
//DEBUG 04-30 22:30:08,905 <==      Total: 1 (BaseJdbcLogger.java:137)
//Emp{eid=1, empName='张三', age=20, sex='男', email='26@qq.com'}
//DEBUG 04-30 22:30:08,930 ==>  Preparing: select * from t_emp where eid=? (BaseJdbcLogger.java:137)
//DEBUG 04-30 22:30:08,931 ==> Parameters: 1(Integer) (BaseJdbcLogger.java:137)
//DEBUG 04-30 22:30:08,932 <==      Total: 1 (BaseJdbcLogger.java:137)
//Emp{eid=1, empName='张三', age=20, sex='男', email='26@qq.com'}
```

> 然后
>
>  很容易得出 ：使一级缓存失效的四种情况
>
> 1. 不同的SqlSession对应不同的一级缓存  
>
> 2. 同一个SqlSession但是查询条件不同
>
> 3. 同一个SqlSession两次查询期间执行了任何一次增删改操作
>
>    ​				增删改，执行无论成败，缓存都会失效
>
> 4. 同一个SqlSession两次查询期间手动清空了缓存
>
>    ```java
>    sqlSession1.clearCache();
>    ```

## MyBatis的二级缓存

> ​	在基本mybatis里我们使用了SqlSessionFactoryBuilder来创建SqlSessionFactory，SqlSessionFactory是一个单例。SqlSessionFactory可以获取多个sqlSession。
>
> 而二级缓存是SqlSessionFactory级别的

二级缓存**开启的条件**：

```
1. 在核心配置文件中，设置全局配置属性cacheEnabled="true"，默认为true，不需要设置
2. 在映射文件(接口对应的xml配置)中设置标签<cache />
3. 二级缓存必须在SqlSession关闭或提交之后有效
4. 查询的数据所转换的实体类类型必须实现序列化的接口
```

使二级缓存失效的情况：

```
两次查询之间执行了任意的增删改，会使一级和二级缓存同时失效
```

测试代码：

```java
@Test
public void testCache3erJi() throws IOException {
    InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
    //true开始的是开启是数据库事务自动提交 不是sqlSession的提交
    SqlSession sqlSession1 = sqlSessionFactory.openSession(true);
    CacheMapper mapper1 = sqlSession1.getMapper(CacheMapper.class);
    System.out.println(mapper1.getEmpByEid(1));
    //提交和关闭 二级缓存才有效
    sqlSession1.close();

    SqlSession sqlSession2 = sqlSessionFactory.openSession(true);
    CacheMapper mapper2 = sqlSession2.getMapper(CacheMapper.class);
    System.out.println(mapper2.getEmpByEid(1));
}
```

```java
public interface CacheMapper {
    Emp getEmpByEid(@Param("eid") Integer eid);
}
```

```xml
<!--开启二级缓存-->
<cache/>

<!--Emp getEmpByEid(@Param("eid") Integer eid);-->
<select id="getEmpByEid" resultType="Emp">
    select * from t_emp where eid=#{eid}
</select>

</mapper>
```

### 二级缓存相关配置

- 在mapper配置文件中添加的cache标签可以设置一些属性

- eviction属性：缓存回收策略  
	- LRU（Least Recently Used） – 最近最少使用的：移除最长时间不被使用的对象。  
	- FIFO（First in First out） – 先进先出：按对象进入缓存的顺序来移除它们。  
	- SOFT – 软引用：移除基于垃圾回收器状态和软引用规则的对象。  
	- WEAK – 弱引用：更积极地移除基于垃圾收集器状态和弱引用规则的对象。
	- 默认的是 LRU
	
- flushInterval属性：刷新间隔，单位毫秒
	- 默认情况是不设置，也就是没有刷新间隔，缓存仅仅调用语句（增删改）时刷新。（也就是失效，就是指清空缓存）
	
- size属性：引用数目，正整数
	- 代表缓存最多可以存储多少个对象，太大容易导致内存溢出
	
- readOnly属性：只读，true/false
	- true：只读缓存；会给所有调用者返回缓存对象的相同实例。因此这些对象不能被修改。这提供了很重要的性能优势。 （ 就是返回真实的指针喽）
	- false：读写缓存；会返回缓存对象的拷贝（通过序列化）。这会慢一些，但是安全，因此默认是false
	
	> 这里的readOnly=true返回缓存对象本身。并且readOnly=true并不是说你只能读但不能修改这个对象，而是一种建议，建议你不要修改这个对象，因为修改了这个对象缓存也就变了，所以不安全
## MyBatis缓存查询的顺序
- 先查询二级缓存，因为二级缓存中可能会有其他程序已经查出来的数据，可以拿来直接使用  
- 如果二级缓存没有命中，再查询一级缓存  
- 如果一级缓存也没有命中，则查询数据库  
- SqlSession关闭之后，一级缓存中的数据会写入二级缓存

## 整合第三方缓存——EHCache

使用第三方缓存来代替本身的二级缓存，一级缓存无法代替

  ### 添加依赖
```xml
<!-- Mybatis EHCache整合包 -->
<dependency>
	<groupId>org.mybatis.caches</groupId>
	<artifactId>mybatis-ehcache</artifactId>
	<version>1.2.1</version>
</dependency>
<!-- slf4j日志门面的一个具体实现 -->
<dependency>
	<groupId>ch.qos.logback</groupId>
	<artifactId>logback-classic</artifactId>
	<version>1.2.3</version>
</dependency>
```

### 创建EHCache的配置文件ehcache.xml
- 名字必须叫`ehcache.xml`
```xml
<?xml version="1.0" encoding="utf-8" ?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="../config/ehcache.xsd">
    <!-- 磁盘保存路径 -->
    CPU上的缓存是用来临时存储cpu和内存交换的数据的，硬盘上的缓存用来临时存储硬盘和内存交换的数据
    <diskStore path="D:\atguigu\ehcache"/>
    <defaultCache
            maxElementsInMemory="1000"
            maxElementsOnDisk="10000000"
            eternal="false"
            overflowToDisk="true"
            timeToIdleSeconds="120"
            timeToLiveSeconds="120"
            diskExpiryThreadIntervalSeconds="120"
            memoryStoreEvictionPolicy="LRU">
    </defaultCache>
</ehcache>
```
### 设置二级缓存的类型
- 在xxxMapper.xml文件中设置二级缓存类型
```xml
<cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
```
### 加入logback日志

推荐使用log4j2+slf4j处理日志

- 存在SLF4J时，作为简易日志的log4j将失效，此时我们需要借助SLF4J的具体实现logback来打印日志。创建logback的配置文件`logback.xml`，名字固定，不可改变

改一下logger name="自己的mapper包路径"

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true">
    <!-- 指定日志输出的位置 -->
    <appender name="STDOUT"
              class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <!-- 日志输出的格式 -->
            <!-- 按照顺序分别是：时间、日志级别、线程名称、打印日志的类、日志主体内容、换行 -->
            <pattern>[%d{HH:mm:ss.SSS}] [%-5level] [%thread] [%logger] [%msg]%n</pattern>
        </encoder>
    </appender>
    <!-- 设置全局日志级别。日志级别按顺序分别是：DEBUG、INFO、WARN、ERROR -->
    <!-- 指定任何一个日志级别都只打印当前级别和后面级别的日志。 -->
    <root level="DEBUG">
        <!-- 指定打印日志的appender，这里通过“STDOUT”引用了前面配置的appender -->
        <appender-ref ref="STDOUT" />
    </root>
    <!-- 根据特殊需求指定局部日志级别 -->
    <logger name="com.atguigu.crowd.mapper" level="DEBUG"/>
</configuration>
```















## sqlsession的生命周期以及相关问题

网上整理 便于理解mybatis缓存中的一些概念

（一）、SqlSessionFactoryBuilder
　　SqlSessionFactoryBuilder的作用就是在于创建SqlSessionFactory，创建成功后，SqlSessionFactoryBuilder就失去了作用，
所以它只能存在于创建SqlSessionFactory的方法中，而不要让其长期存在。

（二）、SqlSessionFactory
　　SqlSessionFactory可以被认为是一个数据库连接池，它的作用是创建SqlSession接口对象。因为MyBatis的本质就是Java对数据库的操作，
所以SqlSessionFactory的生命周期在于于整个MyBatis的应用之中，所以一旦创建了SqlSessionFactory的生命周期就等同于MyBatis的应用周期。

由于SqlSessionFactory是一个对数据库的连接池，所以它占据着数据库的连接资源。如果创建多个SqlSessionFactory，那么就存在多个数据库连接池，
这样不利于对数据资源的控制，也会导致连接资源被消耗光，出现系统宕机等情况，所以尽量避免发生这样的情况。因此在一般的应用中我们往往希望SqlSessionfactory作为一个单例，让它在应用中不共享。

当然：

SqlSeesionFactory默认就是一个单例的，用于创建SqlSession，SqlSessionFactory一旦创建成功，就不会再创建新的SqlSessionFactory

如果和Spring整合后，由spring管理SqlSeesionFactory，spring容器中的bean也是默认开启单例模式的

（三）、SqlSession
　　如果说SqlSessionFactory相当于数据库连接池，那么SqlSession就相当于一个数据库连接（Connection对象），
你可以在一个事务里面执行多条SQL,然后通过它的commit、rollback等方法，提交或者回滚事务。所以它应该存活在一个业务请求中，
处理完整个请求后，应该关闭这条连接，让它归还给SqlSessionFactory，否则数据库资源就很快被消耗精光，系统应付瘫痪，所以用try…catch…fanally语句来保证其正确关闭。

>SqlSession:是一个面向一个面向用户操作数据库的接口，通过SqlSeesionFactory获取SqlSession，每次数据操作都需要创建新的SqlSession，
>
>SqlSession不是线程安全，最佳应用场合是在方法体内，在方法中定义一个SqlSession局部变量，
>
>注意:如果定义为成员变量的话，那么就有可以多个线程获取同一个SqlSession，那就有可能引发线程安全的问题

（四）、Mapper
　　Mapper是一个接口，它由SqlSession所创建，所以它的最大生命周期至多和SqlSession保持一致，尽管它很好用，
但是由于SqlSession关闭，它的数据库连接资源也会消失，所以它的生命周期应该小于等于SqlSession的生命周期。
Mapper代表是一个请求中的业务处理，所以它应该在一个请求中，一旦处理完了相关的业务，就应该废弃它。

[spring中的mybatis的sqlSession是如何做到线程隔离的](https://www.cnblogs.com/yougewe/p/10072740.html)

