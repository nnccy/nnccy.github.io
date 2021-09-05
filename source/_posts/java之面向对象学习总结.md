---
title: java之面向对象学习总结
categories:
  - - java
tags:
  - 后端
  - java
abbrlink: eddaf841
copyright: true
date: 2021-06-25 16:43:36
---

java面向对象学习笔记总结，参考教程[Java基础视频-【深入浅出精华版视频】-刘意-经典27天完整版-黑马程序员](https://www.bilibili.com/video/BV1bt411S7fr?share_source=copy_web)

<!--more-->

## 匿名对象

没用名字的对象，匿名对象调用完毕就是垃圾，可以被垃圾回收器回收。

应用场景

1. 仅仅只调用一次的时候
2. 可以作为实际参数传递

```java
class Student{
    public void show(){
        System.out.println("我爱学习");
    }
}

class StudentDemo{
    public void method(Student s){
        s.show()
    }
}
//带名字的调用
public static void main(String[] args){
    Student s  = new Student();
    s.show();
    
    //匿名对象
	new Student().show();   //仅仅调用一次
    
    StudentDemo sd = new StudentDemo();
    sd.method(new Student());
}

```

## 构造方法

作用：用于对对象的数据进行初始化
格式：

- 方法名和类名相同
- 没有返回值类型，连void都不能有
- 没有返回值

```java
class Student{
    private String name;
    private int age;
    public Student(){           //无参
        System.out.println("这是构造方法");
    }
    public Student(String name,int age){           //带参
        this.name = name;
        this.age = age;
    }
}
Student s = new Student("小明"，12);

```

## static关键字

1. 静态的意思。可以修饰成员变量和成员方法。
2. 静态的特点：
   - 随着类的加载而加载
   - 优先与对象存在
   - 被类的所有对象共享 ，只能由一个对象赋值
     这其实也是我们判断该不该使用静态的依据。
   - 可以通过类名调用
     	既可以通过对象名调用，也可以通过类名调用，建议通过类名调用。

```java
class Student {
	//非静态变量
	int num = 10;
	
	//静态变量
	static int num2 = 20;
}

class StudentDemo {
	public static void main(String[] args) {
		Student s = new Student();
		System.out.println(s.num);
		
		System.out.println(Student.num2);
		System.out.println(s.num2);
	}
}
```

3. 静态的内存图
   ​		静态的内容在方法区的静态区

 [外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-3R6rd2La-1624635226013)(C:\Users\ncy\AppData\Roaming\Typora\typora-user-images\image-20210623223315341.png)]

4. 静态的注意事项；

   - 在静态方法中没有this对象
   - 静态只能访问静态(代码测试过),//因为静态随着累的加载而加载，this是随着对象的创建而存在，静态比对象先存在。

5. 静态变量和成员变量的区别

   - 所属不同
     静态变量：属于类，类变量
     成员变量：属于对象，对象变量，实例变量

   - 内存位置不同
     静态变量：方法区的静态区
     成员变量：堆内存

   - 生命周期不同

     静态变量：静态变量是随着类的加载而加载，随着类的消失而消失
     成员变量：成员变量是随着对象的创建而存在，随着对象的消失而消失

   - 调用不同
     静态变量：可以通过对象名调用，也可以通过类名调用
     成员变量：只能通过对象名调用

6. main方法是静态的
   ​		public:权限最大
   ​		static:不用创建对象调用
   ​		void:返回值给jvm没有意义
   ​		main:就是一个常见的名称。
   ​		String[] args:可以接收数据，提供程序的灵活性，早期为了接收键盘录入的数据

```java
java MainDemo hello world java
java MainDemo 10 20 30
```

## 使用JDK帮助文档

1. 打开帮助文档

2. 点击显示，找到索引，看到输入框

3. 知道你要找谁?以Scanner举例

4. 在输入框里面输入Scanner，然后回车

