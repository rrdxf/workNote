### ajax

#### 1、全局刷新和局部刷新

全局刷新：整个i卢兰其被新的数据覆盖，在网络中传输大量的数据。浏览器需要加载，渲染页面。

局部刷新：在浏览器的内容，发起请求，获取数据，改变页面中的部分内容。其余的页面无需加载和渲染。网络中数据传输量少，给用户的感受好。

ajax做局部刷新的。局部刷新使用的核心对象时 异步对象（XMLHttpRequest）![image-20211008153231573](img/image-20211008153231573.png)

这个一部对象是存在浏览器内存中的，使用jvascript语法创建和使用XMLHttpRequest对象

#### 2、AJAX：Asynchronous JavaScript and XML（异步的JavaScript和XML）

xml：是一种数据格式

ajax是一种做局部刷新的新方法（2003），不是一种语言。主要技术：JavaScript，dom，css，xml等待。核心是JavaScript和xml

JavaScript负责创建异步对象，发送请求，更新dam对象。ajax请求需要服务端数据。

xml：网络中传输的数据格式.使用json替换了xml

```xml
<数据>
    <数据1>宝马1</数据1>
    <数据1>宝马2</数据1>
    <数据1>宝马3</数据1>
</数据>
```

#### 3、使用XMLHttpReqest对象

1. 创建异步对象 var xmlHttp=new XMLRequest();

2. 给异步对象绑定事件。onreadystatechange :当异步对象发情请求，获取了数据都会触发这个事件。这个事件需要指定一个函数，在函数中处理状态的变化。

   ```javascript
   btn.onclick=fun1()
   function fun1(){
   	alert("按钮单击");
   }
   xmlHttp.onreadystatechange=function(){
       处理请求的状态变化。
   }
   ```

   异步对象属性 readystate属性：<img src="img/image-20211008154803284.png" alt="image-20211008154803284" style="zoom:150%;" />

   0：创建异步对象，new XMLHttpRequest();

   1:初始异步请求对象 htmlHttp.open()

   2:发送请求 xmlHttp.send()

   3：从服务端获取了数据，此时3：注意3是异步对象内部使用，获取了原始数据

   4：异步对象把接受的数据处理完毕后，此时人员在4的时候处理数据。在4的时候，开发人员需要更新当前页面

   异步对象的status属性，表示网络请求的状况，200，404，500，需要时当status==200时，表示网络请求是成功的

3. 初始异步请求对象

   异步的方法open()

   xmlHttp.open(请求方式get|post,"服务器端的访问地址"，同步|异步请求，默认是true，异步请求)

   ```javascript
   xmlHttp.open("get","myweb/servlet?",true)
   ```

4. 使用异步对象发送请求

   xmlHttp.send()

   获取服务器返回的数据，使用异步对象的属性responseText使用例子

   ```javascript
   xmlHttp.responseText;
   document.getElementById("name")=data;
   ```

   回调“当请求的状态变化时，异步对象会自动调用onreadystatechange事件对应的函数

#### 第一个项目

jsp：

```html
<%--
  Created by IntelliJ IDEA.
  User: xfff
  Date: 2021/10/8
  Time: 16:30
  To change this template use File | Settings | File Templates.
--%>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>$Title$</title>
    <script type="text/javascript">
      //使用内存中的异步对象，代替浏览器发起请求。异步对象使用js创建和管理
      function doAjax() {

        //1创建异步对象
        var xmlHttp=new XMLHttpRequest();
        //2绑定事件
        xmlHttp.onreadystatechange=function () {
          //处理服务器发挥的数据，更新当前页面
          //alert(xmlHttp.readyState+"xmlHttp.status"+xmlHttp.status)

          if(xmlHttp.readyState==4||xmlHttp.status==200){
              document.getElementById("result").innerText=xmlHttp.responseText;
          }
        }
        //3初始请求数据
        //获取dom对象的value属性值
        var name=document.getElementById("name").value;
        var w=document.getElementById("w").value;
        var h=document.getElementById("h").value;

        var para="?name="+name+"&w="+w+"&h="+h;
        window.alert("/ajaxtest2_war_exploded/ajaxbmi"+para)
        xmlHttp.open("get","/ajaxtest2_war_exploded/ajaxbmi"+para,true)
        //4发起请求
        xmlHttp.send();
      }

    </script>
  </head>
  <body>
  <div>
    <!----------没有用表单------>
    姓名：<input type="text" id="name"/><br>
    体重：<input type="text" id="w"/><br>
    身高：<input type="text" id="h"><br>
    <input type="button" value="计算BMI" onclick="doAjax()">
  </div>
  <div id="result"></div>
  </body>
</html>

```

servlet：

