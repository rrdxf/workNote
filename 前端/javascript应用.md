### javascript应用

#### 一、javascript作用

​	帮助浏览器对用户提出请求进行处理

#### 二、DOM对象

1. DOM=Document Object Model,文档模型对象
2. javascript不呢个直接操作html标签，只能通过html标签关联的DOM对象对HTML标签下达指令

#### 三、DOM对象生命周期![image-20211007200158614](img/image-20211007200158614.png)

1. 浏览器在接收到html文件之后，将html标签加载到浏览器缓存中，每当加载一个html标签的时候，自动为这个实例生成一个对象，对象就是DOM对象
2. 在浏览器关闭之前，或者浏览器请求其他资源文件之前，本次生成的DOM对象一直存活在浏览器缓存中
3. 在浏览器关闭的时候，浏览器缓存中dom对象将要被销毁
4. 在浏览器请求到新的资源之后，浏览器缓存的dom资源被覆盖

#### 四、document对象

1. document对象被称为文档对象
2. document对象用于在浏览器内存中根据定位条件定位DOM对象

#### 五、document对象生命周期

1. 在浏览器将网页中所有标签加载完毕后，在内存中将使用树形 结构存储这些dom对象。在树形结构生成完毕后，有浏览器生成一个document对象管理这颗树（DOM树）**在浏览器将接受网页中标签加载完毕后后，自动在浏览器内生成一个document对下**
2. 浏览器运行期间只会生成一个document对象
3. 在浏览器关闭时，负责将document对象进行销毁
4. 换行标签没有dom对象

#### 六、通过document对象定位DOM对象方式

1. 根据html标签的id属性行为dom对象

   ```javascript
   var domObj=document.getElementById("id属性值");
   ```

   value name id

2. 根据html那么属性定位

   ```javascript
   var domArray=document.getElementByName("name属性");
   ```

   举例：

   ```html
   <input type="checkbox" name="dpNo" value=10>部门10
   <input type="checkbox" name="dpNo" value=20>部门20
   <input type="checkbox" name="dpNo" value=30>部门30
   ```

   ```javascript
   var domArry document.getElementByName("dpNo");
   //通知document对象将所有name属性等于dpNo的标签管理的DOM对象进行定位并且封装到一个数组进行返回。domArry就是一个数组存放本次返回的所有DOM对象
   ```

3. 根据html标签类型定位dom对象

   ```javascript
   var domArray=document.getElementsByTagName('标签类型')
   var domArray=document.getElementsByTagName('p')
   
   ```

##### innertext可以显示标签中间的内容

#### 七、浏览器缓存

![image-20211007202630564](img/image-20211007202630564.png)

#### 八、DOM对象对HTML标签属性操作

##### 1、DOM对象对标签value属性进行取值操作

取值操作

```javascript
var domObj=document.getElementById("one")
var num=domobj.value
```

赋值操作

```javascript
var domObj=document.getElementById("one")
domobj.value='das'
```

##### 2、DOM对象对标签中样式属性进行取值与赋值操作

```javascript
var domObj=document.getElementById("one")
var color=domobj.style.背景演示属性
```

##### 3、赋值操作：

```javascript
var domObj=document.getElementById("one")2var domobj.style.背景演示属性=''
domobj.style.background=''
```

##### 4、DOM对象标签中，状态属性进行取值与赋值操作

​	状态属性：状态属性的值都是boolean类型

​	disabled=true 表示当前标签不可用

​	disabled=false表示当前标签可以使用

​	checked：只存在于radio于checkbox中

​	checked=true

​	checked=false

##### 5、文字显示内容

文件显示内容：只存在于双目标签之间;

```javascript
var domObj=document.getElementById("one")2var domobj.innerte=''

```

#### 6、innertext和innerHTML区别

- innertext与innerHTML都可以对标签文字显示内容属性进行赋值与取值
- innerText只能接受字符串
- innerHTML即可以接受字符串也可以接受标签命令

#### 九、javascript监听时间

##### 1、监听事件：

​	监听用户在任何时候以何种方式对当前标签操作，当监听到相关行为，通知浏览器调用对应javascript函数对当前用户请求进行处理

##### 2、监听事件分类：

###### 1）监听用户何时使用鼠标操作当前标签

1. onclick：监听用户合适使用鼠标**单击**当前标签

2. onmouseover：监听用户合适用鼠标**悬停**在标签上方

   ```html
   <!----this指向当前标签关联的对象--->
   <input type="checkbox" id="one" onclick="fun1(this)">
   ```

   ```javascript
   function fun1(domObj){
   	domObj.style.backgroundColor="yellow"
   }
   function fun2(domObj){
   	domObj.style.backgroundColor="white"
   }
   ```

   

