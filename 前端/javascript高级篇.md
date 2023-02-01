### javascript高级篇

##### 一、arguments

1. JavaScript中，每一个函数都包含一个arguments属性
2. arguments属性是一个数组
3. 在函数调用时，将实参出入到函数的arguments中，再由arguments将数据传递给形参
4. arguments属性存在，可以将JavaScript中函数在调用时传递实参 与形参进行格式，增加参数调用灵活性
5. arguments属性只能在函数体内使用

###### JavaScript的函数重载：

​	当一个函数接收到不同参数时，能提供不同的功能，当fun3即受到一个string类型参数，体现say hello，接受到两个number类型参数，体现加法运算

#### 二、function类型对象

##### 1、介绍

1. function是JavaScript中一种高级数据类型
2. 一个function类型对象用于管理一个具体函数
3. JavaScript中function类型相当于java中method类型

##### 2、function类型对象生命方式

1. 标准声明方式
2. 匿名声明方式

##### 3、function类型对象声明方式

1. function 函数名（参数名）{

   ​	命令

   }![image-20211008124052408](img/image-20211008124052408.png)

##### 4、function类型对象声明方式---匿名声明方式

​	var 函数对象名=function（参数1，参数2）{命令1，命令2};

##### 5、function类型对象创建的时机

​	浏览器在加载<script>标签的时候会加载两次

​	第一次加载，将标签所有以标准形式声明函数对象进行创建

​	第二次加载，将标签所有命令自上而下执行![image-20211008124855205](img/image-20211008124855205.png)

这个会调用失败

##### 京东面试

![image-20211008125207183](img/image-20211008125207183.png)

答案223

#### 三、局部变量和全局变量

##### 1、局部变量

- 定义：在函数体内通过var修饰的变量
- 特征：只能在当前函数执行体内使用，不能再函数执行体外使用

##### 2、全局变量

- 定义：
  - 全局变暖了再当前html文件中所有函数使用
  - 全局变量被声明是，自动分配给window对象作为其属性
- 声明全局变量：
  - 第一种方式：直接再script标签下，通过var声明的变量，就是全局变量![image-20211008125916148](img/image-20211008125916148.png)
  - 第二种方式：再函数执行体内，没有通过var修饰的变量也是全局变量

#### 四、object类型对象特征：

##### 定义

​	JavaScript认为所有构造函数生成对象其数据类型都是object

##### 特征：

​	object类型对象再创建完毕之后，可以根据实际情况，任意添加属性和方法，也可以移除属性和方法

##### 属性维护：

- 第一种维护方案： 
  - 添加属性：object对象.新属性名=值
  - 添加函数：object对象.新函数对象名=function(){}

- 第二种维护方案：
  - 添加属性：object对象[“新属性名”]=值
  - 添加函数：object对象["新函数对象名"]=function（）{}

##### 移除对象属性和方法

​	delete object.属性名

​	deletet object.函数名

#### 五、自定义构造函数

##### 命令：

```JavaScript
function 函数对象名(){}
```

##### 调用:

```javascript
var object类型对象=new 函数对象名();
```

##### 普通函数与构造函数区分

1. 函数没有调用之前，无法区分函数身份，只能根据函数调用形式区分
2. 判断普通函数： var num=函数对象名();
3. 判断构造函数：var num=new 函数对象名()；
4. 返回值：普通函数运行后v需要通过return将执行返回结果，构造函数运行后，直接返回一个object对象，此时，函数return相当于无效

#### 六、JavaScript中this指向问题：

##### 1、JavaScript中this指向与java中this指向完全一致

- 在构造函数，this指向当前构造函数生成object类型对象
- 在普通函数，this指向当前函数的实例对象

![image-20211008132951587](img/image-20211008132951587.png)

##### 京东面试题：用JavaScript实现hashmap

```javascript
 function HashMap(){
            //var obj=new Object()
            this.put=function(key,value){
                this[key]=value;
            }
            this.get=function(key){
                return this[key];
            }
        }
        var hashMap=new HashMap();
        hashMap.put("key1",100);
        hashMap.put("key2",200);
       window.alert(hashMap.get("key1"))
        window.alert(hashMap.get("key2"))
```

#### 七、JSON：

##### 1、前提：JavaScript中得到object类型对象方式

- 方式1：由构造函数生成的对象都是object对象
- 方式2：由JSON数据描述生成对象都是object对象

##### 2、JSON数据描述格式：

​	JavaScript中获得object类型对象简化班

##### 3、标准命令格式：

​	var obj={"属性名1"：值，"属性名2":值}

​	开发人员习惯将由JSON生成object类型对象称为json对象![image-20211008135415644](img/image-20211008135415644.png)

##### 4、json数组：

​	专门存放json对象的数组被称为json数组![image-20211008135713877](img/image-20211008135713877.png)

#### JsonObjectUtil

```java
import java.lang.reflect.Field;

public class ReflectUtil {
    //作用：将任意类型对象转换为json格式字符串返回
    public static void main(String[] args) {
        System.out.println(jsonObject(new User(1,"nike","12631156")));
    }
    public static StringBuilder jsonObject(Object object){
        StringBuilder stringBuilder=new StringBuilder("{");
        //获得当前对象隶属的【class文件】
        Class classFile=null;
        Field [] fieldAryy=null;
        //获得【class文件】所有属性
        classFile=object.getClass();

        //获得当前对象所有属性值
        fieldAryy=classFile.getDeclaredFields();
        for (int i=0;i<fieldAryy.length;i++){
            Field field=fieldAryy[i];
            field.setAccessible(true);//确保私有属性可以在class外部使用

            //将所得属性拼接成为json格式字符串
            stringBuilder.append("\"");
            stringBuilder.append(field.getName());
            stringBuilder.append("\":");
            stringBuilder.append("\"");
            try {
                stringBuilder.append(field.get(object));
            } catch (IllegalAccessException e) {
                e.printStackTrace();
            }
            stringBuilder.append("\"");
            if (i<fieldAryy.length-1){
                stringBuilder.append(",");
            }
        }
        stringBuilder.append("}");
        //获得属性机器值并结为json格式字符串
        return stringBuilder;
    }
}
```

![image-20211008142339881](img/image-20211008142339881.png)

