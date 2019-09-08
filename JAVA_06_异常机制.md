# 六、异常机制

1. **导引问题**

   ​		异常：Exception

   Java异常机制

   ```java
   try{
       copyFile("d:/a.txt","e:/a.txt");
   }catch(Exception e){
       e.printStackTrace();
   }
   ```

   异常机制的本质：当程序出现错误，程序安全退出的机制。

2. **异常的概念**

   ​		异常指程序运行过程中出现的非正常现象。在Java的异常处理机制中，引进了很多用来描述和处理异常的类，成为异常类。其中包含了该类异常的信息和对异常进行处理的方法。

   ​		所谓异常处理，就是指程序在出现问题时依然可以正确的执行完。

   **Java采用面向对象的方式来处理异常** 

   		* **抛出异常**：在执行一个方法时，如果发生异常，则这个方法生成代表该异常的一个对象，停止当前执行路径，并把异常对象提交给JRE。
   		* **捕获异常**：JRE得到该异常后，寻找相应的代码来处理该异常。JRE在方法的调用栈中查找，从生成异常的方法开始回溯，直到找到相应的异常处理代码为止。

3. **异常分类**

   ​		JDK定义了很多异常类，所有异常对象都是派生于Throwable类的一个实例。

   ​		Java对异常进行了分类，不同类型的异常分别用不同的Java类表示，所有异常的根类为java.long.Throwable，Throwable下面又派生了两个子类：Error和Exception。

   ![Java异常类层次结构](https://www.sxt.cn/360shop/Public/admin/UEditor/20170520/1495272017528669.png)

   * **Error**

     ​		Error是程序无法处理的错误，表示运行应用程序中较严重问题。大多是错误与代码编写者执行的操作无关，而表示代码运行时JVM出现的问题。Error表示系统JVM已经处于不可恢复的崩溃状态中。

   * **Exception**

     ​		Exception是程序本身能够处理的异常，如：空指针异常（NullPointerException)、数组下标越界异常（ArrayIndexOutOfBoundsException）等。

     ​		Exception类是所有异常类的父类，其子类对应了各种各样可能出现的异常事件。通常Java的异常可分为：1.RuntimeException  运行时异常；2.CheckedException  已检查异常。

   * **RuntimeException运行时异常**

     ​		派生于RuntimeException的异常，如被0除、数组下标越界、空指针等，其产生比较频繁，因此系统个自动检测并将它们交给缺省的异常处理程序。

     ​		这类异常通常是由编程错误导致的，所以在编写程序时，并不要求必须使用异常处理机制来处理这类异常，经常需要通过增加“逻辑处理来避免这些异常“。

     **ArithmeticException异常：视图除以0**

     ```java
     public class TestException{
         public static void main(String[] args){
             int b = 0;
             if(b != 0){
                 System.out.println(1/b);
             }
         }
     }
     ```

     **NullPointException异常**

     ```java
     public class TestNullPoint{
         public static void main(String[] args){
             String str = null;
             System.out.println(str.charAt(0));  //调用空对象的属性或方法会出现空指针异常
         }
     }
     ```

     **注意事项**

     		1. 在方法抛出异常之后，运行时系统将转为寻找合适的异常处理器。潜在的异常处理器是异常发生时依次残留在调用栈中的方法的集合。当异常处理器所能处理的异常类型与方法抛出的异常类型相符时，即为合适的异常处理器。
       		2. 运行时系统从发生异常的方法开始，依次回查调用栈中的方法，直至找到含有合适异常处理器的方法并执行。当运行时系统遍历调用栈而未找到合适的异常处理器，则运行时系统终止。同时，意味着Java程序的终止。

   * **CheckedException已检查异常**

     ​		所有不是RuntimeException的异常，统称为CheckedException，又被称为“已检查异常”，如IOException、SQLException等。这类异常在编译时就必须做出处理，否则无法通过编译。

     ​		异常的处理方式有两种：使用“try/catch"捕获异常、使用”throws“声明异常。

4. **异常处理方式之一：捕获异常**

   ​		捕获异常是通过3个关键词来实现的：try-catch-finally。用try来执行一段程序，如果出现异常，系统抛出一个异常，可以通过它的类型来捕捉（catch）并处理它，最后一步是通过finally语句为异常处理提供一个统一的出口，finally指定的代码都要被执行(**catch语句可有多条；finally语句最多只能有一条**，根据自己的需要可有可无)。

   * **try**

     ​		try语句指定一段代码，该段代码就是异常捕获并处理的范围。在执行过程中，当任意一条语句产生异常时，就会跳过该条语句中后面的代码。代码中可能会产生并抛出一种或几种类型的异常对象，后面的catch语句要分别对这些异常做处理。

   * **catch**

     ​		常用方法，这些方法均继承自Throwable类。

     ​			-toString()  显示异常的类名和产生异常的原因

     ​			-getMessage()  只显示产生异常的原因

     ​			-printStackTrace()  用来跟踪异常事件发生时堆栈的内容。

     ​		捕获异常时的顺序：

     ​			如果异常类之间有继承关系，在顺序上要注意：越是顶层的类，越放在下面，再不然就把多余的catch省掉。

   * **finally**

     ​		通常在finally中关闭程序块已打开的资源，比如：关闭文件流、释放数据库连接等。

   **注意事项**

     		1. 即使try和catch语句中存在return语句，finally语句也会执行。是在执行完finally语句后再通过return退出。
     		2. finally语句块只有一种情况是不会执行，就是在执行finally之前遇到了System.exit(0)结束。

   ```java
   public class TestException{
       public static void main(String[] args){
           FileReader reader = null;
           try{
               reader = new FileReader("d:/a.txt");
               char c = (char)reader.read();
               char c2 = (char)reader.read();
               System.out.println("" + c + c2);
           }catch(FileNotFoundException e){
               e.printStackTrace();
           }catch(IOException e){
               e.printStackTrace();
           }finally{
               try{
                   if(reader != null){
                       reader.close();
                   }
               }catch(Exception e){
                   e.printStackTrace();
               }
           }
       }
   }
   ```

5. **异常处理方式之二：声明异常**

   ​		当CheckedException产生时，不一定立刻处理它，可以再把异常throws出去。

   ​		如果一个方法中可能产生某种异常，但是并不能确定如何处理这种异常，则应根据异常规范在方法的首部声明该方法可能抛出的异常。

   **注意事项**

   ​		方法重写中声明异常原则：子类重写父类方法时，如果父类方法有声明异常，那么子类声明的异常范围不能超过父类声明的范围。

6. **使用异常机制的建议**

   		* 要避免使用异常处理代替错误处理，这样会降低程序的清晰性，并且效率低下。
   		* 处理异常不可以代替简单测试---只在异常情况下使用异常机制。
   		* 不要进行小粒度的异常处理---应该将整个任务包装在一个try语句块中。
   		* 异常往往在高层处理。