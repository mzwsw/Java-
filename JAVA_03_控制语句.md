# 控制语句

1. 介绍

   流程控制语句是用来控制程序中各语句执行顺序的语句

   * 顺序结构

   * 选择结构

     代表“如果……，则……”的逻辑

   * 循环结构

     代表“如果……，则再继续……”的逻辑

2. 选择结构

   * **if单选择结构**

     if语句对布尔表达式进行一次判定，若判定为真，则执行{}中的语句块，否则跳过该语句块。

     ```java
     /**
      * 测试if语句
      * @author zsk 
      *  *
      */
     public class TestIf {
     	public static void main(String[] args) {
     		//通过掷三个骰子
     		int i = (int)(6 * Math.random()) + 1;  //通过Math.random()产生[0,1)之间的随机数
     		int j = (int)(6 * Math.random()) + 1;
     		int k = (int)(6 * Math.random()) + 1;
     		int count = i + j + k;
     		//如果三个骰子之和大于15，则手气不错
     		if(count > 15) {
     			System.out.println("今天手气不错");
     		}
     		if(count >= 10 && count <=15) {
     			System.out.println("今天手气一般");
     		}
     		if(count < 10) {
     			System.out.println("今天手气不怎么样");
     		}
     		System.out.println("得了" + count + "分");
     	}
     }
     ```

     * Math类的使用

       1. java.lang包中的Math类提供了一些用于数学计算的方法。

       2. Math.random()方法用于产生一个[0,1)之间的double类型的随机数。

          int i = (int)(6 * Math.random()); //产生[0,5]之间的随机整数

     * **注意**

       1. 如果if语句不写{}，则只能作用于后面的第一条语句。
       2. 强烈建议，任何时候都写上{}，即使里面只有一句话！

   * **if-else双选择结构**

     ```
     if(布尔表达式){
     	语句块1
     }else{
     		语句块2
     }
     ```

      当布尔表达式为真时，执行语句块1，否则，执行语句块2。也就是else部分

     ```java
     public class TestIfElse {
         public static void main(String[] args) {
             //随机产生一个[0.0, 4.0)区间的半径，并根据半径求圆的面积和周长
             double r = 4 * Math.random();
            //Math.pow(r, 2)求半径r的平方
             double area = Math.PI * Math.pow(r, 2);
             double circle = 2 * Math.PI * r;
             System.out.println("半径为： " + r);
             System.out.println("面积为： " + area);
             System.out.println("周长为： " + circle);
             //如果面积>=周长，则输出"面积大于等于周长"，否则，输出周长大于面积
             if(area >= circle) {
                 System.out.println("面积大于等于周长");
             } else {
                 System.out.println("周长大于面积");
             }
         }
     }
     ```

     **注**：条件运算符有时候可用于代替if-else，即x ? y : z

   * **if-else if-else多选择结构**

     ```
     if(布尔表达式1){
     	语句块1；
     }else if(布尔表达式2){
     		语句块2；
     }……
     else{
     	语句块3；
     }
     ```

     ```java
     /**
      * 测试ifelseifelse多选择
      * @author zsk
      *
      */
     public class TestIfElseifElse {
     	public static void main(String[] args) {
     		int age = (int)(100 * Math.random());
     		System.out.print("年龄是" + age + "，属于");
     		if(age < 15) {
     			System.out.println("儿童，喜欢玩！");
     		}else if (age < 25) {
     			System.out.println("青年，要学习！");
     		}else if (age <45) {
     			System.out.println("中年，要工作！");
     		}else if (age < 65) {
     			System.out.println("中老年，要补钙！");
     		}else if (age < 85) {
     			System.out.println("老年，多运动！");
     		}else {
     			System.out.println("老寿星，古来稀！");
     		}
     	}
     }
     ```

   * **switch多选择结构**

     ```
     switch(表达式){
     case 值1：
     	语句1；
     	[break];  //可选，一般都会写
     case 值2：
     	语句2；
     	[break]；
     	…… …… ……
     [default:
     默认语句]；
     }
     ```

     ​    switch语句会根据表达式的值从相匹配的case标签处开始执行，**一直执行到break语句处或者是switch语句的末尾**。如果表达式的值与任一case值不匹配，则进入default语句(如果存在default语句的情况)。

     ​    **注意**，当布尔表达式是**等值判断**的情况，可以使用if-else if-else多选择结构或者switch结构，如果布尔表达式**区间判断**的情况，则只能使用if-else if-else多选择结构。

     ```java
     /**
      * 测试switch语句
      * @author zsk
      *
      */
     public class TestSwitch {
     	public static void main(String[] args) {
     		char c = 'a';
     		int rand = (int)(26 * Math.random());
     		char c2 = (char)(c + rand);
     		System.out.println(c2 + ":");
     		switch(c2) {
     		case 'a':
     		case 'e':
     		case 'i':
     		case 'o':
     		case 'u':
     			System.out.println("元音");
     			break;
     		case 'y':
     		case 'w':
     			System.out.println("半元音");
     		default:
     			System.out.println("辅音");
     		}
     	}
     }
     ```

