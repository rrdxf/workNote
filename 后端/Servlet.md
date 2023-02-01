### Servlet规范

![image-20210928173234571](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20210928173234571.png)

##### 一、Servlet规划介绍

1. servlet规范来自javaee中的一种

2. 作用

   - 在Servlet规范中，指定动态资源文件开发步骤
   - 在Servlet规范中，指定Http服务器调用动态资源文件规则
   - 在Servlet规范中，指定Http服务器管理动态资源文件实例对象规则

   

##### 二、Servlet接口规范实现

1. Servlet接口来自于Servlet规范下一个接口，这个接口存在http服务器提供的jar包

2. Tomcat服务器下lib文件有一个servlet-api.jar存放Servlet接口（javax.servlet.http.HttpServlet）

3. Servlet规范中，http服务器能调用的【动态资源文件】必须是一个servlet接口实现类

   例子：

   ```java
   class Student{
   	//不是动态资源文件，Tomcat无权调用
   }
   class Teacher implements Servlet{
   	//合法动态资源文件，Tomcat有权调用
   	Servlet obj=new Teadcher();
   	obj.doGet();
   }
   ```

##### 三、Servlet接口实现类开发步骤

1. 创建一个java类继承与HttpServlet父类，使成为一个Servlet接口实现类

2. 重写HttpServlet父类中的两个方法。doGet方法或者doPost方法
   - 浏览器---》oneServlet.doGet()
   
   - 浏览器---》oneServlet.doPost()
   
   - ```java
     import javax.servlet.ServletException;
     import javax.servlet.http.HttpServlet;
     import javax.servlet.http.HttpServletRequest;
     import javax.servlet.http.HttpServletResponse;
     import java.io.IOException;
     
     /*
     子类-》父类-》a接口
     此时，子类也是a接口实现类
     
     抽象类：降低接口实现类对接口实现过程中的难度
         将接口中不需要使用抽象方法交给抽象类完成
         这样接口实现类只需要对接口需要方法进行重写
     Servlet接口
         init()
         getServletConfig
         getServletInfo
         destory
     
         service() --u哦用
     Tomcat根据Servlet规范调用Servlet接口实现类规则：
         1.tomcat有权创建Servlet接口实现类实例对象
         Servlet oneServlet=new OneServlet();
         2.Tomcat根据是咧对象调用service方法处理当前请求
         oneServlet.service();
      oneServlet--->(abstract)HttpServlet-->(abstract)GenericServlet-->Servlet接口
      this.service() this是oneservlet方法
     
      通过父类决定在何种情况下调用子类的方法----【设计模式】---模板设计模式
     
      HttpServlet：service(){
     
         if（请求方式==GET）{
             this.doGet
         }else if(请求方式==POST){
             this.doPost
         }
      }
     
      Oneservlet: doGet doPost
     
      Servlet oneServlet=new OneServlet();
      oneServlet.service()
     
     复习：
         重写规则
         抽象类作用
         子类实现接口规则
         this指向
         继承规则
      */
     public class OneServlet extends HttpServlet {
         @Override
         protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
             System.out.println("OneSerlet类针对浏览器发送GET请求方式处理");
         }
     
         @Override
         protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
             System.out.println("OneSerlet类针对浏览器发送POST请求方式处理");
         }
     }
     
     ```
   
     
   
3. 将Servlet接口实现类信息【注册】到Tomcat服务器中

   - 网站-》web-》WEB-INF-》web.xml

   - ```xml
     <!--将Servlet接口实现类类路径地址交给Tomcat-->
     <servlet>
     	<servlet-name>mm</servlet-name><!--声明一个变量存储servlet接口实现类路径-->
     	<servlet-class>com.bjpowernode.controller.OneServlet</servlet-class><!--声明servlet接口实现类-->
     </servlet>
     
     <!--为了降低用户访问Servlet接口实现类安纳度，需要设置简短请求别名-->
     <servlet-mapping>
         <servlet-name>mm</servlet-name>
         <url-pattern>/one</url-pattern><!--设置简短请求别名，别名必须以"/"开头-->
     </servlet-mapping>
     ```

     ```java
     Tomcat String mm="com.bjpowernode.controller.OneServlet"
     ```

     如果现在浏览器向Tomcat索要OneServlet

