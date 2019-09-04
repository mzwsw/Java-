# 八、常用类

1. **包装类**

    * 包装类基本知识

      ​		在实际应用中经常需要将基本数据类型转化为对象，以便于操作。为了解决这个问题，Java在设计类时，为每个基本数据类型设计了一个对应的类进行代表，这样八个和基本数据类型对应的类成为包装类。

      ​		这八个类名中，除了Integer和Character类以外，其他六个类的类名和基本数据类型一致，只是首字母大写。

   * 包装类的用途

     ​		主要用途有两种：

     			1. 作为和基本数据类型对应的类型存在，方便涉及到对象的操作，如Object[]、集合等。
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

   