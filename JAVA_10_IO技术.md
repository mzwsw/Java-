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
   
   6. **数据流**
   
      ​		数据流将“基本数据类型与字符串类型”作为数据源，从而允许程序以与机器无关的方式从低层输入输出流中操作Java基本数据类型与字符串类型。
   
      ​		DataInputStream和DataOutputStream提供了可以存取与机器无关的所有Java基础类型数据（如：int、double、String等）的方法。
   
      ​		DataInputStream和DataOutputStream是处理流，可以对其他节点流或处理流进行包装，增加一些更灵活、更高效的功能。
   
      ```java
      public class TestDataStream{
          public static void main(String[] args){
              DataOutputStream dos = null;
              DataInputStream dis = null;
              FileOutputStream fos = null;
              FileInputStream fis = null;
              try{
                  fos = new FileOutputStream("D:/data.txt");
                  fis = new FileInputStream("D:/data.txt");
                  //使用数据流对缓冲流进行包装，新增缓冲功能
                  dos = new DataOutputStream(new BufferedOutputStream(fos));
                  dis = new DataInputStream(new BufferedInputStream(fis));
                  //将如下数据写入到文件中
                  dos.writeChar('a');
                  dos.writeInt(10);
                  dos.writeDouble(Math.random());
                  dos.writeBoolean(true);
                  dos.writeUTF("张世坤");
                  //手动刷新缓冲区；将流中数据写入到文件中
                  dos.flush();
                  //直接读取数据：读取的顺序要与写入的顺序一致，否则不能正确读取。
                  System.out.println("char:" + dis.readChar());
                  System.out.println("int:" + dis.readInt());
                  System.out.println("double:" + dis.readDouble());
                  System.out.println("boolean:" + dis.readBoolean());
                  System.out.println("String:" + dis.readUTF());
              }catch(IOException e){
                  e.printStackTrace();
              }finally{
                  try{
                      if(dos != null){
                          dos.close();
                      }
                  }catch(IOException e){
                      e.printStackTrace();
                  }
                  try{
                      if(dis != null){
                          dis.close();
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
      }
      ```
   
      **注意**：使用数据流时，读取的顺序一定要与写入的顺序一致，否则不能正确读取数据。
   
   7. **对象流**
   
      ​		前面的数据流只能实现对基本数据类型和字符串类型的读写，并不能读取对象（字符串除外），我们要对某个对象进行读写操作，需要新的处理流：ObjectInputStream/ObjectOutputStream。以“对象”为数据源，必须将传输的对象进行序列号与反序列化操作。
   
      ```java
      public class TestObjectStream{
          public static void main(String[] args) throws IOException, ClassNotFoundException{
              write();
              read();
          }
          /**使用对象输出流将数据写入文件*/
          public static void write(){
              //创建Object输出流，并包装缓冲流，增加缓冲功能
              OutputStream os = null;
              BufferedOutputStream bos = null;
              ObjectOutputStream oos = null;
              try{
                  os = new FileOutputStream(new File("d:/bjsxt.txt"));
                  bos = new BufferedOutputStream(os);
                  oos = new ObjectOutputStream(bos);
                  //使用Object输出流
                  //对象流也可以对基本数据类型进行读写操作
                  oos.writeInt(12);
                  oos.writeDouble(3.14);
                  oos.writeChar('A');
                  oos.writeBoolean(true);
                  oos.writeUTF("张世坤");
                  //对象流能够对对象数据类型进行读写操作
                  //Date是系统提供的类，已经实现了序列号接口
                  //如果是自定义类，则需要自己实现序列号接口
                  oos.writeObject(new Date());
              }catch(IOException e){
                  e.printStackTrace();
              }finally{
                  //关闭输出流
                  if(oos != null){
                      try{
                          oos.close();
      				}catch(IOException e){
                          e.printStackTrace();
                      }
                  }
                  if(bos != null){
                      try{
                          bos.close();
      				}catch(IOException e){
                          e.printStackTrace();
                      }
                  }
                  if(os != null){
                      try{
                          os.close();
      				}catch(IOException e){
                          e.printStackTrace();
                      }
                  }
              }
          }
          
          /**使用对象输入流将数据读入程序*/
          public static void read(){
              //创建Object输入流
              InputStream is = null;
              BufferedInputStream bis = null;
              ObjectInputStream ois = null;
              try{
                  is = new FileInputStream(new File("d:/bjsxt.txt"));
                  bis = new BufferedInputStream(is);
                  ois = new ObjectInputStream(bis);
                  //使用Object输入流按照写入顺序读取
                  System.out.println(ois.readInt());
                  System.out.println(ois.readDouble());
                  System.out.println(ois.readChar());
                  System.out.println(ois.readBoolean());
                  System.out.println(ois.readUTF());
                  System.out.println(ois.readObject().toString());
              }catch(ClassNotFoundException e){
                  e.pringStackTrace();
              }catch(IOException e){
                  e.printStackTrace();
              }finally{
                  //关闭流
                  if(ois != null){
                   	try{
                          ois.close();
                      }catch(IOException e){
                          e.printStackTrace();
                      }
                  }
                  if(bis != null){
                   	try{
                          bis.close();
                      }catch(IOException e){
                          e.printStackTrace();
                      }
                  }
                  if(is != null){
                   	try{
                          is.close();
                      }catch(IOException e){
                          e.printStackTrace();
                      }
                  }
              }
          }
      }
      ```
   
      **注意**：
   
      * 对象流不仅可以读写对象，还可以读写基本数据类型。
      * 使用对象流读写对象时，该对象必须序列化与反序列化。
      * 系统提供的类（如Date等）已经实现了序列号接口，自定义类必须手动实现序列化接口。
   
   8. **转换流**
   
      ​		InputStreamReader：将包含字节（用某种字符编码方式表示的字符）的输入流转换成可以产生Unicode码元的读入器。（字节-->字符）
   
      ​		OutputStreamWriter：将使用选定的字符编码方式，把Unicode字符流转换成字节流。（字符-->字节）
      
      ```java
      public class TestConvertStream{
          public static void main(String[] args){
              //创建字符输入和输出流：使用转换流将字节流转换成字符流
              BufferedReader br = null;
              BufferedWriter bw = null;
              try{
                  br = new BufferedReader(new InputStreamReader(System.in));
                  bw = new BufferedWriter(new OutputStreamWriter(System.out));
                  //使用字符输入和输出流
                  String str = br.readLine();
                  //一直读取，直到用户输入exit为止
                  while(!"exit".equals(str)){
                      //写到控制台
                      bw.write(str);
                      bw.newLine();//写一行后换行
                      bw.flush();//手动刷新
                      //再读一行
                      str = br.readLine();
                  }
              }catch(IOException e){
                  e.printStackTrace();
              }finally{
                  //关闭字符输入和输出流
                  if(br != null){
                      try{
                          br.close();
                      }catch(IOException e){
                          e.printStackTrace();
                      }
                  }
                  if(bw != null){
                      try{
                          bw.close();
                      }catch(IOException e){
                          e.printStackTrace();
                      }
                  }
              }
          }
      }
      ```
   