Tomcat会调service()方法 oneServlet.service()

##### 四、Servlet生命周期

1. 网站所有的Servlet接口实现类的实例对象，只能由Http服务器负责创建，开发人员不能动手创建Servlet接口实现类的实例对象

2. 在默认的情况下，Http服务器接受到对于当前Servlet接口实现类第一次请求时自动创建这个Servlet请求时自动创建这个Servlet接口实现类的实例对象

   - 在手动配置情况下，要求Http服务器在启动时自动创建某个Servlet接口实现类的实例对象

   - ```xml
     <servlet>
     	<servlet-name>mm</servlet-name><!--声明一个变量存储servlet接口实现类路径-->
     	<servlet-class>com.bjpowernode.controller.OneServlet</servlet-class><!--声明servlet接口实现类-->
         <load-on-startup>30</load-on-startup><!--填写一个大于0的整数-->
     </servlet>
     
     ```

3. 在Http服务器运行期间，一个Servlet接口实现类只能创建出一个实例对象

4. 在Http服务器关闭的时候，自动将网站中所有的Servlet对象进行销毁

##### 六、HttpServletRequest接口

1. 介绍：
   1. HttpServletRequest接口来自于Servlet规范中，在Tomcat中存在servlet-api.jar
   2. HttpServletRequest接口实现类由Http服务器负责
   3. HttpServletRequest接口责任在doGet/doPost方法运行时读取Http请求协议中信息
   4. 开发人员习惯于将HttpServletRequest接口修饰的对象【请求对象】
2. 作用
   1. 可以读取Http请求协议包中请求行信息
   2. 可以读取保存在Http请求协议包中请求头或者请求体中的参数信息
   3. 可以替代浏览器向Http服务器申请资源文件的调用

##### 七、请求对象和响应对象的生命周期

1. 在http服务器收到浏览器发送到http请求协议包之后，自动为当前的包生成请求对象和一个响应对象

2. 在http服务器调用doGet/doPost方法时，负责将请求对象和响应对象作为实参传递到方法，确保doget/doPost方法顺利执行

3. 在http服务器准备推送Http响应协议包之前，负责将本次请求关联的请求对象和响应对象销毁

   **请求对象和响应对象生命周期贯穿一次请求的处理过程中**

   **请求对象和响应对象相当于用户在服务端的代言人**



##### 八、jdbc

###### 8.1jdbc使用说明

jdbc api允许用户访问任何形式的表格数据，游戏时存储在关系数据库中的数据

执行流程：

- ​	连接数据源，如：数据库
- 为数据库传递查询和更新指令
- 处理数据库响应并返回的结果

###### 8.2jdbc架构

分为双层和三层架构

**双层**

![image-20210928165617664](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20210928165617664.png)

作用：此架构中，java applet或应用直接访问数据源

条件：要求Driver能与访问的数据库交互

机制：用户命令传给数据库或其他数据源，随之结果被返回。

部署：数据源可以在另一台机器上，用户通过网络连接，成为c/s配置（可以是内联网或互联网）

**三层**

![image-20210928165804092](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20210928165804092.png)

侧架构特殊之处在于，引入中间层服务

流程：命令和结构都会经过该层

吸引：可以增加企业数据的访问控制，以及多种类型的更新；另外也可以简化应用的部署，并在多数情况下有新跟那个优势。

历史趋势：以往，性能问题，中间层都用c或者c++编写，随着优化编译器（将java字节码转为高效的特定机器码）和技术的发展，如ejb，java开始用于中间层的开发，这也让java的优势凸显出来，使用java作为服务器代码语言，jdbc随之被重视起来

###### 8.3jdbc编程步骤

加载驱动程序：

```java
Class.forName(driverClass);
//加载mysql驱动
Class.forName("com.mysql.jdbc.Driver");
//加载oracle驱动
CLass.forName("oracle.jdbc.driver.OracleDriver");
```

