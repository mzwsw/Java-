# 五、JAVA面向对象进阶

1. 概述

   ​		本章重点针对面向对象的三大特征：继承、封装、多态进行详细的讲解。另外还包括抽象类、接口、内部类等概念。

   ---

   1. **继承**

      继承更加容易实现类的扩展。extends的意思是“扩展”。子类是父类的扩展。
      
      ```java
      package cn.sxt.oo;
      
      public class TestExtends {
      	public static void main(String[] args) {
      		Student s = new Student("zsk",172,"Java");
      		s.rest();
      		s.study();
      	}
      }
      
      class Person {
      	String name;
      	int height;
      	public void rest() {
      		System.out.println("休息一会！");
      	}
      }
      
      class Student extends Person {
      	String major;
      	public void study() {
      		System.out.println("学习java");
      	} 
      	public Student(String name, int height, String major) {
      		this.name = name;
      		this.height = height;
      		this.major = major;
      	}
      	public Student() {
      		
      	}
      }
      ```
      
   2. **instanceof运算符**
   
      ​		instanceof是二元运算符，左边是对象，右边是类；当对象是右面类或子类所创建对象时，返回true；否则，返回false。
   
   3. **继承使用要点**
   
      1. 父类也称为超类、基类、衍生类等。
      2. Java中只有单继承，没有C++中的多继承。
      3. Java中类没有多继承，接口有多继承。
      4. 子类继承父类，可以得到父类的全部属性和方法（除了构造方法），但不见得可以直接访问（如，父类私有的属性和方法）。
      5. 如果定义一个类时，没有调用extends，则它的父类是：java.lang.Object。
   
   4. **方法的重写override**
   
      ​	子类通过重写父类的方法，可以用自身的行为替换父类的行为。方法的重写是实现多态的必要条件。
   
      ​	**三个要点**：
   
          1. 	“==”：方法名、形参列表相同
             2. 	“<=“：返回值类型和声明类型，子类小于等于父类
             3. 	“>=”：访问权限，子类大于父类。
   
      ```java
      package cn.sxt.oo;
      
      /**
       * 测试方法的重写/覆盖
       * @author zsk
       *
       */
      public class TestOverride {
      	public static void main(String[] args) {
      		Horse h = new Horse();
      		h.run();
      	}
      }
      
      
      class Vehicle {
      	public void run() {
      		System.out.println("跑。。。。");
      	}
      	public void stop() {
      		System.out.println("停止！");
      	}
      }
      
      class Horse extends Vehicle {
      	public void run() {
      		System.out.println("四蹄纷飞");
      	}
      }
      ```
   
2. Object

   1. **Object类基本特性**

      ​		Object类是所有Java类的根基类，也就意味着所有的Java对象都拥有Object类的属性和方法。如果在类的声明中未使用extends关键字指明其父类，则默认继承Object类。

   2. **toString**方法

      ​		Object类中定义有public String toString()方法，其返回值是 String 类型。Object类中toString方法的源码为：

      ```java
      public String toString() {
          return getClass().getName() + "@" + Integer.toHexString(hashCode());
      }
      ```

      * 测试toString方法和重写toString方法

        ```java
        package cn.sxt.oo;
        
        public class TestObject {
        	public static void main(String[] args) {
        		Person2 p = new Person2();
        		p.age = 20;
        		p.name = "zsk";
        		System.out.println("info:"+p);  //打印p，默认调用p.toString()
        		
        		TestObject t = new TestObject();
        		System.out.println(t);
        	}
        }
        
        
        class Person2{
        	String name;
        	int age;
        	
        	@Override
        	public String toString() {
        		return name+"，年龄:"+age;
        	}
        }
        ```

   3. **==和equals方法**

      ​		“==”代表比较双方是否相同。如果是基本类型则表示值相等，如果是引用类型则表示地址相等即是同一个对象。

      ​		Object类中定义有：public boolean equals(Object obj)方法，提供定义“对象内容相等”的逻辑。

      ​		Object的equals方法默认就是比较两个对象的hashcode，是同一个对象的引用时返回true，否则返回false。但是可以根据自己的要求重写equals方法。

      ​		JDK提供的一些类，如String、Data、包装类等，重写了Object的equals方法。

      ```java
      package cn.sxt.oo;
      
      public class TestEquals {
      	public static void main(String[] args) {
      		
      		User u1 = new User(1000,"zsk","123456");
      		User u2 = new User(1000,"zskllb","123456");
      		
      		System.out.println(u1 == u2);
      		System.out.println(u1.equals(u2));
      	}
      }
      
      
      class User {
      	int id; 
      	String name;
      	String pwd;
      	
      	public User(int id, String name, String pwd) {
      		super();
      		this.id = id;
      		this.name = name;
      		this.pwd = pwd;
      	}
      
      	@Override
      	public boolean equals(Object obj) {
      		if (this == obj)
      			return true;
      		if (obj == null)
      			return false;
      		if (getClass() != obj.getClass())
      			return false;
      		User other = (User) obj;
      		if (id != other.id)
      			return false;
      		return true;
      	}
      }
      ```

