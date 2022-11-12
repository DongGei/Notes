springcloud

学习地址：**尚硅谷  阳哥**

参考文档：[陌溪的学习笔记](http://moxi159753.gitee.io/learningnotes/#/README?id=📙陌溪的学习笔记)

Cloud升级

    服务注册中心
        × Eureka
        √ Zookeeper
        √ Consul
        √ Nacos
    
    服务调用
        √ Ribbon
        √ LoadBalancer
    
    服务调用2
        × Feign
        √ OpenFeign
    
    服务降级
        × Hystrix
        √ resilience4j
        √ sentienl
    
    服务网关
        × Zuul
        ! Zuul2
        √ gateway
    
    服务配置
        × Config
        √ Nacos
    
    服务总线
        × Bus
        √ Nacos
+ 微服务、分布式概念、微服务架构 

+ 注册中心：Eureka

+ 负载均衡：Ribbon

+ 声明式调用远程方法：OpenFeign

+ 熔断、降级、监控：Hystrix

+ 网关：Gateway

+ 链路跟踪：Sleuth 

+ 服务注册和配置中心：Spring Cloud Alibaba Nacos

+ 熔断、降级、限流：Spring Cloud Alibaba Sentinel

SpringBoot**专注于快速方便的开发单个个体微服务**。 SpringCloud是关注**全局的微服务协调整理治理框架**，它将SpringBoot开发的一个个单体微服务整合并管理起来，为各个微服务之间提供，**配置管理、服务发现、断路器、路由、微代理、事件总线、全局锁、决策竞选、分布式会话**等等集成服务SpringBoot可以离开SpringCloud独立使用开发项目， 但是SpringCloud离不开SpringBoot ，属于依赖的关系. SpringBoot专注于快速、方便的开发单个微服务个体，SpringCloud关注全局的服务治理框架

![image-20221018202244092](http://bijioss.donggei.top/image-20221018202244092.png)

实际上，Spring Cloud是一个全家桶式的技术栈，包含了很多组件。本文先从其最核心的几个组件入手，来剖析一下其底层的工作原理。**也就是Eureka、Ribbon、Feign、Hystrix、Zuul这几个组件**

### Eureka（停更）->nacos

注册中心 负责服务的注册与发现

### feign

Feign Client会在底层根据你的注解，跟你指定的服务建立连接、构造请求、发起靕求、获取响应、解析响应，等等

**Feign的一个关键机制就是使用了动态代理**

如果你对某个接口定义了@FeignClient注解，Feign就会针对这个接口创建一个动态代理

接着你要是调用那个接口，本质就是会调用 Feign创建的动态代理，这是核心中的核心

Feign的动态代理会根据你在接口上的@RequestMapping等注解，来动态构造出你要请求的服务的地址

最后针对这个地址，发起请求、解析响应

![image-20221018204247179](http://bijioss.donggei.top/image-20221018204247179.png)

###  Ribbon

负载均衡，会帮你在每次请求时选择一台机器

 **此外，Ribbon是和Feign以及Eureka紧密协作，完成工作的，具体如下：**

- 首先Ribbon会从 Eureka Client里获取到对应的服务注册表，也就知道了所有的服务都部署在了哪些机器上，在监听哪些端口号。
- 然后Ribbon就可以使用默认的Round Robin算法，从中选择一台机器
- Feign就会针对这台机器，构造并发起请求。

> 默认使用的最经典的Round Robin轮询算法

###  Hystrix（停更）

解决**服务雪崩**等等问题

大量请求涌过来的时候，订单服务的100个线程都会卡在请求积分服务这块。导致订单服务没有一个线程可以处理请求

**服务订单的线程用完了**

![image-20221018204846041](http://bijioss.donggei.top/image-20221018204846041.png)

Hystrix是隔离、熔断以及降级的一个框架。

隔离：Hystrix会搞很多个小小的线程池，比如订单服务请求库存服务是一个线程池，请求仓储服务是一个线程池，请求积分服务是一个线程池。每个线程池里的线程就仅仅用于请求那个服务。

熔断：某个服务频繁超时，直接将其短路，快速返回mock（模拟/虚拟）值。（快速返回mock就是服务降级

### Zuul -> spring GateWay

微服务网关 ，Zuul目前貌似已经渐渐被spring GateWay替代

Cloud全家桶中有个很重要的组件就是网关，在1.x版本中都是采用的Zuul网关 但在2.x版本中，zuul的升级一直跳票，SpringCloud最后自己研发了一个网关代替Zull，那就是SpringCloud Geteway

就知道有一个网关，所有请求都往网关走，网关会根据请求中的一些特征，将请求转发给后端的各个服务。

![image-20221018212937041](http://bijioss.donggei.top/image-20221018212937041.png)

### 

安全、监控/指标、和限流。Spring Cloud Gateway 使用的Webflux中的reactor-netty响应式编程组件，底层使用了Netty通讯框架。比

### Sleuth

分布式链路请求跟踪

### Config->Nacos

SpringCloud Config

服务端也称为分布式配置中心

客户端则是通过指定的配置中心来管理应用资源，以及与业务相关的配置内容，并在启动的时候从配置中心获取和加载配置信息配置服务器默认采用git来存储配置信息

### Bus->Nacos

一言以蔽之，分布式自动刷新配置功能

消息总线

### Nacos

Nacos就是注册中心＋配置中心的组合 -> **Nacos = Eureka+Config+Bus**

**各中注册中心比较**

| 服务注册与发现框架 | CAP模型 | 控制台管理 | 社区活跃度      |
| ------------------ | ------- | ---------- | --------------- |
| Eureka             | AP      | 支持       | 低(2.x版本闭源) |
| Zookeeper          | CP      | 不支持     | 中              |
| consul             | CP      | 支持       | 高              |
| Nacos              | AP      | 支持       | 高              |

Nacos 支持AP和CP的切换

### Seata

Seata 是一款开源的分布式事务解决方案，致力于提供高性能和简单易用的分布式事务服务。Seata 将为用户提供了 AT、TCC、SAGA 和 XA 事务模式，为用户打造一站式的分布式解决方案。

分布式事务处理过程的一致性ID + 三组件模型

- Transaction ID XID：全局唯一的事务ID
- 三组件的概念 
  - Transaction Coordinator（TC）：事务协调器，维护全局事务，驱动全局事务提交或者回滚
  - Transaction Manager（TM）：事务管理器，控制全局事务的范围，开始全局事务提交或回滚全局事务
  - Resource Manager（RM）：资源管理器，控制分支事务，负责分支注册分支事务和报告