获得数据库连接：

```java
DriverManager.getConnection("jdbc:mysql://localhost:3306/test","root","123456");
```

创建Statement\PreparedStatement对象：

```java
conn.createStatement();
conn.prepareStatement(sql);
```

###### java中PreparedStatement和Statement详细讲解

大家都知道PreparedStatement对象可以防止sql注入**sql注入攻击是通过将而已的sql查询或添加语句插入到应用的输入参数中，再在后台sql服务器上解析执行的攻击，他目前是黑客对数据库进行攻击的最常用手段之一，本课程将**，

##### 九、Servlet小项目：在线考试系统

功能：

- 用户注册：将自己的信息添加到服务端计算机Users.frm文件中去
- 用户登录

###### 一、准备工作

1. 创建用户信息表Users.frm

   ```sql
   CREATE TABLE Users(
   	userId int primary key auto_increment,
   	userName varchar(50),
   	password varchar(50),
   	sex char(1),
   	email varchar(50)
   )
   auto_increment,自增序列 i++
   在插入时，如果不给顶具体用户编号此时根据auto_increment的值递增添加
   ```

2. 在src下com.xf.entity.Users实体类

3. 在src下com.xf.util.JdbcUtil工具类【复用】

4. 在web下WEB-INF下创建lib文件夹，存放mysql提供jdbc实现jar包

![image-20210928174607949](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20210928174607949.png)

##### 十、欢迎资源文件

1. 前提

   - 用户可以记住网站名，但是不会记住网站资源文件名

2. 默认资源文件

   - 用户发送了一个针对某个网站的默认请求时，此时由http服务器自动从当前网站返回的资源网摁键
     - 正常为www.baidu.com
     - 而不是www.baidu/login.html.com

3. 配置规则

   - 规则位置：Tomcat安装位置**/conf/web.xml**

   - 规则命令：

   - ```html
     <welcome-file-list>
     	<welcom-file>index.html</welcome-file>
     	<welcom-file>index.html</welcome-file>
     	<welcom-file>index.jsp</welcome-file>
     </welcome-file-list>
     ```

4. 设置当前网站的默认欢迎资源文件规则

   - 规则位置：网站/web/WEB-INF/web.xml

   - 规则命令：<welcome-flie-list>

   - ```xml
     <welcome-file-list>
     	<welcome-file>login.html</welcome-file><!--可以静态资源做欢迎文件，也可以动态资源做欢迎文件-->
         <welcome-file>myweb/find</welcome-file><!--如果时动态资源做欢迎资源文件前面的/要删掉
     -->
     </welcome-file-list>
     ```

   - 网站设置自定义默认文件定位规则，此时Tomcat自带定位规则失效

##### 十一、http状态码

1. 介绍
   - 由三位数字组成的一个符号
   - http服务器在推送响应包之前，根据本次请求处理的情况将http状态码写入到响应包中状态行上
   - 如过http服务器针对上次请求，返回了对应的资源文件，通过http状态码同志刘阿龙年起如何处理这个结果
   - 如果HTTP服务器针对本次请求，无法返回对应的资源文件，通过http状态码向浏览器解释不能提供服务的原因

2. 分类：

   1. 组成：100-599；分为五个大类

   2. 1xx：

      - 最有特征 100：通知浏览器本次返回的资源文件并不是一个独立的资源文件，需要浏览器在接受响应包之后，继续向http服务器索要需要依赖的其他文件

   3. 2xx：

      - 200，通知浏览器本次返回的资源文件是一个独立的资源文件。浏览器不需要索要其他文件

   4. 3xx：

      - 302，通知浏览器本次返回的不是一个资源文件的内容，而是一个资源文件的地址，需要浏览器根据这个地址自动发起请求来索要这个资源文件

        response.sendRedirect（“资源文件地址”）写入到响应头的location中，而这个行为导致Tomcat将302状态码写入到状态行。这样浏览器不会读取响应体中的内容，而是会根据响应头中的location中的内容发起二次请求。

   5. 4xx：

      - 404：通知浏览器，由于在服务器端没有定位到被访问的资源文件，因此无法提供相关服务
      - 405：通知浏览器，在服务端中已经定位到被访问的资源文件（servlet）但这个servlet对于浏览器采用的请求方式不能处理

   6. 5xx：

      - 500：通知浏览器，在服务端已经定位到被访问的资源文件（servlet）这个servlet可以接受浏览器采用请求方式，但是servlet在处理请求期间，由于java异常导致处理失败

