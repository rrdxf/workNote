# spring cloud

```
链接：https://pan.baidu.com/s/1Nsc8r0pRZ5GGp5MeLuoEvg 
提取码：7oum 
```

https://gitee.com/lixiaogou/cloud2020 

C:\Windows\System32\drivers\etc\hosts

### springcloud更新

![1646378973746](C:\Users\xf\Desktop\学习笔记\后端\assets\1646378973746.png)

### 版本选择：

![1646277466038](C:\Users\xf\Desktop\学习笔记\后端\assets\1646277466038.png)

### 创建Springboot工程：

![1646378495658](C:\Users\xf\Desktop\学习笔记\后端\assets\1646378495658.png)

![1646378520919](C:\Users\xf\Desktop\学习笔记\后端\assets\1646378520919.png)

## 服务注册中心

### Eureka：

#### 什么是服务治理

​	spring cloud封装了netfix公司开发的Eureka模块来实现服务治理

​	再传统的rpc远程调用框架中，管理每个服务与服务之间依赖关系比较复杂，所需要使用服务治理，管理服务于服务之间依赖关系，可以实现服务调用，负载均衡，容错等，实现服务发现与注册。

#### 什么是服务注册与发现

​	Eureka采用了cs的设计结构，Euraka Server作为服务注册功能的服务器，他说服务注册中心。而系统中其他微服务，使用Eureka的客户端连接到Euraka的客户端连接到Eureka Server并位置心跳连接。这样系统的维护人员就可以通过Eureka Server 来监控系统中各个微服务是否正常运行。

​	在服务注册与发现中，又一个注册中心，当服务器穷的那个的时候，回吧当前自己服务器的信息，比如服务地址通讯地址以别名的方式注册到注册中心上。另一方消费者或者生产者，以该别名的 方式去注册中心获取实际的同学滚地址，然后在实现本地rpc调用rpc远程调用框架核心私信：在与注册中心，因为使用注册中心管理每个服务与服务之间的 以来关系。在任何rpc远程框架中，都会又一个注册中心

![1646381036884](C:\Users\xf\Desktop\学习笔记\后端\assets\1646381036884.png)



#### Eureka Server提供服务注册服务

​	各个微服务节点通过配置启动后，回在EurekaServer中进行注册，这样EurekaServer中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观的看到。

#### Eureka Client通过注册中心进行访问

​	是一个Java客户端，用于简化Eureka Server的交互，客户端同时也具备一个内置、使用轮询负载算法的负载均衡器。在应用启动后，回想Eureka Server发送心跳（默认30s以恶搞周期）。如果Eureka Server在多个心跳周期内没有接收到某个节点的心跳。EurekaServer将会从服务注册表中吧这个及服务节点一移除（默认90秒）

#### Eureka集群原理

服务注册：将服务信息注册进注册中西

服务发信：从注册中心上获取服务信息

实质：key服务名取value调用地址

![1646383810085](C:\Users\xf\Desktop\学习笔记\后端\assets\1646383810085.png)

1. 先启动eureka注册中心
2. 启动服务提供者payment支付服务
3. 支付服务情动后会把自身信息（比如服务地址以别名方式注册eureka）
4. 消费者order服务在需要调用接口时，使用服务别名去注册中心获取实际的rpc远程调用地址
5. 消费者而获得调用地址后，底层实际时利用哦个httpclient技术实现远程调用
6. 消费者活得服务地址后会缓存中哎本地jvm中，默认每间隔30秒更新一次是否调用地址

#### 远程调用方式

无论是微服务还是分布式服务，都面临着服务间的远程调用。那么服务间的远程调用。

- rpc：Rmote produce call远程调用过程，类似的还有rmi。自定义数据格式，基于原生tcp通信，速度快，效率高。早期的webserive，现在热门的dubbo，都是rpc的典型。
- http：http其实是一种网络传输协议，基于tcp，规定了数据传输的格式。现在客户端浏览器与服务端基本通信都采用同http协议。也可以哟弄个来远程调用服务。短浅是封装消息臃肿。现在热门的rest份个股，就是通过http实现
- **相同点**：底层基于socket，都可以实现远程调用，都可以实现调用服务
- **rpc和http的区别**
  - rpc要求服务提供和服务调用方法都用相同的技术，http无需关注语言，著需要准寻rest规范
  - rpc的开发要求较多

