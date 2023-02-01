





### Mybatis

### MyBatis SQL Mapper Framwork for java

增强的jdbc

#### 第一章框架概述

##### 1、三层架构

 	界面层：和用户打交道，接受用户的请求参数，显示处理结果（jsp，html。servlet）

​	业务逻辑层：接受了界面层传递的数据，计算逻辑，调用数据库，获取数据

​	数据访问层：及随后访问数据库，执行对数据的擦汗寻，修改，删除等待。

三层对应的包：

- 界面层：controller（servlet）
- 业务逻辑层：service包（service类）
- 数据访问层：dao包

三层类的交互

​	用户使用界面层-》业务逻辑层--》数据访问层（持久层）--》数据库（mysql）

三层对应的处理框架

- 页面层：servlet---springmvc（框架）
- 业务逻辑层：---service类--spring（框架）
- 数据访问层：---dao类---mybatis（框架）、、规模较小

#####   2、框架：（Framework）

​	框架时一个舞台，一个模板

模板：

1. 规定了好一些条款，内容
2. 加入自己的东西

框架是一个模块

- 框架中定义了一些功能，这些功能是可用的。
- 可以加入项目中自己的功能，这些功能可以利用框架中写好的功能

框架是一个软件，半成品的软件，定义好了一些功能，需要加入你的功能就是完整的。基础功能是可重复使用的，可升级的。

框架特点：

1. 框架一般不是全能的，不能做所有事情
2. 框架是针对一个领域有效。特长在某一个方面，比如mybatis做数据库操作强。但是它不能做其他的

##### 3、jdbc缺点（可替代性）

1. 代码比较多，开发效率低
2. 需要关注connection，statement，Resultset对象的创建和销毁
3. 对ResultSet查询的结果，需要自己封装为List
4. 重复的代码比较多
5. 业务代码和数据库的操作混在一起

##### 4、MyBatis框架概述

Mybatis框架

​	mybatis本是apache的一个开源项目iBatis，2010年这个项目有apache Software fundation迁移到了google code，并且改名为MyBatis。2013年迁移到GitHub

​	iBatis一次来源于internet和abatis的组合。是一个基于java的持久层框架。iBatis提供持久层框架包括SQl Maps和Data Access Object（DAOs）

作用

- sql mapper：sql映射
  - 可以把数据库表中的一行数据，映射为一个java对象。一行数据可以看作是一个java对象。操作这个对象，就相当于操作表中的数据
- Data Access Object（DAOs）：数据访问，对数据库进行增删改查

###### mybatis提供了那些功能：

1. 提供了创建Connection，statement，ResultSet的能力，不用开发人员创建这些对象

2. 提供了执行sql语句的能力，不用你执行sql

3. 提供了循环sql，把sql的结果转为java对象，List集合的能力

   ```java
   while(rs.next()){
   	Student stu=new Student();
   	stu.setId(rs.getInt());
   	stuList.add(stu);
   }
   ```

4. 提供了关闭资源的能力，不用我们手动关闭Connection，statement，ResultSet

开发人员的作用是：提供sql语句

最后是：开发人员提供sql语句  --mybatis处理sql---开发人员得到List集合或java对象

总计：

​	mybatis是一个sql映射框架，提供的数据库的操作能力，增强的JDBC，使用mybatis让开发人员集中精神写sql就可以了，不必关心Connection，statement ，resultset的创建和销毁。

#### 第二章

##### 1.主要类的介绍

- Resources:mybatis中的一个类，负责读取主配置文件

  ​	inputStream in=Resources.getResourceAsStream("mybatis.xml");

- SqlSessionFactoryBuilder:创建sqlSessionFactory对象，sqlSessionFactoryBuilder builder=new sqlsessionFactoryBuilder();

  创建sqlsessionfactory对象

  sqlSessionFactory factory =builder.build();

- sqlSessionFactory ：重量级对象，程序创建一个对象耗时比较长，使用资源比较多。在整个项目中，一个就够用了

  sqlsessionfactory接口，接口实现类：DefaultSqlSessionbfactory

  sqlSessionFactory作用：获取sqlsession对象。sqlsession sqlsession=factory.openSession();

  openSession()方法说明

  1. openSession()无参数的，获取是非自动提交事务的sqlsession对象
  2. openssion（boolean）：true 获取自动的，false是非自动的