##### 十二、多个servlet调用规则

1. 前提条件：
   - 某些来自于浏览器发送请求，往往需要服务端中多个servlet协同处理。但是浏览器一次只能访问一个servlet，导致用户需要手动通过浏览器发起多次请求才能得到服务，这样增加用户获得服务难度，导致用户放弃访问
2. 提高用户使用感受规则：
   - 无论一次请求设计多少个servlet，用户只需要手动调用一次请求
3. 多个servlet之间调用规则：
   - 重定向结局方案
   - 请求转发解决方案

##### 十三、重定向解决方案



![image-20211001150402667](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211001150402667.png)

1. 原理：用户第一次通过手动通知浏览器访问oneservlet。oneservlet工作完毕后，将twoservlet地址写u到响应头location属性中，导致tomcat将302状态码写入到状态行。在浏览器接受到响应包后，会读取302状态。此时浏览器子哦对那个根据响应头中location属性地址发起第二次请求，访问twoservlet去完成请求中的任务
2. 实现命令：
   - response.sendRedirect(“请求地址”)将地址写入到响应包中响应头中location属性
3. 特征：
   1. 请求地址：既可以把当前网站内部的资源文件地址发送给浏览器（/网站名/资源文件名），也可以把其他网站资源文件地址发送给浏览器（http://ip地址:端口号/网站名/资源文件名）、
   2. 请求次数:
      - 浏览器至少发送两次请求，但是只有第一次请求时用户受用发送。后续请求都是浏览器自动发送
   3. 请求方式:重定向请求方式是get,凡通过浏览器地址栏发起的请求,都是get
      - 重定向解决方案中,通过地址栏通知浏览器发起下一次请求,因此通过重定向解决方案调用的资源文件接受的请求方式一定是"get"
   4. 缺点:
      - 重定向解决方案在浏览器与服务器之间多次往返,大量时间小号在往返的次数上,增加用户等待服务时间

##### 十四、请求转发解决方案

1. 原理:用户第一次通过手动方式要求浏览器访问oneservlet,oneservlet工作完毕后,通过当前请求对象代替浏览器向HTtp服务器发送请求调用twoservlet,

2. 实现命令:请求对象代替浏览器向Tomcat发送请求

   - ```java
     //通过当前请求生成资源文件,申请报告对象
     RequestDispatcher report=request.getRequestDispatcher("/资源文件名");
     //将报告发送给Tomcat
     report.forward(当前请求对象,当前响应对象);
     ```

3. 优点:

   - 无论多次请求涉及多少个servlet,用户只需要手动通过浏览器发送一此请求
   - servlet之间调用发送在服务端计算机上,节省服务端与浏览器之间往返次数增加处理五福速度

4. 特征:

   - 请求次数:在请求转发过程中,浏览器只发送一次请求
   - 请求地址:只能向Tomcat服务器中申请调用当前网站下资源文件地址request.getRequestDispathcer("/资源文件名");//不要写网站名

##### 十五.多个servlet之间数据共享方案:

1. 数据共享:oneservlet工作完毕后,将产生数据交给Twoservlet来使用
2. servlet规范中,提供四种数据共享方案
   - servletContext接口  跟Android中的application很像
   - Cookie类
   - HttpSession接口
   - HttpServletRequest接口

##### 十六.ServletContext接口:

1. 介绍:

   1. 来自Servlet规范中一个接口.在Tomcat中存在servlet-api.jar在Tomcat中负责提供这个接口实现类
   2. 如果两个Servlet来自于同一个网站.彼此之间通过网站的servletContext实例对象实现数据共享
   3. 开发人员习惯将ServletContext对象成为"全局作用域对象

