### mysql优化

#### 一、mysql的架构介绍

##### 概述：

​	完整的mysql优化需要很深的工地，大公司有专门的DBA写上诉。

##### 数据文件：

- frm文件：存放表结构
- myd文件：存放表数据
- myi文件：存放索引 

##### mysql分层

- 连接层：最上层是一些客户端和连接服务，包含本地sock通信和大多数基于客户端/服务端工具实现的类似于tcp/ip的通信。主要完成一些类似与连接处理，授权认证以及相关的安全方案。在该层上引入了线程池的 概念，为通过认证安全接入的客户端提供线程。同样在该层上可以实现基于ssl的安全连接。服务器也会为安全接入伟哥客户端保证他所具有的操作权限。
- 服务层：第二层架构主要完成大多读书的核心服务共嗯那个，如sql接口，并完成缓存的查询，sql的分析和优化及部分内置函数的执行。所有存储引擎的共嗯那个也在这一层实现。如过程、函数等。在该层，服务器会解析查询并创建相应的内部解析树，并完成相应的 优化如去欸的那个查询表的顺序，是否利用索引等，最后生成相应的操作。如果是select语句，服务器还会查询内部缓存。如果缓存空间勾搭，这样解决大脸读操作环境中可以提升系统性能
- 引擎层：存储引擎，存储引擎正在的负责了mysql中数据的存储和提取，服务器通过api与存储引擎进行通信。不同的存储引擎具有不同的功能
- 存储层：数据存储层，主要是将数据存储在运行于裸设备的文件系统之上，并完成与存储引擎的交互 。

![1646018251949](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646018251949.png)

#####  myisam和innodb的区别

![1646033873555](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646033873555.png)

##### select查询慢的原因：

- 索引
- 多表联查
- sql语句写的不好
- 索引失效
- 服务器调优以及各个参数设置（缓冲、线程数）

##### sql执行顺序

![1646035988370](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646035988370.png)

- select
- from
- where
- group by
- having 
- order by
- limit

![1646034831665](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646034831665.png)

##### join理论（p13）

```sql
CREATE TABLE `tbl_emp` (
`id` int(11) NOT NULL AUTO_INCREMENT,
`name` varchar(20) DEFAULT NULL,
`deptId` int(11) DEFAULT NULL,
PRIMARY KEY (`id`) ,
KEY `fk_dept_id`(`deptId`)
)ENGINE = InnoDB AUTO_INCREMENT = 1 CHARACTER SET = utf8;

CREATE TABLE `tbl_dept` (
`id` int(11) NOT NULL AUTO_INCREMENT,
`deptName` varchar(30) DEFAULT NULL,
`locAdd` varchar(40) DEFAULT NULL,
PRIMARY KEY (`id`)
) ENGINE = InnoDB AUTO_INCREMENT = 1 CHARACTER SET = utf8;
INSERT INTO tbl_dept(deptName,locAdd) VALUES('RD',11);
INSERT INTO tbl_dept(deptName,locAdd) VALUES('HR',12);
INSERT INTO tbl_dept(deptName,locAdd) VALUES('MK',13);
INSERT INTO tbl_dept(deptName,locAdd) VALUES('MIS',14);
INSERT INTO tbl_dept(deptName,locAdd) VALUES('FD',15);

INSERT INTO tbl_emp(NAME,deptId) VALUES('z3',1);
INSERT INTO tbl_emp(NAME,deptId) VALUES('z4',1);
INSERT INTO tbl_emp(NAME,deptId) VALUES('z5',1);
INSERT INTO tbl_emp(NAME,deptId) VALUES('w5',2);
INSERT INTO tbl_emp(NAME,deptId) VALUES('w6',2);
INSERT INTO tbl_emp(NAME,deptId) VALUES('s7',3);
INSERT INTO tbl_emp(NAME,deptId) VALUES('s8',4);
INSERT INTO tbl_emp(NAME,deptId) VALUES('s9',51);
```

两表笛卡尔积

![1646036779278](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646036779278.png)



1、

![1646037009788](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646037009788.png)

![1646034915552](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646034915552.png)

```sql
select * from tbl_dept a inner join tbl_emp b on a.id=b.deptId;
```

2、

![1646037020837](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646037020837.png)

![1646036883073](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646036883073.png)

```sql
select * from tbl_dept a left join tbl_emp b on a.id=b.deptId;
```





3、

![1646037041260](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646037041260.png)![1646037051592](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646037051592.png)

```sql
select * from tbl_dept a right join tbl_emp b on a.id=b.deptId;
```

4、

![1646037162668](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646037193331.png)![1646037202751](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646037202751.png)

```sql
 select * from tbl_dept a right join tbl_emp b on a.id=b.deptId where a.id is not null;
```

5、

![1646037254474](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646037254474.png)

![1646037245676](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646037245676.png)

```sql
select * from tbl_dept a left join tbl_emp b on a.id=b.deptId where b.deptId is not null;

```

#### 二、索引优化分析

排好序的快速查找数据结构

##### 优势：

- 提高检索效率，降低io成本
- 进行排序，降低排序成本降低cpu消耗 

##### 分类

- 单值索引：包含单个列，一个表可以有多个单列索引
- 唯一索引：
- 复合索引：
- 基本语法：

##### 性能分析储备：

1、mysql中有专门负责优化select语句的优化器模块，主要功能通过加u三系统中手机到的信息，为和苦短请求的query提供她认为最有的执行计划（它认为最有的数据检索方式，但不见得是DBA人为是最有的，这部分最好时间）

2、当客户端想Mysql请求一条query，命令解析器完成请求分类，区别出是select并转发给msql query optimizer时，MySql query 首先对整条query优化，去掉一些常量表达式的计算，直接划算成常量。并对query中的查询条件进行简化和转化，如去掉一些无用或者显而易见的条件

##### explain语法：

包含的信息：

**id** 值越大，优先级越高

 **select_type**：![1646041813121](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646041813121.png)

 table 

**type**：访问类型，![1646052464294](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646052464294.png)



 possible_keys key key_lenref rows filtered Extra

![1646101005050](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646101005050.png)

##### 性能分析：

where后面使用到的字段需要建立索引

尽量不要出现using fillesort

<>会使得索引失效



双表连接，索引相反建

##### 索引失效

全值匹配我最爱，最左前缀要遵守；

 带头大哥不能死，中间兄弟不能断；

 索引列上少计算，范围之后全失效；

 Like百分写最右，覆盖索引不写星；

 不等空值还有or，索引失效要少用；

 varchar引号不可丢，SQL高级也不难！ 

![1646105154497](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646105154497.png)



#### 三、查询截取分析

show profile

##### 事务

![1646188336105](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646188336105.png)![1646188454556](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646188454556.png)

#### 四、mysql锁机制

如果索引失效，innodb行锁会变表锁

![1646189886213](C:\Users\xf\Desktop\学习笔记\数据库\assets\1646189886213.png)

#### 五、主从复制

