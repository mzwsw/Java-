# 数据类型和运算符

1. 注释：

   - 单行注释： 使用“//”开头，后面的单行内容均为注释
   - 多行注释：   以“/*”开头以“*/”结尾，在“/*”和“*/”之间的内容为注释，我们也可以使用多行注释作为行内注释。但是在使用时要注意，多行注释不能嵌套使用。
   - 文档注释：   以“/**”开头以“*/”结尾，注释中包含一些说明性的文字及一些JavaDoc标签(后期写项目时，可以生成项目的API)

2. 标识符：

   - 标识符必须以字母、下划线、美元符

   - Java标识符大小写敏感，长度无限制

   - 标识符不可以是Java的关键字

   - 使用规范：

     - 表明类名的标识符：每个单词的首字母大写，如Man，GoodMan

     - 方法名和变量的标识符：第一个单词小写，从第二个单词开始首字母大写“驼峰原则”，如eat(),eatFood()

       ```java
       public class TestIdentifer {
       	public static void main(String[] args) {
       		int a123 =1;
       		//int 123acbc = 1;  //数字不能开头
       		int $a = 1;
       		int _abc =1;
       		int 年龄 = 18;  //可以使用汉字，但是一般不建议
       		
       	}
       
       }
       ```

3. 关键字/保留字

   - 常用：class,static,public,new

4. 变量

   - 本质代表一个“可操作的存储空间“，空间位置确定，里面放置什么值不确定。通过变量名来访问”对应的存储空间“。

   - Java是一种强类型语言，每个变量必须声明其数据类型

     > double salary;
     >
     > long earthPopulation;
     >
     > int age;

   - 不同的数据类型的常量会在内存中分配不同的空间：

     > salary  8个字节大小
     >
     > earthPopulation  8个字节
     >
     > age  4个字节 （每个字节8bit，） 32位

   - **不提倡“一行声明多个变量”，逐一声明每一个变量提高程序的可读性。**

5. 变量分类

   - **局部变量(local  variable)**

     ​    方法或语句块内部定义的变量。生命周期是从声明位置开始到到方法或语句块执行完毕为止。局部变量在使用前必须先声明、初始化(赋初值)再使用。 

   - **成员变量（也叫实例变量  member variable）**

     ​    方法外部、类的内部定义的变量。**从属于对象**，生命周期伴随对象始终。如果不自行初始化，它会自动初始化成该类型的默认初始值

   - **静态变量（类变量 static variable）**

     ​     使用static定义。 **从属于类**，生命周期伴随类始终，从类加载到卸载。 (注：讲完内存分析后我们再深入！先放一放这个概念！)如果不自行初始化，与成员变量相同会自动初始化成该类型的默认初始值

6. 常量（Constant）

   常量通常指的是一个固定的值。

   在Java中，主要利用final关键字来定义一个常量。常量一旦被初始化后不能再更改其值。

   **命名规范**： 大写字母和下划线：MAX_VALUE