3. onmouseout：监听用户何时从标签上方**移开**

   ```haxe
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
       <script type="text/javascript">
          function fun1(doObj) {
               doObj.style.backgroundColor="yellow"
          }
          function fun2(doObj) {
              doObj.style.backgroundColor="white"
          }
       </script>
   </head>
   <body>
   <center>
       <table border="2">
           <tr onmouseover="fun1(this)" onmouseout="fun2(this)" >
               <td>姓名</td>
               <td>电话</td>
               <td>qq</td>
           </tr>
           <tr onmouseover="fun1(this)" onmouseout="fun2(this)">
               <td>那么</td>
               <td>123</td>
               <td>321</td>
           </tr>
           <tr onmouseover="fun1(this)" onmouseout="fun2(this)">
               <td>姓名1</td>
               <td>电话1</td>
               <td>qq123</td>
           </tr >
           <tr onmouseover="fun1(this)" onmouseout="fun2(this)">
               <td>姓2名</td>
               <td>电话2</td>
               <td>qq34214</td>
           </tr>
       </table>
   </center>
   
   </body>
   </html>
   ```

   ![image-20211007220843748](img/image-20211007220843748.png)

   改进

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
       <script type="text/javascript">
           function fun1() {
               this.style.backgroundColor="yellow"
           }
           function fun2() {
               this.style.backgroundColor="white"
           }
           function main() {
               var domArry=document.getElementsByTagName("tr")
               for (var i=1;i<domArry.length;i++){
                   domArry[i].onmouseover=fun1;
                   domArry[i].onmouseout=fun2;
               }
           }
       </script>
   </head>
   <body onload="main()">
   <center>
       <table border="2">
           <tr >
               <td>姓名</td>
               <td>电话</td>
               <td>qq</td>
           </tr>
           <tr >
               <td>那么</td>
               <td>123</td>
               <td>321</td>
           </tr>
           <tr >
               <td>姓名1</td>
               <td>电话1</td>
               <td>qq123</td>
           </tr >
           <tr >
               <td>姓2名</td>
               <td>电话2</td>
               <td>qq34214</td>
           </tr>
       </table>
   </center>
   
   </body>
   </html>
   ```

   

4. onfocus: 监听用户何时通过鼠标让当前标签获得光标

5. onblur：监听用户何时丢失光标

###### 2）监听用户何时使用键盘操作当前标签

1. onkeydown：监听用户何时在当前标签上按下键盘

2. onkeyup：监听用户何时在当前标签上弹起键盘

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <script type="text/javascript">
       var myArry=["allen","simth","tom","tomcat","for"]
       function fun1() {
   
           //读取用户在文本框输入内容
           var content = document.getElementById("one").value
           if (content==""){
               document.getElementById("two").style.display="none"
               return
           }
           //到数组中定位包含了指定内容字符串
           var value=""
   
           for(var i=0;i<myArry.length;i++){
               var str=myArry[i]
               if (str.indexOf(content)!=-1){//只要串中的内容不在里面
                   value+=str+"<br>"
               }
           }
           //将定位字符串作为文字显示内容填充到div标签
           var domObj=document.getElementById("two")
           domObj.innerHTML=value
           domObj.style.display="block"
           if (value==""){
               document.getElementById("two").style.display="none"
           }
       }
   </script>
   <body>
       <center>
           <input type="text" size="50" id="one" onkeyup="fun1()"/><input style="background-color: cyan;color: white" type="button" value="百度一下">
           <div id="two" style="background-color:paleturquoise;width: 400px;height: 300px;display:none"></div>
       </center>
   
   </body>
   </html>
   ```

   ![image-20211007215936599](img/image-20211007215936599.png)

#### 十、onload监听事件：

##### 1、作用：

​	监听浏览器何时将网页中html标签加载完毕

##### 2、意义

​	浏览器每加载一个HTML标签十，自动在内存生成一个dom对象。在浏览器将网页所有标签加载完毕时，意味着网页中所有标签已经生成dom对象

#### 十一、基于DOM对象实现监听时间与HTML标签之间绑定

##### 1、前提：

​	实际开发过程中，统一个加内特时间往往与多个html标签进行绑定，这样增加开发强度，也会增加维护难度

##### 2、命令形式：

​	dom.监听时间名=处理函数名**此处处理函数名后面不能出现（）**

​	

```javascript
var domObj=document.getElementById("one");
domObj.onclick=fun1//注意函数名后面不能有（）
```