2. 工作原理:![image-20211001154757793](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211001154757793.png)

   - 每个网站都存在一个全局作用域对象.这个全局作用域对象相当于一个map.在这个网站中,oneservlet可以将一个数据存入到全局作用域多项,当前网站中其他servlet此时都可以从全局作用域对象得到这个数据进行使用

3. 全局作用对象生命周期

   - 在http服务器启动过程中,自动为当前网站子啊内存中创建一个全局作用域对象

   - 在http服务器运行期间是,一个网站只有一个全局作用域对象

   - 在http服务器运行期间,全局作用对象一致处于存活状态

   - 在http服务器关闭时,负责将当前网站中全局作用域对象进行销毁处理

     **全局作用域对象生命周期贯穿整个运行期间**

4. 命令实现:同一个网站 oneservlet将数据共享给twoservlet

   ```java
   Oneservlet{
   	public void doGet(HttpServletRequest request,HttpServletResponse respone){
   		//通过请求对象向Tomcat索要当前网站中的全局作用域对象
   		ServletContext application=request.getServletContext();
   		//将数据添加到全局作用域对象,作为共享数据
   		application.setAttribute("key1",数据);
   	}
   }
   TwoServlet{
   	public void doGet(HttpServletRequest request,HttpServletResponse respone){
   	
   		//HttpServletResponse respone){
   		//通过请求对象向Tomcat索要当前网站中的全局作用域对象
   		ServletContext application=request.getServletContext();
   		//从全局作用域得到指定关键词对应数据
   		Object 数据=application.getAttribute("key1");
   	}
   }
   ```

   ##### 十七.Cookie

   1. 介绍:

      1. Cookie来自于Servlet规范中的以恶搞工具类,存在于Tomcat提供servlet-api.jar中
      2. 如果两个servlet来自于同一个网站,并且为同一个浏览器/用户提供服务,此时借助于Cookie进行数据共享
      3. Cookie存放当前用户的私人数据,用于在共享数据过程中提高服务质量
      4. 在现实生活场景中,相当于用户在服务端得到会员卡
      5. ![image-20211001161825345](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211001161825345.png)

   2. 原理:

      - 用户通过浏览器第一次向myweb网站发送请求申请oneservlet.oneServlet在运行期间船舰一个Cookie存储与当前用户相关数据OneServlet工作完毕后,将Cookie写入到响应头,脚还给当前浏览器.浏览器收到响应包之后,将cookie存储在浏览器的缓存一段时间后.用户通过同一个浏览器再次向myweb网站发送请求申请Twoservlet时.浏览器需要无条件的将myweb网站之前推送那个过来的cookie,写入到请求头中发送过去
      - 此时Twoservlet在运行时,就可以通过读取请求头中cookie中信息,得到oneserlet共享数据
      - **post要求将请求参数存到请求体中**

   3. 实现命令

      - 同一哥网站one与two借助Cookie共享数据

      - ```java
        OneServlet{
        	public void doGet(HttpServletRequest request,HttpServletResponse response){
        		//创建一个Cookie对象,保存共享数据
        		Cookie card=new Cookie("key1","abc");
                Cookie card1=new Cookie("key2","abc");
                //cookie也相当于map
                //一个cookie只能放一个键值对
                //这个键值对的key与value只能是String
                //键值对中key不能是中文
                //发卡,将cookie写入到响应头中,交给浏览器
                response.addCookie(card);
        	}
        }
        ```

        浏览器:<----------响应头

        浏览器向myweb网站发送请求访问Twoservlet-->请求包

        ```
        TwoServlet{
        	public void doGet(HttpServletRequest request,HttpServletResponse response){
        		//调用请求对象从请求头得到浏览器返回的Cookies
        		Cookie cookieArray[]=request.getCookies();
               //循环得到每一个cookie的key与value
                for(Cookie card:cookieArray){
                	String key=card.getName();
                	String value=card.getValue();
                	//提供一个较好的服务
                }
        	}
        }
        ```

        ![image-20211001165437564](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211001165437564.png)

        刷卡消费流程图

   4. Cookie销毁时机

      - 在默认情况下,Cookie对象存放在浏览器的缓存中,浏览器关闭,Cookie对象会被销毁

      - 在手动设置情况下,可以要求浏览器接受的Cookie存放在户科短计算机的硬盘上,同时需要只当Cookie在银盘上存货的时间.在存货时间范围内,关闭浏览器关闭客户端计算机,关闭服务器都不会导致Cooke被销毁.在存活时间到达时,Cookie自动从硬盘上被删除

        ```java
        cookie.setMaxAge(60);//cookie存活60s
        ```

        

