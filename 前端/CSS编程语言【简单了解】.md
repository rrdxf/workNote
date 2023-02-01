### CSS编程语言【简单了解】

#### 介绍：

1. 是一种专门在浏览器编译并执行的编程语言
2. 用于定位浏览器中html标签并对定位的html中样式属性进行同一管理

#### html标签属性分类

1. 基本属性：

   - 大多数html标签都有属性，是一个非常庞大的群体

   - 比如id属性，相当于身份证号，用于区分html标签

     ```html
     <input type=text id=one/>
     <input type=text id=two/>
     ```

   - 比如name属性，相当于人名，允许一组标签拥有name

2. 样式属性：

   - 是一个非常庞大群体，通知浏览器将html标签中数据在浏览器中以指定形态展示

     ```html
     <div style="background-color:red;color:green;with:300px;height:200px;font-size:50xp"></div>
     ```

     <div style="background-color:red;color:green;with:300px;height:200px;font-size:50px">hahahaha</div>

3. 工作状态属性：

   - 值存在于【表单域标签】中，用于表示【表单域标签】状态。
   - checked存在于radio于checkbox中，表示标签是否被选中
   - disabled
   - readOnly
   - selected

4. 监听属性

   - 监听属性用户于html标签之间进行通信通道，监听属性用于监听用户在何时对当前标签进行何种操作，当指定操作产生时，监听属性将会通知浏览器调用对应javascript方法处理当前请求

   - <script type="text/javascript">
     	function fun1(){
             var myDiv=document.getElementById("one");
             myDiv.style.width="500";
             myDiv.style.height="500";
              myDiv.style.backgroundColor="read";
             myDiv.innerText="hahahahaha";
         }
     </script>
     <center>
         <div id="one" style="width:300px;height:300px;background-color:pink;font-size:20px"
              onmouseover="fun1()"
              >
             我是爸爸
         </div>
     </center>
     
     

#### 样式属性开发难度：

1. 由于网页经常出现大量的html标签拥有相同的样式属性设置，因此导致前端工程师进行大量重复性开发操作
2. 当用户修改需求时，导致前端工程师进行大量重复维护工作

#### css编程语言作用：

1. 通知浏览器将所有满足定位体哦阿健的HTML标签进行统一定位
2. 通知浏览器对已经定位HTML标签中样式属性进行集中统一赋值管理

#### CSS选择器

##### 介绍

1. css选择器，实际上就是一组定位条件用于定位HTML标签
2. css选择器有9个大的分类

##### css选择器语法格式

```html
<html>
	<head>
        <style type="text/css">
            定位条件{
                "样式属性":"值1"；
                 "样式属性2":"值2"
                
            }
        </style>
    </head>
</html>
```

#### id选择器

##### 介绍：

根据html标签中id


```html
<head>
	<style type="text/css">
        #one{
            color:"red"
        }
    </style>
    <p id="one">
        hahaha
    </p>
    <p id="two">
        hahahah2
    </p>
</head>
```

#### 标签类型选择器

##### 介绍

​	根据html标签类型进行选择

##### 语法：

```html
<style type="text/css">
    标签类型名{
        "样式属性1":"值1";
        "样式属性2":"值2"
    }

</style>
```

​	

```html
<html>
    <head>
        <style type="text/css">
            div{
                width:100;
                height:100;
                background-color:green;
                border:1px solid yellow;   
            }
            p{
                color:red;
                font-size:30;
            }
        </style>
    </head>
    <body>
        <div>z1</div>
        <p>
            hahahaahah
        </p>
        
    </body>
</html>
```

#### 层级选择器

##### html标签之间关系

1. ​	关系：

   1. 父子
   2. 兄弟

2. 父子关系

   - 包含

     ```html
     <tr>
     	<td>
         	<input type="text">
         </td>
     </tr>
     ```

3.  兄弟关系

   - 一组标签拥有相同的父标签，并且彼此之间没有任何包含关系，即为兄弟

   - ```html
     <body>
         <div>
             1
         </div>
         <p>
             
         </p>
         <span></span>
     </body>
     ```

4. 层级选择器介绍：

   - 根据标签之间父子或者兄弟关系进行定位

5. 简单层级选择器

   ```html
    <style type="text/css">
               div{
                   width:100;
                   height:100;
                   background-color:green;
                   border:1px solid yellow;   
               }
               p{
                   color:red;
                   font-size:30;
               }
           </style>
   ```

   ![image-20211006201734693](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211006201734693.png)

#### 自定义选择器

##### 介绍

​	如果一组html标签之间没有相同的特征，但是缺需要对指定属性赋值相同内容，此时将自定义选择器绑定到对应标签上

##### 语法

​	

```html
<style type="text/css">
    .自定义选择器名{
        color:red;
    }
    .自定义选择器名2{
        color:yellow;
    }
</style>
<div class="自定义选择器名">
    
</div>
<p class="自定义选择器名2">
    
</p>
```

