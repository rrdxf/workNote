### Mysql

#### 1、mysql卸载

1. 点击安装包，点击下一步，然后点击remove卸载
2. 手动删除program flies中的mysql目录
3. 手动删除programData目录（这是个隐藏目录）中的mysql

#### 2、sql、db、dbms分别是什么，他们之间的关系

- db：database数据库，数据库实际上在硬盘上以文件的i形式存在
- dbms：databas management system 数据库管理系统，常见的由：mysql Oracle 、db2、sybase、sqlserver
- sql：结构化查询语言，是一门标准通用的语言。标准的sql适用于所有的数据库产品，sql属于高级语言。只要能看懂英语单词，写出来的sql语句可以读懂什么意思。sql语句在执行的时候，实际上内除也会先进性编译，然后再执行sql。
- dbms负责执行sql语句，通过执行sql语句来操作db当中的数据
- dbms-（执行）-》sql-（操作）-》db

#### 3、mysql概述

- mysql最初是由msql Ad公司考法的一套关系数据库管理系统
- mysql不仅是最流行的开源数据库，而且是业界成长最快的数据库，每天有七万次下载量，其应用范围从大型企业到转悠的嵌入应用系统。
- mysqlab是由连哥哥瑞典人和一个芬兰人创办
- 再2008年出，sun micrsystem 收购了mysqlab公司。再2009年，oracle收购了sun公司，使mysql并入oracle的数据库产品线

#### 4、什么是表

- 表：table
- table是数据可的基本组成单位，所有的数据都以表格的形式组织，可读性抢
- 一个表包括行和列
  - 行；：被称为数据，记录
  - 列：被称为字段（column）
- 每一个字段包含：字段名、数据类型、相关的约束

#### 5、数据库分类

- dql：查询语句，select都是dql
- DML：insert delete update。对表中的数据进行增删改
- DDL：create drop update，对标结构增删改
- TCL：事务  commit提交事务，rollback回滚事务
- DCL：（数据库控制语言）grant授权、revoke撤销权限

#### 6、导入数据

- 登录mysql数据库管理系统  mysql -uroot -p123456
- 擦好看有哪些数据库：show databases；
- 创建数据库：create database ***；
- 使用数据库：use ***；

- 查看当前数据库有那些表：show tables
- 初始化：source   文件路径

####  7、sql脚本理解

当一个文件的扩展名是.sql，并且改文件中比那些了大量的sql语句，我们称这样的文件为sql脚本

#### 8、简单的查询语句（DQL）

##### 1）条件查询

```sql
select 字段，字段...
from 表名
where 条件

select ename from emp where sal=500;

//找出工资高于3000
select ename,sal from emp where sal>3000;
//不等于3000
select ename,sal from emp where sal<>3000;

```

##### 2）between ....and ....

```sql
select ename from emp
where ename between 'a' and 'b';


select ename,sal from emp where sal between 1000 and 2000;[1000,2000]
```

##### 3）数据库中，null不是一个值，代表什么也没有，不是一个值，不能用等号衡量  只能用is null或者is not null；

```sql
select ename,sal,comm from emp where comm is not null;


select ename,job from emp where job='a' or job='b';
select ename,job from emp where job in('a','b');//in后面的值不是区间，是具体的值

select ename,sal,deptno from emp where sal>1000 and deptno >20 or detno=30;
//and比or的优先级高


```

##### 3)模糊查询:

- %代表任意多个字符，_代表任意一个字符

```sql
//找出名字中有o的
select 
ename 
from emp 
where ename like '_A%'//名字第二个字母是a的  '\是转义字符'
```

##### 4)排序：asc 升序   desc降序

```sql
select
	ename，sql
from emp
order by sal //升序 默认升序


//按照工资降序排，当工资一样，按照名字升序排
select ename,sal from order by sal desc,ename asc;//约靠前的字段，主导地位约强

//找出工作岗位是a的员工，并且要求按照薪资降序排
select 
	ename,job,sal
from 
	emp
where
	job='a'
order by
	sal desc;
```

##### 5)分组函数:特点是输入多行，最终结果是1行，而且自动忽略null

- count

- sum

- avg

- max

- min

- 所有的分株函数都属对某一组数据进行操作

  ```sql
  //找出工资总和
  select sum(sal) from emp
  
  ```

##### 6)单行处理函数：输入一行，输出一行。

```sql
//计算每个员工年薪
select ename,(sal+comm)*12 as year yearsal from emp;//数据库中，有一个null进行了运算，结果都是null
select ename,(sal+ifnull(comm,0))*12 as year yearsal from emp;
```