- sqlSession：是个接口定义了操作数据库的方法如：selectone，selectList，insert（），update（），delete，commit，roll

  使用要求：sqlsession对象不是线程安全的，需要在方法内部使用，在执行sql语句之前，使用opensession获取sqlsession对象。在执行完sql语句后，需要关闭它，执行sqlsession.close，这样能保证是线程安全的

##### 获取sqlSession对象工具类

```java
public class MyBatisUtil {
    private static SqlSessionFactory factory=null;
    static {
        String config="mybatis.xml";
        try {
            InputStream in= Resources.getResourceAsStream(config);
            factory=new SqlSessionFactoryBuilder().build(in);
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
    //获取sqlsession的方法
    public static SqlSession getSqlSession(){
        SqlSession sqlSession=null;
        if (factory!=null){
            sqlSession=factory.openSession();
        }
        return sqlSession;
    }
}
```

##### mapper.xml代码

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http//mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.xf.dao.StudentDao">
    <select id="selectStudents" resultType="org.xf.domain.Student">
        select id,name,email,age from student order by id
    </select>
</mapper>

<!--
sql映射文件：写sql语句的，mybatis会执行这些sql
1.指定约束文件
    <!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http//mybatis.org/dtd/mybatis-3-mapper.dtd">
        mybatis-3-mapper.dtd是越苏文件的名称，扩展名是dtd
2、约束文件作用：限制，检查在当前文件中出现的标签，属性必须符合mybatis的要求
3、mapper是当前文件的标签，必须的
namespace：叫做命名空间，唯一值的，可以是自定义的字符串。
要求使用dao接口的全限定名称
4、在当前文件中，可以使用特定的标签，表示数据库的特定操作
<select>
<update>
<insert>
<delete>
-->
```

##### mybatis.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DID Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC">
                <property name="..." value="..."/>
            </transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="${url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="org/mybatis/example/BlogMapper.xml"/>
    </mappers>
</configuration>
```

##### mytatis的helloworld代码

##### 目录结构

![image-20211011204323086](img/image-20211011204323086.png)

*******

##### 主类代码

```java
public static void main( String[] args ) throws IOException {
        //访问mybatis读取student数据
        //1.定义mybatis主配置文件的名称，从类路径的根目录开始
        String config="mybatis.xml";

        //2.读取这个config文件
        InputStream in=Resources.getResourceAsStream(config);

        //3.创建了sqlsessionFactoryBuilder对象
        SqlSessionFactoryBuilder builder=new SqlSessionFactoryBuilder();

        //4.创建sqlSessionFactory对象
        SqlSessionFactory factory=builder.build(in);

        //[重要]5.获取sqlsession对象，从sqlsessionfactory中获取sqlsession
        SqlSession sqlSession=factory.openSession();

        //[重要]6.指定要执行的sql语句的表示。sql映射文件中的namespace+"."+标签的id值
        String sqlId="org.xf.dao.StudentDao"+"."+"selectStudents";

        //7.执行sql语句，通过sqlId找到语句
        c

        //8.输出结果
        list.forEach(student -> System.out.println(student));

        //9.关闭sqlsession对象
        sqlSession.close();
    }
```

##### pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>org.xf</groupId>
  <artifactId>hello-mybatis</artifactId>
  <version>1.0-SNAPSHOT</version>



  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.2</version>
    </dependency>
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.13</version>
    </dependency>
  </dependencies>

    
    <!--
	此处biud里面，resource的路径必须配置两个
	一个是到src/main/java的路径
	一个是到src/main路径
·	如果不配置后者，则无法将mybatis文件编译至classess文件，使得代码不能运行


