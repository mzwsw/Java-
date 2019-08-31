# 七、数组

1. **概述和特点**

   * 定义

     ​    	数组是相同类型数据的有序集合。数组描述的是相同类型的若干个数据，按照一定的先后次序排列组合而成。其中，每一个数据称为一个元素，每个元素可以通过一个索引（下标）来访问它们。数组的三个特点：
     
     		1. 长度是确定的。数组一旦被创建，它的大小就是不可以改变的。
       		2. 元素必须是相同类型，不允许出现混合类型。
       		3. 数组类型可以是任何数据类型，包括基本类型和引用类型。
     
   * 建议：
   
     ​		数组变量属引用类型，数组也是对象，数组中的每个元素相当于该对象的成员变量。Java中对象是在堆中的，因此数组无论保存原始类型还是其他对象类型，数组对象本身是在堆中保存的。
   
2. **声明和初始化**

   * 声明

     数组的声明有两种方式

     ```java
     type[] arr_name; //推荐使用这种
     type arr_name[];
     ```

     **注意事项**

     		1. 声明的时候并没有实例化任何对象，只有在实例化数组对象是，JVM才分配空间，这时才与长度有关。
       		2. 声明一个数组的时候并没有数组被真正创建。
       		3. 构造一个数组，必须制定长度。

     ```java
     public class TestArray{
         public static void main(String args[]){
             int[] s = null; //声明数组；
             s = new int[10];  //给数组分配空间
             for(int i = 0; i < 10; i++){
                 s[i] = 2*i+1; //给数组元素赋值
                 System.out.println(s[i]);
             }
         }
     }
     ```

     ![基本类型数组内存分配](https://www.sxt.cn/360shop/Public/admin/UEditor/20170522/1495418560857133.png)

     ```java
     class Man{
         private int age;
         private int id;
         public Man(int id,int age) {
             super();
             this.age = age;
             this.id = id;
         }
     }
     public class AppMain {
         public static void main(String[] args) {
             Man[] mans;  //声明引用类型数组； 
             mans = new Man[10];  //给引用类型数组分配空间；
              
             Man m1 = new Man(1,11);
             Man m2 = new Man(2,22);  
              
             mans[0]=m1;//给引用类型数组元素赋值；
             mans[1]=m2;//给引用类型数组元素赋值；
         }
     }
     ```

     ![引用类型数组内存分配图](https://www.sxt.cn/360shop/Public/admin/UEditor/20170522/1495418626975934.png)

   * 初始化

     ​		数组的初始化有三种：静态初始化、动态初始化、默认初始化。

     ```java
     /**
      * 测试数组的三种初始化方式
      * @author zsk
      *
      */
     
     public class Test02 {
     	public static void main(String[] args) {
     		int[] a = {2,4,5};   //静态初始化
     		
     		int[] b = new int[3];   //默认给数组的元素赋值。赋值规则和成员变量默认赋值规则一样
     		
     		int[] c = new int[2];
     		c[1] = 1;
     		c[2] = 2;    //动态初始化
     		
     	}
     }
     ```

3. **数组的遍历、循环、拷贝等**

   * 数组的遍历

     ​		数组元素下标的合法区间：[0,length-1]。可以通过下标来遍历数组中的元素。for循环

   * for-each循环（只读不能修改）

     ​		增强for循环for-each专门用于**读取数组或集合**中所有的元素，即对数组进行遍历。

     ```java
     public class Test {
         public static void main(String[] args) {
             String[] ss = { "aa", "bbb", "ccc", "ddd" }; //静态初始化
             for (String temp : ss) {   //foreach循环，只能读取，不能修改
                 System.out.println(temp);
             }
         }
     }
     ```

     