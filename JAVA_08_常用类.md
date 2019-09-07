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

    		 long now = System.currentTimeMillis();

    1. Date时间类（java.util.Date)

       ​		标准Java类库中包含一个Date类。它的对象表示一个特定的瞬间，精确到毫秒。
    
         * Date()  分配一个Date对象，并初始化此对象为系统当前的日期和时间，可以精确到毫秒。
    
         * Date(long date)  分配Date对象并初始化此对象，以表示自从标准基准时间以来的指定毫秒数。
    
         * boolean after(Date when)  测试此日期是否在指定日期之后。
    
         * boolean before(Date when)  测试此日期是否在指定日期之前。
    
         * boolean equals(Object obj)  比较两个日期的相等性。
    
         * long getTime()  返回自基准时间以来Date对象表示的毫秒数。
    
         * String to String()  把此Date对象转换为以下形式的String：
    
           ​				dow mon dd hh:mm:ss zzz yyyy
    
       ```java
       import java.util.Date;
       public class TestDate {
           public static void main(String[] args) {
               Date date1 = new Date();
               System.out.println(date1.toString());
               long i = date1.getTime();
               Date date2 = new Date(i - 1000);
               Date date3 = new Date(i + 1000);
               System.out.println(date1.after(date2));
               System.out.println(date1.before(date2));
               System.out.println(date1.equals(date2));
               System.out.println(date1.after(date3));
               System.out.println(date1.before(date3));
               System.out.println(date1.equals(date3));
               System.out.println(new Date(1000L * 60 * 60 * 24 * 365 * 39L).toString());
           }
       }
       ```
    
    2. DateFormat类和SimpleDateFormat类
    
       作用：把时间对象转化成指定格式的字符串。反之，把指定格式的字符串转化成时间对象。
    
       DateFormat是一个抽象类，一般使用它的子类SimpleDateFormat类来实现。
    
       ```java
       import java.text.DateFormat;
       import java.text.ParseException;
       import java.text.SimpleDateFormat;
       import java.util.Date;
       
       /**
        * 测试时间对象和字符串之间的互相转换
        * DateFormat抽象类和SimpleDateFormat实现类的使用
        * @author zsk
        *
        */
       public class TestDateFormat {
       	public static void main(String[] args) throws ParseException {
       		//new出SimpleDateFormat对象
       		DateFormat s1= new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
       		DateFormat s2 = new SimpleDateFormat("yyyy-MM-dd");
       		//将时间对象转换成字符串
       		String daytime = s1.format(new Date());
       		System.out.println(daytime);
       		System.out.println(s2.format(new Date()));
       		System.out.println(new SimpleDateFormat("hh:mm:ss").format(new Date()));
       		//将符合指定格式的字符串转化成时间对象。字符串格式需要和指定格式一致。
       		String time = "2007-10-7";
       		Date date = s2.parse(time);
       		System.out.println("date1:" + date);
       		time = "2007-10-7 20:15:30";
       		date = s1.parse(time);
       		System.out.println("date2:" + date);
       	}
       }
       ```
    
       格式化字符还有很多，见API。
    
    3. Calendar日历类
    
       ​		Calendar类是一个抽象类，提供了关于日期计算的相关功能，比如：年、月、日的展示和计算。
    
       ​		GregorianCalendar是Calendar的一个具体子类，提供了世界上大多数国家或地区使用的标准日历。
    
       ```java
       /**
        * 测试日期类的使用
        * @author zsk
        *
        */
       public class TestCalendar {
       	public static void main(String[] args) {
               //得到相关日期元素
       		Calendar calendar = new GregorianCalendar(2999,10,9,22,10,50);
       		int year = calendar.get(Calendar.YEAR);
       		int month = calendar.get(Calendar.MONTH);
       		System.out.println(year);
       		System.out.println(month);
       		//设置日期
       		Calendar c2 = new GregorianCalendar();
       		c2.set(Calendar.YEAR, 2999);
       		c2.set(Calendar.MONTH, 3);  //月份 ： 0-11
       		//日期计算
       		Calendar c3 = new GregorianCalendar();
       		c3.add(Calendar.MONTH, -7);
       		//日历对象和时间对象转化
       		Date d = c3.getTime();
       		Calendar c4 = new GregorianCalendar();
       		c4.setTime(new Date());
       		long g  = System.currentTimeMillis();
       		printCalendar(c4);
       	}
       	static void printCalendar(Calendar c) {
       		int year = c.get(Calendar.YEAR);
       		int month = c.get(Calendar.MONTH) + 1;
       	    int day = c.get(Calendar.DAY_OF_MONTH);
       	    int date = c.get(Calendar.DAY_OF_WEEK) - 1; // 星期几
       	    String week = "" + ((date == 0) ? "日" : date);
       	    int hour = c.get(Calendar.HOUR);
       	    int minute = c.get(Calendar.MINUTE);
       	    int second = c.get(Calendar.SECOND);
       	    System.out.printf("%d年%d月%d日,星期%s %d:%d:%d\n", year, month, day,  
       	                        week, hour, minute, second);
       	}
       }
       ```
    
       ---
    
       ```java
       /**
        * 可视化日历程序
        * @author zsk
        *
        */
       public class TestCalendarDate {
       	public static void main(String[] args) throws ParseException {
       		System.out.println("请输入日期（格式为2020-10-2）：");
       		Scanner scanner = new Scanner(System.in);
       		String str = scanner.nextLine();
       		DateFormat df = new SimpleDateFormat("yyyy-MM-dd");
       		Date date = df.parse(str);
       		Calendar c = new GregorianCalendar();
       		c.setTime(date);
       		int day = c.get(Calendar.DATE);
       		c.set(Calendar.DATE, 1);
       		int dow = c.get(Calendar.DAY_OF_WEEK);
       		int max = c.getActualMaximum(Calendar.DATE);
       		System.out.println("日\t一\t二\t三\t四\t五\t六");
       		for(int i = 0; i < dow - 1; i++) {
       			System.out.println("\t");
       		}
       		
       		for(int i = 1; i <= max; i++) {
       			if(c.get(Calendar.DATE) == day) {
       				System.out.print(c.get(Calendar.DATE) + "*\t");
       			}else {
       				System.out.print(c.get(Calendar.DATE) + "\t");
       			}
       			
       			if(c.get(Calendar.DAY_OF_WEEK) == Calendar.SATURDAY) {
       				System.out.println();
       			}
       			c.add(Calendar.DATE, 1);
       		}
       
       	}
       }
       ```
    