3. **序列化与反序列化**

   1. **定义**

      ​		当两个进行远程通信时，彼此可以发送各种类型的数据。无论是何种类型的数据，都会以二进制序列的形式在网络上传送。比如，我们可以通过http协议发送字符串信息；我们也可以在网络上直接发送Java对象。发送方需要把这个Java对象转换为字节序列，才能在网络上传送；接收方则需要把字节序列再恢复为Java对象才能正常读取。

      ​		把Java对象转换为字节序列的过程称为对象的序列化。把字节序列恢复为Java对象的过程称为对象的反序列化。

      ​		序列化的作用：

      * 持久化：把对象的字节序列永久地保存到硬盘上，通常存放在一个文件中。以后服务器session管理，hibernate将对象持久化实现。
      * 网络通信：在网络上传送对象的字节序列。比如：服务器之间的数据通信。

   2. **序列化涉及的类和接口**

      ​		ObjectOutputStream代表对象输出流，它的writeObject（Object obj）方法可对参数指定的obj对象进行序列化，把得到的字节序列写到一个目标输出流中。

      ​		ObjectInputStream代表对象输入流，它的readObject()方法从一个源输入流中读取字节序列，再把他们反序列化为一个对象，并将其返回。

      ​		只有实现了Serializable接口的类的对象才能被序列化。Serializable接口是一个空接口，只起到标记作用。
      
   3. **序列化/反序列化的步骤和实例**
   
      ```java
      //Person类实现Serializable接口后，Person对象才能被序列化
      class Person implements Serializable{
          //添加序列化ID，它决定着是否能够成功反序列化！
          private static final long serialVersionUID = 1L;
          int age;
          boolean isMan;
          String name;
          
          public Person(int age, boolean isMan, String name){
              super();
              this.age = age;
              this.isMan = isMan;
              this.name = name;
          }
          
          @Override
          public String toString(){
              return "Person [age=" + age + ",isMan=" + isMan + ",name=" + name + "]";
          }
      }
      
      public class TestSerializable{
          public static void main(String[] args){
              FileOutputStream fos = null;
              ObjectOutputStream oos = null;
              ObjectInputStream ois = null;
              FileInputStream fis = null;
              try{
                  //通过ObjectOutputStream将Person对象的数据写入到文件中，即序列化。
                  Person person = new Person(18,true,"zsk");
                  //序列化
                  fos = new FileOutputStream("d:/c.txt");
                  oos = new ObjectOutputStream(fos);
                  oos.writeObject(person);
                  oos.flush();
                  //反序列化
                  fis = new FileInputStream("d:/c.txt");
                  //通过ObjectInputStream将文件中二进制数据反序列化为Person对象：
                  ois = new ObjectInputStream(fis);
                  Person p = (Person) ois.readObject();
                  System.out.println(p);
              }catch(ClassNotFoundException e){
                  e.printStackTrace();
              }catch(IOException e){
                  e.printStackTrace();
              }finally{
                  if (oos != null) {
                      try {
                          oos.close();
                      } catch (IOException e) {
                          e.printStackTrace();
                      }
                  }
                  if (fos != null) {
                      try {
                          fos.close();
                      } catch (IOException e) {
                          e.printStackTrace();
                      }
                  }
                  if (ois != null) {
                      try {
                          ois.close();
                      } catch (IOException e) {
                          e.printStackTrace();
                      }
                  }
                  if (fis != null) {
                      try {
                          fis.close();
                      } catch (IOException e) {
                          e.printStackTrace();
                      }
                  }
              }
          }
      }
      ```
   
      **注意**：
   
      * static属性不参与序列化
      * 对象中的某些属性如果不想被序列化，不能使用static，而是使用transient修饰。
      * 为了防止读和写的序列化ID不一致，一般指定一个固定的序列化ID。
   