##### 十七.HttpSession接口:

1. 介绍:

   1. httpsession接口来自于Servlet规范下一个接口.存在于Tomcat中servlet-api.jar其实实现类由http服务器提供.Tomcat提供实现类存在于servlet-api.jar
   2. 如果两个servlet来自于同一个网站,并且为同一个浏览器提供服务,此时借助于HttpSession对象进行数据共享
   3. 开发人员习惯于将HttpSession接口修饰对象称为会话作用域对象

2. ###### HttpSession和Cookie区别:[面试题]

   - 存储位置不同:
     - cookie存放在客户端计算机中
     - HttpSession存放在服务端的内存中
   - 数据类型:
     - cookie对象存储共享数据类型只能是String
     - HttpSession对象可以存储任意类型的数据
   - 数据数量:
     - 一个Cookie对象只能存储一个共享数据
     - HttpSession使用map集合存储共享数据,所以可以存储任意数量共享数据
   - 参照物:
     - cookie相当于客户在服务端**会员卡**
     - HttpSession相当于客户端在服务端的**私人保险柜**

3. 命令实现:同一个网站下oneservlet将数据传递给twoServlet]

   ```java
   OneServlet{
   	public void doGet(){
   		//调用请求对象向Tomcat索要当前用户在服务端的私人储物柜
   		HttpSession session=request.getSession();
   		//2将数据添加到私人储物柜中
   		session.setAttribute("key1",共享数据);
   		
   	}
   }
   ```

   浏览器访问/myWeb中twoServlet

   ```java
   OneServlet{
   	public void doGet(){
   		//调用请求对象向Tomcat索要当前用户在服务端的私人储物柜
   		HttpSession session=request.getSession();
   		//2从绘画作用域中得到Oneyservlet提供的共享数据
   		Object 共享数据=session.getAttribute("key1");
   		
   	}
   }
   ```

4. Http服务器如何将用户与HttpSession关联起来

   1. cookie

5. getSession()与getSession(false)

   1. getSession():如果当前客户在服务器有了自己的私人储物柜,要求tomcat将这个私人储物归进行返回如果当前用户在服务器端尚未拥有自己的私人储物柜要求tomcat为当前用户创建一个全新的私人储物柜
   2. getSession(false):如果当前用户已有私人储物柜,那么返回.如果当前用户没有私人储物柜,返回null

6. HttpSession销毁时机:

   1. 用户与HttpSession关联时使用的Cookie只能存放在浏览器缓存中.
   2. 在浏览器关闭时,意味着用户与他的HttpSession关系被切断
   3. 由于Tomcat无法检测合适关闭,因此在浏览器关闭时并不会导致Tomcat将浏览器关联的HttpSession进行销毁
   4. 为了解决这个问题,Tomcat为每一个HttpSession**对象设置一个空闲时间**默认是30分钟,如果当前HttpSession空闲时间达到30分钟,此时Tomcat认为用户已经放弃了自己的HttpSession,此时Tomcat会销毁掉这个Httpsession

7. HttpSession空闲时间手动设置

   - 在当前网站/web/WEB-INF/web.xml

     ```xml
     <session-config>
     	<session-timeout>5</sessopm-timeout><!--当前网站session空闲最大时间为5分钟-->
     </session-config>
     ```

     

   