##### 7)group by 和having

- group by 按照某个字段或者某些字段进行分组

- having ：having是对分组之后的数据进行再次过滤

  ```sql
  //案例：找出每个工作岗位的最高薪资
  select max(sal) from emp group by job;
  ```

  **分组函数一般都会和group by联合使用，这也是为什么会称为联合很熟的原因**

  并且任何一个分组函数（count，sum，avg，max，min）都是在group by语句执行结束之后执行的

  ```sql
  select ename,sal from emp where sal>avg(sal);
  //编译错误 原因：sql语句中有一个语法规则，分组函数不可直接在where子句中。
  //因为：group by 是在where执行之后才执行的
  ```

##### 8)语句执行顺序：

​	select  5

​	from 1

​	where  2

​	group by  3

​	having 	4

​	order by  6

​	limit 7

当一条语句中有group by ，select 后面只能跟分组函数

##### 9)having

找出每个部门的最高薪资，要求显示薪资大于2500的数据

```sql
select max(sal),depno from emp group by deptno having max(sal)>2900;//效率低

找出每个部门的平均薪资，要求显示薪资大于2000的
select deptno,avg(sal) from emp group by deptno having avg(sal)>2000;
```

分组之后算出来的用having ，不用算选where，where后面不能用分组函数

##### 10)查询结果去重

```sql
select distinct job from emp//distinct去重
```

distinct只能出现在所有字段的最前放

##### 11)连接查询

###### 内连接：

在实际开发中，大部分的情况下都不是单表中查询数据，一般都是多张表联合查询取出最终的结果。

在实际开发中，一般一个业务都会有多张表。

1. 连接查询分类？

   - 根据语法出现划分。包括

2. 在表的连接查询方面有一种现象，被称为笛卡尔积现象

   - 案例找出每个员工的部门名称，要求显示员工名和部门名

     ```sql
     select enamel,dname from emp, dept;
     ```

   - 笛卡尔积现象：当两张表进行连接查询的时候，没有任何条件进行闲置，最终的查询结果体是两张表记录的乘积

3. 避免笛卡及现象

   - 避免笛卡尔积现象，会减少记录的匹配次数吗？不会，次数还是56次，只不过显示的是有效记录。

   - ```sql
     select e.ename,b.bname from emp e,dept d
     where e.deptno=d.deptno;//sql92老语法
     ```

4. 内连接值等职连接：最大特点是：条件是等量关系

   - 查询每个员工的部门名称，要求显示员工名和部门名

     ```sql
     select//不常用
     	e.ename,d.dname
     from
     	emp e,dept d
     where 
     	e.deptno=d.deptno
     	
     //99sql
     select 
     	e.ename,d.dname
     from 
     	emp e
     join dept d
     on e.deptno=d.deptno;
     //语法结构清晰一i但，表连接条件和条件分离开
     ```

5. 自联结：最大的特点是，一张表堪称两张表。自己连接自己

   - 找出每个员工的上级领导，显示员工名和对应领导名

     ```sql
     select a.ename as '员工名'，b.ename as '领导名'
     from emp a
     /*inner*/ join emp b
     on a.mgr=b.empno
     ```

###### 外连接：

1. 什么是外连接，和内连接的区别

   - 内连接：假设a和b进行连接，使用内连接的话，凡是a表和b表能够匹配上的记录查询出来，这就是内连接。
   - 外连接：假设a和b进行连接，使用外连接的话，ab两张表中有一张表是主表，一张表是附表。主要拆线呢主表中的数据，捎带查询副表，当副标中的数据没有和主表中的数据匹配上，附表自动模拟出null值与之匹配

2. 分类：

   - 左外连接（左连接）：表示左边的这张表是主表
   - 右连接：表示右边的这张表是主表
   - 左连接有右连接的写法，反之亦然

3. 案例：

   ```sql
   //找出每个员工的上级领导（所有员工必须查不出来）
   select a.ename as'员工名',b.ename as '领导名'
   from emp a
   left /*outer*/ join emp b//outer可以省略
   on a.mgr=b.empno
   ```

   外连接特点：主表的数据无条件查询出来

   ```sql
   //找出那个部门没有员工
   select
   a.*,b.*
   from emp a
   join dept b
   no e.depno=d.deptno
   where e.ename is null;
   ```

#### 9、子查询

###### 1）什么是子查询？子查询都可以出现在哪里？