4. **Math类**

    		java.lang.Math提供了一系列静态方法用于科学计算；方法的参数和返回值类型一般为double型。
   
   **常用方法**：
   
   		* abs 绝对值
   		* acos，asin，atan，cos，sin，tan  三角函数
   		* sqrt  平方根
   		* pow(double a, double b)   a的b次幂
   		* max(double a, double b)  取大值
   		* min(double a, double b)  取小值
   		* ceil(double a)  大于a的最小整数
   		* floor(double a)  小于a的最大整数
   		* random()  返回0.0到1.0的随机数
   		* long round(double a)  double型的数据a转换成long型（四舍五入）
   		* toDegrees(double angrad)  弧度->角度
   		* toRadians(double angdeg)  角度->弧度
   
   ```java
   		//取整相关操作
           System.out.println(Math.ceil(3.2));
           System.out.println(Math.floor(3.2));
           System.out.println(Math.round(3.2));
           System.out.println(Math.round(3.8));
           //绝对值、开方、a的b次幂等操作
           System.out.println(Math.abs(-45));
           System.out.println(Math.sqrt(64));
           System.out.println(Math.pow(5, 2));
           System.out.println(Math.pow(2, 5));
           //Math类中常用的常量
           System.out.println(Math.PI);
           System.out.println(Math.E);
           //随机数
           System.out.println(Math.random());// [0,1)
   
   		Random rand = new Random();
           //随机生成[0,1)之间的double类型的数据
           System.out.println(rand.nextDouble());
           //随机生成int类型允许范围之内的整型数据
           System.out.println(rand.nextInt());
           //随机生成[0,1)之间的float类型的数据
           System.out.println(rand.nextFloat());
           //随机生成false或者true
           System.out.println(rand.nextBoolean());
           //随机生成[0,10)之间的int类型的数据
           System.out.print(rand.nextInt(10));
           //随机生成[20,30)之间的int类型的数据
           System.out.print(20 + rand.nextInt(10));
           //随机生成[20,30)之间的int类型的数据（此种方法计算较为复杂）
           System.out.print(20 + (int) (rand.nextDouble() * 10));
   ```
   
