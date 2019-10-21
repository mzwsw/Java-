# 注解、反射、字节码、类加载机制

## 一、注解

1. **什么是注解**

   * Annotation的作用：
     * 不是程序本身，可以对程序作出解释。（这一点，跟注释没什么区别）
     * 可以被其他程序（比如：编译器等）读取。（注解信息处理流程，是注解和注释的重大区别。如果没有注解信息处理流程，则注解毫无意义）
   * Annotation的格式：
     * 注解是以“@注释名”在代码中存在的，还可以添加一些参数值，例如：@SuppressWarnings(value="unchecked")。
   * Annotation在哪里使用？
     * 可以附加在package，class，method，field等上面，相当于给它们添加了额外的辅助信息，我们可以通过反射机制编程实现对这些元数据的访问。

2. **内置注解（1）**

   * @Override

     ​		定义在java.lang.Override中，此注解只适用于修辞方法，表示一个方法声明打算重写超类中的另一个方法声明。

   * @Deprecated

     ​		定义在java.lang.Deprecated中，此注解可用于修辞方法、属性、类，表示不鼓励程序员使用这样的元素，通常是因为它很危险或存在更好的选择。

   * @SuppressWarnings

     ​		定义在java.lang.SuppressWarnings中，用来抑制编译时的警告信息

     ​		与之前两个注解不同，你需要添加一个参数才能正确使用，这些参数值都是定义好的，选择性使用。

     ​		deprecation		使用了过时的类或方法的警告

     ​		unchecked		执行了未检查的转换时的警告，如使用集合时未指定泛型

     ​		fallthrough		当在switch语句使用时发生case穿透

     ​		path		在类路径、源文件路径等中有不存在路径的警告

     ​		serial		当在可序列化的类上缺少serialVersionUID定义时的警告

     ​		finally		任何finally子句不能完成时的警告

     ​		all		关于以上所有情况的警告

     * @SuppressWarnings("unchecked")
     * @SuppressWarnings(value={"unchecked", "deprecation"})

3. **自定义注解**

   * 使用@interface自定义注解时，自动继承了java.lang.annotation.Annotation接口

   * 要点：

     ​		@interface用来声明一个注解

   * 格式为：

     ​		public @interface 注解名{定义体}

     ​		其中的每一个方法实际上是声明了一个配置参数。

     ​		方法的名称就是参数的名称

     ​		返回值类型就是参数类型（返回值类型只能是基本类型、Class、String、enum）

     ​		可以通过default来声明参数的默认值

     ​		如果只有一个参数成员，一般参数名为value

   * 注意：

     ​		注解元素必须要有值。我们定义注解元素时，经常使用空字符串、0作为默认值。

     ​		也经常使用负数（比如：-1）表示不存在的含义。

4. **元注解**

   * 元注解的作用就是负责注解其他的注解。Java定义了4个标准的meta-annotation类型，它们被用来提供对其他annotation类型作说明。

   * 这些类型和它们所支持的类在java.lang.annotation包中可以找到

     @Target		@Retention		@Documented		@Inherited

   * **@Target**

     * 用于描述注解的使用范围（即：被描述的注解可以用在什么地方）

       package（包）		PACKAGE

       类、接口、枚举、Annotation		TYPE

       类型成员（方法、构造方法、成员变量、枚举值）		CONSTRUCTOR:用于描述构造器

       ​																							FIELD：用于描述域

       ​																							METHOD：用于描述方法

       方法参数和本地变量		LOCAL_VARIABLE：用于描述局部变量

       ​											PARAMETER：用于描述参数

   * **@Retention**

     * 作用：

       ​		表示需要在什么级别保存该注解信息，用于描述注解的生命周期

       SOURCE		在源文件中有效（即源文件保留）

       CLASS		在class文件中有效（即class保留）

       RUNTIME		在运行时有效（即运行时保留），可以被反射机制读取
   
