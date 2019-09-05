# 八、常用类

1. **包装类**

    * 包装类基本知识

      ​		在实际应用中经常需要将基本数据类型转化为对象，以便于操作。为了解决这个问题，Java在设计类时，为每个基本数据类型设计了一个对应的类进行代表，这样八个和基本数据类型对应的类成为包装类。

      ​		这八个类名中，除了Integer和Character类以外，其他六个类的类名和基本数据类型一致，只是首字母大写。

   * 包装类的用途

     ​		主要用途有两种：

     				1. 作为和基本数据类型对应的类型存在，方便涉及到对象的操作。
        				2. 包含每种基本数据类型的相关属性如最大值、最小值等，以及相关的操作方法。

     ```java
     /**
      * 测试包装类
      * integer用法，其他类似
      * @author zsk
      *
      */
     public class TestWrapper {
     	public static void main(String[] args) {
     		//基本数据类型转化成Integer对象
     		Integer int1 = new Integer(10);
     		Integer int2 = Integer.valueOf(20);   //官方推荐这种写法
     		//Integer对象转化成int
     		int a  = int1.intValue();
     		//字符串转化成Integer对象
     		Integer int3 = Integer.parseInt("334");
     		Integer int4 = new Integer("999");
     		//Integer对象转化成字符串
     		String str1 = int3.toString();
     		//一些常见int类型相关的常量
     		System.out.println("int能表示的最大整数:"+Integer.MAX_VALUE);
     	}
     }
     ```

   * 自动装箱和拆箱

     ​		自动装箱和拆箱就是将基本数据类型和包装类之间进行自动的互相转换。

     **自动装箱**

     ​		基本类型的数据处于需要对象的环境中时，会自动转为“对象”。

     **自动拆箱**

     ​		每当需要一个值时，对象会自动转成基本数据类型，没必要显式调用intValue()等

     ```java
     /**
      * 测试自动装箱和拆箱
      * @author zsk
      *
      */
     public class TestAutoBox {
     	public static void main(String[] args) {
     		Integer a = 234;  //实际是Integer a = Integer.valueOf(234);  自动装箱
     		int b = a;  //实际是int b = a.intValue();    自动拆箱
     		
     		Integer c = null;
     		int d = c;  //此处其实是：c.intValue(),因此抛空指针异常。
     		
     		
     	}
     }
     ```

   * 包装类的缓存问题

     ​		缓存处理的原理为：如果数据在-128~127这个区间，那么在类加载时就已经为该区间的每个数值创建了对象，并将这256个对象存放到一个名为cache的数组中。每当自动装箱过程发生时(或者手动调用valueOf()时)，就会先判断数据是否在该区间，如果在则直接获取数组中对应的包装类对象的引用，如果不在该区间，则会通过new调用包装类的构造方法来创建对象。

     ```java
     		Integer in1 = -128;
     	    Integer in2 = -128;
     	    System.out.println(in1 == in2);//true 因为-128在缓存范围内
     	    System.out.println(in1.equals(in2));//true
     	    Integer in3 = 1234;
     	    Integer in4 = 1234;
     	    System.out.println(in3 == in4);//false 因为1234不在缓存范围内
     	    System.out.println(in3.equals(in4));//true
     ```

2. **String类**

   ​		String类对象代表不可变的Unicode字符序列，因此我们可以将String对象成为“不可变对象”。
   
   ​		在遇到字符串常量之间的拼接时，编译器会作出优化，即在编译期间就会完成字符串的拼接。
   
   **常用方法**：
   
   		* String类的下述方法能创建并返回一个新的String对象：concat()、replace()、substring()、toLowerCase()、toUpperCase()、trim()。
   		* 提供查找功能的方法：endsWiths()、startsWith()、indexOf()、lastIndexOf()。
   		* 提供比较功能的方法：equals()、equalsIgnoreCase()、compareTo()。
   		* 其他方法：charAT()、length()。
   
   **StringBuffer和StringBuilder**
   
   ​		两者非常类似，均代表可变的字符序列。这两个类都是抽象类AbstractStringBuilder的子类。
   
   ​		区别：StringBuffer线程安全，效率低。StringBuilder线程不安全，效率高（推荐采用此类）
   
   ```java
   public class TestStringBufferAndBuilder 1{
       public static void main(String[] args) {
           /**StringBuilder*/
           StringBuilder sb = new StringBuilder();
           for (int i = 0; i < 7; i++) {
               sb.append((char) ('a' + i));//追加单个字符
           }
           System.out.println(sb.toString());//转换成String输出
           sb.append(", I can sing my abc!");//追加字符串
           System.out.println(sb.toString());
           /**StringBuffer*/
           StringBuffer sb2 = new StringBuffer("中华人民共和国");
           sb2.insert(0, "爱").insert(0, "我");//插入字符串
           System.out.println(sb2);
           sb2.delete(0, 2);//删除子字符串
           System.out.println(sb2);
           sb2.deleteCharAt(0).deleteCharAt(0);//删除某个字符
           System.out.println(sb2.charAt(0));//获取某个字符
           System.out.println(sb2.reverse());//字符串逆序
       }
   }
   ```
   
   **使用陷阱**
   
   ​		String一经初始化后，就不会再改变其内容。对String字符串的操作实际上是对其副本的操作，原来的字符串一点没有改变。
   
   ```java
   /**
    * 测试频繁字符串修改时的效率
    * @author zsk
    *
    */
   public class TestStringBuffer {
   	public static void main(String[] args) {
           /**使用String进行字符串的拼接*/
           String str8 = "";
           //本质上使用StringBuilder拼接, 但是每次循环都会生成一个StringBuilder对象
           long num1 = Runtime.getRuntime().freeMemory();//获取系统剩余内存空间
           long time1 = System.currentTimeMillis();//获取系统的当前时间
           //字符串循环添加时，一定不能使用下面这种方法
           for (int i = 0; i < 5000; i++) {
               str8 = str8 + i;//相当于产生了10000个对象
           }
           long num2 = Runtime.getRuntime().freeMemory();
           long time2 = System.currentTimeMillis();
           System.out.println("String占用内存 : " + (num1 - num2));
           System.out.println("String占用时间 : " + (time2 - time1));
           /**使用StringBuilder进行字符串的拼接*/
           StringBuilder sb1 = new StringBuilder("");
           long num3 = Runtime.getRuntime().freeMemory();
           long time3 = System.currentTimeMillis();
           for (int i = 0; i < 5000; i++) {
               sb1.append(i);
           }
           long num4 = Runtime.getRuntime().freeMemory();
           long time4 = System.currentTimeMillis();
           System.out.println("StringBuilder占用内存 : " + (num3 - num4));
           System.out.println("StringBuilder占用时间 : " + (time4 - time3));
       }
   }
   ```
   
3. **时间处理相关类**

    ​		使用long类型的变量来表示时间。如果想获得现在时刻的“时刻数值”，可以使用

    		> long now = System.currentTimeMillis();

    

    