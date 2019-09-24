# 十、IO技术

1. **基本概念和IO入门**

   * 输入（Input）是指：可以让程序从外部系统获得数据（核心含义是“读”，读取外部数据）。
   * 输出（Output）是指：程序输出数据给外部系统从而可以操作外部系统。（核心含义是“写”）。

   java.io包为我们提供了相关的API。

   1. **数据源**

      数据源data source，提供数据的原始媒介。常见的数据源有：数据库、文件、其他程序、内存等。

      数据源分为：源设备、目标设备。

      * 源设备：为程序体用数据，一般对应输入流。
      * 目标设备：程序数据的目的地，一般对应输出流。

   2. **流的概念**

      流是一个抽象、动态的概念，是一连串连续动态的数据集合。

      **输入/输出流的划分是相对程序而言的，并不是相对数据源**

   3. **一个简单的IO流程序及深入理解**

      ​		当程序需要读取数据源的数据时，就会通过IO流对象开启一个通向数据源的流，通过这个IO流对象的相关方法可以顺序读取数据源中的数据。
   
      **使用流读取文件内容（不规范写法，仅用于测试）**
      
      ```java
      import java.io.*
      public class TestIO1{
      	public static void main(String[] args){
      	try{
      		//创建输入流
      		FileInputStream fis = new FileInputStream("d:/a.txt");
      		//一个字节一个字节的读取数据
      		int s1 = fis.read();//打印输入字符a对应的ascii码值97
      		int s2 = fis.read();
      		int s3 = fis.read();
      		int s4 = fis.read();
      		System.out.println(s1);
      		System.out.println(s2);
      		System.out.println(s3);
      		System.out.println(s4);
      		//流对象使用完，必须关闭！不然，总占用系统资源，最终会造成系统崩溃！
      		fis.close();
      	}catch(Exception e){
      		e.printStackTrace();
      	}
      	}
      }
      ```
      
      **使用流读取文件内容（经典代码）**
      
      ```java
      import java.io.*
      public class TestIO02{
      	public static void main(String[] args){
      		FileInputStream fis = null;
      		try{
      			fis = new FileInputStream("d/a.txt");
      			StringBuilder sb = new StringBuilder();
      			int temp = 0;
      			//当temp等于-1时，表示已经到了文件结尾，停止读取
      			while((temp = fis.read()) != -1){
      				sb.append((char)temp);
      			}
      			System.out.println(sb);
      		}catch(Exception e){
      			e.printStackTrace();
      		}finally{
      			try{
      				//这种写法，保证了即使遇到异常情况，也会关闭流对象。
      				if(fis != null){
      					fis.close();
      				}
      			}catch(IOException e){
      				e.printStackTrace();
      			}
      		}
      	}
      }
      ```
      
   4. **Java中流的概念细分**
   
      * 按流的方向分类：
        1. 输入流：数据流向是数据源到程序（以InputStream、Reader结尾的流）。
        2. 输出流：数据流向是程序到目的地（以OutputStream、Write结尾的流）。
      * 按处理的数据单元分类：
        1. 字节流：以字节为单位获取数据，命名上一Stream结尾的流一般是字节流，如FileInputStream、FileOutputStream。
        2. 字符流：以字符为单位获取数据，命名上以Reader/Writer结尾的流一般是字符流，如FileReader、FileWriter。
      * 按处理对象不同分类：
        1. 节点流：可以直接从数据源或目的地读写数据，如FileInputStream、FileReader、DataInputStream等。
        2. 处理流：不直接连接到数据源或目的地，是“处理流的流”。通过对其他流的处理提高程序的性能，如BufferedImputStream、BufferedReader等。处理流也叫包装流。
        3. 节点流处于IO操作的第一线，所有操作必须通过它们进行；处理流可以对节点流进行包装，提高性能或提高程序的灵活性。
   
   5. **Java中IO流类的体系**
   
      ​	Java为我们提供了各种各样的IO流，我们可以根据不同的功能及性能要求挑选合适的IO流。
   
       1. InputStream/OutputStream
   
          字节流的抽象类
   
      	2. Reader/Writer
   
          字符流的抽象类
   
      	3. FileInputStream/FileOutputStream
   
          节点流：以字节为单位直接操作“文件”
   
      	4. ByteArrayInputStream/ByteArrayOutputStream
   
          节点流：以字节为单位直接操作“字节数组对象”。
   
      	5. ObjectInputStream/ObjectOutputStream
   
          处理流：以字节为单位直接操作"对象"。
   
      	6. DataInputStream/DataOutputStream
   
          处理流：以字节为单位直接操作“基本数据类型与字符串类型”。
   
      	7. FileReader/FileWriter
   
          节点流：以字符为单位直接操作“文本文件”（注意：只能读写文本文件）。
   
      	8. BufferedReader/BufferedWriter
   
          处理流：将Reader/Writer对象进行包装，增加缓存功能，提高读写效率。
   
      	9. BufferedInputStream/BufferedOutputStream
   
          处理流：将InputStream/OutputStream对象进行包装，增加缓存功能，提高读写效率。
   
      	10. InputStreamReader/OutputStreamWriter
   
           处理流：将字节流对象转化成字符流对象。
   
      	11. PrintStream
   
           处理流：将OutputStream进行包装，可以方便地输出字符，更加灵活。
   
   6. **四大IO抽象类**
   
      ​		InputStream/OutputStream和Reader/Writer类是所有IO流类的抽象父类。
   
      * **InputStream**
   
        ​		此抽象类是表示字节输入流的所有类的父类。InputStream是一个抽象类，它不可以实例化。数据的读取需要由它的子类来实现。根据及节点的不同，它派生了不同的节点流子类。
   
        ​		继承自InputStream的流都是用于向程序输入数据，且数据的单位为字节（8bit）。
   
        ​		常用方法：
   
        ​		int read()：读取一个字节的数据，并将字节的值作为int类型返回。如果未读出字节则返回-1.
   
        ​		void close()：关闭输入流对象，释放相关系统资源。
   
      * **OutputStream**
   
        ​		此抽象类是表示字节输出流的所有类的父类。输出流接收输出字节并将这些字节发送到某个目的地。
   
        ​		常用方法：
   
        ​		void write(int n)：向目的地中写入一个字节。
   
        ​		void close()：关闭输出流对象，释放相关系统资源。
   
      * **Reader**
   
        ​		Reader用于读取的字符流抽象类，数据单位为字符。
   
        ​		int read()：读取一个字符的数据，并将字符的值作为int类型返回。如果未读出字符则返回-1.
   
        ​		void close()：关闭流对象，释放相关系统资源。
   
      * **Writer**
   
        ​		Writer用于写入的字符流抽象类，数据单位为字符。
   
        ​		void writer(int n)：向输出流中写入一个字符。
   
        ​		void close()：关闭输出流对象，释放想法系统资源。
   