3. 循环结构

   * 循环结构分为两大类：一类是当型；一类是直到型。

   1. **while循环**

      ​	在循环开始时，会计算一次“布尔表达式”的值，若条件为真，执行循环体。对于后来每一次的循环，都会重新计算一次。

      ```
      while(布尔表达式){
      	循环体
      }
      ```

      ```java
      /**
       * 测试while循环
       * @author zsk
       *
       */
      public class TestWhile {
      	public static void main(String[] args) {
      		//计算1+2+3+4...+100累加的和
      		int i = 1;
      		int sum = 0;
      		while(i <= 100) {
      			sum = sum + i;
      			i++;
      		}
      		
      		System.out.println(sum);
      	}
      }
      ```

   2. **for循环**

      ​	for循环语句是支持迭代的一种通用结构，是最有效、最灵活的循环结构。

          ```java
      for (初始表达式; 布尔表达式; 迭代因子) {
            循环体;
      }

          ```
          
          ```

      ​	Java里能用到逗号运算符的地方屈指可数，其中一处就是for循环的控制表达式。在控制表达式的初始化和步进控制部分，我们可以使用一系列由逗号分隔的表达式，而且那些表达式均会独立执行。

      ```java
      public class Test11 {
          public static void main(String[] args) { 
              for(int i = 1, j = i + 10; i < 5; i++, j = i * 2) {
                  System.out.println("i= " + i + " j= " + j); 
              } 
          }
      }
      ```

      ```java
      /**
       * 测试for循环
       * @author zsk
       *
       */
      public class TestFor{
          public static void main(String args[]) {
              int sum = 0;
              //1.求1-100之间的累加和
              for (int i = 0; i <= 100; i++) {
                  sum += i;
              }
              System.out.println("Sum= " + sum);
              //2.循环输出9-1之间的数
              for(int i=9;i>0;i--){
                  System.out.print(i+"、");
              }
              System.out.println();
              //3.输出90-1之间能被3整除的数
              for(int i=90;i>0;i-=3){
                  System.out.print(i+"、");
              }
              System.out.println();
          }
      }
      ```

      * 初始化部分可设置任意数量的定义，但都属于同一类型。
      * **约定**：只在for语句的控制表达式中写入与循环变量初始化，条件判断和迭代因子相关的表达式
      * for语句的初始化部分声明的变量，其作用域为整个for循环体，不能在循环外部使用该变量。

   3. **嵌套循环**

      在一个循环语句内部再嵌套一个或多个循环，称为嵌套循环

      ```java
      /**
       * 测试嵌套循环
       * @author zsk
       *
       */
      public class Testcycle {
      	public static void main(String[] args) {
      		for(int i = 1; i <= 5; i++) {
      			for(int j = 1; j <=5; j++) {
      				System.out.print(i + "\t");
      			}
      			System.out.println();
      		}
      	}
      }
      ```

      ```java
      /**
       * 测试乘法口诀表
       * @author zsk
       *
       */
      public class TestMultiply {
      	public static void main(String[] args) {
      		
      		for(int i = 1; i < 10; i++) {
      			for(int j = 1; j <= i; j++) {
      				System.out.print(j + "*" + i + "=" + (j*i) + "\t");
      			}
      			System.out.println();
      		}
      	}
      }
      ```

   4. **break语句和continue语句**

      * 在任何循环语句的主体部分，均可使用break控制循环的流程。break用于**强行退出循环**，不执行循环中的剩余语句。

        ```java
        /**
         * 测试break语句
         * @author zsk
         *
         */
        public class TestBreak {
        	public static void main(String[] args) {
        		int total = 0;//定义计数器
                System.out.println("Begin");
                while (true) {
                    total++;//每循环一次计数器加1
                    int i = (int) Math.round(100 * Math.random());
                    //当i等于88时，退出循环
                    if (i == 88) {
                        break;
                    }
                }
                //输出循环的次数
                System.out.println("Game over， used " + total + " times.");
        	}
        }
        ```

      *  continue 语句用在循环语句体中，用于终止某次循环过程，即**跳过循环体中尚未执行的语句，接着进行下一次是否执行循环的判定**。

        ```java
        /**
         * 测试continue语句
         * @author zsk
         *
         */
        public class TestContinue {
        	public static void main(String[] args) {
        		int count = 0;//定义计数器
        		for(int i = 100; i <= 150; i++){
                    //如果是3的倍数，跳过本次循环，继续进行下一次
        			if(i % 3 == 0){	
        				continue;
        			}
        			System.out.print(i + ",");
        			count++;
        			if(count % 5 == 0){
        				System.out.println();
        			}
        		}
        	}
        }
        ```

   5. **带标签的break和continue语句**

      “标签”是指后面跟一个冒号的标识符，例如：“label:”。对Java来说唯一用到标签的地方是在循环语句之前。而在循环之前设置标签的唯一理由是：我们希望在其中嵌套另一个循环，由于break和continue关键字通常只中断当前循环，但若随同标签使用，它们就会中断到存在标签的地方。

      ```java
      /**
       * 测试带标签的continue
       * @author zsk
       *
       */
      public class TestLabelContinue {
      	public static void main(String[] args) {
      		outer: for (int i = 101; i < 150; i++) {
                  for (int j = 2; j < i / 2; j++) {
                      if (i % j == 0){
                          continue outer;
                      }
                  }
                  System.out.print(i + "  ");
              }
      	}
      }
      ```

4. 语句块

   ​    语句块(有时叫做复合语句)，是用花括号扩起的任意数量的简单Java语句。块确定了局部变量的作用域。块中的程序代码，作为一个整体，是要被一起执行的。

5. 方法

   ​    方法就是一段用来完成特定功能的代码片段，类似于其它语言的函数。

   ​    方法用于定义该类或该类的实例的行为特征和功能实现。 方法是类和对象行为特征的抽象。方法很类似于面向过程中的函数。面向过程中，函数是最基本单位，整个程序由一个个函数调用组成。面向对象中，整个程序的基本单位是类，方法是从属于类和对象的。

   **方法声明格式**

   > [修饰符1 修饰符2 ……]	返回值类型	方法名（形参列表）{
   >
   > ​	Java语句；
   >
   > }

   **方法的调用方式**

   ​	对象名.方法名（实参列表）

   ​	**注**：如无返回值，必须指定为void

   ```java
   public class Test20 {
       /** main方法：程序的入口 */
       public static void main(String[] args) {
           int num1 = 10;
           int num2 = 20;
           //调用求和的方法：将num1与num2的值传给add方法中的n1与n2
           // 求完和后将结果返回，用sum接收结果
           int sum = add(num1, num2);
           System.out.println("sum = " + sum);//输出：sum = 30
           //调用打印的方法：该方法没有返回值
           print();
       }
       /** 求和的方法 */
       public static int add(int n1, int n2) {
           int sum = n1 + n2;
           return sum;//使用return返回计算的结果
       }
       /** 打印的方法 */
       public static void print() {
           System.out.println("北京尚学堂...");
       }
   }
   ```

   **注意事项**

    	1. 实参的数目、数据类型和次序必须和所调用的方法声明的形式参数列表匹配。
    	2. return 语句终止方法的运行并指定要返回的数据。
    	3. Java中进行方法调用中传递参数时，遵循值传递的原则(传递的都是数据的副本)。
    	4. 基本类型传递的是该数据值的copy值。
    	5. 引用类型传递的是该对象引用的copy值，但指向的是同一个对象。

6. 方法的重载(overload)

   ​       方法的重载是指一个类中可以定义多个方法名相同，但参数不同的方法。 调用时，会根据不同的参数自动匹配对应的方法。

   * **注意**：重载的方法，实际是完全不同的方法，只是名称相同而已!

     ​      构成方法重载的条件：

     1. 不同的含义：形参类型、形参个数、形参顺序不同
2. 只有返回值不同不构成方法的重载
     3. 只有形参的名称不同，不构成方法的重载
   
```java
   /**
    * 测试重载
    * @author zsk
    *
    */
   public class TestOverload {
   	public static void main(String[] args) {
   		 System.out.println(add(3, 5));// 8
   	        System.out.println(add(3, 5, 10));// 18
   	        System.out.println(add(3.0, 5));// 8.0
   	        System.out.println(add(3, 5.0));// 8.0
   	        // 我们已经见过的方法的重载
   	        System.out.println();// 0个参数
   	        System.out.println(1);// 参数是1个int
   	        System.out.println(3.0);// 参数是1个double
   	}
   	
   	/** 求和的方法 */
       public static int add(int n1, int n2) {
           int sum = n1 + n2;
           return sum;
       }
       // 方法名相同，参数个数不同，构成重载
       public static int add(int n1, int n2, int n3) {
           int sum = n1 + n2 + n3;
           return sum;
       }
       // 方法名相同，参数类型不同，构成重载
       public static double add(double n1, int n2) {
           double sum = n1 + n2;
           return sum;
       }
       // 方法名相同，参数顺序不同，构成重载
       public static double add(int n1, double n2) {
           double sum = n1 + n2;
           return sum;
       }
       //编译错误：只有返回值不同，不构成方法的重载
       public static double add(int n1, int n2) {
           double sum = n1 + n2;
           return sum;
       }
       //编译错误：只有参数名称不同，不构成方法的重载
       public static int add(int n2, int n1) {
           double sum = n1 + n2;         
           return sum;
       }  
   }
   ```
   
7. 递归结构

   ​        递归是一种常见的解决问题的方法，即把问题逐渐简单化。递归的基本思想就是“自己调用自己”，一个使用递归技术的方法将会直接或者间接的调用自己。

   ​       递归结构包括两个部分：

   ​      1. 定义递归头。解答：什么时候不调用自身方法。如果没有头，将陷入死循环，也就是递归的结束条件。

   ​      2. 递归体。解答：什么时候需要调用自身方法。

   **递归的缺陷**：

   ​      简单的程序是递归的优点之一。但是递归调用会占用大量的系统堆栈，内存耗用多，在递归调用层次多时速度要比循环慢的多，所以在使用递归时要慎重。

   **注意事项**

   ​	任何能用递归解决的问题也能使用迭代解决。当递归方法可以更加自然地反映问题，并且易于理解和调试，并且不强调效率问题时，可以采用递归;

   ​      在要求高性能的情况下尽量避免使用递归，递归调用既花时间又耗内存。

   ```java
   public class TestRecu {
       public static void main(String[] args) {
           long d1 = System.currentTimeMillis();  
           System.out.printf("%d阶乘的结果:%s%n", 10, factorial(10));
           long d2 = System.currentTimeMillis();
           System.out.printf("递归费时：%s%n", d2-d1);  //耗时：32ms
       }
       /** 求阶乘的方法*/
       static long  factorial(int n){
           if(n==1){//递归头
               return 1;
           }else{//递归体
               return n*factorial(n-1);//n! = n * (n-1)!
           }
       }
   }
   ```
