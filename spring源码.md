# spring源码

### 铺垫

#### 1.xml信息封装

>  我们可以预测的大致顺序
>  ![image-20221105121213650](http://bijioss.donggei.top/image-20221105121213650.png)
>
>  xml里的bean 封装成**对象定义信息**更容易进一步的 创建 注入 
>
>  ![image-20221105121713338](http://bijioss.donggei.top/image-20221105121713338.png)
>
>  BeanDefinition的属性：可以看出是对xml里信息的封装
>
>  ![ ](http://bijioss.donggei.top/image-20221105122410535.png)
>
>  ```java
>  public interface BeanDefinitionReader 定义规范
>  ```
>
>  
>
>  ![image-20221105125946001](http://bijioss.donggei.top/image-20221105125946001.png)

#### 2.使用反射

> 为什么不用new创建对象？而用反射
>
> 使用反射可以获取到类的所有属性 所有方法 所有的构造器 所有注解 等
>
> 灵活 动态创建

#### 3 .bean的作用范围

> bean的作用范围
>
>  默认单例
>
> ![image-20221105123426868](http://bijioss.donggei.top/image-20221105123426868.png)
>
> 

>  反射的3种方法
>
> ![image-20221105124428339](http://bijioss.donggei.top/image-20221105124428339.png)

#### 4.bpp和bfpp的区别 和使用

> bpp和bfpp的区别  
>
> ![image-20221105130409827](http://bijioss.donggei.top/image-20221105130409827.png)

> bfpp的使用范围
>
> ![image-20221105130553189](http://bijioss.donggei.top/image-20221105130553189.png)
>
> 自己写一个bean工厂后置处理
>
> ![image-20221105130739218](http://bijioss.donggei.top/image-20221105130739218.png)
>
> 使用beanFactory的方法可以自定义处理   拿到BeanDefinition 进行修改
>
> 使用自己的这个 bean工厂后置处理：
>
> 配置文件配置
>
> ![image-20221105130956967](http://bijioss.donggei.top/image-20221105130956967.png)
>
> 之后就可使用了 在启动项目后就会生效

#### 5.实例化和初始化

> 创建对象  --> 实例化和初始化
>
> ![image-20221105155415897](http://bijioss.donggei.top/image-20221105155415897.png)

#### 6 .实例化的流程，Aware接口

> 实例化的流程
>
> ![image-20221105160932950](http://bijioss.donggei.top/image-20221105160932950.png)
>
> 1. init-method在哪？ 如果不定义的话就是什么都不干 
>
> 使用案例：
>
> ![image-20221105155541894](http://bijioss.donggei.top/image-20221105155541894.png)
>
> ![image-20221105155632361](http://bijioss.donggei.top/image-20221105155632361.png)
>
> 2. Aware到底是什么接口？
>
> 例子
>
> 实现了BeanNameAware接口，必须实现setBeanName（String  beanName）方法,然后自己定义一个成员变量在setBeanName方法中{this.beanName=beanName}; 这样就拿到了beanName。
>
> 例子二
>
> 实现了ApplicationContextAware接口,定义一个成员变量在setApplicationContext方法中{this.applicationContext=applicationContext}; 
>
> 先拿到哪个 就对实现哪个接口
>
> ![image-20221105171521979](http://bijioss.donggei.top/image-20221105171521979.png)
>
> 相关：如果在实现了某个aware 对调用对应的方法
>
> ![image-20221105171337175](http://bijioss.donggei.top/image-20221105171337175.png)

![image-20221105155838030](http://bijioss.donggei.top/image-20221105155838030.png)

#### 7. BeanPostProcess.before和.after

>  BeanPostProcess.before和BeanPostProcess.after的作用是什么？
>
> aop的实现是实现了PostProcess接口 然后写了after方法 在里面使用代理模式 创建了代理对象 

#### 8.监听器 广播器

>  在上面的**实例化到完整对象**各个阶段之间
>
> ![image-20221105182720874](http://bijioss.donggei.top/image-20221105182720874.png)
>
> ![image-20221106183920803](http://bijioss.donggei.top/image-20221106183920803.png)



![image-20221105163351678](http://bijioss.donggei.top/image-20221105163351678.png)



#### 9.FactoryBean和BeanFactory的区别

> 了解 FactoryBean
>
> FactoryBean和BeanFactory的区别
>
> ![image-20221105194925212](http://bijioss.donggei.top/image-20221105194925212.png)
>
> BeanFactory是个Factory，也就是IOC容器或对象工厂，FactoryBean是个Bean。在Spring中，**所有的Bean都是由BeanFactory(也就是IOC容器)来进行管理的**。但对FactoryBean而言，**这个Bean不是简单的Bean，而是一个能生产或者修饰对象生成的工厂Bean,它的实现与设计模式中的工厂模式和修饰器模式类似** spring整体流程
>
> ```java
> public class A implements FactoryBean<B> {
> 	
>     @Override
>     public B getObject() throws Exception {
>         return new B();
>     }
> 
>     @Override
>     public Class<?> getObjectType() {
>         return A.class;
>     }
> 
>     @Override
>     public boolean isSingleton() {
>         return false;
>     }
> 
> 
> }
>   public static void main(String[] args) throws Exception {
>         ClassPathXmlApplicationContext applicationContext = new ClassPathXmlApplicationContext("bean1.xml");
> 
>         A aBean1 =(A) applicationContext.getBean("&a");
>         System.out.println("aBean = " + aBean1);
>       	//加&符号 拿到的工厂本身 所以拿到的是A类的对象
> 
>         A aBean2 =(A) applicationContext.getBean("a");
>         System.out.println("aBean = " + aBean2);
>         //class spring_ioc.test.B cannot be cast to class spring_ioc.test.A  因为默认调用的getObject 所以拿到了B类的对象
> 
>         B bObject = aBean1.getObject();
>         System.out.println("aOobject = " + bObject);
>      
> 
>     }
> 
>     <bean id="a" class="spring_ioc.test.A">
> 
>     </bean>
> ```
>

### 

> 了解：
>
> ![image-20221105171842526](http://bijioss.donggei.top/image-20221105171842526.png)
>
> 比如bfpp不是一个方法 其实也是一个容器对象 是一个bean

### 整体代码简要分析

spring整体流程**大纲图**：

![image-20221105200524886](http://bijioss.donggei.top/image-20221105200524886.png)

spring整体流程大纲 **代码调用顺序简要分析**

从new ClassPathXmlApplication()开始 到底干了点啥？

![110617464178_0扫描全能王 2022-11-06 17.35_1](http://bijioss.donggei.top/110617464178_0%E6%89%AB%E6%8F%8F%E5%85%A8%E8%83%BD%E7%8E%8B%202022-11-06%2017.35_1.jpg)

下面简单概括一下上面的初始化步骤

1. prepareRefresh ： 初始化前的准备工作，例如对系统属性或者环境变量进行准备及验证。在某些情况下项目的使用需要读取某些系统变量，那么在启动时候，就可以通过准备函数来进行参数的校验。
2. obtainFreshBeanFactory ：初始化BeanFactory，并进行XML 文件读取（如果需要的话）。 这一步之后ApplicationContext就具有BeanFactory 所提供的功能，也就是可以进行Bean的提取等基础操作了。
3. prepareBeanFactory ：对BeanFactory 进行各种功能填充。
4. postProcessBeanFactory ： 对 BeanFactory 做额外处理。默认没有实现
5. invokeBeanFactoryPostProcessors ： 激活各种BeanFactory 处理器(调用了各种BeanFactoryPostProcessor)。其中最为关键的是 ConfigurationClassPostProcessor ，在这里完成了配置类的解析，生成注入容器中的bean 的 BeanDefinition。解析@Configuratiqn、@Bean、@lmport、 @PropertySource 
6. registerBeanPostProcessors ：注册和创建拦截bean创建的bean处理器。BeanPostProcessor 在这一步已经完成了创建。
7. initMessageSource ：为上下文初始化Message 源，即对不同语言的消息体进行国际化处理
8. initApplicationEventMulticaster ：初始化应用消息广播器，并放入"applicationEventMulticaster" bean 中
9. onRefresh ：留给子类来初始化其他bean
10. registerListeners ：在所有注册的bean中查找listener bean，注册到消息广播器中
11. finishBeanFactoryInitialization ：初始化剩下的实例(非惰性)，在这里调用了getBean方法，创建了非惰性的bean实例
12. finishRefresh ：完成刷新过程，通知生命周期处理器 lifecycleProcesseor 刷新过程，同时发出ContextRefreshEvent 通知别人。

[源码下载教程](https://mp.weixin.qq.com/s?__biz=MzI4OTE2NTk1NQ==&mid=2649580209&idx=1&sn=a976ed38bb6f29f9a6aa2da2bdeaeec2&chksm=f42a855dc35d0c4b9637f95b8cde9babaaee0f47e175923c8071dac6f965ceb6a77a9a7fd83a&token=2139939783&lang=zh_CN#rd)spring源码使用了gradle管理

源码：

1. 调用同名构造

```java
public ClassPathXmlApplicationContext(String configLocation) throws BeansException {
   this(new String[] {configLocation}, true, null);
}
```

2. 

```java
public ClassPathXmlApplicationContext(String[] configLocations, boolean refresh, ApplicationContext parent)
      throws BeansException {

   super(parent);
    //setConfigLocations(configLocations)方法用来设置配置路径， 支持多个配置文件以数组方式同时传入
   setConfigLocations(configLocations);
   if (refresh) {
      refresh();
   }
}
```

3. 

```java
public void refresh() throws BeansException, IllegalStateException {
   synchronized (this.startupShutdownMonitor) {
      // Prepare this context for refreshing.
      prepareRefresh();

      // Tell the subclass to refresh the internal bean factory.
      ConfigurableListableBeanFactory beanFactory = obtainFreshBeanFactory();

      // Prepare the bean factory for use in this context.
      prepareBeanFacto ry(beanFactory);

      try {
         // Allows post-processing of the bean factory in context subclasses.
         postProcessBeanFactory(beanFactory);

         // Invoke factory processors registered as beans in the context.
         invokeBeanFactoryPostProcessors(beanFactory);

         // Register bean processors that intercept bean creation.
         registerBeanPostProcessors(beanFactory);

         // Initialize message source for this context.
         initMessageSource();

         // Initialize event multicaster for this context.
         initApplicationEventMulticaster();

         // Initialize other special beans in specific context subclasses.
         onRefresh();

         // Check for listener beans and register them.
         registerListeners();

         // Instantiate all remaining (non-lazy-init) singletons.
         finishBeanFactoryInitialization(beanFactory);

         // Last step: publish corresponding event.
         finishRefresh();
      }

      catch (BeansException ex) {
         if (logger.isWarnEnabled()) {
            logger.warn("Exception encountered during context initialization - " +
                  "cancelling refresh attempt: " + ex);
         }

         // Destroy already created singletons to avoid dangling resources.
         destroyBeans();

         // Reset 'active' flag.
         cancelRefresh(ex);

         // Propagate exception to caller.
         throw ex;
      }

      finally {
         // Reset common introspection caches in Spring's core, since we
         // might not ever need metadata for singleton beans anymore...
         resetCommonCaches();
      }
   }
}
```

3.1 

```java
protected void prepareRefresh() {
   this.startupDate = System.currentTimeMillis();
   this.closed.set(false);
   this.active.set(true);

   if (logger.isInfoEnabled()) {
      logger.info("Refreshing " + this);
   }

   // Initialize any placeholder property sources in the context environment
   initPropertySources();

   // Validate that all properties marked as required are resolvable
   // see ConfigurablePropertyResolver#setRequiredProperties
   getEnvironment().validateRequiredProperties();

   // Allow for the collection of early ApplicationEvents,
   // to be published once the multicaster is available...
   this.earlyApplicationEvents = new LinkedHashSet<ApplicationEvent>();
}
```

3.2 

```java
protected ConfigurableListableBeanFactory obtainFreshBeanFactory() {
   refreshBeanFactory();
   ConfigurableListableBeanFactory beanFactory = getBeanFactory(); //--->
   if (logger.isDebugEnabled()) {
      logger.debug("Bean factory for " + getDisplayName() + ": " + beanFactory);
   }
   return beanFactory;
}
```

```java
protected DefaultListableBeanFactory createBeanFactory() {
   return new DefaultListableBeanFactory(getInternalParentBeanFactory());
}
```

4.

```java
protected void prepareBeanFactory(ConfigurableListableBeanFactory beanFactory) {
   // Tell the internal bean factory to use the context's class loader etc.
   beanFactory.setBeanClassLoader(getClassLoader());
   beanFactory.setBeanExpressionResolver(new StandardBeanExpressionResolver(beanFactory.getBeanClassLoader()));
   beanFactory.addPropertyEditorRegistrar(new ResourceEditorRegistrar(this, getEnvironment()));
 
   // Configure the bean factory with context callbacks.
   beanFactory.addBeanPostProcessor(new ApplicationContextAwareProcessor(this));
   beanFactory.ignoreDependencyInterface(EnvironmentAware.class);
   beanFactory.ignoreDependencyInterface(EmbeddedValueResolverAware.class);
   beanFactory.ignoreDependencyInterface(ResourceLoaderAware.class);
   beanFactory.ignoreDependencyInterface(ApplicationEventPublisherAware.class);
   beanFactory.ignoreDependencyInterface(MessageSourceAware.class);
   beanFactory.ignoreDependencyInterface(ApplicationContextAware.class);

   // BeanFactory interface not registered as resolvable type in a plain factory.
   // MessageSource registered (and found for autowiring) as a bean.
   beanFactory.registerResolvableDependency(BeanFactory.class, beanFactory);
   beanFactory.registerResolvableDependency(ResourceLoader.class, this);
   beanFactory.registerResolvableDependency(ApplicationEventPublisher.class, this);
   beanFactory.registerResolvableDependency(ApplicationContext.class, this);

   // Register early post-processor for detecting inner beans as ApplicationListeners.
   beanFactory.addBeanPostProcessor(new ApplicationListenerDetector(this));

   // Detect a LoadTimeWeaver and prepare for weaving, if found.
   if (beanFactory.containsBean(LOAD_TIME_WEAVER_BEAN_NAME)) {
      beanFactory.addBeanPostProcessor(new LoadTimeWeaverAwareProcessor(beanFactory));
      // Set a temporary ClassLoader for type matching.
      beanFactory.setTempClassLoader(new ContextTypeMatchClassLoader(beanFactory.getBeanClassLoader()));
   }

   // Register default environment beans.
   if (!beanFactory.containsLocalBean(ENVIRONMENT_BEAN_NAME)) {
      beanFactory.registerSingleton(ENVIRONMENT_BEAN_NAME, getEnvironment());
   }
   if (!beanFactory.containsLocalBean(SYSTEM_PROPERTIES_BEAN_NAME)) {
      beanFactory.registerSingleton(SYSTEM_PROPERTIES_BEAN_NAME, getEnvironment().getSystemProperties());
   }
   if (!beanFactory.containsLocalBean(SYSTEM_ENVIRONMENT_BEAN_NAME)) {
      beanFactory.registerSingleton(SYSTEM_ENVIRONMENT_BEAN_NAME, getEnvironment().getSystemEnvironment());
   }
}
```

5. finishBeanFactoryInitialization(beanFactory);

   ```java
   protected void finishBeanFactoryInitialization(ConfigurableListableBeanFactory beanFactory) {
      // Initialize conversion service for this context.
       //ConversionService是一个服务接口用于类型转换，这也是转换系统的入口点。 这一步没有做什么处理
      if (beanFactory.containsBean(CONVERSION_SERVICE_BEAN_NAME) &&
            beanFactory.isTypeMatch(CONVERSION_SERVICE_BEAN_NAME, ConversionService.class)) {
         beanFactory.setConversionService(
               beanFactory.getBean(CONVERSION_SERVICE_BEAN_NAME, ConversionService.class));
      }
   
      //
      if (!beanFactory.hasEmbeddedValueResolver()) {
         beanFactory.addEmbeddedValueResolver(new StringValueResolver() {
            @Override
            public String resolveStringValue(String strVal) {
               return getEnvironment().resolvePlaceholders(strVal);
            }
         });
      }
   
      //  先获取实现了LoadTimeWeaverAware接口的bean名称，LoadTimeWeaverAware是类加载时织入的意思。getBean则是让实现LoadTimeWeaverAware的对象提前实例化。
      String[] weaverAwareNames = beanFactory.getBeanNamesForType(LoadTimeWeaverAware.class, false, false);
      for (String weaverAwareName : weaverAwareNames) {
         getBean(weaverAwareName);
      }
   
      // Stop using the temporary ClassLoader for type matching.
      beanFactory.setTempClassLoader(null);
   
      // Allow for caching all bean definition metadata, not expecting further changes.
      beanFactory.freezeConfiguration();
   
      // Instantiate all remaining (non-lazy-init) singletons.
      beanFactory.preInstantiateSingletons();
   }
   ```



三级缓存

```java
/** Cache of singleton objects: bean name --> bean instance */
private final Map<String, Object> singletonObjects = new ConcurrentHashMap<String, Object>(256);

/** Cache of singleton factories: bean name --> ObjectFactory */
private final Map<String, ObjectFactory<?>> singletonFactories = new HashMap<String, ObjectFactory<?>>(16);

/** Cache of early singleton objects: bean name --> bean instance */
private final Map<String, Object> earlySingletonObjects = new HashMap<String, Object>(16);
```

### 黑马相关面试教学

![image-20221228161228775](http://bijioss.donggei.top/image-20221228161228775.png)

就是准备了一个environment 是ApplicationContext的一个成员变量，Environment中就是一些后续可能用到的键值信息，比如：application.properties中的

![image-20221228164742472](http://bijioss.donggei.top/image-20221228164742472.png)

beanFactory也作为了ApplicationContext的一个成员变量，ApplicationContext继承了BeanFactory

但是在使用时 是这种组合关系

![image-20221228171353770](http://bijioss.donggei.top/image-20221228171353770.png)

右边绿色新多的就是在第3步完成的

![image-20221228171628332](http://bijioss.donggei.top/image-20221228171628332.png)

![image-20221228171950555](http://bijioss.donggei.top/image-20221228171950555.png)

多加了一些beanDefinition

![image-20221230204630189](http://bijioss.donggei.top/image-20221230204630189.png)

注册了并没有用到它呢，在实例化、依赖注入、初始化阶段 才会用到  可能在11步

![image-20221231171220301](http://bijioss.donggei.top/image-20221231171220301.png)

![image-20221231171650826](http://bijioss.donggei.top/image-20221231171650826.png)

![image-20221231171802776](http://bijioss.donggei.top/image-20221231171802776.png)

体现了模板方法设计模式

![image-20221231174805506](http://bijioss.donggei.top/image-20221231174805506.png)

![image-20221231172630837](http://bijioss.donggei.top/image-20221231172630837.png)

在beanDefinitionMap非延迟单例对象  在这个时候创建出来bean （Beanpp 就会派上用场

![image-20221231175438863](http://bijioss.donggei.top/image-20221231175438863.png)

bean的生命周期

![image-20221231203256120](http://bijioss.donggei.top/image-20221231203256120.png)

![image-20230102122320535](http://bijioss.donggei.top/image-20230102122320535.png)

2级 解决代理情况下的循环依赖  循环依赖要产生代理对象 。3级 解决循环依赖

![image-20230102175427337](http://bijioss.donggei.top/image-20230102175427337.png)

![image-20230102180538871](http://bijioss.donggei.top/image-20230102180538871.png)

![image-20230102183426654](http://bijioss.donggei.top/image-20230102183426654.png)

![image-20230102181537031](http://bijioss.donggei.top/image-20230102181537031.png)

 