5. 看包java.lang包下的类不需要导入，其他的全部需要导入。	

   要导入：
   java.util.Scanner

6. 再简单的看看类的解释和说明，别忘了看看该类的版本

7. 看类的结构
   	成员变量	字段摘要 	
   	构造方法	构造方法摘要 
   	成员方法 	方法摘要

8. 学习构造方法	

   - 有构造方法	就创建对象
   - 没有构造方法	成员可能都是静态的

9. 看成员方法

   - 左边
     是否静态：如果静态，可以通过类名调用
     返回值类型：人家返回什么，你就用什么接收。
   - 右边
     看方法名：方法名称不要写错
     参数列表：人家要什么，你就给什么；人家要几个，你就给几个

## 代码块

用{}括起来的代码。

1. 分类：

   - 局部代码块
     出现在方法中，用于限定变量的生命周期，及早释放，提高内存利用率。
   - 构造代码块
     在类中的成员位置，在成员方法和构造方法外，把多个构造方法中相同的代码可以放到这里，每个构造方法执行前，首先执行构造代码块，以后无论调用哪个构造方法都会先执行构造代码块。
   - 静态代码块
     	在类中的成员位置，用static修饰对类的数据进行初始化，仅仅只执行一次。

   ```java
   static{}
   ```

   2. 静态代码块,构造代码块,构造方法的顺序问题?
      	静态代码块 > 构造代码块 > 构造方法

## 继承

把多个类中相同的成员给提取出来定义到一个独立的类中。然后让这多个类和该独立的类产生一个关系，
	   这多个类就具备了这些内容。这个关系叫继承。

1. Java中如何表示继承呢?格式是什么呢?
   - 用关键字extends表示
   - 格式：

```java
class 子类名 extends 父类名 {}

class Person {
	public void eat() {
		System.out.println("吃饭");
	}
	
	public void sleep() {
		System.out.println("睡觉");
	}
}

class Student extends Person {}

class Teacher extends Person {}

class ExtendsDemo {
	public static void main(String[] args) {
		Student s = new Student();
		s.eat();
		s.sleep();
		System.out.println("-------------");
		
		Teacher t = new Teacher();
		t.eat();
		t.sleep();
	}
}
```

2. 继承的好处：
   - 提高了代码的复用性
   - 提高了代码的维护性
   - 让类与类产生了一个关系，是多态的前提
3. 继承的弊端：
   - 让类的耦合性增强。这样某个类的改变，就会影响其他和该类相关的类。
     	原则：低耦合，高内聚。
          			耦合：类与类的关系
          			内聚：自己完成某件事情的能力
   - 打破了封装性
4. Java中继承的特点
   - Java中类只支持单继承
   - Java中可以多层(重)继承(继承体系)
5. 继承的注意事项：
   - 子类不能继承父类的私有成员（私有成员变量和成员方法）
   - 子类不能继承父类的构造方法，但是可以通过super去访问
   - 不要为了部分功能而去继承
6. 什么时候使用继承呢? 
   - 继承体现的是：is a的关系。
   - 采用假设法
7. Java继承中的成员关系
   - 成员变量
     a:子类的成员变量名称和父类中的成员变量名称不一样，这个太简单
     b:子类的成员变量名称和父类中的成员变量名称一样，这个怎么访问呢?
     子类的方法访问变量的查找顺序：（就近原则）
     在子类方法的局部范围找，有就使用。
     在子类的成员范围找，有就使用。
     在父类的成员范围找，有就使用。
     找不到，就报错。
   - 构造方法
     - 子类的构造方法默认会去访问父类的无参构造方法
       是为了子类访问父类数据的初始化
     - 父类中如果没有无参构造方法，怎么办?
       子类通过super去明确调用带参构造
       子类通过this调用本身的其他构造，但是一定会有一个去访问了父类的构造
       让父类提供无参构造
     - 成员方法
       子类的成员方法和父类中的成员方法名称不一样，这个太简单
       子类的成员方法和父类中的成员方法名称一样，这个怎么访问呢?
       通过子类对象访问一个方法的查找顺序：
       在子类中找，有就使用
       在父类中找，有就使用
       找不到，就报错