5. **注解作业**

   * 什么是ORM（Object Relationship Mapping）
     * 类和表结构对应
     * 属性和字段对应
     * 对象和记录对应
   * 使用注解完成类和表结构的映射广西
     * 学习了反射机制后，我们可以定义注解处理流程读取这些注解，实现更复杂的功能。

   ```java
   //类ZskStudent
   @ZskTable("tb_student")
   public class ZskStudent {
   	
   	@ZskField(columnName = "id", type = "int", length = 10)
   	private int id;
   	@ZskField(columnName = "sname", type = "varchar", length = 10)
   	private String studentName;
   	@ZskField(columnName = "age", type = "int", length = 3)
   	private int age;
   	
   	public int getId() {
   		return id;
   	}
   	public void setId(int id) {
   		this.id = id;
   	}
   	public String getStudentName() {
   		return studentName;
   	}
   	public void setStudentName(String studentName) {
   		this.studentName = studentName;
   	}
   	public int getAge() {
   		return age;
   	}
   	public void setAge(int age) {
   		this.age = age;
   	}	
   }
   
   //类注解
   @Target(value= {ElementType.TYPE})
   @Retention(RetentionPolicy.RUNTIME)
   public @interface ZskTable {
   	String value();
   }
   
   //属性注解
   @Target(value= {ElementType.FIELD})
   @Retention(RetentionPolicy.RUNTIME)
   public @interface ZskField {
   	String columnName();
   	String type();
   	int length();
   }
   
   //解析程序
   /**
    * 使用反射读取注解的信息，模拟处理注解信息的流程
    * @author zsk
    *
    */
   public class Demo {
   	public static void main(String[] args) {
   		try {
   			Class clazz = Class.forName("cn.zsk.test.annotation.ZskStudent");
   			
   			//获得类的所有有效注解
   			Annotation[] annotations = clazz.getAnnotations();
   			for(Annotation a : annotations) {
   				System.out.println(a);
   			}
   			
   			//获得类的指定注解
   			ZskTable zt = (ZskTable) clazz.getAnnotation(ZskTable.class);
   			System.out.println(zt.value());
   			
   			//获得类的属性的注解
   			Field f =  clazz.getDeclaredField("studentName");
   			ZskField zskField =  f.getAnnotation(ZskField.class);
   			System.out.println(zskField.columnName() + "----" + zskField.length());
   			
   			//根据获得的表明、字段的信息，拼出DDl语句，然后使用JDBC执行这个SQL，在数据库中生成相关的表
   		} catch (Exception e) {
   			e.printStackTrace();
   		}
   	}
   }
   ```

## 二、反射

1. **反射机制 reflection**

   ​		指的是可以用于运行时加载、探知、使用编译期间完全未知的类。

   ​		程序在运行状态中，可以动态加载一个只有名称的类，对于任意一个已加载的类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；

   	Class c = Class.forName("com.bjsxt.test.User");

   ​		加载完之后，在堆内存中，就产生了一个Class类型的对象（一个类只有一个Class对象），这个对象就包含了完整的类的结构信息。我们可以通过这个对象看到类的结构。这个对象就像一面镜子，透过这个镜子看到类的结构，所以，我们称之为：反射。

2. **Class类介绍**

   * java.lang.Class类十分特殊，用来表示java中类型（class/interface/enum/annotation/primitive type/void）本身。
     * Class类的对象包含了某个被加载类的结果。一个被加载的类对应一个Class对象。
     * 当一个class被加载，或当加载器（class loader）的defineClass()被JVM调用，JVM便自动生成一个Class对象。
   * Class类是Reflection的根源
     * 针对任何想动态加载、运行的类，唯有先获得相应的Class对象
   * **Class类的对象如何获取？**
     * 运用getClass()
     * 运用Class.forName()（最常用）
     * 运用.class语法