###### javaweb中ServletContext HttpSession 以及HttpServletRequest理解应用

1. request

   - request是表示一个请求,装固态发出关于土匪去北方区域呼吁关于差异部分华北土匪request,他的作用域:仅在当前请求中有效.

   - 用处:常用于服务期间同一请求不同页面之间的参数传递,请应用于表单的控件值传递.
   - 方法:request.setAttribute();request.getAttribute();request.removeAttribute();requets.getParameter()

2. session

   - 服务器会为每个绘画创建一个session对象,所以session总的数据可供当前会话中所有servlet共享.
   - 会话:用户打开浏览器会话开始,知道关闭浏览器会话才会借宿.一次会话期间只会创建一个session对象.
   - 用处:常用于web开发中的登录验证页面(当用户登录成功后浏览器分配其一个session键值对).
   - 方法:session.setAttribute();session.getAttribute();session.removeAttribute();
   - 被销毁
     - session超时
     - 客户端关闭后,再也访问不到和该客户端对应的session,在超时后销毁
     - 调用session.invalidate();

3. Application(ServletContext)

   - 作用范围:所有的用户都可以取得此信息,此信息在整个服务器上被保留.Application属性范围值,只要设置一次,则所有的网页窗口都可以取得数据.ServletContext在服务器启动时创建,在服务器关闭时销毁,一个javaweb应用值创建一个ServletContext对象,所有的客户端在访问服务器是都共享一个个ServletConetxt对象;ServletContext对象一般用于在多个客户端间共享数据时使用

##### 十八.HttpServletRequest接口实现数据共享

1. 介绍:

   1. 在同一个网站中,如果两个servlet之间通过**请求转发**进行调用,彼此之间共享同一个请求协议包.而一个请求协议包值对应一个请求对象.因此servlet之间共享同一哥请求对象,此时可以利用这个请求对象在两个servlet之间实现数据共享
   2. 在请求对象时间servlet之间数据共享功能是,开发人员将请求对象称为**请求作用域对象**

2. 命令实现: Oneservlet通过请求转发申请调用TwoServlet是,需要给TwoServlet提供共享数据

   ```java
   public void doGet(HttpServletRequest request,HttpServletResponse response){
       //将数据添加到请求作用域对象attribute属性
       request.setAttribute("key1",数据);
       //向tomcat中申请调用twoServlet
       request.getRequestDispacher("/two").forward(request,response);
       
   }
   public void doGet(HttpServletRequest request,HttpServletResponse response){
       //从当前请求对象得到OneServlet写入到共享数据
       Object 数据=request.getAtrribute("key1"); 
   }
   ```

   ##### 二十、servlet规范扩展-----监听器接口

1. 1介绍：

   1. 一组来自于servlet规范下接口，共有8个接口，在Tomcat存在servlet-api.jar包
   2. 监听接口需要由开发人员亲自实现，http服务器提供jar包没有对应的实现类
   3. 监听器接口用于监控**作用域对象生命周期变化时刻**以及**作用域对象共享数据变化时刻**

2. 作用域对象：

   1. 在servlet规范中，认为在服务端内存中可以在某些条件下为两个servlet之间提供数据共享方案的对象，被称为**作用域对象**
   2. servlet规范下作用域对象：
      - servletContext：全局作用域对象
      - httpSession：会话作用域对象
      - HttpServletRequest：请求作用域对象

3. 监听器接口实现类开发规范：三步

   1. 根据监听的实际情况，选择对应的监听器接口进行实现
   2. 重写监听器接口声明**监听事件处理方法**
   3. 在web.xml文件中将监听器接口实现类注册到Http服务器

4. ServletContextListener接口（全局）

   1. 作用：通过这个接口合法的检测全局作用域对象被初始化时刻以及被销毁时刻

   2. 监听时间处理方法

      ```java
      public void contextInitlized();//在全局作用域对象被Http初始化调用
      public void contextDestory();//在被http销毁时
      ```