```java
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;


public class AjaxBmiServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //接受参数
        String name=request.getParameter("name");
        String weight=request.getParameter("w");
        String height=request.getParameter("h");
        System.out.println(name+weight+height);
        //计算bmi
        float h=Float.valueOf(height);
        float w=Float.valueOf(weight);
        float bmi=w/(h*h);

        //判断bmi范围
        String msg="";
        if (bmi<18.5){
            msg="比较瘦";
        }else if (bmi>=18.5&&bmi<=23.9){
            msg="正常";

        }else if (bmi>24&&bmi<=27){
            msg="较胖";
        }else {
            msg="很胖";
        }
        msg=name+"爸爸,您的体重"+msg+"您的BMI是"+String.valueOf(bmi);
        //响应ajax需要的数据，使用httpservletresponse
        response.setContentType("text/html;charset=utf-8");
        PrintWriter printWriter=response.getWriter();
        printWriter.print(msg);
        printWriter.flush();
        printWriter.close();
    }
}
```

#### 第二个项目

##### 数据库：省份表：

```sql
set foreign_key_checks=0;
create table 'province'(
	'id' int(11) NOT null auto_increment,
    'name' varchar(255) default null comment'省份名称',
    'jiancheng' varchar(255) default null commnet'简称',
    'shenghui' varchar(255) default null,
    primary key('id')
)engine=InnoDB AUTO_INCREMENT=10 CHARSET=utf-8;
insert into 'province' values('1','河北','冀','石家庄');
insert into 'province' values('2','山西','晋','太原');
insert into 'province' values('3','内=内蒙古','蒙','呼和浩特');
insert into 'province' values('4','辽宁','辽','沈阳');
insert into 'province' values('5','江苏','苏','南京');
insert into 'province' values('6','浙江','浙','杭州');
insert into 'province' values('7','安徽','皖','合肥');
insert into 'province' values('8','福建','闽','福州');
insert into 'province' values('9','江西','赣','南昌');



```

##### 城市表：

```sql
set foreign_key_checks=0;
create table 'city'(
	'id' int(11) not null auto_increment,
    'name' varchar(255) default null,
    'provinceid' int(11) default null,
    primary key('id')
)engine=InnoDB auto_increment=17 default charset=utf-8;

insert into 'city' values('1','石家庄','1');
insert into 'city' values('2','秦皇岛','1');
insert into 'city' values('3','保定','1');
insert into 'city' values('4','张家口','1');
insert into 'city' values('5','南昌','9');
insert into 'city' values('6','九江','9');
insert into 'city' values('7','宜春','9');
insert into 'city' values('8','福州','8');
insert into 'city' values('9','厦门','8');
insert into 'city' values('10','泉州','8');
insert into 'city' values('11','龙岩','8');
insert into 'city' values('12','太原','2');
insert into 'city' values('13','大同','2');
insert into 'city' values('14','呼和浩特','3');
insert into 'city' values('15','包头','3');
insert into 'city' values('16','h','3');

```

##### jdbc

```java
 //根据id获取名称
    public String queryProviceNameById(Integer proId){
        System.out.println(proId);
        Connection conn=null;
        PreparedStatement pst=null;
        ResultSet rs=null;
        String sql="";
        String name=null;
        try {
            Class.forName("com.mysql.jdbc.Driver");
            conn=DriverManager.getConnection("jdbc:mysql://localhost:3306/test?useSSL=false","root","123456");

            sql="select name from province where id=?";
            pst=conn.prepareStatement(sql);
            pst.setInt(1,proId);
            rs=pst.executeQuery();
            while (rs.next()){//只有一条记录的时候
                name=rs.getString("name");
            }
            /*
            if(rs.next){
                name=rs.getString("name");
            }
             */
        } catch (ClassNotFoundException | SQLException e) {
            e.printStackTrace();
        }finally {
            if (rs!=null){
                try {
                    rs.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if (pst!=null){
                try {
                    pst.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
            if (conn!=null){
                try {
                    conn.close();
                } catch (SQLException throwables) {
                    throwables.printStackTrace();
                }
            }
        }
        return name;
    }
```

#### json

json发起请求---servlet（返回的一个json格式的字符串{name:"河北",jiancheng:"冀","shenghui":"石家庄"}

json分类:

1. json对象，jsonObject，这种对象的格式 名称：值，也可以看作key：value格式
2. json数组，JSONArray，基本格式[{name:"河北",jiancheng:"冀","shenghui":"石家庄"},{}]

##### 为什么用json？

1. json格式好理解
2. json格式数据在多种语言中，比较容易处理。使用javascript读写json格式的数据比较容易
3. json格式数据他占用的空间下，在网络传输比较快，用户体验好

##### 关于json工具库

gson（google）

fastjson：速度快，不是符合json处理规范的

jackson：性能好，规范好

json-lib：性能好，依赖多

#### 异步和同步

- 异步：open（get，url，true）,在send之后执行其它的代码，可以同时执行多个异步请求
- 同步：open（get，url，false），一次只能执行一个异步请求，必须一个请求处理完成后，才能执行其他的请求处理
