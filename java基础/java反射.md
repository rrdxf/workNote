### java反射

#### 1、反射获取Class的三种方式

![image-20211004161417551](C:\Users\xfff\AppData\Roaming\Typora\typora-user-images\image-20211004161417551.png)

```java
 public static void main(String[] args) throws ClassNotFoundException {
        /*
            Class.forname()
                1.静态方法
                2.方法参数是一个字符串
                3.字符串需要的是一个完整类
                4.完整类名必须带有包名。java.lang 包不能省略
             三种方式：
                Class.forName()
                对象.getClass();
                任何类型.class
         */
        //1.获取字节码文件
        Class c1=Class.forName("java.lang.String");
        Class c2=Class.forName("java.util.Data");

        //java中任何一个对象都一个：getClass（）
        String s="abc";
        
        Class x=s.getClass();//x代表String.class，代表string类型
        
        Class z=String.class;//z代表String类型
    }
```

#### 2、关于idea的路径问题

这种方法获取文件的绝对路径使通用的

```java
public static void main(String[] args) throws FileNotFoundException {
        //这种方式的路径缺点使：移植性差，在idea中默认的当前路径使project的根。
        //这个代码假设离开了idea，换到了其他位置，可能当前路径不是project跟了，那么这个路径就无效了
        //FileReader reader=new FileReader("");
        //FileReader reader=new FileReader("test/classinfo.properties");
        //
        /*
        Thread.currentThread()当前线程对象
        getContextClassLoader()使线程对象的方法，可以获取到当前线程类加载器对象
        getResource()这是类加载器对象的方法，当前线程的类加载器默认从类的根路径下加载资源
         */
        //以下代码怎么样移植，都不会变
        String path=Thread.currentThread().getContextClassLoader().getResource("classinfo.properties").getPath();
        System.out.println(path);
    }
```



#### 3、获取当前绝对路径的工具类封装

**注:比**

```java

 //以下代码怎么样移植，路径都可以使用
 //从类的根目录下，就是src下开始
public class GetAbsolutPathUtil {
    //获取绝对路径以String的返回值返回
    public static String getPath(String name){
        return Thread.currentThread().getContextClassLoader().getResource(name).getPath();
    }
    
    //以流的形式返回
    public static InputStream getFileStrem(String name){
        return Thread.currentThread().getContextClassLoader().getResourceAsStream(name);
    }
}

```

#### 4、类加载器关于jdk中的类加载器

专门负责加载类的命令/工具

classLoader

1. jdk中自带了三个类加载器
   - 启动类加载器/父
   - 扩展类加载器/母
   - 应用类加载器
2. 假设代码
   - string s="acv";
   - 代码再开始执行之前，会将所需要全部类加载到jvm当中，通过类加载器加载，看到以上代码类加载器会找String.class文件，找到就加载
   - 如何加载：首先通过启动类加载器加载rt.jar
     - rt.jar中都是jdk最核心的类库
     - 如果通过启动类加载器加载不到
     - 会通过扩展类加载器，会加载ext/*.jar
     - 如果扩展类加载器也没有找到，会同应用类加载器
     - 应用类加载器专门加载classpath中的jar

#### 5、java中为了保证类加载的安全，使用了双亲委派机制。

优先从启动类加载器中加载，这个称为父，父无法加载到，再从扩展类加载器中加载，这个称为母，双亲委派，如果都加载不到，才会考虑吧从应用类加载器中加载。直到加载到为止。