3. **反射机制的常见作用**

   * 动态加载类、动态获取类的信息（属性、方法、构造器）

     **示例：获取类的信息**

     ```java
     /**
      * 应用反射的API，获取类的信息（类的名字、属性、方法、构造器等）
      * @author zsk
      *
      */
     public class Demo {
     	public static void main(String[] args) {
     		String path = "cn.zsk.test.User";
     		
     		try {
     			Class clazz = Class.forName(path);
     			
     			//获取类的名字
     			System.out.println(clazz.getName());
     			System.out.println(clazz.getSimpleName());   //获得类名：User
     			
     			//获得属性信息
     			//Field[] fields = clazz.getFields();   //只能获得public修饰的field
     			Field[] fields = clazz.getDeclaredFields();  //获得所有的field
     			Field f = clazz.getDeclaredField("uname"); //获得指定属性
     			System.out.println(fields.length);  
     			System.out.println(f);
     			
     			//获得方法信息
     			Method[] methods = clazz.getDeclaredMethods();
     			Method m01 = clazz.getDeclaredMethod("getUname", null);
     			Method m02 = clazz.getDeclaredMethod("setUname", String.class); //如果方法有参，则必须传递参数类型
     			for(Method m : methods) {
     				System.out.println("方法："  + m);
     			}
     			
     			//获得构造器
     			Constructor[] constructors = clazz.getDeclaredConstructors();
     			Constructor c = clazz.getDeclaredConstructor(null); //获得指定构造器，此为无参构造器
     			System.out.println(c);
     			for(Constructor s : constructors) {
     				System.out.println("构造器：" + s);
     			}
     			
     		}catch(Exception e) {
     			e.printStackTrace();
     		}
     		
     	}
     }
     ```

   * 动态构造对象

   * 动态调用类和对象的任意方法、构造器

   * 动态调用和处理属性

     **示例：构造对象、动态调用属性和方法等**

     ```java
     /**
      * 通过反射API动态的操作：构造器、方法、属性
      * 
      * @author zsk
      *
      */
     public class Demo1 {
     	public static void main(String[] args) {
     String path = "cn.zsk.test.User";
     		
     		try {
     			Class<User> clazz = (Class<User>)Class.forName(path);
     			
     			//通过反射API动态调用构造方法，构造对象
     			User u = clazz.newInstance();  //其实是调用了User的无参构造方法
     			System.out.println(u);
     			
     			Constructor<User> c = clazz.getDeclaredConstructor(int.class, int.class, String.class);
     			User u2 = c.newInstance(1001, 18, "zsk");
     			System.out.println(u2.getAge());
     			
     			//通过反射API动态调用普通方法
     			User u3 = clazz.newInstance();
     			Method method = clazz.getDeclaredMethod("setUname", String.class);
     			method.invoke(u3, "zsk");    //u3.setUname("zsk");
     			System.out.println(u3.getUname());
     			
     			//通过反射API动态操作属性
     			User u4 = clazz.newInstance();
     			Field f = clazz.getDeclaredField("uname");
     			f.setAccessible(true);  //这个属性不需要做安全检查，直接访问
     			f.set(u4, "zsk");    //通过反射直接写属性
     			System.out.println(u4.getUname());
     			System.out.println(f.get(u4));   //通过反射直接读属性的值
     			
     			
     			
     		}catch(Exception e) {
     			e.printStackTrace();
     		}
     		
     	}
     }
     ```

   * 获取泛型信息

   * 处理注解

4. **反射机制性能问题**

   * setAccessible
     * 启用和禁用访问安全检查的开关，值为true则指示反射的对象在使用时应该取消Java语言访问检查。值为false则指示反射的对象应该实施Java语言访问检查。并不是为true就能访问为false不能访问。
     * 禁止安全检查，可以提高反射的运行速度。
   * 可以考虑使用：cglib/javaassist字节码操作

5. **反射操作泛型（Generic）**

   * Java采用泛型擦除的机制来引入泛型。Java中的泛型仅仅是给编译器javac使用的，**确保数据的安全性和免去强制类型转换的麻烦**。但是，一旦编译完成，所有的和泛型有关的类型全部擦除。
   * 为了通过**反射操作这些类型**以迎合实际开发的需要，Java新增了**ParameterizedType，GenericArrayType，TypeVariable和WildcardType**几种类型来**代表不能被归一到Class类中的类型但是又和原始类型齐名的类型。**

   ***

   * ParameterizedType：表示一种参数化的类型，比如Collection<String>
   * GenericArrayType：表示一种元素类型是参数化类型或者类型变量的数组类型
   * TypeVariable：是各种类型变量的公共父接口
   * WildcardType：代表一种通配符类型表达式，比如：?，? extends Number, ? super Integer [wildcard是一个单词：就是“通配符]

