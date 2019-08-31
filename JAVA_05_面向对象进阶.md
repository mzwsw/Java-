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
   
      * "=="：方法名、形参列表相同。
      
      			* “<=”：返回值类型和声明类型，子类小于父类
   			* “>=”：访问权限，子类大于父类
      
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

   ​	**若是构造方法的第一行没有显式的调用super(...)或者this(...)，那么Java默认都会调用super()，含义是调用父类的无参数构造方法**。这的super()可以省略。

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
      
   3. **封装的使用细节**
   
      **类的属性的处理**
   
        1. 一般使用private访问权限
   
        2. 提供相应的get/set方法来访问相关属性，这些方法通常是public修饰的，以提高对属性的赋值与读取操作（boolean变量的get方法是is开头！！）。
   
        3. 一些只用于本类的辅助性方法可以用private修饰，希望其他类调用的方法用public修饰。
   
           ```java
           public class Person5 {
           	public static void main(String[] args) {
           		Boy b = new Boy();
           //		b.name = "zsk";    编译错误，变量不可见
           		b.setName("zsk");
           		b.setAge(-45);
           		System.out.println(b);
           		
           		Boy b1 = new Boy("llb",14);
           		System.out.println(b1);
           	}
           }
           
           
           class Boy {
           	private String name;
           	private int age;
           	
           	public Boy() {
           		
           	}
           	
           	public Boy(String name, int age) {
           		this.name = name;
           		//构造方法中不要直接赋值，应该调用setAge方法
           		setAge(age);
           	}
           
           	public String getName() {
           		return name;
           	}
           
           	public void setName(String name) {
           		this.name = name;
           	}
           
           	public int getAge() {
           		return age;
           	}
           
           	public void setAge(int age) {
           		//在赋值之前可以先进行判断
           		if(age < 130 && age >=1) {
           			this.age = age;
           		}else {
           			System.out.println("请输入正确的年龄！");
           		}
           	}
           	
           	@Override
           	public String toString() {
           		return "Person [name =" + name + ", age =" + age + "]";
           	}
           }
           ```
   
5. 多态

   ​		多态指的是同一个方法调用，由于对象不同可能会有不同的行为。

   ​		多态的要点：

   * 多态是方法的多态，不是属性的多态。
   * 多态的存在要有**3个必要条件：继承，方法重写，父类引用指向子类对象**。
   * 父类引用指向子类对象后，用该父类引用调用子类重写的方法，此时多态就出现了。

   ```java
   /**
    * 测试多态
    * @author zsk
    *
    */
   public class TestPolym {
   	public static void main(String[] args) {
   		Animal a = new Animal();
   		animalCry(a);
   		
   		Dog d = new Dog();
   		//传的具体是哪一类就调用哪一个类的方法。大大提高了程序的可扩展性
   		animalCry(d);
   		animalCry(new Cat());
   	}
   	
   	//有了多态，只需要让增加的这个类继承Animal类就可以了
   	static void animalCry(Animal a) {  
   		a.shout();
   	}
   	
   	/*如果没有多态，这里需要写很多重载的方法。
   	 * 每增加一种动物，就需要重载一种动物的叫法。
   	 * static void animalCry(Dog d){
   	 * 		d.shout();
   	 * }
   	 * static void animalCry(Cat c){
   	 * 		c.shout();
   	 * }
   	 */
   }
   
   
   class Animal {
       public void shout() {
           System.out.println("叫了一声！");
       }
   }
   class Dog extends Animal {
       public void shout() {
           System.out.println("旺旺旺！");
       }
       public void seeDoor() {
           System.out.println("看门中....");
       }
   }
   class Cat extends Animal {
       public void shout() {
           System.out.println("喵喵喵喵！");
       }
   }
   ```

   ​		上例是多态最常见的一种方法，即父类引用做方法的形参，实参可以是任意的子类对象，可以通过不同的子类对象实现不同的行为方式。
   