5. **File类**

     * 基本用法

       ​		java.io.File类：代表文件和目录。在开发中，读取文件、生成文件、删除文件、修改文件的属性时经常用到本类。

       **File类的常见构造方法：public File(String pathname)**

       ​		以pathname为路径创建File对象，如果pathname是相对路径，则默认的当前路径在系统属性user.dir中存储，

       ```java
       public class TestFile{
           public static void main(String[] args) throws Exception{
               System.out.println(System.getProperty("user.dir"));
               File f = new File("a.txt");  //相对路径：默认放到user.dir目录下面
               f.createNewFile();  //创建文件
               File f2 = new File("d:/b.txt");  //绝对路径
               f2.createNewFile();
           }
       }
       ```

       **通过File对象可以访问文件的属性**

       ```java
               File f = new File("d:/b.txt");
               System.out.println("File是否存在："+f.exists());
               System.out.println("File是否是目录："+f.isDirectory());
               System.out.println("File是否是文件："+f.isFile());
               System.out.println("File最后修改时间："+new Date(f.lastModified()));
               System.out.println("File的大小："+f.length());
               System.out.println("File的文件名："+f.getName());
               System.out.println("File的目录路径："+f.getPath());
       ```

       **通过File对象创建空文件或目录（在该对象所指的文件或目录不存在的情况下）**

       ​		createNewFile()				创建新的File

       ​		delete()								删除File对应的文件

       ​		mkdir()								创建一个目录；中间某个目录缺少，则创建失败

       ​		mkdirs()								创建多个目录；中间某个目录缺失，则创建该缺失目录

       ```java
       public class TestFile{
           public static void main(String[] args) throws Exception{
               File f = new File("d:/c.txt");
               f.createNewFile();  //在d盘下生成c.txt文件
               f.delete();  //将该文件或目录从硬盘上删除
               File f2 = new File("d:/电影/华语/大陆");
               boolean flag = f2.mkdir();  //目录结构中有一个不存在，则不会创建整个目录树
           	System.out.println(flag);  //创建失败
               
               boolean flag1 = f2.mkdirs();  //目录中有不存在的没关系，创建整个目录树
               System.out.println(flag);  //创建成功
           }
       }
       ```

    * 递归遍历目录结构和树状展示

      ```java
      /**
       * 测试递归打印文件目录
       * @author zsk
       *
       */
      public class TestFileTree {
      	public static void main(String[] args) {
      		File f = new File("d:/电影");
      		printFile(f, 0);
      	}
      	
      	/**
      	 * 打印文件名称
      	 * @param f 文件名称
      	 * @param level层次数
      	 */
      	static void printFile(File f, int level) {
      		//输出层次数
      		for(int i =0; i < level; i++) {
      			System.out.println("-");
      		}
      		//输出文件名
      		System.out.println(f.getName());
      		//如果file是目录，则获取子文件列表，并对每个子文件进行相同操作
      		if(f.isDirectory()) {
      			File[] files = f.listFiles();
      			for(File temp:files) {
      				//递归调用该方法：注意层级+1
      				printFile(temp, level+1);
      			}
      		}
      	}
      }
      ```

6. **枚举**

    格式如下：

    ```java
    enum 枚举名{
    	枚举体（常量列表）
    }
    ```

    ​		枚举体就是放置一些常量。所有的枚举类型隐性的继承自java.lang.Enum。枚举实质上还是类！而每个被枚举的成员实质就是一个枚举类型的实例，默认都是public static final修饰的。可以通过枚举类型名使用它们。

    **建议**

    		* 当你需要定义一组常量时，可以使用枚举类型。
    		* 尽量不要使用枚举的高级特性，事实上高级特效都可以使用普通类来实现。

    ```java
    public class TestEnum{
        public static void main(String[] args){
            //枚举遍历
            for(Week k : Week.values()){
                System.out.println(k);
            }
            //switch语句中使用枚举
            int a = new Random().nextInt(4); //生成0，1,2,3的随机数
            switch (Season.values()[a]){
                case SPRING:
                    System.out.println("春天");
                    break;
                case SUMMER:
                    System.out.println("夏天");
                    break;
                case AUTUMN:
                    System.out.println("秋天");
                    break;
                case WINDTER:
                    System.out.println("冬天");
                    break;
            }
        }
        
    }
    //季节
    enum Season{
        SPRING, SUMMER, AUTUMN, WINDTER
    }
    //星期
    enum Week{
        周一,周二, 周三, 周四, 周五, 周六, 周日
    }
    ```

    