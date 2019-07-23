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

     