#### Eureka发现机制

![1646446857399](C:\Users\xf\Desktop\学习笔记\后端\assets\1646446857399.png)

![1646446894069](C:\Users\xf\Desktop\学习笔记\后端\assets\1646446894069.png)

```java
@Resource
    private DiscoveryClient discoveryClient;
```

#### Eureka自我保护模式

##### 概述

​	保护模式主要用于一组客户端和Eureka Server之间存在网络区分场景下的保护。一旦进入保护模式 **Eureka Server将会尝试保护其服务注册表中的信息，不再删除服务注册表中底层数据，也就是不会注销任何服务**

![1646447055164](C:\Users\xf\Desktop\学习笔记\后端\assets\1646447055164.png)

![1646447065240](C:\Users\xf\Desktop\学习笔记\后端\assets\1646447065240.png)

### zookeeper

#### zookeper版本冲突问题

springcloud中自带了3.5.3的版本，需要先exclusions排除

![1646451992818](C:\Users\xf\Desktop\学习笔记\后端\assets\1646451992818.png)

### Consul

#### 简介

​	是一套开源的分布式的服务发现和配置管理系统、由HashiCorp公司用Go语言开发

​	提供了微服务系统中的服务治理、配置中心、控制总线等功能。这些功能中的每一个都可以根据需要单独使用，也可以一起使用以构建全方位服务网络，总之consul提供了一种完整的服务网络解决方案

### 三个注册中心的异同点

![1646483663032](C:\Users\xf\Desktop\学习笔记\后端\assets\1646483663032.png)

A：高可用Availability（可用性）

C：数据一致（Consistency强一致性）

P：Partition tolerance （分区容错性）

cap理论关注粒度是数据，不是整体系统设计的

![ 1646482880046](C:\Users\xf\Desktop\学习笔记\后端\assets\1646482880046.png)



### Nacos



## 服务调用

### Ribbon

SpringCloud Ribbon是基于Netflix Ribbon实现的一套客户端负载均衡的工具

简单来说，Ribbon是Netflix发布的开源项目，主要功能是提供客户端的软件负载均衡算法和服务调用。Ribbon客户端组件提供完善的配置项如连接超市，重试等。简单的说， 就是在配置文件中列出Load Balancer后面所有的及其，Ribbon回自主你基于某种规则（如简单轮状，随机连接等）去连接这些及其。我们很容易使用Ribbon实现自定义的负载均衡算法

#### 负载均衡

简单的说就是用户的请求平坦的分二批到多个服务上，从而达到系统的HA（高可用）

常见的负载均衡由软件 Nginx 、LVS，硬件F5等

#### Ribbon本地负载均衡客户端Vs Nginx服务端负载均衡区别

Nginx是服务器负载均衡，客户端所有请求都会交给nginx，然后由nginx实现转发请求。即负载均衡是由服务端实现的

Ribbon本地负载均衡，在调用微服务接口的时候，会在注册中心上获取注册信息服务列表之后缓存到jvm本地，从而在本地实现Rpc远程调用技术

#### Ribbon负载均衡算法

![1646485142944](C:\Users\xf\Desktop\学习笔记\后端\assets\1646485142944.png)

负载均衡算法：rest接口第几次请求数%服务器集群数量=实际调用服务器位置下标，每次服务重启后，rest接口计数从1开始

#### 源码分析

##### volatile

https://www.cnblogs.com/dolphin0520/p/3920373.html

volatile 禁止语句重拍，不保证原子性，保证可见性

![1646486383094](C:\Users\xf\Desktop\学习笔记\后端\assets\1646486397204.png)

### OpenFeign

#### Feign

fein旨在编写java http客户端变得容易