- select语句中嵌套select语句，被嵌套的select语句是子查询。子查询可以出现在哪里

  ```sql
  select
  	...(select)
  from
  	...(select)
  where
  	...(select)
  ```

###### 2）where子句中使用子查询

案例：找出高于平均薪资的员工信息

```sql
select * from emp where sal>avq(sal)//错误写法，where后面不能直接使用分组函数
//嵌套写
select * from emp where sal>(select avg(sal) from emp);
```

###### 3)from后面嵌套子查询

案例：找出每个部门平均薪水的薪资等级

```sql
//找出每个部门平均薪水
select deptno,avg(sal) as avgsal from emp group by deptno;

//将以上查询结果当作零食表t，让t和salgrade连接
select
	t.*,s.grade
from 
	(select deptno,avg(sal) as avgsal from emp group by deptno;) t 
join 
	salgrade s
on t.avgsal between s.losal and s.hisal;
```

案例：

```sql
//找出每个部门平均的薪水等级
//找出每个员工的薪水等级
select 
	e.ename,e.sal,s.grade
from
	emp e
join 
	salgrade s
on 
	e.sal between s.losal and s.higsal
group by 
	e.deptno;

```

4)在select后面嵌套子查询

案例：找出每个员工所在部门名称，要求显示员工名和部门名

```sql
select e.ename,(select d.dname from dept d where e.deptno=d.deptno) as dname
from 
	emp e;
```

###### 4)union(可以将查询结果相加)

案例：找出工作岗位是 a和b的员工

- 第一种：select * from emp where job='a' or job='b';
- 第二种：select * from emp where job in('a','b');
- 第三种：select * from emp where job='a' union select * from emp where job='b'; 

两张不相干的表拼接在一起

###### 5）limit（重点、分页查询）

1. limit是mysql特有的，其他数据库中没有，不通用

2. limit取结果集中的部分数据，这是他的作用

3. 语法机制： limit  startIndex length

   - startIndex表示其实位置

   - length表示取几个

   - 案例

     ```sql
     select ename,sal from emp order by sal desc;
     select ename,sal from emp order by desc limit 0,5;
     select ename,sal from emp order by desc limit 5;
     
     ```

4. limit是sql语句最后执行的环节

5. 案例：找出员工工资排名在第4到第9的员工

   ```sql
   select ename,sal from emp order by sal desc limit 3,6
   ```

6. 通用的标准分页sql

   - 每页显示3条记录

   - 0，3

   - 3，3

   - 6，3

   - 9，3

   - 12，3

   - 煤业显示pageSize记录：第pageNo页：(pageNo-1)*pageSzie，pageSize

     ```java
     int pageNo=2;
     int pageSize=10;
     limit((pageNo-1)*pageSize,pageSize);
     ```

7. 创建表:

   ```sql
   creat table 表明(
   	字段名1 数据类型
   	字段名2 数据类型
   	.......
   );
   ```

   - 关于Mysql中的字段数据类型
     - int整数型
     - bigint
     - float
     - double
     - char定长字符串
     - varchar可边长字符串（stringBuffer/stringBuider）
     - date
     - BLOB二进制大对象（存储图片、视频等流媒体信息）Binary Large OBject
     - CLOB字符大对象（存储较大文本，比如，可以存储4g的字符串）
   - char和varchar的选择
   - BLOB和CLOB类型

8. insert语句插入数据

   - 语法格式：insert into 表名(名1，名2...) values（值1，’zhangsan ‘）

   - ```sql
     insert into t_student(no ,name,classno,birth) values(.........)
     //一次插入多行数据
     insert into t_student(no ,name,classno,birth) values(.........),(),();
     ```

9. 表的复制

   - create table 表名 as select语句；

10. 查询结果插入到一张表中

    - insert into dept1 select * from dept;

11. 修改表中的数据

    ```sql
    udate det1 set loc='a',/*注意这里是，不是and*/dname='b' where deptno=10;
    
    ```

12. 删除数据

    - delete from 表名 where

13. 删除大表（数据量特别大）

    - truncate table empl;//表被截断，不会回滚，永久丢失

###### 6）约束？常见约束

1. 什么是约束（唯一约束）   password（非空约束）在创建表的时候，可以给表单字段添加相应的越苏，添加约束的目的是为了保证表中数据的合法性、有效性、完整性

