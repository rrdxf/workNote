### super 、this、 final 、static汇总

#### final

final关键字主要用在三个地方：变量、方法、类。

1. 对于一个final变量，如果时基本数据类型的变量，则其数值一旦在初始化之后不能更改。如果时引用类型的变量。则在对其初始化之后，便不能再让其指向另一个对象
2. 当final修饰一个类时，表明这个类不能被继承。final类中的所有成员方法都会被隐式地指定为final方法。
3. 使用final方法的原因有两个。第一，吧方法锁定，以防任何继承类修改它的涵义。二。效率问题。在早期java实现版本中，将会final方法转为内嵌调用。但是如果方法过于庞大。可能看不到内嵌调用带来的任何性能提升。类中所有的private方法都隐式的指定为final

#### static

static关键字主要有以下四种使用场景：

1. 修饰成员变量和成员方法：被static修饰的成员属于类，不属于单个这个类的某个对象，被类中所有对象共享，并且建议通过类名调用。被static声明的成员变量属于静态成员变量，静态变量存放在java内存区域的方法区。
2. 静态代码块：静态代码块定义在类中方法外，静态代码块在非静态代码块之前执行（静态代码块--》非静态代码块---》构造方法）该类不管创建多少对象，静态代码块只执行一次
3. 静态内部类（static修饰类的话，只能修饰内部类）：静态内部类与非静态内部类之间存在一个最大区别：非静态内部类在编译完成之后会隐含地保存一个引用，该引用时指向创建它的外围类，但静态内部类却没有。没有引用意味着：
   1. 他的创建不需要依赖外围类的创建。
   2. 它不能使用任何外围类的非static成员放法。
4. 静态导包：import static这两个关键字连用可以指定导入某个类中的指定静态资源，并且不需要使用类名调用类中静态成员，可以直接使用类中静态成员变量和成员方法

#### this

this关键字用于引用类的当前实例

```java
class Manager{
	Employees[] employees;
	void managerEmployees(){
		int totalEmp=this.employees.length;
		System.out.println("Total employees"+totalEmp);
		this.report();
	}
	void report(){}
}
```

在上面的实例中，this关键字用于两个地方：

- this.employees.length 访问类Manager的当前实例变量
- this.report() 调用类Manager的当前实例方法

此关键字是可选的，这意味着如果上面的示例在不使用此关键字的情况下表现相同。但是，使用此关键字可能会使代码简单易读

#### super

super关键字用于从子类访问父类的变量和方法：

```java
pubic class Super{
	protected int number;
	protected showNumber(){
		System.out.println("number="+number);
	}
	
}
public class Sub extends Super{
	void bar(){
		super.number=10;
		super.showNumber();
	}
}
```

上面的例子中，Sub类访问父类成员变量number并调用其父类Super的showNumeb（）方法。

使用this和super要注意的问题：

- 在构造器中使用super（）调用父类中的其他构造方法时，该语句必须处于构造器的首行，否则编译器会报错，另外，this调用本类的中的其他构造器时，也要放在首航，
- this，super不能用在stattic方法中

被static修饰的成员属于类，不属于单个实例对象，对类中所有对象共享。而this代表对奔雷对象的引用。指向奔雷对象。而super代表对父类对象的引用，指向父类对象。所以，this和super时属于对象范畴的东西，而静态方法是属于类范畴的东西

#### static关键字

##### 修饰成员变量和成员方法

被static修饰的成员属于类，不属于单个这个类的某个对象，而被楼中所有对象共享，并且建议通过类名调用。被static声明的成员变量属于静态成员变量，静态变量存放在java内存区域的方法区

方法去于java堆一样，是各个相乘共享的内存区域，它用于存储已被虚拟机加载的类星系，常量，静态变量。即时编译器比那一后的代码等数据。虽然java虚拟机规范把方法区描述为堆的一个逻辑部分，但是它却有一个别名，叫做Non—Heap,目的应该是与java堆区分开

HotSpot虚拟机中方法区也通常被称为“永久代”，本质上两者并不等价。仅仅是因为HotSpot虚拟机设计团队用永久代来实现方法区而已，这样HotSpot虚拟机的垃圾收集器就可以管理java堆一样管理这部分内存。