前面在使用RIbbon+RestTemplate时利用RestTemplate对http请求的封装处理，形成了一套模板化的调用方法。但是在实际开发中，由于对服务依赖调用可能不止一处，往往一个接口会被多出调用，所以通常都会针对每个服务自行封装一些客户端类来包装这些依赖服务的调用。所以，feign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义。在Feign的实现下，我们只需要创建一个接口，并使用注解的方式来配置他。即可完成对服务提供放的接口绑定，简化了使用Spring cloud Ribbon时，自动封装服务调用客户端的开发量

### 超时控制

```yaml
# openfign 的底层时由ribbon控制，那么openfign的时间控制由ribbon控制
ribbon:
  ReadTimeout: 6000
  ConnectTimeout: 6000
```



## 服务降级

### Hystrix X

#### 服务降级

#### 服务熔断

#### 服务限流

#### 服务雪崩

### 概念

Hystrix时一个用于处理分布式系统的 延迟和容错的开源库，在分布式系统里，v许多依赖不可避免的会调用失败，比如超市，异常等，Hystirx能够保证在一个依赖出问题的情况下，不会导致灯体服务失败，避免级联故障，提高分布式弹性

“断路器”本身时一种开关装置，某个服务单元发生故障之后，通过断路器的故障监控，像调用方返回一个符合预期的，可处理的备选相应，而不是长时间的等待或者抛出调用方法无法处理的 异常，这样就保证了服务调用放的线程不会被长时间，不必要的占用，从而避免了股灾在分布式系统中的蔓延

```java
@HystrixCommand(fallbackMethod = "paymentInfo_TimeoutHandler",commandProperties = {
            @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "3000")
    })//兜底方法，超时时间
```

## 服务熔断

```java

    //============熔断机制
    @HystrixCommand(fallbackMethod = "paymentCircuiBreaker_fallback",commandProperties = {
            @HystrixProperty(name = "circuitBreaker.enabled",value = "true"),// 是否开启断路器
            @HystrixProperty(name = "circuitBreaker.requestVolumeThreshold",value = "10"),//请求次数
            @HystrixProperty(name = "circuitBreaker.sleepWindowInMilliseconds",value = "10000"),//时间窗口
            @HystrixProperty(name = "circuitBreaker.errorThresholdPercentage",value = "60")//失败率达到多少
    })
```

### 概念

就好比保险丝  服务降级-》进而熔断-》恢复调用链路

### 糊涂工具包

```xml
<dependency>
	<groupId>cn.hutool</groupId>
	<artifactId>hutool-all</artifactId>
</dependency>
```

请求数量过多拉闸，进行fallback处理，然后恢复服务

### Hystrix 仪表盘Dashboard

需要在被监控的主启动类里面加

```java
    /**
     * 注意：新版本Hystrix需要在主启动类中指定监控路径
     * 此配置是为了服务监控而配置，与服务容错本身无关，spring cloud升级后的坑
     * ServletRegistrationBean因为springboot的默认路径不是"/hystrix.stream"，
     * 只要在自己的项目里配置上下面的servlet就可以了
     *
     * @return ServletRegistrationBean
     */
    @Bean
    public ServletRegistrationBean getServlet() {
        HystrixMetricsStreamServlet streamServlet = new HystrixMetricsStreamServlet();
        ServletRegistrationBean registrationBean = new ServletRegistrationBean(streamServlet);

        // 一启动就加载
        registrationBean.setLoadOnStartup(1);
        // 添加url
        registrationBean.addUrlMappings("/hystrix.stream");
        // 设置名称
        registrationBean.setName("HystrixMetricsStreamServlet");
        return registrationBean;
    }
```

## 服务网关

![1646658428047](C:\Users\xf\Desktop\学习笔记\后端\assets\1646658428047.png)

### 概念

路由是构建网关的基本模块，它由ID，目标URI，一系列的断言和过滤器组成，如果断言为true则匹配该路由

![1646659307309](C:\Users\xf\Desktop\学习笔记\后端\assets\1646659307309.png)

## 服务配置

### Config

不重启刷新客户端

![1646739764605](C:\Users\xf\Desktop\学习笔记\后端\assets\1646739764605.png)

### Bus消息总线

实现分布式自动刷新配置功能

Spring Cloud Bus配合Spring cloud Config使用可以实现配置的动态刷新。

