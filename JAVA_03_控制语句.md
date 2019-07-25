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
   
   3. 嵌套循环
   
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
   
      