6. **反射操作注解（annotation）**

   * 可以通过反射API：getAnnotations，getAnnotation获得相关的注解信息

     ```java
     //获得类的所有有效注解
     Annotation[] annotations = clazz.getAnnotations();
     for(Annotation a : annotations){
         System.out.println(a);
     }
     //获得类的指定的注解
     ZskTable zt = (ZskTable)clazz.getAnnotation(ZskTable.class);
     System.out.println(zt.value());
     
     //获得类的属性的注解
     Field f = clazz.getDeclaredField("studentName");
     ZskField zskField = f.getAnnotation(ZSkField.class);
     System.out.println(zskField.columnName() + "---" + zskField.type() + "---" + zskField.length());
     ```


## 三、动态编译

1. **动态编译**

   * 动态编译的应用场景

     * 可以做一个浏览器端编写java代码，上传服务器编译和运行的在线评测系统。
     * 服务器动态加载某些类文件进行编译

   * 动态编译的两种做法

     * 通过Runtime调用javac，启动新的进程去操作

       Runtime run = Runtime.getRuntime();

       Process process = run.exec("javac -cp d:/myjava/ HelloWorld.java");

     * **通过JavaCompiler动态编译**

       ```java
       public static int compileFile(String sourceFile){
           //动态编译
           JavaCompiler compiler ToolProvider.getSystemJavaCompiler();
           int result = compiler.run(null, null, null, sourceFile);
           System.out.println(result == 0 ? "编译成功" : "编译失败");
           return result;
       }
       ```

       1. 第一个参数：为Java编译器提供参数
       2. 第二个参数：得到Java编译器的输出信息
       3. 第三个参数：接收编译器的错误信息
       4. 第四个参数：可变参数（是一个String数组）能传入一个或多个Java源文件
       5. 返回值：0便是编译成功，非0表示编译失败

2. **动态运行编译好的类**

   * 通过Runtime.getRuntime()运行启动新的进程运行

     ```java
     Runtime run = Runtime.getRuntime();
     Process process = run.exec("java -cp d:/myjava HelloWorld");
     //Process process = run.exec("java -cp" + dir + "" + classFile);
     ```

   * 通过反射运行编译好的类

     ```java
     //通过反射运行程序
     public static void runJavaClassByReflect(String dir, String classFile) throws Exception {
         try{
             URL[] urls = new URL[]{new URL("file:/" + dir)};
             URLClassLoader loader = new URLClassLoader(urls);
             Class c = loader.loadClass(classFile);
             //调用加载类的main方法
             c.getMethod("main", String[].class).invoke(null, (Object)new String[]{});
             
         }catch(Exception e){
             e.printStackTrace();
         }
     }
     ```

## 三、脚本引擎执行javascript代码

1. **脚本引擎**

   * 脚本引擎介绍

     * 使得Java应用程序可以通过一套固定的接口与各种脚本引擎交互，从而达到在Java平台上调用各种脚本语言的目的。
     * Java脚本API是联通Java平台和脚本语言的桥梁
     * 可以把一些复杂异变的业务逻辑交给脚本语言处理，这又大大提高了开发效率。

   * 获得脚本引擎对象

     ```java
     //获得脚本引擎
     ScriptEngineManager sem = new ScriptEngineManager();
     ScriptEngine engine = sem.getEngineByName("javascript");
     ```

   * Java脚本API为开发者提供了如下功能：
   
     * 获取脚本程序输入，通过脚本引擎运行脚本并返回运行结果，**这是最核心的接口**。
   
       ​		注意是：接口。Java可以使用各种不同的实现，从而通用的调用js、groovy、python等脚本。
   
       ​		Js使用了：Rhino
   
       ​		Rhino是一种使用Java语言编写的JavaScript的开源实现。
   
     * 通过**脚本引擎**的运行上下文在脚本和Java平台间交换数据
   
     * 通过Java应用程序调用脚本函数。
   
2. **Rhino介绍**

   Rhino是一种使用Java语言编写的JavaScript的开源实现，原先由Mozilla开发，现在被集成进入JDK6.0。