```java
/*
A:定义一个手机类。
B:通过研究，我发明了一个新手机，这个手机的作用是在打完电话后，可以听天气预报。
按照我们基本的设计，我们把代码给写出来了。
但是呢?我们又发现新手机应该是手机，所以，它应该继承自手机。
其实这个时候的设计，并不是最好的。
因为手机打电话功能，是手机本身就具备的最基本的功能。
所以，我的新手机是不用在提供这个功能的。
但是，这个时候，打电话功能就没有了。这个不好。
最终，还是加上这个功能。由于它继承了手机类，所以，我们就直接使用父类的功能即可。
那么，如何使用父类的功能呢?通过super关键字调用
*/
class Phone {
	public void call(String name) {
		System.out.println("给"+name+"打电话");
	}
}

class NewPhone extends Phone {
	public void call(String name) {
		//System.out.println("给"+name+"打电话");
		super.call(name);
		System.out.println("可以听天气预报了");
	}
}

class ExtendsDemo9 {
	public static void main(String[] args) {
		NewPhone np = new NewPhone();
		np.call("林青霞");
	}
}
```

方法重写的注意事项

- 父类中私有方法不能被重写
  因为父类私有方法子类根本就无法继承
- 子类重写父类方法时，访问权限不能更低(public 不能降低 private)
  最好就一致
- 父类静态方法，子类也必须通过静态方法进行重写
  其实这个算不上方法重写，但是现象确实如此，至于为什么算不上方法重写，多态中我会讲解
  子类重写父类方法的时候，最好声明一模一样。



## this和super

this代表本类对应的引用。
super代表父类存储空间的标识(可以理解为父类引用,可以操作父类的成员)
怎么用呢?

- 调用成员变量
  this.成员变量 调用本类的成员变量
  super.成员变量 调用父类的成员变量
- 调用构造方法
  this(...)	调用本类的构造方法
  super(...)	调用父类的构造方法

继承中构造方法的关系

- 子类中所有的构造方法默认都会访问父类中空参数的构造方法
- 为什么呢? 
  因为子类会继承父类中的数据，可能还会使用父类的数据。
  所以，子类初始化之前，一定要先完成父类数据的初始化。
  注意：子类每一个构造方法的第一条语句默认都是：super();
- 调用成员方法
  this.成员方法 调用本类的成员方法
  super.成员方法 调用父类的成员方法

如果父类没有无参构造方法，那么子类的构造方法会出现什么现象呢?
	报错。
如何解决呢?	

- 在父类中加一个无参构造方法
- 通过使用super关键字去显示的调用父类的带参构造方法
- 子类通过this去调用本类的其他构造方法
  	子类中一定要有一个去访问了父类的构造方法，否则父类数据就没有初始化。


```java
class Son extends Father {
	public Son() {
		super("随便给");
		System.out.println("Son的无参构造方法");
		//super("随便给");
	}
	
	public Son(String name) {
		//super("随便给");
		this();
		System.out.println("Son的带参构造方法");
	}
}
```

注意事项：
	this(...)或者super(...)必须出现在第一条语句上。
	如果不是放在第一条语句上，就可能对父类的数据进行了多次初始化，所以必须放在第一条语句上

## final关键字

1. 是最终的意思，可以修饰类，方法，变量。
2. 特点：
   - 它修饰的类，不能被继承。
   - 它修饰的方法，不能被重写。
   - 它修饰的变量，是一个常量。
3. 其他
   - 局部变量
     a:基本类型 值不能发生改变
     b:引用类型 地址值不能发生改变，但是对象的内容是可以改变的
   - 初始化时机
     a:被final修饰的变量只能被修饰一次，只能初始化一次。
     b:常见的给值
       定义的时候。(推荐)
       构造方法中。