5. ServletContextAttributeListener接口：（全局）

   1. 作用：检测全局作用域对象共享数据变化的时候

   2. 监听时间处理方法

      ```java
      public void contextAdd();在全局作用域对象添加数据
      public void contextReplaced();在更新时
      public void contextRemove();在删除时
      ```

6. 全局作用域对象共享数据变化时刻

   - ```java
     ServletContext application=request.getServletContext();
     application.setAtrribute("key1",100);//新增数据
     application.setAtrribute("key1",200);//更新数据
     application.removeAtrribute("key1");//删除
     ```

     

##### 十九、servlet亏站规范----Filter接口（过滤器接口）

1. 介绍：

   1. 来自于servlet规范下接口，在Tomcat中存在于servlet-api.jar包
   2. Filter接口实现类由开发人员负责提供，Http服务器不负责提供
   3. Filter接口在Http服务器调用资源文件之前，对http服务器进行拦截

2. 具体作用：

   1. 拦截Http服务器，帮助Http服务器检测当前请求合法性
   2. 拦截Http服务器，对当前请求进行增强操作

3. Filter接口实现类开发步骤：三步

   1. 创建一个java类Filter接口

   2. 重写Filter接口中doFilter方法

   3. web.xml将过滤器接口实现类注册到Http服务器

      ```java
      public class OneFilter implements Filter {
          @Override
          public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
              //1.通过拦截请求对象得到请求包参数信息，从而得到来访用户的真是年龄
              String age=servletRequest.getParameter("age");
              if (Integer.valueOf(age)<70){
                  //将拦截请求对象和响应对象交给tomcat，由tomcat继续调用资源文件
                  filterChain.doFilter(servletRequest,servletResponse);
              }  else{
                  servletResponse.setContentType("text/html;charset=utf-8");
                  PrintWriter  printWriter=servletResponse.getWriter();
                  printWriter.print("<center><font style='color:red;font-size:40px'>hahahah</font></center>");
              }
          }
      }
      
      ```

   4. Filter拦截地址格式

      1. 命令格式

         ```xml
         <filter-mapping>
         	<filter-name>oneFilter</filter-name>
         	<url-pattern>拦截地址</url-pattern>
         </filter-mapping>
         ```

      2. 命令作用

         - 拦截地址通知Tomcat在调用何种资源文件之前需要调用OneFilter过滤进行拦截

      3. 要求Tomcat在调用一个具体文件之前，来调用OneFilter拦截

         ```xml
         <url-parttern>/img/mm.jpg</url-pattern>
         ```

      4. 要求tomcat在调用某一个文件夹所有资源文件之前，来调用OneFilter拦截

         ```xml
         <url-parttern>/img/*</url-pattern>
         ```

      5. 要求tomcat在调用某种类型文件之前，来调用OneFilter拦截

         ```xml
         <url-parttern>*.jpg</url-pattern>
         ```

      6. 要求tomcat在调用网站中任意文件时，来调用oneFilter拦截

         ```xml
         <url-parttern>/*</url-pattern>
         ```

         

控制浏览器三要素：

1. 控制浏览器请求地址，避免出现404
2. 控制请求采访时，避免出现405
3. 控制请求参数，超链接，表单域标签

请求协议包：

- 请求行 url：请求地址 method：请求方式   请求参数get/post
- 请求头 ： Cookie  
- 空包行
- 请求体

解释：

- 请求行 url：请求类型/请求资源路径、协议的版本和类型
- 请求头 ： 一些键值对，一般有w3c定义，浏览器与web服务器之间都可以发送，表示特定的某种含义  
- 空包行：请求头与请求体之间用一个空行隔开
- 请求体：要发送的数据   post方式会用

多个servlet调用规则

1. 重定向解决方案
2. 请求转发方案

多个servlet数据共享：

1. servletContext
2. cookie
3. httpsession
4. httpservletRequest

响应包

- 状态行：
- 响应头：content-type coolkied
- 空白行
- 响应体：返回文件内容，文件命令，动态文件运行后结果

防止恶意登录：

- 使用httpsession令牌
- 使用过滤器