2. 常见的约束：

   - 非空约束 not null
   - 唯一约束 unique
   - 主键约束 primary key
   - 外键约束 foreign key
   - 检测约束 check 主义oracle数据库右check越苏，但是mysql没有，目前，mysql不支持该约束

   ###### 非空约束not null

   ```
   drop table if exists t_user;
   create table t_user(
   	id int,
   	username varchar(255) not null,
   	password varchar(255)
   );
   ```

   ###### 唯一性约束（unique）

   - 唯一约束修饰的字段具有唯一性，不能重复。但可以为null

   ###### 主键约束

   - ```sql
     create table t_user(
     	id int primary key,
     	varchar name
     
     )
     ```

   - 主键字段数据不能为null，也不能为空

   - 主键相关的术语主键约束，主键字段，主键值

   - 主键作用：

     - 表的设计三范式中的要求，第一范式要求任何一张表都应该有主键。
     - 主键的作用：是一条记录的唯一标识

   - 分类：

     - 字段数量：

       - 单一主键
       - 符合主键

     - 根据主键性质：

       - 自然主键:主键值最好是一个和业务没有关系的自然数
       - 业务主键：主键值跟业务挂钩

     - **一张表的主键约束只能由1个**

     - 使用表级约束定义主键

       ```sql
       create table t_user(
       	id int ,
       	username varchar(255),
       	password varchar(255),
       	primary key(id)
       );
       ```

     - mysql提供主键值自增(非常重要)

       ```sql
       create table t_user(
       	id int primary key auto_increment,
       	username varchar(255),
       	password varchar(255),
       	primary key(id)
       );
       
       ```

   ###### 外键约束

   - 外键：foreign key

   - 字段：

   - 外键值

   - 业务背景

     - 请设计数据库表，用来维护学生和班级的信息
     - t_class:cno(pk),cname
     - t_student:sno(pk),sname,classno(fk)

   - t_student中的classno字段引用t_class表中的cno字段，此时t_student叫做子表。t_class叫做父表

   - 添加数据时，线添加父表，再创建子表

   - 删除表时，先删除父表，再删除父表

     ```sql
     create table t_class(
     	cno int primary key auto_increament,
     	cname varchar(255)
     );
     create table t_student(
     	sno int primary key auto_increment,
     	sname varchar(),
     	classno int,
     	foreign key(classno) reference t_class(cno)
     ); //外键可以为null
     ```

   - 外键字段引用其他表的字段的时候，必须是主键吗？

     - 可以不是主键，但是必须有unique约束

#### 10、存储引擎

就是表的存储方式

1. 完整的加标语句

   ```sql
   create table t_student(
   	sno int primary key auto_increment,
   	sname varchar(),
   	classno int,
   	foreign key(classno) reference t_class(cno)
   )ENGINE=InnoDB DEFAULT CHARSET=utf-8; //外键可以为null
   ```

2. 存储引擎

   - 存储硬气这个名字只有再mysql中存在。（oracle中有对应的机制）
   - mysql支持很多存储引擎，每一个存储引擎都对应了一种不同的存储方式。每一个存储引擎都有自己的优缺点，需要再合适的时机选择合适的存储引擎

3. 查看当前mysql支持的存储引擎

4. 常见存储引擎

   - myisam 不支持事务。最常用的存储引擎，但不是默认的
     - 特征：
       - 格式文件-存储结构的定义（.frm）
       - 数据文件-存储表行的内容（.MYD）
       - 索引文件-存储表上索引（.MYI）
     - 灵活的Auto_INCrement字段处理
     - 可被转换为压缩、只读表节省空间
   - Innodb存储引擎
     - 支持事务。这种存储引擎的数据安全得到保障。行级锁，外键
     - 特征： 
       - 每个Innodb表再数据库目录中以.frm格式表示
       - innodb表空间tablespace被用存储表的内容
       - 提供一组用来记录事务性活动的日志文件
       - 用commit提交，savepoint及rollback回滚支持事务处理
       - 提供acid兼容
       - 在mysql服务器崩溃后提供自动恢复
       - 多版本和行级锁
       - 支持外键及引用的完整性，包括级联删除和更新
   - memory
     - 缺点：不支持事务，数据容易丢失，因为所有数据和索引都是存储在内存中
     - 优点：查询速度最快，表级锁机制，不包含text和BLOB字段

#### 11、事务（Transaction）

1. 什么是事务

   - 一个事务时一个完整的业务逻辑单元，不再分

     - 比如银行账户a向b转100

       ```sql
       update t_act set balance =banlance-100 where actno='a'
       update t_act set balance =banlance+100 where actno='b'
       
       ```

       以上两体跳DML语句必须同时成功或者同时失败，不允许出现一体哦啊成功，一条失败。

       想要保证以上的两条dml语句同时成功或者同时失败，那么就需要使用数据库的事务机制