## 多态

1. 同一个对象在不同时刻体现出来的不同状态。
   猫可以是猫的类型。猫 m = new 猫();
   同时猫也是动物的一种，也可以把猫称为动物。
   动物 d = new 猫();
   在举一个例子：水在不同时刻的状态

2. 多态的前提：
   有继承或者实现关系。
   有方法重写。其实没有也可以，但是如果没有就没有意义。
   有父类或者父接口引用指向子类对象。

   父 f = new 子

​		多态的分类：

```java
	a:具体类多态		class Fu {}		class Zi extends Fu {}		Fu f = new Zi();	b:抽象类多态		abstract class Fu {}		class Zi extends Fu {}				Fu f = new Zi();	c:接口多态		interface Fu {}		class Zi implements Fu {}				Fu f = new Zi();
```

3. 多态中的成员访问特点

   - 成员变量
     编译看左边，运行看左边
   - 构造方法 
     子类的构造都会默认访问父类构造，对父类的数据进行初始化
   - 成员方法
     编译看左边，运行看右边
   - 静态方法
     编译看左边，运行看左边
     因为成员方法有重写。

4. 多态的好处：

   - 提高代码的维护性(继承体现)
   - 提高代码的扩展性(多态体现)（工具类:不想重写）

5. 多态的弊端：
   	父不能使用子的特有功能。（在子类中新写的方法，不能使用，扩充后的功能，不能使用，只能创建子类对象）子可以当作父使用，父不能当作子使用。

   多态的弊端：
   		不能使用子类的特有功能。
   怎么用呢?

   - 创建子类对象调用方法即可。(可以，但是很多时候不合理。而且，太占内存了)
   - 把父类的引用强制转换为子类的引用。(向下转型)
     对象间的转型问题：
     向上转型：
     		Fu f = new Zi();
     向下转型：
     		Zi z = (Zi)f; //要求该f必须是能够转换为Zi的。	

6. 多态中的转型

   - 向上转型
     从子到父
   - 向下转型
     从父到子

## 抽象类

1. 把多个共性的东西提取到一个类中，这是继承的做法。
   但是呢，这多个共性的东西，在有些时候，方法声明一样，但是方法体。
   也就是说，方法声明一样，但是每个具体的对象在具体实现的时候内容不一样。
   所以，我们在定义这些共性的方法的时候，就不能给出具体的方法体。
   而一个没有具体的方法体的方法是抽象的方法。
   在一个类中如果有抽象方法，该类必须定义为抽象类。
2. 抽象类的特点
   - 抽象类和抽象方法必须用关键字abstract修饰
   - 抽象类中不一定有抽象方法,但是有抽象方法的类一定是抽象类
   - 抽象类不能实例化
   - 抽象类的子类
     是一个抽象类。
     是一个具体类。这个类必须重写抽象类中的所有抽象方法。
3. 抽象类的成员特点：
   - 成员变量
     有变量，有常量
   - 构造方法
     有构造方法
   - 成员方法
     有抽象，有非抽象
4. 抽象类的几个小问题
   - 抽象类有构造方法，不能实例化，那么构造方法有什么用?
     用于子类访问父类数据的初始化
   - 一个类如果没有抽象方法,却定义为了抽象类，有什么用?
     为了不让创建对象
   - abstract不能和哪些关键字共存
     a:final	冲突
     b:private 冲突
     c:static 无意义

## 接口

1. 猫狗案例，它们仅仅提供一些基本功能。
   比如：猫钻火圈，狗跳高等功能，不是动物本身就具备的，
   是在后面的培养中训练出来的，这种额外的功能，java提供了接口表示。

