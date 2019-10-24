# JVM核心

## 一、JVM运行和类加载全过程

1. **类加载全过程**

   * 为什么研究类加载全过程？

     * 有助于了解JVM运行过程
     * 更深入了解java动态性，（解热部署、动态加载），提高程序的灵活性

   * **类加载机制**

     ​		JVM把class文件加载到内存，并对数据进行校验、解析和初始化，最终形成JVM可以直接使用的Java类型的过程

     * 加载

       ​		将class文件字节码内容加载到内存中，并将这些镜头数据转换成方法区中的运行时数据结构，在堆中生成一个代表这个类的java.lang.Class对象，作为方法区类数据的访问入口。这个过程需要类加载器参与。

     * 链接

       ​		将Java类的二进制代码合并到JVM的运行状态之中的过程

       * 验证：确保加载的类信息符合JVM规范，没有安全方面的问题
       * 准备：正式为类变量（static变量）分配内存并设置类变量初始化值的阶段，这些内存都将在方法区中进行分配。
       * 解析：虚拟机常量池内的符号引用替换为直接引用的过程

     * 初始化

       * 初始化阶段是执行类构造器<clinit>()方法的过程。类构造器<clinit>()方法是由编译器自动收集类中的所有类变量的赋值动作和静态语句块（static块）中的语句合并产生的。
       * 当初始化一个类的时候，如果发现其父类还没有进行过初始化，则需要先进行其父类的初始化
       * 虚拟机会保证一个类的<clinit>()方法在多线程环境中被正确加锁和同步。

   * 类的主动引用（一定会发生类的初始化）

     * new一个类的对象
     * 调用类的静态成员（除了final常量）和静态方法
     * 使用java.lang.reflect包的方法对类进行反射调用
     * 当虚拟机启动，java Hello，则一定会初始化Hello类。即：先启动main方法所在的类
     * 当初始化一个类，如果其父类没有被初始化，则先会初始它的父类

   * 类的被动引用（不会发生类的初始化）

     * 当访问一个静态域时，只有真正声明这个域的类才会被初始化

       * 通过子类引用父类的静态变量，不会导致子类初始化

     * 通过数组定义类引用，不会触发此类的初始化

       ```java
       	A[] as = new A[10];
       ```

     * 引用常量不会触发此类的初始化（常量在编译阶段就存入调用类的常量池中了）

## 二、深入类加载器

1. **类加载器的作用**

   * 作用

     ​		将class文件字节码内容加载到内存中，并将这些静态数据转换成方法区中的运行时数据结构，在堆中生成一个代表这个类的java.lang.Class对象，作为方法区类数据的访问入口。

   * 类缓存

     ​		标准的JavaSE类加载器可以按要求查找类，但一旦某个类被加载到类加载器中，它将维持加载（缓存）一段时间。不过JVM垃圾收集器可以回收这些Class对象。

2. **java.class.ClassLoader类**

   * 作用

     * java.lang.ClassLoader类的基本职责就是根据一个指定的类的名称，找到或者生成其对应的字节代码，然后从这些字节代码中定义出一个Java类，即java.lang.Class类的一个实例。
     * ClassLoader还负责加载Java应用所需的资源。

   * 相关方法

     * getParent()		返回该类加载器的父类加载器

       loadeClass(String name)		加载名称为name的类，返回的结果是java.lang.Class的实例。

       findClass(String name)		查找名称为name的类，返回的结果是java.lang.Class类的实例。

       findLoaderClass(String name)		查找名称为name的已经被加载过的类，返回的结果是

       ​                                                      	  java.lang.class类的实例

       defineClass(String name, byte[] b, int off, int len)		把字节数组b中的内容转换成java类，返回

       ​                                                  的结果是java.lang.Class类的实例。这个方法被声明为final的

       resolveClass(Class<?> c)		链接指定的Java类

     * 对于以上给出的方法，表示类名称的name参数的值时类的二进制名称。需要注意的是内部类的表示，如com.example.Sample$1和com.example.Sample$Inner等表示方式。