```java
/**
 * 测试脚本引擎执行javascript代码
 * @author zsk
 *
 */
public class Demo01 {
	public static void main(String[] args) throws Exception {
		
		//获得脚本引擎对象
		ScriptEngineManager sem = new ScriptEngineManager();
		ScriptEngine engine = sem.getEngineByName("javascript");
		
		//定义变量，存储到引擎上下文中
		engine.put("msg", "zsk is a good man");
		String str = "var user = {name: 'zsk',age:18,schools:['清华大学','南京邮电大学']};"; 
		str += "print(user.name);";
		
		//执行脚本
		engine.eval(str);
		engine.eval("msg = '南邮 is a good school';");
		System.out.println(engine.get("msg"));  //既可以被脚本语言获取，也可以被java获取
		
		System.out.println("-------------------------------------");
		
		
		//定义函数
		engine.eval("function add(a,b){var sum = a + b; return sum;}");
		//取得调用接口
		Invocable jsInvoke = (Invocable) engine;
		//执行脚本中定义的方法
		Object result1 = jsInvoke.invokeFunction("add", new Object[] {13, 20});
		System.out.println(result1);
		
		//导入其他java包，使用其他包中的java类
		String jsCode = "importPackage(java.util); var list = Arrays.asList([\"南京邮电大学\",\"清华大学\",\"北京大学\"])";
		engine.eval(jsCode);
		
		List<String> list2 = (List<String>)engine.get("list");
		for (String temp : list2) {
			System.out.println(temp);
		}
		
		//执行一个js文件（将a.js置于项目的src下即可
		URL url = Demo01.class.getClassLoader().getResource("a.js");
		FileReader fr = new FileReader(url.getPath());
		engine.eval(fr);
		fr.close();	
	}
}
```

## 四、JAVA字节码操作

1. **字节码操作**

   * JAVA动态性的两种常见实现方式：
     * 字节码操作
     * 反射
   * 运行时操作字节码可以让我们实现如下功能：
     * 动态生成新的类
     * 动态改变某个类的结构（添加/删除/修改    新的属性/方法）
   * 优势：
     * 比反射开销小，性能高
     * JAVAssist性能高于反射，低于ASM

2. **常见的字节码操作类库**

   * BCEL

     Byte Code Engineering Library(BCEL)

     BCEL在实际的JVM指令层次上进行操作（BCEL拥有丰富的JVM指令级支持）而Javassist所强调的是源代码级别的工作。

   * ASM

     是一个轻量级java字节码操作框架，直接涉及到JVM底层的操作和指令

   * CGLIB

     是一个强大的，高性能，高质量的Code生成类库，基于ASM实现。

   * Javassist

     是一个开源的分析、编辑和创建Java字节码的类库。性能较ASM差，但是使用简单。

3. **JAVAssist库的API详解**

   * javassist的最外层的API和JAVA的反射包中的API颇为类似
   * 它主要由CtClass，CtMethod，以及CrFiled几个类组成。用于执行和JDK反射API中java.lang.Class,java.lang.reflect.Method,java.lang.reflect.Field相同的操作。

4. **JAVAssist库的简单使用**

   * 创建一个全新的类
   * 使用XJAD反编译工具，将生成的class文件反编译成JAVA文件

   ```java
   /**
    * 测试使用javassist生成一个新的类
    * @author zsk
    *
    */
   public class Demo01 {
   	public static void main(String[] args) throws Exception {
   		
   		ClassPool pool = ClassPool.getDefault();
   		CtClass cc = pool.makeClass("cn.zsk.bean.Emp");
   		
   		//创建属性
   		CtField f1 = CtField.make("private int empno;", cc);
   		CtField f2 = CtField.make("private String ename;", cc);
   		cc.addField(f1);
   		cc.addField(f2);
   		
   		//创建方法
   		CtMethod m1 = CtMethod.make("public int getEmpno(){return empno;}", cc);
   		CtMethod m2 = CtMethod.make("public void setEmpno(int empno){this.empno = empno;}", cc);
   		cc.addMethod(m1);
   		cc.addMethod(m2);
   		
   		//添加构造器
   		CtConstructor constructor = new CtConstructor(new CtClass[] {CtClass.intType, pool.get("java.lang.String")}, cc);
   		constructor.setBody("{this.empno = empno; this.ename = ename;}");
   		cc.addConstructor(constructor);
   		cc.debugWriteFile("d:/myjava");
   		System.out.println("生成类成功！");
   	}
   }
   ```

5. **JAVAssist库的API详解**

   * 方法操作
     * 修改已有方法的方法体（插入代码到已有方法体）
     * 新增方法
     * 删除方法