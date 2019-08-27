# JAVA面向对象基础

1. 面向过程和面向对象

      * 面向对象和面向过程的总结：
            * 都是解决问题的思维方式，都是代码组织的方式
            * 解决简单问题可以使用面向过程
            * 解决复杂问题：宏观上使用面向对象把握，微观处理上仍然是面向过程

   * 面向对象具有三大特征：**封装性、继承性和多态性**

2. 对象的进化史

3. 对象和类的概念

   类可以看做是一个模板，或者图纸，系统根据类的定义来造出对象。类就是对象的抽象。

   类：class。对象：Object，instance(实例)。

   总结：

      * 对象是具体的事物；类是对对象的抽象；
      * 类可以看成一类对象的模板，对象可以看成该类的一个具体实例。
      * 类是用于描述同一类型的对象的一个抽象概念，类中定义了这一类对象所应具有的共同的属性、方法。

   ---

   1. 第一个类的定义

      ```java
      //每一个源文件必须有且只有一个public class，并且类名和文件名保持一致！
      public class Car{    
      }
      class Tyre{    //一个java文件可以同时定义多个class
      }
      class Engine{
      }
      class Seat{
      }
      ```

      一般类有三种常见的成员：**属性field、方法method、构造器constructor**。都可以定义零个或多个。

      示例：简单学生类编写

      ```java
      public class SxtStu {
      	
      	// 属性field 
      	int id;
      	String name;
      	int age;
      	
      	void study() {
      		System.out.println("我在学习！！！");
      	}
      	
      	void play() {
      		System.out.println("我在玩游戏！！！");
      	}
      	
          //构造方法，用于创建这个类的对象，无参的构造方法可以由系统自动创建
      	SxtStu(){
      	}
      	
      	//程序执行的入口
      	public static void main(String[] args) {
      		SxtStu stu = new SxtStu();  //创建一个对象
      		stu.play();
   	    }
      }
      ```
      
   2. 属性
   
      属性用于定义该类或该类对象包含的数据或者说静态特征。属性作用范围是整个类体。
   
      ```java
       [修饰符] 属性类型 属性名 = [默认值];
      ```
   3. 方法
   
      方法用于定义该类或该类实例的行为特征和功能实现。方法是类和对象行为特征的抽象。方法很类似于面向过程中的函数。面向对象中，整个程序的基本单位是类，方法是从属于类和对象的。
   
      ```java
      [修饰符] 方法返回值类型 方法名(形参列表){
          //n条语句
      }
      ```
   
   4. 一个典型类的定义和UML图
   
      ```java
      class Computer {
          String brand;  //品牌
      }
      public class SxtStu {
          // field
          int id;
          String sname;
          int age;
          Computer comp;
          void study() {
              System.out.println("我正在学习！使用我们的电脑，"+comp.brand);
          }
          SxtStu() {
          }
          public static void main(String[] args) {
              SxtStu stu1 = new SxtStu();
              stu1.sname = "张三";
              Computer comp1 = new Computer();
               comp1.brand = "联想";
              stu1.comp = comp1;
              stu1.study();
          }
      }
      ```
   
