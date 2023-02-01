### JDBC

#### 1、JDBC是什么

- java DataBase Connectivity（java语言连接数据库）

#### 2、JDBC本质（java.sql.*;）

- JDBC是SUN公司指定的一套接口interface

- 接口有调用者和实现者

- 面向接口调用、面向接口写实现类，这都属于接口编程

- 为什么面向接口编程？

  - 解耦合：降低程序的耦合度，提高编程的扩展力

  - 堕胎机制就算非常典型的：详细抽象编程。

    - ```java
      Animal a=new Cat();
      Animal a=new Dog();
      public void feed(Animal a){//面向父类型编程
      	
      }
      不建议
      	Dog d =new Dog();
      	Cat c=new Cat();
      ```

- 思考：为什么SUN指定一套JDBC接口？

  - 因为每一个数据库的底层实现原理都不一样
  - Oracle数据库有自己的原理
  - MySql数据库也有自己的原理
  - MS SqlServer数据库也有自己的原理
  - 每一个数据库产品都有自己独特的实现原理![image-20211003224922472](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211003224922472.png)

![image-20211003230048783](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211003230048783.png)

#### 3、JDBC开发前准备工作

- 从官网下载驱动，然后将其配置到环境

#### 4、JDBC编程六步

1. 注册驱动：告诉java程序即将要连接那个牌子的数据库

   ```java
   public static void main(String [] args){
   	//1.加载驱动
       try{
           java.sql.Driver driver=new com.mysql.jdbc.Driver();
       	DriverManager.registerDriver(driver);
       //2.获取连接
           /*
           	url:统一资源定位符（网络中某个资源的绝对路径）
           	https：//www.baidu.com/这就是URL
           	URL包括哪几部分
           		协议
           		IP
           		PORT
           		资源名
           	http://182.61.200.7:80/index.html
           	http://通信协议
           	182.61.20.7服务器ip地址
           	80服务器上的软件端口
           	index.html是服务器上某个资源名
           	什么是通信协议，有什么用
           		通信协议是通信之前就提前定好的数据传送格式
           		数据包具体怎么传数据，格式提前订好的
           		
           		全国普通话协议
           	Oracle协议的URL：
           		jdbc:oracle:thin:@localhost
           :1521:数据库协议名
           */
           String url="jdbc:mysql://localhost:3306/数据库实例名";
           String user="root";
           String password="123456"
          
       	Connection conn=DriverManager.getConnection(url,user,password) ;
           //com.mysql.jdbc.JDBC4Connection@41cf53f9
       	  //3.获取数据库操作对象
       Statement stmt=conn.createStatement();
     
       //4.执行sql语句
         String sql="insert into dept() values()";
       //专门执行DML语句，delete update insert
       //返回值是影响数据库中的记录条数
       int count=stmt.executeUpdate(sql)；
           sout(count==1?"保存成功":"保存失败");
       }catch(SQLException e ){
           e.printStackTrace();
       }finally{
           //6.关闭资源通道
           try{//资源从小到大依次关闭
               if(stmt!=null)
                   stmt.close();
            
           }catch(SQLException e){
               e.printStackTrace();
               
       }
           try{
               conn.close();
           }catch(SQLException e){
               e.printStackTrace();
           }
     
       //5.处理结果集
       
       
   }
   ```

   ```java
   //jdbc完成delete
   class Main{
   	Connection conn=null;
   	Statement stmt=null;
   	public void main(String [] args){
           try{
               //1、注册驱动
   		Driver driver=new com.mysql.jdbc.Driver();
           DriverManager.registerDriver(driver);
          
   		//2、获取连接
            conn=DriverManager.getConnection("jdbc:mysql://localhost:3306/test","root","123456");
   		//3、获取数据库操作对象
           stmt=conn.createStatement();
   		//4、执行sql语句
           String sql="delete from dept where deptno=40";
           stmt.excuteUpdate(sql);
           }catch(SQLException e){
           	e.printStackTrace();
           }finally{
           	try{
                   if(stmt!=null)
                   stmt.close();
               }catch(SQlException e ){
                   e.printStackTrace();
               }
               try{
                   if(conn!=null)
                   conn.close();
               }catch(SQlException e ){
                   e.printStackTrace();
               }
           }
   		
   		//5、操作结果集
   		//6、关闭资源
   	}
   }
   ```

   ```java
   //注册驱动的另一种方式
   class Main{
   	Connection conn=null;
   	Statement stmt=null;
   	public void main(String [] args){
           try{
               //1、注册驱动
               //注册驱动的第一种方法
   		Driver driver=new com.mysql.jdbc.Driver();
           DriverManager.registerDriver(driver);
          //第二种方式  常用的
              
               /*
               	一下方式不用返回值，因为我们只想类加载动作。
               */
            Class.forName("com.mysql.jdbc.Driver");
   		//2、获取连接
            conn=DriverManager.getConnection("jdbc:mysql://localhost:3306/test","root","123456");
   		
           }
   	}
   }
   ```

   ```java
   //将数据库中所有信息配置到文件中
   /*
   	实际开发中不建议把连接数据库的信息谢思道java程序中
   */
   class Main{
       //使用资源绑定器绑定属性配置文件
       ResourceBundle bundle=ResourceBundle.getBundle("jdbc");
       String driver=bundle.getString("driver");
       String user=bundle.getString("user");
       String password=bundle.getString("password");
       String url=bundle.getString("url");
      
       
   	Connection conn=null;
   	Statement stmt=null;
   	public void main(String [] args){
           try{
               //1、注册驱动
   		class.forName("com.mysql.jdbc.Driver");
          //mysql8的话"com.mysql.cj.jdbc,Driver"
   		//2、获取连接
            conn=DriverManager.getConnection(url,user,password);
   		//3、获取数据库操作对象
           stmt=conn.createStatement();
   		//4、执行sql语句
           String sql="delete from dept where deptno=40";
           stmt.excuteUpdate(sql);
           }catch(SQLException e){
           	e.printStackTrace();
           }finally{
           	try{
                   if(stmt!=null)
                   stmt.close();
               }catch(SQlException e ){
                   e.printStackTrace();
               }
               try{
                   if(conn!=null)
                   conn.close();
               }catch(SQlException e ){
                   e.printStackTrace();
               }
           }
   		
   		//5、操作结果集
   		//6、关闭资源
   	}
   }
   ```

   ```properties
   //jdbc.properties
   
   user=com.mysql.jdbc.Driver
   url=jdbc:mysql://localhost:3306/test
   user=root
   password=123456
   
   ```

   ```java
   /*
   处理查询结果集
   */
   public class main{
       public static void main(String [] args){
       	Connection conn=null;
           Statement stmt=null;
           ResultSet rs=null;
       	try{
               Class.forName("com.mysql.jdbc.Driver");
               conn.DriverManager.getConnection(url,user,password);
            stmt=conn.createStatement();
               String sql="select * from user";
               rs=stmt.excuteQuery(sql);
               //处理查询结果集
               while(rs.next()){
                   rs.getString(3);
                   rs.getString(2);
                   rs.getString(1);//不论数据库中数据是什么类型，都以String的形式返回
                  //rs.getString("empno");//可以用列名获取
                   
                  /*
                  getInt()
                  getDouble()
                  */
               }
               
           }catch(SQlException e){
               e.printStackTrace();
       }finally{
           	rs.close();
               stmt.close();
               conn.close();
           }
       
   }
   ```

   

