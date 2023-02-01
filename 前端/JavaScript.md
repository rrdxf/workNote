### JavaScript基础

![image-20211007172315589](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211007172315589.png)



#### 一、介绍

- 是一种专门在浏览器编译并执行的编程语言
- 处理用户与浏览器之间请求问题
- javascript采用弱类型编程语言风格对面向对象思想来进行实现的编程语言

#### 二、若编程语言份个股vs强类型编程语言风格

##### 强类型：

​	认为对象行为应该收到修饰类型的严格约束。

​	java采用强类型编程语言风格对面向对象思想来进行实现的编程语言

```java
class student{
	public String name;
	public void sayHello(){
		sout("hello")
	
	}
}

```

##### 弱类型：

​	认为对象行为不应该收到修饰的约束，可以根据实际需要决定当前对象调用的属性和方法

```javascript
//var用来定义一个对象
var stu =new Object();//stu对象相当于【啊q】
stu.car="laosilaisi";
stu.play=function(){return "天天打游戏"}
stu.play();
```

#### 三、javascript变量声明方式

##### 1命令格式：

javascript可以用;结尾，也可以不用；结尾

​	var 变量名；

​	var 变量名 =值

​	var 变量名1，2=值

##### 2注意：

​	在javascript变量/对象,在声明不允许指定修饰类型，只能通过var来进行修饰

#### 四、javascript中标识符命令规则：

1. 标识符只能由四种符号：英文字母，数字，下划线，美元符号
2. 标识符首字母不能以数字开头
3. 标识符不能采用javascript中当中的关键字

##### 五，javascript数据类型

1. 分类：基本数据类型  & 高级引用类型
2. 基本数据类型： 数字（number），字符串类型（string），布尔类型（boolean）
3. 数字类型：javascript中将整数与小数合称为number
4. 字符串类型 ：字符与字符串合称string可以使用‘’也能用“”
5. 布尔类型：true ，false
6. 高级引用类型：
   1. object
   2. function
7. object ：javascript通过构造函数new出来的就是obect类型
8. function类型 ： 相当于java中（java.lang.reflect.Method）

##### javascript是弱类型编程语言，根据变量赋值内容来判断变量数据类型

javascript中变量的数据类型可以根据赋值内容进行动态改变

#### 六、javascript中特殊值

##### 1、undefined：

所有变量在没有赋值时，都是undefined，由于javascrpt根据变量的赋值来判断变量类型，此时由于变量没有赋值，因此javascript无法判断当前遍历数据类型，此时返回也是undefined，因此初学者将undefined也理解为是一种数据类型，这种理解是错误的

##### 2、null

​	javascript中当一个对象赋值为null是，表示对象引用了一个空内存，这个空内存既不能存储数据，也不能读取数据，此时这个对象数据类型，在javascript依然认为是object

##### 3、NaN 

​	javascript中当一个变量被赋值为NaN，表示接受了一个非法数字（132合法数字 abc123非法数字），此时这个遍历数据类型，在javascript依然认为number类型

```javascript
var num1=NaN;
var num2=parseInt("123abc");
```

![image-20211007182520374](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211007182520374.png)

##### 4、infinity:

​	javaScript中当一个变量赋值为infinity，表示接受了一个无穷大数字，此时依然为number

![image-20211007182725820](img/image-20211007182725820.png)

![image-20211007182738289](img/image-20211007182738289.png)

#### 七、javascript中的控制语句

```javascript
var age=23
if(age=60){
	window.alert("老年人");
}else{
	window.alert('中年人');

}
for(var i=0;i<=5;i++){
    
    
}
```

#### 八、javascript中函数声明的方式

##### 1、命令格式

​	function 函数()(形参，形参名2){

​	javascript命令

​	return 返回结果

}

##### 2、注意

1. javascript中，所有函数在声明时，都需要使用function进行修饰
2. 所有函数在声明时，静止只当函数返回类型
3. 形参不能使用var来修饰，也不能用数据类型修饰
4. 如果有返回值，此时应该通过return 进行返回

#### 九、javascript函数调用方式

##### 1、浏览器并不会自动调用javascript函数

##### 2、可以通过命令行方式来调用java函数

##### 3、同绑定在html标签上监听时间通知浏览器

