2. 和事务相关的语句只有DML语句。（insert delete update）

   - 因为他们这三个语句都是和数据库中数据相关的，事务的存在为了保证数据的完整性，安全性。

3. 假设所有业务都能用一条DML语句搞定，就没有使用事务的必要。

   - 通常一个业务都需要多条DML语句共同联合完成

4. 事务原理：![image-20211003172133223](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211003172133223.png)

5. 事务特性ACID

   - A原子性：事务是最小的工作单元，不可再分
   - C一致性：同时保证多条DML语句同时成功或者失败
   - I隔离性：事务a与b直接有隔离性
   - D持久性：说得是，最终数据必须持久化到硬盘中，事务才算成功的结束。

6. 事务之间的隔离性

   - 事务隔离性存在隔离级别，理论上级别包括四个：
     - 第一级别：读未提交（read uncommitted）对方事务还没提交，我们当前事务可以读取到对方未提交的数据。会出现脏读现象：读到了脏的数据，不稳定的数据
     - 第二级别：读已提交（read committed）对方事务提，解决了脏读现象。交之后的数据我方可以读取到。存在问题：不可重复读
     - 第三级别：可重复读（repeatable read）这种隔离界别解决了不可重复读的问题。存在问题：读取到的数据是幻像
     - 第四级别：序列化/串行化。解决了所有问题。效率低，需要事务排队
   - oracle数据库默认级别是：读已提交（二级）
   - mysql是可重复读（三级）

7. 演示事务(start transaction)

   - mysql事务默认情况下是自动提交的

#### 12、索引

1. 什么是索引

   - 索引相当于一本书的目录，通过目录可以快速的找到对应的资源。在数据库方面，查询一张表的时候有两种健身方式
     - 全表扫描
     - 根据索引检索（效率很高）
   - 为什么效率高
     - 其实最根本的原理是缩小了扫描范围
   - 索引虽然可以提高效率，但是不能随意的添加，因为索引也是数据库当中的对象，也需要数据库不断的维护。是由维护成本的。比如，表汇总的数据经常被修改，这样就不适合添加索引，因为数据一旦修改，索引需要重新排序，进行维护
   - select ename,sal from emp whee ename=’a‘
   - 当ename字段上没有添加索引的时候，以上sql语句会进行全表扫描，扫描ename字段中所有的值
   - 当ename字段上有索引的时候，以上sql语句会根据索引扫描，快速定位

2. 怎么创建索引对象，怎么删除

   ```sql
   create index 索引名称 on 表名;
   drop index 索引名 on 表名;
   ```

3. **什么时候考虑差索引**

   - 数据量大。
   - 该字段很少DML操作
   - 该字段经常被查找

4. 注意：主键和具有unique字段的会自动添加索引。根据主键查效率很高，尽量根据主键检索

5. 查看sql语句explain +“选择语句”+\

6. 索引底层采用的数据结构：B+Tree

7. 实现的基本原理

   ```sql
   select ename from emp where ename='a'
   //当enamel字段上没有索引的时候，会进行全表扫描，效率低，给ename字段添加索引
   /*
   首先是ename字段，这个时候会查看ename字段有没有对应的索引。结果找到了ename字段对应的索引对象：emp_ename_index,然后通过索引检索。首先ename='SMITH'，首先定位s取，继续定位m区，然后缩短数量，很快定位SMITH
   */
   create index emp_index on emp(ename);
   ```

   ![image-20211003182032457](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211003182032457.png)

8. ​	通过B Tree缩小扫描范围，底层索引进行了排序，分区，索引会携带数据在表中的物理地址，最终通过索引检索到数据之后，获取关联的物理地址，通过物理地址定位到表中的数据，效率是最高的

   ```sql
   select ename from emp where ename='a'
   //通过索引之后：
   select ename from emp where 物理地址=0x22222
   ```

9. 索引分类

   - 单一索引：给单个字段添加索引
   - 符合索引：给多个字段联合起来添加1个索引
   - 主键索引：主键上回自动添加索引
   - 唯一索引：由unique约束的字段上回自动添加索引

10. 什么时候索引回失效

    - select ename from emp where ename like '%A%';
    - 模糊查询的时候，第一个通配符使用的是%，这个时候索引是失效的。