4. **装饰器**

   1. **装饰器模式**

      ​		装饰器模式是GOF23种设计模式中较为常用的一种模式。它可以实现对原有类的包装和装饰，使新的类具有更强的功能。

      ```java
      class Iphone{
          private String name;
          public Iphone(String name){
              this.name = name;
          }
          public void show(){
              System.out.println("我是" + name + ",可以在屏幕上显示")；
          }
      }
      
      class TouyingPhone{
          public Iphone phone;
          public TouyingPhone(Iphone p){
              this.phone = p;
          }
          //功能更前的方法
          public void show(){
              phone.show();
              System.out.println("还可以投影，在墙壁上显示")；
          }
      }
      
      public class TestDecoration{
          public static void main(String[] args){
              Iphone phone = new Iphone("iphone30");
              phone.show();
              System.out.println("============装饰后：");
              TouyingPhone typhone = new TouyingPhone(phone);
              typhone.show();
          }
      }
      ```

   2. **IO流体系中的装饰器模式**

      ​		IO流体系中大量使用了装饰器模式，让流具有更强的功能、更强的灵活性。比如

      > FileInputStream fis = new FileInputStream(src);
      >
      > BufferedInputStream bis = new BufferedInputStream(fis);

      ​		显然BufferedInputStream装饰了原有的FileInputStream，让普通的FileInputStream也具备了缓存功能，提高了效率。