2. 获取连接：表示jvm的进程和数据库进程之间的通道打开了，这属于进程间之通信，重量级的使用完要关闭通道

3. 获取数据库对象：获取数据库操作对象

4. 执行sql语句：执行sql语句

5. 处理查询结果集：处理查询结果集

6. 释放资源：使用完资源之后一定要关闭资源。java和数据库属于进程间的通信，开启之后一定要关闭

#### 5、idea配置驱动

![image-20211004093009440](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211004093009440.png)

![image-20211004093024605](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211004093024605.png)

#### 6、sql注入问题

![image-20211004094451723](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211004094451723.png)

![image-20211004094440004](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211004094440004.png)

![image-20211004094416366](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211004094416366.png)

![image-20211004095632625](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211004095632625.png)

这样输的话

```sql
select * from user where name='fdsa' and password=fdsa' or '1'='1'
```

**导致sql注入的根本原因是：**

​	用户输入的信息中航油sql语句的关键字，并且这些关键字参与sql语句的编译

#### 7、关于PreparedStatement

预编译的数据库操作对象

问题解决：

​	只要用户提供的i西南西不参与sql语句的百衲衣过程，就不会有问题

​	即使用户提供的信息中含有sql语句的关键字，但是没有参与编译，不起作用

​	要想用户信息不参与sql语句的比那一，那么必须用java.sql.PreparedStatement