支持 rabbitmq 和 kafka

#### 原理

![1646788628847](C:\Users\xf\Desktop\学习笔记\后端\assets\1646788628847.png)

configClient实例都监听mq中同一个topic。当一个服务刷新数据的时候，它会把这个信息放入到topic中，这样七踏建通同一个topic的服务就能得到通知，然后去更新自身的配置

```
cmd 发送命令，一次配置，处处生效

curl -X POST "http://localhost:3344/actuator/bus-refresh"

//只刷新3355
curl -X POST "http://localhost:3344/actuator/bus-refresh"/config-client:3355"
```

## 消息驱动

### spring-stream

是一个创建消息驱动微服务的框架

应用程序通过inputs和outputs来与spring cloud stream中binder对象交互，通过我们配置来binding，而spring cloud stream

是整合各种消息中间件的东西

![1646792476651](C:\Users\xf\Desktop\学习笔记\后端\assets\1646792476651.png)

#### 重复消费

不同组可以重复消费

同一组会发生竞争关系，只有一个可以消费

## 链路监控

### Seuth

![1646798664634](C:\Users\xf\Desktop\学习笔记\后端\assets\1646798664634.png)

# springcloud Alibabab

## Nacos

### 介绍

**Nacos=Eureka+Config+Bus**

Nacos（Dynamic Naming and Configuration Service）前四个字母分别为Naming和Configuration的前两个字符，最后的s为Service

### 功能

替代Eureka做服务注册中心

替代Config做服务配置中心

### 使用

安装 cmd bin 

startup.cmd 

然后访问：localhost:8848/nacos

### 注册中心

A:高可用，所有的请求都会收到响应

C:是所有节点在同一时间看到的数据是一致的

![1646810681410](C:\Users\xf\Desktop\学习笔记\后端\assets\1646810681410.png)

nacos可以切换cp或者ap

#### 如可选择模式：

如果不需要存储服务级别的信息且服务实例是通过nacos-client注册，能够保持心跳上报，那么就可以选择AP模式。当前主流的服务如Spring cloud 和 Dubbo服务，都适用于AP模式，AP模式为了服务的可能性而减弱了一致性，因此AP模式下只支持注册零时实例



如果需要在服务级别或者存储配置信息，那么cp是必须的，k8s服务和dns服务则适用于cp模式，cp模式下则支持注册持久化实例，此时则是以Raft协议为集群运行模式，该模式下注册实例之前必须西安注册服务，如果服务不存在，则会返回错误

### 服务配置中心

Nacos同springcloud-config一样，在项目初始化时，要保证先从配置中心进行配置拉取，拉取配置之后，才能保证项目的正常启动



springboot中配置文件的加载是存在优先级顺序的，bootstrap优先级高于application

![1646812150086](C:\Users\xf\Desktop\学习笔记\后端\assets\1646812150086.png)

#### nacos mysql配置

```properties
spring.datasource.platform=mysql
db.num=1
db.url.0=jdbc:mysql://192.168.220.132/nacos_config?characterEncoding=utf-8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true
db.user=root
db.password=Gtq106882064*
```

#### nacos Linux集群配置中的坑

今天配置nacos集群的时候，老是只可以启动一个3333端口，第二第三个nacos的时候，老是开不出三个来

后来发现是内存不够了，nacos单个运行内存要两个g

![1646920497881](C:\Users\xf\Desktop\学习笔记\后端\assets\1646920497881.png)

如果是虚拟机学习的话，正常情况两个g都不够用，所以nacos必然会挂掉，这个单个内存已经达到了37%。如果启动两个的话，是可以启动的，但是第二个nacos端口不可访问。所以需要修改内存。

vim startup.sh

这是原来的内存分配![1646920741203](C:\Users\xf\Desktop\学习笔记\后端\assets\1646920741203.png)

将所有的内存分配之后就减小1/4![1646920683130](C:\Users\xf\Desktop\学习笔记\后端\assets\1646920683130.png)

修改之后的三个nacos的内存占用只有17%

这样就完成了nacos的集群配置

![1646920621041](C:\Users\xf\Desktop\学习笔记\后端\assets\1646920621041.png)