5. **Apache IOUtils和FileUtils的使用**

   ​		Apache-commons工具包中提供了IOUtils/FileUtils，可以让我们非常方便的对文件和目录进行操作。类库中提供了更加简单、功能更加强大的文件操作和IO流操作功能。

   1. **FileUtils的妙用**

      ​		常用方法介绍

      cleanDirectory：清空目录，但不删除目录

      contentEquals：比较两个文件的内容是否相同

      copyDirectory：将一个目录内容拷贝到另一个目录。可以通过FileFilter过滤需要拷贝的文件

      copyFile：讲一个文件拷贝到一个新地址

      copyFileToDirectory：讲一个文件拷贝到某个目录下

      copyInputStreamToFile：讲一个输入流中的内容拷贝到某个文件

      deleteDirectory：删除目录

      deleteQuietly：删除文件

      listFiles：列出指定目录下的所有文件

      openInputStream：打开指定文件的输入流

      readFileToString：将文件内容作为字符串返回

      readLines：将文件内容按行返回到一个字符串数组中

      size：返回文件或目录的大小

      write：将字符串内容直接写到文件中

      writeByteArrayToFile：将字节数组内容写到文件中

      writeLines：将容器中的元素的toString方法返回的内容一次写入文件中

      writeStringToFile：将字符串内容写到文件中

      **读取文件内容，并输出到控制台（只需一行代码）**

      ```java
      import java.io.File;
      import org.apache.commons.io.FileUtils;
      public class TestUtils1{
          public static void main(String[] args) throws Exception {
              String content = FileUtils.readFileToString(new File("d:/a.txt"),"gbk");
              System.out.println(content);
          }
      }
      ```

      **目录拷贝，并使用FileFilter过滤目录和以html结尾的文件**

      ```java
      public class TestUtils2{
          public static void main(String[] args) throws Exception {
              FileUtils.copyDirectory(new File("d:/aaa"),new File("d:/bbb"),new FileFilter(){
                  @Override
                  public boolean accept(File pathname){
                      //使用FileFilter过滤目录和以html结尾的文件
                      if(pathname.isDirectory() || pathname.getName().endsWith("html")){
                          return true;
                      }else{
                          return false;
                      }
                  }
              });
          }
      }
      ```

   2. **IOUtils的妙用**

      buffer方法：将传入的流进行包装，变成缓冲流。并可以通过参数指定缓冲大小。

      closeQueitly方法：关闭流。

      contentEquals方法：比较两个流中的内容是否一致。

      copy方法：将输入流中的内容拷贝到输出流中，并可以指定字符编码。

      copyLarge方法：将输入流中的内容拷贝到输出流中，适合大于2G内容的拷贝。

      lineIterator方法：返回可以迭代每一行内容的迭代器。

      read方法：将输入流中的部分内容读入到字节数组中。

      readFully方法：将输入流中的所有内容读入到字节数组中。

      readLine方法：读入输入流内容中的一行。

      toBufferedInputStream，toBufferedReader：将输入转为带缓存的输入流。

      toByteArray，toCharArray：将输入流的内容转为字节数组、字符数组。

      toString：将输入流或数组中的内容转化为字符串。

      write方法：向流里面写入内容。

      writeLine方法：向流里面写入一行内容。

      **IOUtils的方法**

      ```java
      public class TestUtils3{
          public static void main(String[] args) throws Exception {
              String content = IOUtils.toString(new FileInputStream("d:/a.txt"),"gbk");
              System.out.println(content);
          }
      }
      ```

      