6. 对象的转型

   ​		父类引用指向子类对象，这个过程称为向上转型，属于自动类型转换。

   ​		向上转型后的父类引用变量只能调用它编译类型的方法，不能调用它运行时类型的方法。这时，我们需要进行强制类型转换，称之为向下转型。

   ```java
   public class TestCasting {
       public static void main(String[] args) {
           Object obj = new String("我在学JAVA"); // 向上可以自动转型
           // obj.charAt(0) 无法调用。编译器认为obj是Object类型而不是String类型
           /* 编写程序时，如果想调用运行时类型的方法，只能进行强制类型转换。
            * 不然通不过编译器的检查。 */
           String str = (String) obj; // 向下转型
           System.out.println(str.charAt(0)); // 位于0索引位置的字符
           System.out.println(obj == str); // true.他们俩运行时是同一个对象
       }
   }
   ```

   ​		在向下转型过程中，必须将引用变量转成真实的子类类型（运行时类型），否则会出现类型转换异常ClassCastException。

   ```java
   public class TestCasting2 {
       public static void main(String[] args) {
           Object obj = new String("我在学JAVA");
           //真实的子类类型是String，但是此处向下转型为StringBuffer
           StringBuffer str = (StringBuffer) obj;
           System.out.println(str.charAt(0));
       }
   }
   ```

7. final关键字

   **作用**：

   * 修饰变量：即常量，一旦赋了初值，不能被重新赋值。
   * 修饰方法：该方法不可被子类重写。但是可以被重载。
   * 修饰类：修饰的类不能被继承。比如：Math、String等。
   
8. 抽象方法和抽象类

   * **抽象方法**

     ​		使用abstract修饰的方法，没有方法体，只有声明。定义的是一种“规范”，就是告诉子类必须要给抽象方法提供具体的实现。

   * **抽象类**

     ​		包含抽象方法的类就是抽象类。通过abstract方法定义规范，然后要求子类必须定义具体事项。通过抽象类，可以做到严格限制子类的设计，使子类之间更加通用。

     **抽象类的使用要点**：

     		* 有抽象方法的类只能定义成抽象类
     		* 抽象类不能实例化，即不能用new来实例化抽象类
     		* 抽象类可以包含属性、方法、构造方法。但是构造方法不能用来new实例，只能用来被子类调用。
     		* 抽象类只能用来被继承
     		* 抽象方法必须被子类实现

     ```java
     /**
      * 测试抽象类、抽象方法
      * @author lenovo
      *
      */
     //抽象类
     abstract class Animal {
     	abstract public void shout();
     }
     
     class Dog extends Animal {
     	//子类必须实现父类的抽象方法，否则编译错误
     	public void shout() {
     		System.out.println("汪汪汪");
     	}
     	public void seeDoor() {
     		System.out.println("看门");
     	}
     }
     
     //测试抽象类
     public class TestAbstract {
     	public static void main(String[] args) {
     		Dog a = new Dog();
     		a.shout();
     		a.seeDoor();
     	}
     }
     ```

9. 接口

   1. **接口的使用**

      * 接口和抽象类的区别

        ​		接口就是比“抽象类”还“抽象”的“抽象类”，可以更加规范的对子类进行约束。全面的专业的实现了：规范和具体实现的分离。

        ​		抽象类提供某些具体实现，接口不提供任何具体实现，接口中所有方法都是抽象方法。接口是完全面向规范的，规定了一批类具有的公共方法规范。

      * 接口的本质探讨

        接口就是规范，定义的是一组规则。

      * 区别

        		1. 普通类：具体实现
          		2. 抽象类：具体事项，规范（抽象方法）
          		3. 接口：规范！

   2. **定义接口**

      ```java
      [访问修饰符] interface 接口名 [extends 父接口1,父接口2]{
          常量定义;
          方法定义;
      }
      ```

      说明：

      		1. 访问修饰符：只能是public或默认
        		2. 接口名：和类名采用相同的命名机制
        		3. extends：**接口可以多继承**（类只能单继承）
        		4. 常量：接口中的属性只能是常量，总是：public static final修饰，不写也是。
        		5. 方法：接口中的方法只能是抽象方法，总是public abstract。省略也是。

      **要点**：

      		* 子类通过implements来实现接口中的规范。
      		* 接口不能创建实例，但是可用于声明引用变量类型。
      		* 一个类实现了接口，必须实现接口中所有的方法，并且这些方法只能是public的
      		* JDK1.8之后，接口中可以包含普通的静态方法。

   3. **接口的多继承**

      ​		接口完全支持多继承。和类的继承类似，子接口扩展某个父接口，将会获得父接口中所定义的一切。

      ```java
      interface A {
          void testa();
      }
      interface B {
          void testb();
      }
      /**接口可以多继承：接口C继承接口A和B*/
      interface C extends A, B {
          void testc();
      }
      public class Test implements C {
          public void testc() {
          }
          public void testa() {
          }
          public void testb() {
          }
      }
      ```

      