## Sentinel

### 作用

sentinel具有特殊

- 丰富的应用场景。承受了十年的双十一流量场景。秒杀、消息削峰、集群流量控制、实时熔断下游不可应用等
- 完备的实时监控。Sentinel同时提供实时的监控功能。您可以在控制台中看到接入应用的单台及其机秒杀数据
- 广泛的开源生态
- 完善的spi口占点：提供简单易用、完善的spi扩展接口。

#### hstrix对比

- 需要自己搭建平台
- 没有一套web界面可以给我们进行更加细粒度话的配置留控，速率控制、服务熔断，服务降级

#### Sentinel对比

- 单独一个组件，可以独立出来
- 直接界面化的粒度统一配置

![1646921771009](C:\Users\xf\Desktop\学习笔记\后端\assets\1646921771009.png)

### 实现熔断和限流

**约定>配置>编码**

都可以写在代码里，但是我们本次还是大规模的学习使用配置和注解的方式，尽量少些代码

#### 使用

![1646963054100](C:\Users\xf\Desktop\学习笔记\后端\assets\1646963054100.png)

fallback是java的异常

blockhandler是sentinel的异常，流量限制

## seata（3+1）

![1646968366363](C:\Users\xf\Desktop\学习笔记\后端\assets\1646968366363.png)

### 事务唯一id

### TC-事务协调者

维护全局和分支事务的状态

### TM-事务管理器

定义全局事务的范围，开始全局事务、提交或回滚事务

### RM-资源管理器

管理分支事务处理的资源，与tc交谈以注册分支事务和报告分支事务的状态，并驱动分支事务提交或回滚

![1646968953290](C:\Users\xf\Desktop\学习笔记\后端\assets\1646968953290.png)![1646969057601](C:\Users\xf\Desktop\学习笔记\后端\assets\1646969057601.png)

#### seata分布式数据库验证

```sql
--- seata  分布式事务
-- the table to store GlobalSession data
create
database seata;
USE
seata;
drop table if exists `global_table`;
create table `global_table`
(
    `xid`                       varchar(128) not null,
    `transaction_id`            bigint,
    `status`                    tinyint      not null,
    `application_id`            varchar(32),
    `transaction_service_group` varchar(32),
    `transaction_name`          varchar(128),
    `timeout`                   int,
    `begin_time`                bigint,
    `application_data`          varchar(2000),
    `gmt_create`                datetime,
    `gmt_modified`              datetime,
    primary key (`xid`),
    key                         `idx_gmt_modified_status` (`gmt_modified`, `status`),
    key                         `idx_transaction_id` (`transaction_id`)
);

-- the table to store BranchSession data
drop table if exists `branch_table`;
create table `branch_table`
(
    `branch_id`         bigint       not null,
    `xid`               varchar(128) not null,
    `transaction_id`    bigint,
    `resource_group_id` varchar(32),
    `resource_id`       varchar(256),
    `lock_key`          varchar(128),
    `branch_type`       varchar(8),
    `status`            tinyint,
    `client_id`         varchar(64),
    `application_data`  varchar(2000),
    `gmt_create`        datetime,
    `gmt_modified`      datetime,
    primary key (`branch_id`),
    key                 `idx_xid` (`xid`)
);

-- the table to store lock data
drop table if exists `lock_table`;
create table `lock_table`
(
    `row_key`        varchar(128) not null,
    `xid`            varchar(96),
    `transaction_id` long,
    `branch_id`      long,
    `resource_id`    varchar(256),
    `table_name`     varchar(32),
    `pk`             varchar(36),
    `gmt_create`     datetime,
    `gmt_modified`   datetime,
    primary key (`row_key`)
);

-- - seata_order
create
database IF NOT EXISTS seata_order ;
USE
seata_order;
DROP TABLE IF EXISTS `t_order`;
CREATE TABLE `t_order`
(
    `int`        bigint(11) NOT NULL AUTO_INCREMENT,
    `user_id`    bigint(20) DEFAULT NULL COMMENT '用户id',
    `product_id` bigint(11) DEFAULT NULL COMMENT '产品id',
    `count`      int(11) DEFAULT NULL COMMENT '数量',
    `money`      decimal(11, 0) DEFAULT NULL COMMENT '金额',
    `status`     int(1) DEFAULT NULL COMMENT '订单状态:  0:创建中 1:已完结',
    PRIMARY KEY (`int`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '订单表' ROW_FORMAT = Dynamic;

DROP TABLE IF EXISTS `undo_log`;
CREATE TABLE `undo_log`
(
    `id`            bigint(20) NOT NULL AUTO_INCREMENT,
    `branch_id`     bigint(20) NOT NULL,
    `xid`           varchar(100) NOT NULL,
    `context`       varchar(128) NOT NULL,
    `rollback_info` longblob     NOT NULL,
    `log_status`    int(11) NOT NULL,
    `log_created`   datetime     NOT NULL,
    `log_modified`  datetime     NOT NULL,
    `ext`           varchar(100) DEFAULT NULL,
    PRIMARY KEY (`id`),
    UNIQUE KEY `ux_undo_log` (`xid`,`branch_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