3. super关键字

   ​	super是直接父类对象的引用。可以通过super来访问父类中被子类覆盖的方法或属性。

   ​	使用super调用普通方法，语句没有位置限制，可以在子类中随便调用。

   ​	**若是构造方法的第一行没有显式的调用super(...)或者this(...)，那么Java默认都会调用super()，含义是调用父类的无参数构造方法。**这的super()可以省略。

   ```java
   public class TestSuper01 {
   		public static void main(String[] args) {
   			new ChildClass().f();
   		}
   }
   
   
   class FatherClass {
   	public int value;
   	public void f() {
   		value = 100;
   		System.out.println("FatherClass.value=" + value);
   	}
   }
   
   class ChildClass extends FatherClass {
   	public int value;
   	public void f() {
   		super.f();   //调用父类对象的普通方法
   		value = 200;
   		System.out.println("ChildClass.value=" + value);
   		System.out.println(value);
   		System.out.println(super.value);  //调用父类对象的成员变量
   	}
   }
   ```

   ---

   1. **继承树追溯**

      * 构造方法调用顺序：

        ​    构造方法第一句总是：super(...)来调用父类对应的构造方法。所以，流程就是：先向上追溯到Object，然后再依次向下执行类的初始化块和构造方法，知道当前子类为止。

        ​	注：静态初始化块调用顺序，与构造方法调用顺序一样。

        ```java
        public class TestSuper02 {
        	public static void main(String[] args) {
        		System.out.println("开始创建一个ChildClass对象........");
        		new ChildClass2();
        	}
        }
        
        class FatherClass2 {
        	public FatherClass2() {
        		System.out.println("创建FatherClass");
        	}
        }
        
        class ChildClass2 extends FatherClass2 {
        	public ChildClass2() {
        		System.out.println("创建ChildClass");
        	}
        }
        ```

4. 封装

   1. **封装的作用和含义**

      ​		封装就是把对象的属性和操作结合为一个独立的整体，并尽可能隐藏对象的内部实现细节。

      ​		程序设计追求“高内聚，低耦合”。高内聚就是类的内部数据操作细节自己完成，不允许外部干涉；低耦合是仅暴露少量的方法给外部使用，尽量方便外部调用。

      **编程中封装的优点**：

      1. 提高代码的安全性；
      2. 提高代码的复用性；
      3. “高内聚”：封装细节，便于修改内部代码，提高可维护性；
      4. “低耦合”：简化外部调用，便于调用者使用，便于扩展和协作。

   2. **封装的实现**-使用访问控制符

      ​		Java是使用“访问控制符”来控制哪些细节需要封装，哪些细节需要暴露。

      1. **private** 表示私有，只有自己类能访问
      2. **default** 表示没有修饰符修饰，只有同一个包的类能访问
      3. **protected** 表示可以被同一个包的类以及其他包中的子类访问
      4. **public** 表示可以被该项目的所有包中的所有类访问