```java
//定义动物培训接口interface AnimalTrain {	public abstract void jump();}//抽象类实现接口abstract class Dog implements AnimalTrain {}//具体类实现接口class Cat implements AnimalTrain {	public void jump() {		System.out.println("猫可以跳高了");	}}class InterfaceDemo {	public static void main(String[] args) {		//AnimalTrain是抽象的; 无法实例化		//AnimalTrain at = new AnimalTrain();		//at.jump();				AnimalTrain at = new Cat();		at.jump();	}}
```

2. 接口的特点：
   - 接口用关键字interface修饰
     interface 接口名 {}
   - 类实现接口用implements修饰
     	class 类名 implements 接口名 {}
   - 接口不能实例化
   - 接口的实现类
     a:是一个抽象类。意义不大。
     b:是一个具体类，这个类必须重写接口中的所有抽象方法。
3. 接口的成员特点：
   - 成员变量
     只能是常量
     默认修饰符：public static final
   - 构造方法
     没有构造方法
   - 成员方法
     只能是 的
     默认修饰符：public abstract
4. 类与类,类与接口,接口与接口
   - 类与类
     继承关系，只能单继承，可以多层继承
   - 类与接口
     实现关系，可以单实现，也可以多实现。
     还可以在继承一个类的同时，实现多个接口
   - 接口与接口
     继承关系，可以单继承，也可以多继承
5. 抽象类和接口的区别(自己补齐)?
   - 成员区别
     抽象类：变量，常亮；有抽象方法；抽象方法，非抽象方法
     接口：常亮；抽象方法
   - 关系区别:
     类与类：继承，单继承
     类与接口：实现关系，可以单实现，也可以多实现。
     接口与接口：	继承关系，可以单继承，也可以多继承
   - 设计理念不同 
     抽象类：is a，抽象类中定义的是共性功能。
     接口：like a，接口中定义的是扩展功能。



## 包

1. 其实就是文件夹
2. 作用：
   - 区分同名的类
   - 对类进行分类管理
     a:按照功能分
     b:按照模块分
3. 包的定义(掌握)
   package 包名;
   多级包用.分开。
4. 注意事项：(掌握)
   - package语句必须在文件中的第一条有效语句
   - 在一个java文件中，只能有一个package
   - 如果没有package，默认就是无包名
5. 带包的编译和运行
   - 手动式
     a:编写一个带包的java文件。
     b:通过javac命令编译该java文件。
     c:手动创建包名。
     d:把b步骤的class文件放到c步骤的最底层包
     e:回到和包根目录在同一目录的地方，然后运行，带包运行。
   - 自动式，可以自动生成文件夹，并将class文件加到文件夹中
     a:编写一个带包的java文件。
     b:javac编译的时候带上-d即可
           javac -d . HelloWorld.java
     c:回到和包根目录在同一目录的地方，然后运行,带包运行。

导包

1. 我们多次使用一个带包的类，非常的麻烦，这个时候，Java就提供了一个关键字import。
2. 格式：
   import 包名...类名;
   另一种：
   import 包名...*;(不建议)，只导包下你想要的那个类就可以。
3. package,import,class的顺序
   	package > import > class

## 权限等级

[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-IOfKzruG-1624635226015)(C:\Users\ncy\AppData\Roaming\Typora\typora-user-images\image-20210625221827006.png)]

常见的修饰符(理解)

1. 分类：
   	权限修饰符：private,默认,protected,public
      		状态修饰符：static,final
      		抽象修饰符：abstract
2. 常见的类及其组成的修饰
   	类：
      			默认,public,final,abstract
      			常用的：public
      	成员变量：
      	private,默认,protected,public,static,final
      	常用的：private

构造方法：
	private,默认,protected,public
	常用的：public

成员方法：
	private,默认,protected,public,static,final,abstract
	常用的：public

3. 另外比较常见的：
   	public static final int X = 10;
   	public static void show() {}
   	public final void show() {}
   	public abstract void show();

​	

## 内部类

1. 把类定义在另一个类的内部，该类就被称为内部类。
2. 内部类的访问规则
   - 可以直接访问外部类的成员，包括私有
   - 外部类要想访问内部类成员，必须创建对象