create
database IF NOT EXISTS seata_storage;
USE
seata_storage;
DROP TABLE IF EXISTS `t_storage`;
CREATE TABLE `t_storage`
(
    `int`        bigint(11) NOT NULL AUTO_INCREMENT,
    `product_id` bigint(11) DEFAULT NULL COMMENT '产品id',
    `total`      int(11) DEFAULT NULL COMMENT '总库存',
    `used`       int(11) DEFAULT NULL COMMENT '已用库存',
    `residue`    int(11) DEFAULT NULL COMMENT '剩余库存',
    PRIMARY KEY (`int`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '库存' ROW_FORMAT = Dynamic;
INSERT INTO `t_storage`
VALUES (1, 1, 100, 0, 100);

DROP TABLE IF EXISTS `undo_log`;
CREATE TABLE `undo_log`
(
    `id`            bigint(20) NOT NULL AUTO_INCREMENT,
    `branch_id`     bigint(20) NOT NULL,
    `xid`           varchar(100) NOT NULL,
    `context`       varchar(128) NOT NULL,
    `rollback_info` longblob     NOT NULL,
    `log_status`    int(11) NOT NULL,
    `log_created`   datetime     NOT NULL,
    `log_modified`  datetime     NOT NULL,
    `ext`           varchar(100) DEFAULT NULL,
    PRIMARY KEY (`id`),
    UNIQUE KEY `ux_undo_log` (`xid`,`branch_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;

CREATE
database IF NOT EXISTS seata_account;
USE
seata_account;
DROP TABLE IF EXISTS `t_account`;
CREATE TABLE `t_account`
(
    `id`      bigint(11) NOT NULL COMMENT 'id',
    `user_id` bigint(11) DEFAULT NULL COMMENT '用户id',
    `total`   decimal(10, 0) DEFAULT NULL COMMENT '总额度',
    `used`    decimal(10, 0) DEFAULT NULL COMMENT '已用余额',
    `residue` decimal(10, 0) DEFAULT NULL COMMENT '剩余可用额度',
    PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '账户表' ROW_FORMAT = Dynamic;

INSERT INTO `t_account`
VALUES (1, 1, 1000, 0, 1000);

DROP TABLE IF EXISTS `undo_log`;
CREATE TABLE `undo_log`
(
    `id`            bigint(20) NOT NULL AUTO_INCREMENT,
    `branch_id`     bigint(20) NOT NULL,
    `xid`           varchar(100) NOT NULL,
    `context`       varchar(128) NOT NULL,
    `rollback_info` longblob     NOT NULL,
    `log_status`    int(11) NOT NULL,
    `log_created`   datetime     NOT NULL,
    `log_modified`  datetime     NOT NULL,
    `ext`           varchar(100) DEFAULT NULL,
    PRIMARY KEY (`id`),
    UNIQUE KEY `ux_undo_log` (`xid`,`branch_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

![1647006031914](C:\Users\xf\Desktop\学习笔记\后端\assets\1647006031914.png)

### 原理：

beforeimage和afterimage