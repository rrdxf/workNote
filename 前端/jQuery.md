### jQuery

​	jQuery是一款主流浏览器的JavaScript库，封装了JavaScript相关方法调用，简化JavaScript对html DOM操作

#### 1、jQuery是js库

​	库：相当于java工具类，库是存放东西的，jQuery是存放js代码的地方，放的是用JavaScript写的function

​	jQuery是一个快速，小巧，功能丰富的JavaScript库，它用过易于使用的api在大量浏览器中运行，使得HTML文档遍历和操作，事件处理，动画和Ajax变得简单，通过多功能性和可扩展的结合，jQuery该变量数百万人编写JavaScript的方式。

![image-20211009120725582](img/image-20211009120725582.png)

##### 优点：

1. 写少代码，做多事情

2. 免费，开源且轻量级的js库，容量很小

3. 兼容市面上主流浏览器，如IE，Firefox，Chrome

4. 能够处理HTML/JSP/XML、CSS、DOM、事件、实现动画效用，也能提供异步AJAX功能

5. 出错后有提示

6. 不用再在html里面通过<script>标签插入一大堆js命令

   1. 通过id属性：document.getElementById()
   2. class属性：getElementClassName()
   3. 通过标签名：getElementByTagName()

   上面代码可以看出，javascript方法名太长了，大小写的组合太多了，编写代码效率，容易出错。jQuery分别使用

   ```
   ${"#id"},${".class名"},${"标签名"}封装了上面js方法
   ```

#### 2、dom对象和jQuery对象

​	dom对象，使用JavaScript的语法创建的对象叫做dom对象，也就是js对象。var obj =document.getElementById()；obj是dom对象，也叫js对象

​	jquery对象：使用jquery语法表示对象叫做jQuery对象，注意：jQuery表示的对象都是数组。例如var jobj=$("#text"),jobj就是使用jquery表示的对象。也就是jquery对象，它是一个数组。现在数组中就一个值。

​	dom对象可以和jquery对象互相转换

​		dom对象可以转为jquery，语法：$(dom对象)

​		jquery对象也可以转为dom对象，语法：从数组中获取第一个对象，第一个对象就是dom对象，使用[0]或者get{0}

​	为什么要转换：目的是要使用对象的方法。

​	当你用dom对象是，可以使用dom对象的属性或者方法。如果你想要使用jquery提供的函数，必须是jquery对象才可以

#### 3、选择器：

​	就是一个字符串，用来定位dom对象的，定位了dom对象，就可以通过jquery的函数操作dom

##### 常用的选择器：

1. id选择器：语法：$("#dom对象的id值")

   通过dom对象的id定位dom对象的。通过id找对象，id在当前页面中是唯一值

2. class选择器，语法：$(".class样式名")

   class表示css中的样式，使用样式的名称定位dom对象

3. 标签选择器：语法${"标签名称"}

   使用标签名称定位dom对象的

4. 全部选择器：${"*"}

5. 组合选择器: ${".class样式名","标签名称","#dom对象的id值"}

##### 4、表单选择器

​	使用<input>标签type属性值，定位dom对象的方式

​	语法：

```javascript
${":text"},${":password"}
```

##### 5、过滤器：在定位了dom对象后，根据一些条件筛选过滤器

1. ${"选择器:firts"}:第一个dom对象
2. ${"选择器:last"}:数组中最后一个对象
3. ${"选择器:eq(数组的下标)"}:获取指定下标的所有dom对象
4. ${"选择器:lt(下标)"}:获取小于下标的所有dom对象
5. ${"选择器:gt(下标)"}:获取大于下标的所有dom对象

#### 4、jquery中给dom对象绑定事件

1. $(选择器).事件名称(事件的处理函数)

   $(选择器):定位dom对象，dom对象可以有多个，这些dom对象都绑定事件了

   事件名称：就是js中去掉on的部分

   ​	jquery中的事件名称，就是click，都是小写

   事件处理函数：就是一个function，当事件发生时，执行这个函数的内容

   ```javascript
   $("#btn").click(function(){
   	alert("按钮单击了")
   })
   ```

2. on事件绑定 //此功能更加的丰富

   $(选择器).on(事件名称，事件的处理函数)

   事件名称：就是js中去掉on的部分，例如js中onclick，这里就是click时间的处理函数：function定义

   例如

   ```
   <input type="button" id="btn">
   $("#btn").on("click",function(){函数体})
   ```

   

#### 5、过滤器

![image-20211009172923987](img/image-20211009172923987.png)

![image-20211009173022689](img/image-20211009173022689.png)

#### 6、函数：

var、text（相当于innertext）、attr

remove（）：删除父子对象

![image-20211009175228790](img/image-20211009175228790.png)

empty（）：删除子标签

![image-20211009175256141](img/image-20211009175256141.png)

append（）：增加dom对象

![image-20211009175518296](img/image-20211009175518296.png)

html（）：操作innerhtml

![image-20211009175726277](img/image-20211009175726277.png)

each语法

1. 可以对数组，json，dom数组循环处理。数组，json中的每个成员都会调用一次处理函数

   ```javascript
   var arr={1,2,3}
   var json={"name":"list","age":20}
   var obj=${":text"}
   //语法
   $.each(循环内容，处理函数)//表示使用jquery的each，循环数组，每个成员都会执行后面的处理函数一次。
   #:相当于是java的一个类名
   each：就是类中的静态方法
   静态方法调用，可以使用类名.方法名
   
   
   ```

   ![image-20211009180937819](img/image-20211009180937819.png)

#### 7、ajax的封装

使用jquery的函数，实现ajax请求的处理。

没有jQuery之前，使用XMLHttpRequest做ajax。

1. $.ajax():jquery中实现ajax的核心函数
2. $.post():使用post方式做ajax请求
3. $.get():使用get方式发送ajax请求

#### 8、ajax函数封装使用说明

```javascript
$.ajax({名称：值，名称：值})
```

asyno:是一个boolean值，默认是true，表示异步请求

contentType:一个字符串，表示从浏览器发送服务器的参数的类型。可以不写。例如你想表示请求的参数是json格式的，可以写application/json

data：可以是字符串，数组，json，表示请求的参数值，常用的是json

dataType：表示期望从服务器端返回的数据格式，可选的有：xml，html，text，json。当我们使用$.ajax时，会把datatype的值发送给服务器，那我们的servlet能够读取到dataType的值，就知道你的浏览器需要的时json或者xml数据，那么服务器就可以返回你需要的数据格式。

error：一个function，表示当请求发生错误时，执行函数

error：function(){发生错误时执行} 

success：一个function，请求成功了，从服务端返回了数据，会执行success指定函数，之前使用xmlhttpRequest对象。

url:请求的地址

type:请求的方式

```javascript
$.ajax({
	async:true,
	contentype:"applicationi/json",
	data:{name:"list",age:20}
	dataType:"json",
	error:function(){
		//请求错误时，执行的函数
	
	}
	success:function(data){
		//data就是responseText，是jQuery处理后的数据
	},
        url:"bmiajax",
        type:"get"/"post"

})
```

**主要使用url，data，datatype，success**