4. 面向对象的内存分析

      Java虚拟机的内存可以分为三个区域：**栈stack、堆heap、方法区method area**

      ------

      - 栈的特点如下：
        1. 栈描述的是**方法执行的内存模型**，每个方法被调用都会创建一个栈帧（存储局部变量、操作数、方法出口等）
        2. JVM为每个线程创建一个栈，用于存放该线程执行方法的信息（实际参数、局部变量）
        3. 栈属于**线程私有**，不能实现线程间的共享！
        4. 栈的存储特性是**“先进后出，后进先出”**
        5. 栈是由系统自动分配，速度快！栈是一个连续的内存空间！
      - 堆的特点：
        1. 堆用于存储**创建好的对象和数组**（数组也是对象）
        2. JVM只有一个堆，被所有线程共享
        3. 堆是一个不连续的内存空间，分配灵活，速度慢！
      - 方法区（静态区）的特点：
        1. JVM只有一个方法区，被所有线程共享！
        2. 方法区实际也是堆，只是用于存储类、常量相关的信息！
        3. 用来存放程序中永远不变或唯一的内容（类信息【class对象】、静态变量、字符串常量）
      
      ![alt 内存图](https://www.sxt.cn/360shop/Public/admin/UEditor/20171026/1509008324820095.png)
      
5. 构造方法

      构造器也叫构造方法(constructor)，用于对象的初始化。构造器是一个创建对象时被自动调用的特殊方法，目的是对象的初始化。构造器的名称应与类的名称一致。Java通过new关键字来调用构造器，从而返回该类的实例，是一种特殊的方法。
   
   **声明格式**：
   
   ```java
   [修饰符] 类名(形参列表){
       //n条语句
   }
   ```
   
   **要点**：
   
   1. 通过new关键字调用！
   2. 构造器虽然有返回值，但是不能定义返回值类型（返回值的类型肯定是本类），不能再构造器里使用return返回某个值。
   3. 如果我们没有定义构造器，则编译器会自动定义一个无参的构造函数。如果已定义则编译器不会自动添加！
   4. 构造器的方法名必须和类名一致！
   
   ```java
   /**
    * 测试构造方法
    * @author zsk
    *
    */
   class Point{
   	double x,y;
   	
   	public Point(double _x, double _y) {
   		x = _x;
   		y = _y;
   	}
   	
   	public double getDistance(Point p) {
   		return Math.sqrt((x - p.x) * (x - p.x) + (y - p.y) * (y - p.y));
   	}
   }
   
   public class TestConstructor {
   	public static void main(String[] args) {
   		Point p = new Point(3.0, 4.0);
   		Point origin = new Point(0.0, 0.0);
   		System.out.println(p.getDistance(origin));
   	}
   }
   ```
   
6. 构造方法的重载

      构造方法也是方法，只不过有特殊的作用而已。与普通方法一样，构造方法也可以重载。

      ```java
      public class User{
          int id;
          String name;
          String pwd;
          public User(){
              
          }
          public User(int id, String name){
              super();
              this.id = id;
              this.name = name;
          }
          public User(int id, String name, String pwd){
              this.id = id;
              this.name = name;
              this.pwd = pwd;
          }
          public static void main(String[] args){
              User u1 = new User();
              User u2 = new User(101,"zsk");
              User u3 = new User(100,"zsk","123456");
          }
      }
      ```

      **雷区**：

      ​	如果方法构造中形参名与属性名相同时，需要使用this关键字区分属性与形参。如上例：

      ​	this.id 表示属性id； id表示形参id

7. 垃圾回收机制（Garbage Collection）

      * **垃圾回收原理和算法**

        * 内存管理

          Java的内存管理很大程度指的就是对象的管理，其中包括对象空间的分配和释放

          对象空间的分配：使用new关键字创建对象即可

          对象空间的释放：将对象赋值null即可。垃圾回收器负责回收所有“不可达”对象的内存空间。

        * 垃圾回收过程

          任何一种垃圾回收算法要做两件事：

          1. 发现无用的对象
          2. 回收无用对象占用的内存空间

          Java的垃圾回收器通过相关算法发现无用对象，并进行清除和整理。

        * 垃圾回收相关算法

          1. 引用计数法

             堆中每个对象都有一个引用计数。被引用一次，计数加1。被引用变量值为null，则计数减1，直到计数为0，则表示变成无用对象。优点是算法简单，缺点是“循环引用的无用对象”无法被识别。

          2. 引用可达发（根搜索算法）

             程序把所有的引用关系看作一张图，从一个节点GC ROOT开始，寻找对应的引用节点，找到这个节点以后，继续寻找这个节点的引用节点，当所有的引用节点寻找完毕之后，剩余的节点则被认为是没有被引用到的节点，即无用的节点。

      * **通用的分代垃圾回收机制**

        ​        分代垃圾回收机制，是基于：不同的对象的生命周期是不一样的。因此，不同生命周期的对象可以采用不同的回收算法，以提高回收效率。我们将对象分为三种状态：年轻代、年老代、持久代。JVM将堆内存划分为Eden、Survivor和Tenured/Old空间。

        1. 年轻代：所有新生成的对象首先都是放在Eden区。（Minor GC）
        2. 年老代：年轻代中经历了N(默认15)次垃圾回收后仍存活的对象，会被放到老年代中。（Major GC和Full GC）
        3. 持久代：用于存放静态文件，如java类、方法等。

        **垃圾回收过程**：

        1. 新创建的对象，绝大多数都会存储在Eden中
        2. 当Eden满了不能创建新对象，则触发垃圾回收（GC），将无用对象清理掉，然后剩余对象复制到某个Survivor中，如S1，同时清空Eden区
        3. 等Eden再次满了，会将S1中的不能清空的对象存到另一个Survivor中，如S2，同时将Eden中不能清空的对象复制到S1中，保证Eden和S1均被清空。
        4. 重复多次（默认15）Survivor中没有被清理的对象，会被复制到老年代Old区中
        5. 当Old区满了，则会出发一个一次完整的垃圾回收（Full GC：对系统性能会产生影响）

      * **JVM调优和Full GC**

        在JVM调优的过程中，很大一部分工作就是对Full GC的调节。

      * **开发中容易造成内存泄漏的场景**

        （后面补充）

8. this关键字

      * **对象创建的过程和this的本质**

        ​    构造方法是创建Java对象的重要途径，通过new关键字调用构造器时，构造器也确实返回该类的对象，但这个对象并不是完全由构造器负责创建。创建一个对象分为如下四步：

        1. 分配对象空间，并将对象成员变量初始化为0或空
        2. 执行属性值的显式初始化
        3. 执行构造方法
        4. 返回对象的地址给相关的变量

        ​    this的本质就是“创建好的对象的地址”！**由于在构造方法调用前，对象已经创建。因此，在构造方法中也可以使用this代表“当前对象”。**

        ​	this最常的用法：

        1. 在程序中产生二义性之处，应使用this来指明当前对象;普通方法中，this总是指向调用该方法的对象。构造方法中，this总是指向正要初始化的对象。
        2. 使用this关键字调用重载的构造方法，避免相同的初始化代码。但只能在构造方法中用，并且必须位于构造方法的第一句。
        3. this不能用于static方法中。

      * **程序示例**

        ```java
        /**
         * 测试this关键字
         * @author zsk
         *
         */
        public class User {
        
        	int id;
        	String name;
        	String pwd;
        	
        	public User() {     //构造方法
        		
        	}
        	
        	public User(int id, String name) {    //重载构造方法
        		System.out.println("正在初始化已经创建好的对象：" + this);
        		this.id = id;
        		this.name = name;
        	}
        	
        	public void login() {
        		System.out.println(this.name + "登录！");   // 不写this效果一样
        	}
        	
        	public static void main(String[] args) {
        		User u3 = new User(101,"zsk");
        		System.out.println("打印对象：" + u3);
        		u3.login();
        	}
        }
        ```

9. static关键字

   ​		在类中，用static声明的成员变量为静态成员变量，也成为类变量。生命周期和类相同，整个应用程序执行期间都有效。
   
   ​		核心要点：1. static修饰的成员变量和方法，从属于类
   
   ​							2. 普通变量和方法从属于对象
   
       /**
       *测试static关键字的用法
       * @author zsk
       *
       */
         public class User2 {
         int id; // id
         String name; // 账户名
         String pwd; // 密码
       
         static String company = "北京腾讯"; // 公司名称
       
         public User2(int id, String name) {
             this.id = id;
             this.name = name;
         }
       
         public void login() {
             printCompany();  //普通方法可以调用静态成员
             System.out.println(company); 
             System.out.println("登录：" + name);
         }
       
         public static void printCompany() {
         //         login();//调用非静态成员，编译就会报错
             System.out.println(company);
         }
       
         public static void main(String[] args) {
             User2 u = new User2(101, "高小七");
             User2.printCompany();
             User2.company = "北京阿里";
             User2.printCompany();
         }
         }
   
10. 静态初始化块

        ​		构造方法用于对象的初始化！静态初始化块，用于类的初始化操作！在静态初始化块中不能直接访问非static成员。

        **注意事项**：

        ​		静态初始化块执行顺序（学完继承再看）：

      			1. 上溯到Object类，先执行Object的静态初始化块，再向下执行子类的静态初始化块，直到我们的静态初始化块为止
         			2. 构造方法执行顺序和上面顺序一样。

        ```java
            static {
                System.out.println("执行类的初始化工作");
                company = "北京阿里";
                printCompany();
            } 
        ```

11. 参数传值机制

       ​	Java中，方法中所有参数都是“值传递”，也就是“传递的是值的副本”。复印件改变不会影响原件。

       * **基本数据类型参数的传值**

         传递的是值的副本。副本改变不会影响原件。

       * **引用类型参数的传值**

         传递的是值的副本。但是引用类型指的是“对象的地址”。因此，副本和原参数都指向了同一个“地址”，改变“副本指向地址对象的值，也意味着原参数指向对象的值也发生了改变”。

       ```java
       /**
        * 测试参数传递
        * @author zsk	
        *
        */
       public class User4 {
       	int id;
       	String name;
       	String pwd;
       	
       	public User4(int id, String name) {
       		this.id = id;
       		this.name = name;
       	}
       	
       	public void testParameterTransfer01(User4 u) {
       		u.name = "zsk";
       	}
       	
       	public void testParameterTransfer02(User4 u) {
       		u = new User4(200,"zzz");
       	}
       	
       	public static void main(String[] args) {
       		User4 u1 = new User4(100, "sss");
       		
       		u1.testParameterTransfer01(u1);  //指向同一个对象，会发生改变
       		System.out.println(u1.name);
       		
       		u1.testParameterTransfer02(u1);  //u1地址穿给u，新对象地址赋给u，而u1不变
       		System.out.println(u1.name);
       	}
       }
       ```

12. 包

       ​	包机制是Java中**管理类**的重要手段。开发中，我们会遇到大量同名的类，通过包我们很容易对解决类重名的问题，也可以实现对类的有效管理。包对于类，相当于文件夹对于文件的作用。

13. package

       package的使用有**两个要点**：

       1. 通常是类的第一句非注释性语句。
       2. 包名：域名倒着写即可，再加上模块名，便于内部管理类。
    
    **注意事项**：
    
    1. 写程序时都要使用包。
    2. com.gao和com.gao.car，这两个包没有包含关系，是两个完全独立的包。
    
    ---
    
    * **JDK中的主要包**
    
      java.long	包含一些java语言的核心类，如String、Math，提供常用功能。可以不导入直接使用。
    
      java.awt	包含了构成抽象窗口工具集的多个类
    
      java.net	包含执行与网络相关的操作的类
    
      java.io	包含能提供多种输入/输出功能的类
    
      java.util	包含一些实用工具类。
    
    * **导入类import**
    
      **注意**：
    
         1. Java会默认导入java.long包下所有的类，因此这些类我们可以直接使用。
    
         2. 如果导入两个同名的类，只能用包名+类名来显式调用相关类。
    
            ```java
            import java.sql.Date;
            import java.util.*;//导入该包下所有的类。会降低编译速度，但不会降低运行速度。
             
            public class Test{
                public static void main(String[] args) {
                    //这里指的是java.sql.Date
                    Date now; 
                    //java.util.Date因为和java.sql.Date类同名，需要完整路径
                    java.util.Date  now2 = new java.util.Date();
                    System.out.println(now2);      
                    //java.util包的非同名类不需要完整路径
                    Scanner input = new Scanner(System.in);    
                }
            }
            ```
    
    * 静态导入
    
      ​	作用是用于导入指定类的静态属性，则可以直接使用静态属性。
    
      ```java
      //以下两种静态导入的方式二选一即可
      import static java.lang.Math.*;//导入Math类的所有静态属性
      import static java.lang.Math.PI;//导入Math类的PI属性
      ```
    
      