​	preparedStatement接口继承了java.sql.PreparedStatement

​	preparedStatement是属于编译的数据库操作对象

​	preparedStatement的原理是，预先对sql语句的框架进行编译，然后给sql语句传值

```java
public class main{
    public static void main(String [] args){
    	Connection conn=null;
        //Statement stmt=null;
        PreparedStatement ps=null;
        ResultSet rs=null;
    	try{
            Class.forName("com.mysql.jdbc.Driver");
            conn=conn.DriverManager.getConnection(url,user,password);
         stmt=conn.createStatement();
            String sql="select * from user where user=? and psd=?";
            //其中一个？表示一个占位符，一个？接受一个值，主义：占位符不能用单引号括起来
            //程序执行到此处，会发送sql语句给DBMS，然后DBMS进行sql语句的预编译
            ps=conn.prepareStatement(sql);
            //给占位符传值（第一个？下标是1，然后是2，jdbc中所有的下表从1开始）
            ps.setString(1,"root");
			ps.setInt(2,100);
            rs=ps.excutQuery();
            //处理查询结果集
            while(rs.next()){
                rs.getString(3);
                rs.getString(2);
                rs.getString(1);//不论数据库中数据是什么类型，都以String的形式返回
               //rs.getString("empno");//可以用列名获取
                
               /*
               getInt()
               getDouble()
               */
            }
            
        }catch(SQlException e){
            e.printStackTrace();
    }finally{
        	rs.close();
            stmt.close();
            conn.close();
        }
    
}
```

#### 8、statement和preparedstatement对比

- statement存在sql注入问题，preparedstatement解决了sql注入问题
- statment是编译一次执行一次，preparedStatement是编译一次可执行多次，preparedstatement效率会高一些
- preparedstatement会在编译阶段左类型的安全检查

综上所述，preparedstatement使用较多。既有在极少数情况下用statement

**什么情况下用statement？**

​	业务方面要求必须支持sql注入的时候。statement支持sql注入。范式业务方面要求是需要进行sql语句拼接的。必须使用statement

#### 9、jdbc事务机制

1. ​	jdbc中的事务是自动提交的

   - 只要任意执行一条dml语句，则自动提交一次。这是jdbc默认的事务行为。但是在实际的业务当中，通常都是n条dml同时成同时失败，才符合场景。

   - **conn.setAutoCommit(boolean autoCommit)**//设置自动提交事务

   - **conn.commit();**

   - ```java
     try{
     conn.setAutoCommit(boolean autoCommit)
     
     
     conn.commit();
     }catch(){
     	if(conn!=null){
     		conn.rollback();
     	}
     }
     ```

#### 10、JDBCUtil封装

```java
public class DButil{
    /*
    	工具类的方法都是静态的，不需要new对象，直接采用类名使用
    */
    static{
        try{
            Class.forName("com.mysql.jdbc.Driver");
        }catch(SQLException e ){
            e.printStactTrace();
        }
    }
    private DBUtil(){}
    
    //获取连接对象
    public static Connection getConnection() throws SQLException{
    	return DriverManager.getConnection("jdbc:mysql://localhost:3306/test","root","123456");
    }
    
    //关闭资源
    public static void close(Connection conn,Statement ps,ResultSet rs){
        if(rs!=null){
            try{
                rs.close();
            }catch(SQLException e){
                e.printStactTrace();
            }
            
        }
       
        if(ps!=null){
            try{
                ps.close();
            }catch(SQLException e){
                e.printStactTrace();
            }
            
        }
         if(conn!=null){
            try{
                conn.close();
            }catch(SQLException e){
                e.printStactTrace();
            }
            
        }
    }
        
}
```

#### 11、行级锁（悲观锁）

```sql
select * from emp where job='a' for update;
```

**for update**悲观锁，在事务完成之前，job=a的记录全部上锁，其他事务不能访问

****

![image-20211004105504486](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211004105504486.png)

****

#### 12、乐观锁

会回滚，rollback

![image-20211004105605407](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211004105605407.png)

第三个撒花

