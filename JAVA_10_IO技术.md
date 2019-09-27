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
   
2. **流分类**

   1. **文件字节流**

      ​		FileInputStream通过字节的方式读取文件，适合读取所有类型的文件（图像、视频、文本文件等）。Java也提供了FileReader专门读取文本文件。

      ​		FileOutputStream通过字节的方式写数据到文件中，适合所有类型的文件。Java也提供了FileWriter专门写入文本文件。

      **将字符串/字节数组的内容写入到文件中**

      ```java
      import java.io.FileOutputStream;
      import java.io.IOException;
      public class TestFileOutputStream{
          public static void main(String[] args){
              FileOutputStream fos = null;
              String string = "zsk";
              try{
                  //true表示内容会追加到文件末尾；false表示重写整个文件内容。
                  fos = new FileOutputStream("d:/a.txt",true);
                  //该方法是直接将一个字节数组写入文件中；而write(int n)是写入一个字节
                  fos.write(string.getBytes());
              }catch(Exception e){
                  e.printStackTrace();
              }finally{
                  try{
                      if(fos != null){
                          fos.close();
                      }
                  }catch(IOException e){
                      e.printStackTrace();
                  }
              }
          }
      }
      ```

      ​		上例中用到一个write方法：void write（byte[] b)，该方法不再一个字节一个字节的写入，而是直接写入一个字节数组；另外其还有一个重载的方法：void write(byte[] b,int off,int length)，这个方法也是写入一个字节数组，但是我们可以指定从字节数组的哪个位置开始写入，写入长度是多少。

      **利用文件流实现文件的复制**

      ```java
      public class TestFileCopy{
          public static void main(String[] args){
              //将a.txt内容拷贝到b.txt
              copyFile("d:/a.txt","d:/b.txt");
          }
          /**
           * 将src文件的内容拷贝到dec文件
           */
          static void copyFile(String src, String dec){
        FileInputStream fis = null;
              FileOutputStream fos = null;
              //为了提高效率，设置缓存数组！（读取的字节数据会暂存到该字节数组中）
              byte[] buffer = new byte[1024];
              int temp = 0;
              try{
                  fis = new FileInputStream(src);
                  fos = new FileOutputStream(dec);
                  //边读边写
                  //temp指的是本次读取的真实长度，temp等于-1时表示读取结束
                  while((temp = fis.read(buffer)) != -1){
                      /*将缓存数组中的数据写入文件中，注意：写入的是读取的真实长度；
                       *如果使用fos.write(buffer)方法，那么写入的长度将会是1024，即缓存
                       *数组的长度*/
                      fos.write(buffer,0,temp);
                  }
              }catch{
                  e.printStackTrace();
              }finally{
                  //两个流需要分别关闭
                  try{
                      if(fos != null){
                          fos.close();
                      }
                  }catch(IOException e){
                      e.printStackTrace();
                  }
                  try{
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
      
      **注意**：
      
      	* 为了减少对硬盘的读写次数，提高效率，通常设置缓存数组。相应的，读取时使用的方法为：read（byte[] b);写入时的方法为：write(byte[] b, int off, int length)。
      	* 程序中如果遇到多个流，每个流都需要单独关闭，防止其中一个流出现异常后导致其他流无法关闭。
      
   2. **文件字符流**
   
      ​		处理文本文件，一般可以使用文件字符流，它以字符为单位进行操作。
   
      **使用FileReader与FileWriter实现文本文件的复制**
   
      ```java
      public class TestFileCopy2{
          public static void main(String[] args){
              //写法和使用Stream基本一样。只不过，读取时是读取的字符。
              FileReader fr = null;
              FileWriter fw = null;
              int len = 0;
              try{
                  fr = new FileReader("d:/a.txt");
                  fw = new FileWriter("d:/d.txt");
                  //为了提高效率，创建缓冲用的字符数组
                  char[] buffer = new char[1024];
                  //边读边写
                  while((len = fr.read(buffer)) != -1){
                      fw.write(buffer,0,len);
                  }
              }catch(FileNotFoundException e){
                  e.printStackTrace();
              }catch(IOException e){
                  e.printStackTrace();
              }finally{
                  try{
                      if(fw != null){
                      	fw.close();
                  	}
                  }catch(IOException e){
                      e.printStackTrace();
                  }
                  try{
                      if(fr != null){
                          fr.close();
                      }
                  }catch(IOException e){
                      e.printStackTrace();
                  }
              }
          }
      }
      ```
   
   3. **缓冲字节流**
   
      ​		Java缓冲流本身并不是具有IO流的读写与写入功能，只是在别的流（节点流或其他处理流）上加上缓冲功能提高效率，就像是把别的流包装起来，因此缓冲流是一种处理流（包装流）。
   
      ​		当对文件或其他数据源进行频繁的读写操作时，效率比较低，这时如果使用缓冲流就能够更高效的读写信息。因为缓冲流是先将数据缓存起来，然后当缓存区存满后或者手动刷新时再一次性的读取到程序或写入目的地。
   
      ​		因此，缓冲流还是很重要的，我们再IO操作时记得加上缓冲流来提升性能。
   
      ​		BufferedInputStream和BufferedOutputStream这两个是缓冲字节流，通过内部缓存数组来提高操作流的效率。
   
      **使用缓冲流实现文件的高效率复制**
   
      ```java
      public class TestBufferedFileCopy1{
          public static void main(String[] args){
              //使用缓冲字节流实现复制
              long time1 = System.currentTimeMillis();
              copyFile1("D:/电影/华语/大陆/zsk.mp4","D:/电影/华语/大陆/zskllb.mp4");
              long time2 = System.currentTimeMillis();
              System.out.println("缓冲字节流话费的时间为：" + (time2- time1));
          }
          
          /**缓冲字节流实现的文件复制方法*/
          static void copyFile1(String src, String dec){
              FileInputStream fis = null;
              BufferedInputStream bis = null;
              FileOutputStream fos = null;
              BufferedOutputStream bos = null;
              int temp = 0;
              try{
                  fis = new FileInputStream(src);
                  fos = new FileOutputStream(dec);
                  //使用缓冲字节流包装文件字节流，增加缓冲功能，提高效率
                  //缓存区的大小（缓存数组的长度）默认是8192，也可以自己指定大小。
                  bis = new BufferedInputStream(fis);
                  bos = new BufferedOutputStream(fos);
                  while((temp = bis.read()) != -1){
                      bos.write(temp);
                  }
              }catch(Exception e){
                  e.printStackTrace();
              }finally{
                  //注意：增加处理流后，注意流的关闭顺序："后开的先关"
                  try{
                     if(bos != null){
                         bos.close();
                     } 
                  }catch(IOException e){
                      e.printStackTrace();
                  }
                  try{
                      if(bis != null){
                          bis.close();
                      }
                  }catch(IOException e){
                      e.printStackTrace();
                  }
                  try{
                      if(fos != null){
                          fos.close();
                      }
                  }catch(IOException e){
                      e.printStackTrace();
                  }
                  try{
                      if(fis != null){
                          fis.close();
                      }
                  }catch(IOException e){
                      e.printStackTrace();
                  }
              }
          }
          
          /**普通字节流实现的文件复制的方法*/
          static void copyFile2(String src, String dec){
              FileInputStream fis = null;
              FileOutputStream fos = null;
              int temp = 0;
              try{
                  fis = new FileInputStream(src);
                  fos = new FileOutputStream(dec);
                  while((temp = fis.read()) != -1){
                      fos.write(temp);
                  }
              }catch(Exception e){
                  e.printStackTrace();
              }finally{
                  try{
                      if(fos != null){
                          fos.close();
                      }
                  }catch(IOException e){
                      e.printStackTrace();
                  }
                  try{
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
   
   4. **缓冲字符流**
   
      ​		BufferedReader/BufferedWriter增加了缓存机制，大大提高了读写文本文件的效率，同时，提供了更方便的按行读取的方法：readLine()；处理文本时，我们一般可以使用缓冲字符流。
   
      **使用BufferedReader与BufferedWriter实现文本文件的复制**
   
      ```java
      public class TestBufferedFileCopy2{
          public static void main(String[] args){
              //注：处理文本文件时，实际开发中可以是先用如下写法，简单高效！
              FileReader fr = null;
              FileWriter fw = null;
              BufferedReader br = null;
             	BufferedWriter bw = null;
              String tempString = "";
              try{
                  
              }catch(FileNotFoundException e){
                  e.printStackTrace();
              }catch(IOException e){
                  e.printStackTrace();
              }finally{
                  try{
                      if(bw != null){
                          bw.close();
                      }
                  }catch(IOException e1){
                      e1.printStackTrace();
                  }
                  try{
                      if(br != null){
                          br.close();
                      }
                  }catch(IOException e1){
                      e1.printStackTrace();
                  }
                  try{
                      if(fw != null){
                          fw.close();
                      }
                  }catch(IOException e){
                      e.printStackTrace();
                  }
                  try{
                      if(fr != null){
                          fw.close();
                      }
                  }catch(IOException e){
                      e.pringStackTrace();
                  }
              }
          }
      }
      ```
   
      **注**：
   
      		* readLine()方法是BufferedReader特有的方法，可以对文本文件进行更加方便的读取操作。
      		* 写入一行后要记得使用newLine()方法换行。
   
   5. **字节数组流**
   
      ​		ByteArrayInputStream和ByteArrayOutputStream经常用在需要流和数组之间转化的情况！
   
      ​		ByteArrayInputStream是把内存中的“某个字节数组对象”当做数据源。
   
      **简单测试ByteArrayInputStream的使用**
   
      ```java
      public class TestByteArray{
          public static void main(String[] args){
              //将字符串转变成字节数组
              byte[] b = "abcdefg".getBytes();
              test(b);
          }
          public static void test(byte[] b){
              ByteArrayInputStream bais = null;
              StringBuilder sb = new StringBuilder();
              int temp = 0;
              //用于保存读取的字节数
              int num = 0;
              try{
                  //该构造方法的参数是一个字节数组，这个字节数组就是数据源
                  bais = new ByteArrayInputStream(b);
                  while((temp = bais.read()) != -1){
                      sb.append((char)temp);
                      num++;
                  }
                  System.out.println(sb);
                  System.out.print("读取的字节数：" + num)；
              }finally{
                  try{
                      if(bais != null){
                          bais.close();
                      }
                  }catch(IOException e){
                      e.printStackTrace();
                  }
              }
          }
      }
      ```
   
      