7. 基本数据类型

   数据类型分为两大类：基本数据类型和引用数据类型

   基本数据类型定义有3类8种

   ![alt 数据类型分类](https://www.sxt.cn/360shop/Public/admin/UEditor/20170607/1496834727293971.png "数据类型分类")

   1. 整型变量和整型常量

      整型用于表示没有小数部分的数值，它允许是负数。整型的范围与运行Java代码的机器无关。

      - **Java 语言整型常量的四种表示形式**

        - 十进制整数，如：99, -500, 0
        - 八进制整数，要求以 0 开头，如：015
        - 十六进制数，要求 0x 或 0X 开头，如：0x15
        - 二进制数，要求0b或0B开头，如：0b01110011

      - Java语言的整型常数默认为int型，声明long型常量可以后加‘ l ’或‘ L ’ 。

        ```java
        long a = 55555555;  //编译成功，在int表示的范围内(21亿内)。
        long b = 55555555555;//不加L编译错误，已经超过int表示的范围。
        long b = 55555555555L;
        ```

   2. 浮点型变量/常量

      * 带小数的数据在Java中称为浮点型。浮点型可分为float类型和double类型。

      * float类型又被称作单精度类型，尾数可以精确到7位有效数字，在很多情况下，float类型的精度很难满足需求。而double表示这种类型的数值精度约是float类型的两倍，又被称作双精度类型，绝大部分应用程序都采用double类型。浮点型常量默认类型也是double。

        ```java
        //使用科学计数法赋值
        double f = 314e2;  //314*10^2-->31400.0
        double f2 = 314e-2; //314*10^(-2)-->3.14
        ```

      * float类型的数值有一个后缀F或者f ，没有后缀F/f的浮点数值默认为double类型。也可以在浮点数值后添加后缀D或者d， 以明确其为double类型

        ```java
        float  f = 3.14F;
        double d1  = 3.14;
        double d2 = 3.14D;
        ```

      * **浮点类型float，double的数据不适合在不容许舍入误差的金融计算领域。如果需要进行不产生舍入误差的精确数字计算，需要使用BigDecimal类。**

        ```java
        /**
           * 测试浮点型
           * @author zsk
           *
           */
          public class TestPrimitiveDataType {
          	public static void main(String[] args) {
          		float a = 3.14f;
          		double b = 6.28;
          		double c = 6.28E-2;
          		
          		System.out.println(c);
          		
          		//浮点数是不精确的，不能用于比较！
          		float f = 0.1f;
          		double d = 1.0/10;
          		System.out.println(f==d);   //结果为false
          		
          		float d1 = 423432423f;
          		float d2 = d1+1;
          		if(d1==d2){
          		   System.out.println("d1==d2");//输出结果为d1==d2
          		}else{
          		    System.out.println("d1!=d2");
          		}
          	}
          }
        ```

   3. 字符型变量/常量

      * 字符型在内存中占2个字节，在Java中使用单引号来表示字符常量。例如’A’是一个字符，它与”A”是不同的，”A”表示含有一个字符的字符串。

      * Java 语言中还允许使用转义字符 ‘\’ 来将其后的字符转变为其它的含义。

        ```java
        /**
         * 测试字符类型
         * @author zsk
         *
         */
        public class TestPrimitiveDataType02 {
        	public static void main(String[] args) {
        		char a = 'T';
        		char b = '张';
        		
        		
        		//转义字符
        		System.out.println(""+'a' + 'b');
        		System.out.println(""+'a' + '\t'+'b');
        		System.out.println(""+'a' + '\''+'b');   //a'b
        		
        		//String就是字符序列
        		String d = "abc";
        	}
        }
        ```

   4. boolean类型变量/常量

      * boolean类型有两个常量值，true和false，在内存中占一位（不是一个字节），不可以使用 0 或非 0 的整数替代 true 和 false ，这点和C语言不同。
      * **Less is More！！请不要这样写：if ( flag == true )，只有新手才那么写。关键也很容易写错成if(flag=true)，这样就变成赋值flag 为true而不是判断！老鸟的写法是if ( flag )或者if ( !flag)**
   
8. 运算符（operator）

   分类

   * 算数运算符 + - * / %
   * 赋值运算符 =
   * 扩展运算符 += -=
   * 关系运算符 < > >= <= !=
   * 逻辑运算符 && || !
   * 位运算符 & | ^ ~
   * 条件运算符 ? 
   * 字符串连接符 +

   ***

   1. 算数运算符

      * **二元运算符的运算规则：**

      * 整数运算

        1. 如果两个操作数有一个为long，则结果也为long
        2. 如果没有long时，结果为int。即使操作数全为short，byte，结果也是int

      * 浮点运算

        1. 如果两个操作数有一个为double，则结果为double
        2. 只有两个操作数都为float，则结果才为float

        ```java
        /**
         * 测试操作符
         * @author zsk
         *
         */
        public class TestOperator01 {
        	public static void main(String[] args) {
        		byte a = 1;
        		int    b = 2;
        		long b2 = 3;
        		//byte    c = a + b;   //报错
        		//int c2 = b2 + b;    //报错
        		 
        		
        		float f1 = 3.14f;
        		float d = b + b2;    //自动转换
        		
        		//float d2 = f1 + 6.2;   //报错
        	}
        }
        ```

      * 取模运算

        1. 操作数可以为浮点数，一般使用整数，结果为“余数”。“余数”符号和**左边操作数**相同

      * **一元运算符**

      * 算术运算符中++，--属于一元运算符，该类运算符只需要一个操作数。

      ```java
      int a = 3;
      int b = a++;   //执行完后,b=3。先给b赋值，再自增。a=4
      System.out.println("a="+a+"\nb="+b);
      a = 3;
      b = ++a;   //执行完后,a=4,b=4。a先自增，再给b赋值
      System.out.println("a="+a+"\nb="+b);
      ```

      ***

   2. 赋值运算符

      * a  += b  等价   a = a + b  、 a *= b  等价  a = a * b

      * ```
        a *= b+3  等价   a = a*(b+3)
        ```

      ***

   3. 关系运算符

      * 关系运算符用来进行比较。**运算的结果是布尔值**
      * ==、!= 是所有（基本和引用）数据类型都可以使用
      * <、<=、>、>= 仅针对数值类型（byte/short/int/long,float/double.以及char）

      ***

   4. 逻辑运算符

      * 逻辑运算的操作数和运算结果都是boolean值

      * 逻辑与（&）、逻辑或（|）、逻辑非（!）、逻辑异或（^）【相同为false，不同为true】

      * 短路与（&&）  只要有一个false，则直接返回false

      * 短路非（||）   只要有一个true，则直接返回true

        ```java
        public class TestOperator02 {
        	public static void main(String[] args) {
        		
        		//1>2的结果为false，那么整个表达式的结果即为false，将不再计算2>(3/0)
        		boolean c = 1>2 && 2>(3/0);
        		System.out.println(c);
        		//1>2的结果为false，那么整个表达式的结果即为false，还要计算2>(3/0)，0不能做除数，//会输出异常信息
        		boolean d = 1>2 & 2>(3/0);
        		System.out.println(d);
        	}
        }
        ```

      ***

   5. 位运算符

      * ~取反、&按位与、|按位或、^按位异或、<<左移运算，左移一位相当于乘2、>>右移运算，右移一位相当于除2取商

        ```java
        int a = 3*2*2;
        int b = 3<<2; //相当于：3*2*2;
        int c = 12/2/2;
        int d = 12>>2; //相当于12/2/2;
        ```

      * &和|既是逻辑运算符，也是位运算符。如果两侧操作数都是boolean类型，就作为逻辑运算符。如果两侧的操作数是整数类型，就是位运算符。

   6. 字符串连接符
   
      * “+”运算符两侧的操作数中**只要有一个是字符串(String)类型**，系统会自动将另一个操作数转换为字符串然后再进行连接。
   
   7. 条件运算符
   
      * 语法格式：x ? y : z 如果x为true，则输出y，如果x为false，则输出z；
   
        ```java
        int score = 80; 
        int x = -100;
        String type =score<60?"不及格":"及格"; 
        int flag = x > 0 ? 1 : (x == 0 ? 0 : -1);
        System.out.println("type= " + type);
        System.out.println("flag= "+ flag);
        ```
   
   8. 运算符优先级
   
      * 不需要去刻意的记这些优先级，**表达式里面优先使用小括号来组织！！**
   
      * 逻辑与、逻辑或、逻辑非的优先级一定要熟悉！***（逻辑非>逻辑与>逻辑或）***。如：
   
        a||b&&c的运算结果是：a||(b&&c)，而不是(a||b)&&c 
   
9. 类型转换

   1. 自动类型转换

      * 指的是容量小的数据类型可以自动转换成容量大的数据类型（**容量大小是指表示范围**）

      * 实线表述无数据丢失的自动类型转换，虚线表示在转换时可能会有精度的损失。

        ![alt 图片](https://www.sxt.cn/360shop/Public/admin/UEditor/20170516/1494906265693111.png)

      * 整型常量可以直接赋值给byte、short、char等类型变量，而不需要进行强制类型转换，只要不超出其表述范围即可。

        ```java
        /**
         * 测试类型转换
         * @author zsk
         *
         */
        public class TestTypeConvert {
        	public static void main(String[] args) {
        		int a = 324;
        		long b = a;
        		double d = b;
        		//a = b;  //报错，long类型不能转换成int
        		//long e = 3.23F;  //报错，float不能转换成long
        		float f = 23432L;
        		
        		//特例
        		byte b2 = 123;  //123属于int类型，但是byte可以表述-127~127，所以可以转换
        	}
        }
        
        ```

   2. 强制类型转换

      * 又被称为造型，用于量式的转换一个数值的类型。

      * 运算符“()”中的type表示将值var想要转换成的目标数据类型

        ```java
        double x = 3.14;
        int nx = (int)x;  //值为3，直接舍掉小数部分
        char c = 'a';
        int d = c + 1;
        System.out.println((char)d);  //输出为b
        ```

   3. 基本类型转化时常见错误和问题

      * 操作比较大的数时，要留意是否溢出

      * 不要命名名字为l的变量，l容易和1混淆。long类型使用大写L不要用小写

        ```java
        int money = 1000000000; //10亿
        int years = 20;
        //返回的total是负数，超过了int的范围
        int total = money*years;
        System.out.println("total="+total);  //输出为负数
        //返回的total仍然是负数。默认是int，因此结果会转成int值，再转成long。但是已经发生了数据丢失
        long total1 = money*years; 
        System.out.println("total1="+total1);  //输出为负数
        //返回的total2正确:先将一个因子变成long，整个表达式发生提升。全部用long来计算。
        long total2 = money*((long)years); 
        System.out.println("total2="+total2);
        ```

10. 简单的键盘输入与输出

    * Scanner

      ```java
      import java.util.Scanner;
      
      /**
       * 测试Scanner
       * @author zsk
       *
       */
      public class TestScanner {
      	public static void main(String[] args) {
      		Scanner scanner = new Scanner(System.in);
      		System.out.println("请输入名字：");
      		String name = scanner.nextLine();
      		System.out.println("请输入你的爱好：");
      		String favor = scanner.nextLine();
      		System.out.println("请输入你的年龄");
      		int age = scanner.nextInt();
      		
      		System.out.println("##########");
      		System.out.println(name);
      		System.out.println(favor);
      		System.out.println(age);
      	}
      }
      ```

      
