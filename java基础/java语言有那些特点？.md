### java语言有那些特点？

1. java为纯面向对象语言，他能够直接反应现实生活中的对象
2. 具有木平台无关性，java利用java虚拟机运行字节码，无论是Windows还是Linux还是max平台对java程序进行编译，编译后的程序可在其他平台运行

### static

作用：

- 为某种特定数据类型或对象分配与创建对象个数无关的单一的存储空间

  使得某个方法或属性与类而不是对象关联在一起，即在不创建对象的情况下可通过类直接调用方法或使用类的属性

static使用方式：

- 修饰成员变量。用static修饰的静态变量在内存中只有一个副本。只要静态变量所在的类被加载，这个静态变量就会被分配空间，可以使用类.静态变量和对象.静态变量的方法使用
- 修饰成员方法。static修饰的方法无需创建对象就可以被调用。static方法中不能使用this和super关键词，不能调用非static方法。只能访问所属类的静态成员变量和静态成员方法。
- 代码块。jvm在加载类的时候会执行static代码块。static代码块常用于初始化静态变量。static代码块只会被执行一次
- 修饰内部类。static内部类可以不依赖外部类实例对象而被实例化。静态内部类不能与外部类有相同的名字，不能访问普通成员。只能访问外部类中的静态成员和静态成员方法

### 代码执行顺序

1. 父类静态代码块
2. 子类静态代码块
3. 父类构造代码块
4. 父类构造函数
5. 子类构造代码块
6. 子类构造函数
7. 普通代码块

### String和StringBuffer区别

string用于字符串操作，属于不可变类。String对象一旦被创建，其值不能被修改，而StringBuffer是可变类，当对象创建后，仍然可以对其值进行修改。

### ==和equals区别

==比较是引用，equals比较的是内容

1. 如果变量是基础数据类型，==用于比较其对应值是否相等。如果变量指向的是对象，==用于比较两个对象是否指向同一块存储空间。
2. equals是object类提供的方法之一，每个java类都继承自object类，所以，每个对象都具有equals这个方法。object类中定义的equals方法内部类是直接调用==比较。但通过覆盖的方法可以让他不是比较引用而是比较内容