这里配置的原因即使，让maven的compile将.xml文件编译至target/classes目录中，如果不配置，则compile默认只将.java文件编译至target目录。但是，我们项目运行，需要依托mybatis主类配置文件 mybatis.xml和sql语句映射文件xxxDao.xml
-->
  <build>
    <resources>
      <resource>
        <directory>src/main/java</directory><!--所在目录-->
        <includes><!--包括目录下的.properties.xml文件都会扫描到-->
          <include>**/*.properties</include>
          <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
      </resource>
      <resource>
        <directory>src/main</directory><!--所在目录-->
        <includes><!--包括目录下的.properties.xml文件都会扫描到-->
          <include>**/*.properties</include>
          <include>**/*.xml</include>
        </includes>
        <filtering>false</filtering>
      </resource>
    </resources>

  </build>
</project>

```



##### mybatis.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DID Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<!--  环境配置 ：数据库连接信息
            default 必须和某个enviroment的id值一样
            告诉mybatis使用那个数据库连接信息。也就是访问那个数据库-->
    <environments default="ssm">
<!--        一个数据库的配置环境
            id：一个唯一值，自定义表示环境名称-->
        <environment id="ssm">
<!--            transactionManager:mybatis事务类型
                type：jdbc表示使用jdbc中的connection对象的commit，rollback做事务处理-->
            <transactionManager type="JDBC"/>

<!--            datasource:表示数据源，连接数据库的
                type：表示数据源的类型，POOLED表示使用连接池-->
            <dataSource type="POOLED">

<!--                driver，user，username，password是固定的不能自定义-->
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/ssm?useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
<!--        一个mapper标签指定一个文件的位置
            从类路径开始的路径信息。
            target/classes类路径-->
        <mapper resource="org/xf/dao/StudentDao.xml"/>
    </mappers>
</configuration>
<!--
mybatis的住配置文件：主要定义了数据库的配置信息，sql
-->
```

##### studentdao.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http//mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="org.xf.dao.StudentDao">
    <select id="selectStudents" resultType="org.xf.domain.Student">
        select id,name,email,age from student order by id
    </select>
</mapper>

<!--
sql映射文件：写sql语句的，mybatis会执行这些sql
1.指定约束文件
    <!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http//mybatis.org/dtd/mybatis-3-mapper.dtd">
        mybatis-3-mapper.dtd是越苏文件的名称，扩展名是dtd
2、约束文件作用：限制，检查在当前文件中出现的标签，属性必须符合mybatis的要求
3、mapper是当前文件的标签，必须的
namespace：叫做命名空间，唯一值的，可以是自定义的字符串。
要求使用dao接口的全限定名称
4、在当前文件中，可以使用特定的标签，表示数据库的特定操作
<select>
<update>
<insert>
<delete>
-->
```

##### studentdao.java

```java
public interface StudentDao {
    //查询student表的所有的数据
    public List<Student> selectStudents();
}

```

##### Student.java

```java
public class Student {
    private Integer id;
    private String name;
    private String email;
    private Integer age;
}
```

![image-20211011221004939](img/image-20211011221004939.png)

#### 第三章

##### 1、动态代理：使用sqlsession.getMapper（dao接口.class）获取这个dao接口的对象

##### 2、传入参数：从java代码中把数据传入到mapper文件的sql语句中

1. parameterType：卸载mapper文件中的一个属性。表示接口中方法 的参数类型。

   例如studentDao接口

   public Student selectStudentById(Integer id)![image-20211013170137171](img/image-20211013170137171.png)

2. 多个参数，使用@Param命名参数

   接口public List<Stduent>selectMulitParam(@Para("myname")String name,@Param("myage")Integer age)![image-20211013174016685](img/image-20211013174016685.png)

   

##### $和#两个占位符的区别

#：告诉没有batis使用实际的参数值替代，并用prepareStatement对象执行sql语句，#{..}替代sql语句的？。这样更安全更迅速，也是通常的首选做法。![image-20211013175349904](img/image-20211013175349904.png)

$:使用时，sql语句做的是字符串的拼接，使用的是statement对象执行sql，效率比preparedStatement较低，并且会有sql注入到 问题。

$可以用来替换列名或者表名

可以替换表名或者列名，你能确定数据是安全的。可以使用$

1. #使用？在sql语句中做占位符，使用preparedStatement执行sql，效率高

##### 3、mybatis输出的结果

###### resulttype结果类型

指sql语句执行完毕后，数据转为java的对象，java类型是任意的

resultType它的值：类型全限定名称，类型的别名，

![image-20211013170137171](img/image-20211013170137171.png)

###### 定义类型的别名

1. 在mybatis主配置文件中定义![image-20211013181624707](img/image-20211013181624707.png)
2. ![image-20211013181802931](img/image-20211013181802931.png)用类的包名定义，然后类名就算别名，类名不区分大小写

###### 返回值是map：

map的时候，最多只能返回一行数据

###### resultMap：结果映射：

指定列名和java对象的属性对应关系

1. 你自定义列值赋值给那个属性
2. 当列名和属性名不一样时，可以这么用

##### 4、模糊查询

```xml
<select id"select" resultType="Student">
	select id ,name,email,age from student where name like #{name}
</select>

<select id"select" resultType="Student">
	select id ,name,email,age from student where name like "%" #{name} "%"
</select>
```

#### 第四章

##### 动态sql：

sql的内容是变化的，可以根据条件获取到不同的sql语句。

主要是where部分发生变化

实现：使用的是mybatis提供的标签if where foreach

###### if

```xml
<if test="判断java对象 的属性值">

</if>
```

![image-20211013184203102](img/image-20211013184203102.png)

###### where

用来包含多个if的，当多个if有一个成立，where会自动增加一个where关键字，并去掉if中多余的and or![image-20211013184642936](img/image-20211013184642936.png)

###### foreach

循环java中的数组，list集合的，主要用在sql的in语句中。学生id是1001，1002

select *from student where id in{1001,1002,1003}

```xml
<foreach collection="" item="" open="" close="" separator=""></foreach>
```

collection：表示接口中的参数的类型，如果数数组，用arry如果是list集合用list

item 自定义的，表示数组和集合成员的变量

oppen：循环开始时的字符

close：循环结束时的字符

separator：集合成员之间的分隔符

![image-20211013190537532](img/image-20211013190537532.png)

![image-20211013190617918](img/image-20211013190617918.png)

#### 第五章

##### 1、数据库的属性配置文件：

1. 把数据库连接信息放到一个单独的文件中。和mybatis主配置文件分开。目的时便于修改。定义数据，格式时key=value

   key：一般使用，做多级目录

   ```properties
   jdbc.dridbcver=com.mysql.Driver
   jdbc.url-jdbc:mysql//
   jdbc.username=root
   jdbc.password=123456
   ```

2. 在mybatis的主配置文件，用property标签指定文件位置

![image-20211013192911880](img/image-20211013192911880.png)

#### 第六章

##### PageHelper数据分页

做数据分页![image-20211013193414345](img/image-20211013193414345.png)



pox.xml中放入





在mybati.xml  environment标签前面放入

```xml
<plugins>
    <plugin interceptor="com.github.pagehelper.PageInterceptor"/>
</plugins>
```

```xml
<plugins>
    <plugin interceptor="com.github.pagehelper.PageInterceptor">
        <!--reasonable：分页合理化参数，默认值为false,直接根据参数进行查询。
         当该参数设置为 true 时，pageNum<=0 时会查询第一页， pageNum>pages（超过总数时），会查询最后一页。-->
        <!--<property name="reasonable" value="true"/>-->
    </plugin>
</plugins>
```

#### 第七章缓存

##### 一、一级缓存

​	mybatis对缓存提供支持，但是在没有配置的默认情况下，它只开启一级缓存，一级缓存只是相对于同一个sqlSession而言。所以在参数和sql完全一样的情况下，我们使用同一个sqlsession对用一个mapper方法，往往只执行一次sql，因为使用selsession第一次查询后，mybatis会将其放在缓存中，以后再查询的时候，如果没有声明需要刷新，并且缓存没有超时的情况下，sqlsession都会取出当前缓存的数据，而不会再次发送sql到数据库![image-20211014140502106](img/image-20211014140502106.png)

###### 1、一级缓存的生命周期

**一级缓存指的就是sqlsession，在sqlsession中有一个数据区域，是map结构，这个区域就是一级缓存区域。一级缓存中的key是由sql语句、条件、statement等信息组成一个唯一值。一级缓存中的value，就是查询出的结果对象。
Map<String,Object>
如果查完后，增删改操作，清空缓存
**

1. mybatis在开启一个数据库会话时，会创建一个新的sqlsession对象，sqlsession对象中会有一个新的executor对象。executor中会有一个新的executor对象，executor对象中持有一个新的perpetualChache对象；当前会话结束时，sqlsession对象及其内部的executor对象还有perpetualcache对象也一并释放掉。
2. 如果sqlsession调用了close方法，会释放掉一级缓存perpetualcache对象，一级缓存将不可用
3. 如果sqlsession调用了clear方法，会清空perpetualcache对象中的数据，但是该对象任然可以使用
4. sqlsession中执行了任何一个update操作，都会清空perpetualcache对象的数据，但是该对象会继续使用

###### 2、怎么判断两次查询时完全相同的查询

​	mybatis认为，对于两次查询，如果以下条件完全一样，那么就认为他们是完全相同的两次查询。

1. 传入的statementId
2. 查询时要求的结果集中的结果范围
3. 这次查询所产生的最终要传递给jdbc java.sql.preparedstatement的sql语句字符串（boundSql.getSql）
4. 传递给java.sql.Statement要设置的参数值

##### 二、二级缓存



​	mybatis的二级缓存是application级别的缓存，它可以提高对数据库查询的效率，以提高应用的性能![img](img/XIC78@{O2[M{QNJU]7X$%MU.png)

​	sqlsessionfactory层面上的二级缓存默认是不开启的，二级缓存的开启需要进行配置，实现二级缓存的时候，mybatis要求返回的pojo必须是可序列化的。也就是要求实现serializable接口，配置方法很简单，只需要在映射xml文件配置就可以开启缓存<cahce/>,如果我们配置了二级缓存就意味着：

- 映射语句文件中的所有select语句将被缓存。
- 映射语句文件中的所有insert、update和delete语句会刷新缓存
- 缓存会使用默认的Least Recentil Used（LRU，最近最少使用的算法）来回收
- 根据时间表，比如No flush Interval，（CNFI美哟刷新间隔），缓存不会以任何时间顺序刷新
- 缓存会存储列表集合或者对象（无论查询方法返回什么）的1024个引用
- 缓存会被视为是read/write（可读/可写）的缓存，意味着对象检索不是共享的，而且可以安全的被调用者修改，不干扰其他调用者或线程所作的现在修改

###### 映射文件的缓存配置

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yihaomen.mybatis.dao.StudentMapper">
    <!--开启本mapper的namespace下的二级缓存-->
    <!--
        eviction:代表的是缓存回收策略，目前MyBatis提供以下策略。
        (1) LRU,最近最少使用的，一处最长时间不用的对象
        (2) FIFO,先进先出，按对象进入缓存的顺序来移除他们
        (3) SOFT,软引用，移除基于垃圾回收器状态和软引用规则的对象
        (4) WEAK,弱引用，更积极的移除基于垃圾收集器状态和弱引用规则的对象。这里采用的是LRU，
                移除最长时间不用的对形象

        flushInterval:刷新间隔时间，单位为毫秒，这里配置的是100秒刷新，如果你不配置它，那么当
        SQL被执行的时候才会去刷新缓存。

        size:引用数目，一个正整数，代表缓存最多可以存储多少个对象，不宜设置过大。设置过大会导致内存溢出。
        这里配置的是1024个对象

        readOnly:只读，意味着缓存数据只能读取而不能修改，这样设置的好处是我们可以快速读取缓存，缺点是我们没有
        办法修改缓存，他的默认值是false，不允许我们修改
    -->
    <cache eviction="LRU" flushInterval="100000" readOnly="true" size="1024"/>
    <resultMap id="studentMap" type="Student">
        <id property="id" column="id" />
        <result property="name" column="name" />
        <result property="age" column="age" />
        <result property="gender" column="gender" typeHandler="org.apache.ibatis.type.EnumOrdinalTypeHandler" />
    </resultMap>
    <resultMap id="collectionMap" type="Student" extends="studentMap">
        <collection property="teachers" ofType="Teacher">
            <id property="id" column="teach_id" />
            <result property="name" column="tname"/>
            <result property="gender" column="tgender" typeHandler="org.apache.ibatis.type.EnumOrdinalTypeHandler"/>
            <result property="subject" column="tsubject" typeHandler="org.apache.ibatis.type.EnumTypeHandler"/>
            <result property="degree" column="tdegree" javaType="string" jdbcType="VARCHAR"/>
        </collection>
    </resultMap>
    <select id="selectStudents" resultMap="collectionMap">
        SELECT
            s.id, s.name, s.gender, t.id teach_id, t.name tname, t.gender tgender, t.subject tsubject, t.degree tdegree
        FROM
            student s
        LEFT JOIN
            stu_teach_rel str
        ON
            s.id = str.stu_id
        LEFT JOIN
            teacher t
        ON
            t.id = str.teach_id
    </select>
    <!--可以通过设置useCache来规定这个sql是否开启缓存，ture是开启，false是关闭-->
    <select id="selectAllStudents" resultMap="studentMap" useCache="true">
        SELECT id, name, age FROM student
    </select>
    <!--刷新二级缓存
    <select id="selectAllStudents" resultMap="studentMap" flushCache="true">
        SELECT id, name, age FROM student
    </select>
    -->
</mapper>
```

###### 主配置文件的二级缓存配置

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <settings>
        <!--这个配置使全局的映射器(二级缓存)启用或禁用缓存-->
        <setting name="cacheEnabled" value="true" />
        .....
    </settings>
    ....
</configuration>
```



































![image-20211011181434381](img/image-20211011181434381.png)

![image-20211011181454743](img/image-20211011181454743.png)

![image-20211011181503361](img/image-20211011181503361.png)

![image-20211011181521674](img/image-20211011181521674.png)