3. 内部类的分类
   - 成员内部类
   - 局部内部类

```java
class Outer {	private int num = 10;	//成员位置	/*	class Inner {			}	*/		public void method() {		//局部位置		class Inner {				}	}}class InnerClassDemo2 {	public static void main(String[] args) {			}}
```



4. 成员内部类
   - private 为了数据的安全性
   - static 为了访问的方便性
     	成员内部类不是静态的：
       		**外部类名.内部类名 对象名 = new 外部类名.new 内部类名();**
     成员内部类是静态的：(成员内部类被静态修饰后，是不能通过外部对象访问的)
       		**外部类名.内部类名 对象名 = new 外部类名.内部类名();**

5. 成员内部类的面试题(填空)
   	30,20,10

```java
	class Outer {		public int num = 10;				class Inner {			public int num = 20;						public viod show() {				int num  = 30;								System.out.println(num);				System.out.println(this.num);				System.out.println(Outer.this.num);			}		}	}
```

6. 局部内部类
   - 局部内部类访问局部变量必须加final修饰。
   - 为什么呢?
     	因为局部变量使用完毕就消失，而堆内存的数据并不会立即消失。
       		所以，堆内存还是用该变量，而改变量已经没有了。
       		为了让该值还存在，就加final修饰。
       		通过反编译工具我们看到了，加入final后，堆内存直接存储的是值，而不是变量名。

```java
class Outer {	private int num  = 10;		public void method() {		//int num2 = 20;		final int num2 = 20;		class Inner {			public void show() {				System.out.println(num);				//从内部类中访问本地变量num2; 需要被声明为最终类型				System.out.println(num2);//20			}		}				System.out.println(num2);				Inner i = new Inner();		i.show();	}}class InnerClassDemo5 {	public static void main(String[] args) {		Outer o = new Outer();		o.method();	}}
```



7. 匿名内部类(掌握)
   - 是局部内部类的简化形式
   - 前提
     	存在一个类或者接口
   - 格式:
     	new 类名或者接口名() {
       			重写方法;
       		}
   - 本质：
     	其实是继承该类或者实现接口的子类匿名对象

```java
interface Inter {	public abstract void show();	public abstract void show2();}class Outer {	public void method() {		//一个方法的时候		/*		new Inter() {			public void show() {				System.out.println("show");			}		}.show();		*/				//二个方法的时候		/*		new Inter() {			public void show() {				System.out.println("show");			}						public void show2() {				System.out.println("show2");			}		}.show();				new Inter() {			public void show() {				System.out.println("show");			}						public void show2() {				System.out.println("show2");			}		}.show2();		*/				//如果我是很多个方法，就很麻烦了		//那么，我们有没有改进的方案呢?		Inter i = new Inter() { //多态			public void show() {				System.out.println("show");			}						public void show2() {				System.out.println("show2");			}		};				i.show();		i.show2();	}}class InnerClassDemo6 {	public static void main(String[] args) {		Outer o = new Outer();		o.method();	}}
```



8. 匿名内部类在开发中的使用
   	我们在开发的时候，会看到抽象类，或者接口作为参数。
   	而这个时候，我们知道实际需要的是一个子类对象。
   	如果该方法仅仅调用一次，我们就可以使用匿名内部类的格式简化。
   	

```java
interface Person {	public abstract void study();}class PersonDemo {	public void method(Person p) {		p.study();	}}class PersonTest {	public static void main(String[] args) {		PersonDemo pd = new PersonDemo();		pd.method(new Person() {			public void study() {				System.out.println("好好学习，天天向上");			}		});	}}
```



## 小知识

1. 同一个文件夹下，不同的类写在同一文件和不同文件下效果是一样的。
2. 将构造方法私有，或者没有构造方法 ，外界就不能创建对象了，可通过静态，用类名访问（8-1）