3. **类加载器的层次结构（树状结构）**

   * 引导类加载器（bootstrap class loader)
     * 它用来加载Java的核心库（JAVA_HOME/jre/lib/rt.jar或sun.boot.class.path路径下的内容），是用原生代码来实现的，并不继承自java.lang.ClassLoader。
     * 加载扩展类和应用程序类加载器。并制定他们的父类加载器
   * 扩展类加载器（extensions class loader)
     * 用来加载Java的扩展库(JAVA_HOME/jre/ext/*.jar或java.ext.dirs路径下的内容)。Java虚拟机的实现会提供一个扩展库目录。该类加载器在此目录里面查找并加载Java类。
     * 由sun.misc.Launcher$ExtClassLoader实现
   * 应用程序类加载器（application class loader）
     * 它根据Java应用的类路径（classpath，java.class.path路径下的内容）来加载Java类。一般来说，Java应用的类都是由它类加载的。
     * 由sun.misc.Launcher$AppClassLoader实现
   * 自定义类加载器
     * 开发人员可以通过继承java.lang.ClassLoader类的方式实现自己的类加载器，以满足一些特殊的要求。

4. **类加载器的代理模式**

   * 代理模式

     交给其他加载器来加载指定的类

   * 双亲委托机制

     * 就是某个特定的类加载器在接到加载类的请求时，首先将加载任务委托给父类加载器，依次追溯，直到最高的类加载器，如果父类加载器可以完成加载任务，就成功返回；只有父类加载器无法完成此加载任务时，才自己去加载。
     * 双亲委托机制就是为了保证Java核心库的类型安全。
       * **这种机制就保证不会出现用户自己能定义java.lang.Object类的情况。**
     * 类加载器除了用于加载类，也是安全的最基本的屏障

   * 双亲委托机制是代理模式的一种

     * 并不是所有类加载器都采用双亲委托机制。
     * tomcat服务器类加载器也是使用代理模式，所不同的是它是首先尝试去加载某个类，如果找不到再代理给父类加载器。这与一般类加载器的顺序是反的。

5. **java.class.ClassLoader类的API**

   * 相关方法

     * getParent()		返回该类加载器的父类加载器

       loadeClass(String name)		加载名称为name的类，返回的结果是java.lang.Class的实例。

       * 此方法负责加载指定名字的类，首先会从已加载的类中去寻找，如果没有找到；从parent ClassLoader[ExtClassLoader]中加载；如果没有加载到，则从Bootstrap ClassLoader中尝试加载（findBootstrapClassOrNull方法），如果还是加载失败，则自己加载。如果还不能加载，抛出异常ClassNotFoundException。
       * 如果要改变类的加载顺序可以覆盖此方法；

       findClass(String name)		查找名称为name的类，返回的结果是java.lang.Class类的实例。

       findLoaderClass(String name)		查找名称为name的已经被加载过的类，返回的结果是

       ​                                                            java.lang.class类的实例

       defineClass(String name, byte[] b, int off, int len)		把字节数组b中的内容转换成java类，返回

       ​															的结果是java.lang.Class类的实例。这个方法被声明为final的

       resolveClass(Class<?> c)		链接指定的Java类

       **注**：表示类名称的name参数的值时类的二进制名称。需要注意的是内部类的表示，如com.example.Sample$1和com.example.Sample$Inner等表示方式。

6. **自定义类加载器**

   * 文件系统类加载器

   * 自定义类加载器的流程：

     1. 首先检查请求的类型是否已经被这个类装载器装载到命名空间中了，如果已经装载，直接返回；否则转入步骤2
     2. 委派类加载请求给父类加载器（更准确的说应该是双亲类加载器，真个虚拟机中各种类加载器最终会呈现树状结构），如果父类加载器能够完成，则返回父类加载器加载的Class实例；否则转入步骤3
     3. 调用本类加载器的findClass（…）方法，试图获取对应的字节码，如果获取的到，则调用defineClass（…）导入类型到方法区；如果获取不到对应的字节码或者其他原因失败，返回异常给loadClass（…）， loadClass（…）转抛异常，终止加载过程（注意：这里的异常种类不止一种）。

     * **注**：**被两个类加载器加载的同一个类，JVM不认为是相同的类**

     ```java
     /**
      * 自定义文件系统类加载器
      * @author zsk
      *
      */
     public class FileSystemClassLoader extends ClassLoader {
     	
     	private String rootDir;
     	
     	public FileSystemClassLoader(String rootDir) {
     		this.rootDir = rootDir;
     	}
     	
     	@Override
     	protected Class<?> findClass(String name) throws ClassNotFoundException{
     		
     		Class<?> c = findLoadedClass(name);
     		
     		//应该先查询有没有加载过这个类，如果已经加载，则直接返回加载好的类。如果没有，则重新加载
     		if(c != null) {
     			return c;
     		}else {
     			ClassLoader parent = this.getParent();
     			
     			try {
     				c = parent.loadClass(name);  //委派给父类加载
     			}catch(Exception e) {
     //				e.printStackTrace();
     			}
     			
     			
     			if(c != null) {
     				return c;
     			}else {
     				byte[] classData = getClassData(name);
     				if(classData == null) {
     					throw new ClassNotFoundException();
     				}else {
     					c = defineClass(name, classData, 0, classData.length);
     				}
     			}
     		}
     		return c;
     	}
     	
     	private byte[] getClassData(String classname) {
     		String path = rootDir + "/" + classname.replace('.', '/') + ".class";
     		
     		//IOUtils,可以使用它将流中的数据转成字节数组
     		InputStream is = null;
     		ByteArrayOutputStream baos = new ByteArrayOutputStream();
     		
     		try {
     			is = new FileInputStream(path);
     			
     			byte[] buffer = new byte[1024];
     			int temp = 0;
     			while((temp = is.read(buffer)) != -1) {
     				baos.write(buffer, 0, temp);
     				
     			}
     			
     			return baos.toByteArray();
     			
     		}catch(Exception e) {
     			e.printStackTrace();
     			return null;
     		}finally {
     			if(is != null) {
     				try {
     					is.close();
     				} catch (IOException e) {
     					e.printStackTrace();
     				}
     			}
     			if(baos != null) {
     				try {
     					baos.close();
     				} catch (IOException e) {
     					e.printStackTrace();
     				}
     			}
     		}
     	}
     	
     }
     
     
     
     /**
      * 测试自定义的FileSystemClassLoader
      * @author zsk
      *
      */
     public class Demo01 {
     	public static void main(String[] args) throws Exception {
     		FileSystemClassLoader loader = new FileSystemClassLoader("f:/myjava");
     		FileSystemClassLoader loader2 = new FileSystemClassLoader("f:/myjava");
     		
     		Class<?> c = loader.loadClass("cn.zsk.bean.HelloWorld");
     		Class<?> c2 = loader.loadClass("cn.zsk.bean.HelloWorld");
     		Class<?> c3 = loader2.loadClass("cn.zsk.bean.HelloWorld");
     		
     		Class<?> c4 = loader2.loadClass("java.lang.String");
     		
     		
     		System.out.println(c.hashCode());
     		System.out.println(c2.hashCode());
     		System.out.println(c3.hashCode());    //同一个类被不同的加载器加载，JVM认为是不同的类
     		System.out.println(c4.hashCode());    
     		System.out.println(c4.getClassLoader());   //引导类加载器
     		System.out.println(c3.getClassLoader());  //自定义的类加载器
     		
     	}
     }
     ```

   * 网络类加载器

     ```java
     /**
      * 网络自定义文件系统类加载器
      * @author zsk
      *
      */
     public class NetClassLoader extends ClassLoader {
     	
     	//cn.zsk.bean.User		-->  www.zsk.cn/myjava/    cn/zsk
     	private String rootUrl;
     	
     	public NetClassLoader(String rootUrl) {
     		this.rootUrl = rootUrl;
     	}
     	
     	@Override
     	protected Class<?> findClass(String name) throws ClassNotFoundException{
     		
     		Class<?> c = findLoadedClass(name);
     		
     		//应该先查询有没有加载过这个类，如果已经加载，则直接返回加载好的类。如果没有，则重新加载
     		if(c != null) {
     			return c;
     		}else {
     			ClassLoader parent = this.getParent();
     			
     			try {
     				c = parent.loadClass(name);  //委派给父类加载
     			}catch(Exception e) {
     //				e.printStackTrace();
     			}
     			
     			
     			if(c != null) {
     				return c;
     			}else {
     				byte[] classData = getClassData(name);
     				if(classData == null) {
     					throw new ClassNotFoundException();
     				}else {
     					c = defineClass(name, classData, 0, classData.length);
     				}
     			}
     		}
     		return c;
     	}
     	
     	private byte[] getClassData(String classname) {
     		String path = rootUrl + "/" + classname.replace('.', '/') + ".class";
     		
     		//IOUtils,可以使用它将流中的数据转成字节数组
     		InputStream is = null;
     		ByteArrayOutputStream baos = new ByteArrayOutputStream();
     		
     		try {
     			
     			URL url = new URL(path);
     			is = url.openStream();
     			
     			byte[] buffer = new byte[1024];
     			int temp = 0;
     			while((temp = is.read(buffer)) != -1) {
     				baos.write(buffer, 0, temp);
     				
     			}
     			
     			return baos.toByteArray();
     			
     		}catch(Exception e) {
     			e.printStackTrace();
     			return null;
     		}finally {
     			if(is != null) {
     				try {
     					is.close();
     				} catch (IOException e) {
     					e.printStackTrace();
     				}
     			}
     			if(baos != null) {
     				try {
     					baos.close();
     				} catch (IOException e) {
     					e.printStackTrace();
     				}
     			}
     		}
     	}
     	
     }
     ```

     