#### 13、视图（view）

1. 什么是试图：

   - 站在不同的角度区看到数据（同一张表的数据，通过不同的角度去看）

2. 怎么创建试图？

   - ```sqlite
     create view myview as select empno,ename from emp;
     ```

3. 对试图进行增删改查，回影响到原表数据。（通过视图影响原表）

4. 面向试图操作

5. 视图的作用？

   - 保密性问题，可以隐藏表的实现细节。高级保密级别只会向外提供相关的试图。不会提高查询效率

#### 14、DBA命令（数据库的导入导出）

1. 将数据库中的数据导出

   - 在cmd下：mysqldump 、***.sql -uroot -p123

2. 导入

   - ```sql
     create data xf
     use xf
     source xxx
     ```

#### 15、数据库设计三范式

1. 什么是设计模式
   - 设计表的依据。按照这个三范式设计的表，不会出现数据冗余
2. 三范式：
   - 第一范式：任何一张表都有对应的主键。并且每一个字段不可再分
   - 第二范式：所有非主键字段完全依赖主键，不能产生部分依赖：**多对多 ，三张表，关系表两外键**
   - 三范式：所有非主键字段直接依赖主键，不能产生传递依赖:**一对多两张表，多的表加外键**
   - 提醒：在实际开发中，以满足客户需求为主，有时候会拿冗余换速度
3. 一对一怎么设计：
   - 用户信息和用户登录表是两张表
   - 用户登录表和用户详细信息表
   - 设计方案有两种方法
     - 主键共享：![image-20211003185658016](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211003185658016.png)
     - 外键唯一（由一对多演变过来，外键加unique约束，相当于是不能多，就是一对一）：![image-20211003185834719](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211003185834719.png)

#### 

### 复习

#### 事务的特性



A:原子性：事务是最小的工作单元，不可再分

C:一致性：事务必须保证多条DML语句同时成功或者同时失败

I：隔离性：事务a与事务b之间具有隔离，是单独互不影响的

D：持久性：最终数据必须持久化到硬盘文件中，事务才算成功结束

事务隔离性存在隔离级别：

第一级别：读未提交（read uncommited），对方事务还没有提交，我们事务可以读取未提交的数据

存在脏读现象：表示读到了脏数据。不稳定的，都是在内存里面

第二级别：读已提交（read committed）对方事务提交之后的数据我方可以读取到。

存在问题：不可重复读

第三级别：可重复读（repeated read）

解决了不可重复读问题，存在读取到的数据是幻想

幻读

第四级别：序列化/串行化    事务必须串行



隔离，原子，持久，一致

#### 关于幻读与其解决：

1、多版本并发控制（MVCC）（快照读/一致性读）

以innodb为例。可以理解为每一行中都冗余了两个字段。一个行的创建版本，一个是行的删除版本。具体的版本号（trx_id）存在information_schema.INNODB_TRX表中

版本号（trx_id）随着每次事务的开启自增。

事务每次读取数据的时候都会创建版本小于当前事务版本的数据，以及国企版本大于当前版本的数据。

原理：

#### 数据库外键设置：

1. cascade：在父表上update/delete记录时，同步update/delete掉子表的匹配记录
2. set null：在父表上update/delete记录时，将子表上匹配记录的列设为null，要注意子表的外键不能位not null
3. no action：如果子表中有匹配的记录，则不郧西对父表对应候选键进行update/delete操作
4. restrict：和no action意义
5. set default 附表有变更时，子表将外键设置成一个默认的值，但innodb不能识别

#### 索引类型：

1. 主键索引（primary）：主键索引是一种唯一索引，但它必须指定位primary key，每个表只能有一个主键

   ```sql
   alert table tablename add primary key()
   ```

   

2. 唯一索引（unique）:索引列的所有值都只能出现一次，即必须唯一，值可以位空

3. 普通索引（key）：基本的所有类型，值可以位空，没有唯一性的限制

   ```sql
   alter table table_name add index();
   ```

   

4. 全文索引（fulltext）：全文索引可以在varchar、char，text类型的列上创建。可以通过alter table或create index闯进，对于到规模的数据库，通过alter table或者create index命令创建全文索引要比把记录插入带有全文索引的空表快。不支持中文

   ```sql
   alter table  xx add fulltext（）
   ```

#### utf-8和utf-8mb4

mysql中的utf8并不是真正的utf-8，mysqlutf8最多支持三个字节，而emoji表情，一些特殊的中文字符需要四个字节才能存储。因此会报错。











