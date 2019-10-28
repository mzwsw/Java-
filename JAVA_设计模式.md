# 设计模式

1. **创建型模式：**

   单例模式、工厂模式、抽象工厂模式、建造者模式、原型模式

2. **结构型模式：**

   适配器模式、桥接模式、装饰模式、组合模式、外观模式、享元模式、代理模式

3. **行为型模式：**

   模板方法模式、命令模式、迭代器模式、观察者模式、中介者模式、备忘录模式、解释器模式、状态模式、策略模式、职责链模式、访问者模式。

## 一、单例模式

1. 核心作用：

   保证一个类只有一个实例，并且提供一个访问该实例的全局访问点。

2. 常见应用场景：

   * Windows的Task Manager（任务管理器）就是很典型的单例模式
   * windows的Recycle Bin（回收站）也是典型的单利应用。在整个系统运行过程中，回收站一直维护着仅有的一个实例。
   * 项目中，读取配置文件的类，一般也只有一个对象。
   * 网站的计数器，一般也是采用单例模式实现，否则难以同步。

3. 单例模式的优点

   * 由于单例模式只生成一个实例，减少了系统性能开销，当一个对象的产生需要比较大的资源时，如读取配置、产生其他依赖对象时，则可以通过在应用启动时直接产生一个单例对象，然后永久驻留内存的方式来解决。
   * 单例模式可以在系统设置全局的访问点，优化环共享资源访问，例如可以设计一个单例类，负责所有数据表的映射处理。

4. 常见的五中单例模式实现方式：

   * 主要：
     * 饿汉式（线程安全，调用效率高。但是，不能延时加载）
     * 懒汉式（线程安全，调用效率不高。但是，可以延时加载）
   * 其他：
     * 双重检测锁式（由于JVM底层内部模型原因，偶尔会出问题。不建议使用）
     * 静态内部类式（线程安全，调用效率高。但是，可以延时加载）
     * 枚举单利（线程安全，调用效率高，不能延时加载）

5. 饿汉式实现（单例对象立即加载）

   ```java
   public class SingletonDemo02{
       private static /*final*/ SingletonDemo02 s = new SingletonDemo02();
       
       private SingletonDemo02(){}  //私有化构造器
       
       private static /*synchronized*/ SingletonDemo02 getInstance(){
           return s;
       }
   }
   
   public class Client{
       public static void main(String[] args){
           SingletonDemo02 s = SingletonDemo02.getInstance();
           SingletonDemo02 s2 = SingletonDemo02.getInstance();
           System.out.println(s == s2);  //输出结果为true
       }
   }
   ```

   * 饿汉式单例模式代码中，static变量会在类加载时初始化，此时也不涉及多个线程对象访问该对象的问题。虚拟机保证只会装载一次该类，肯定不会发生并发访问的问题。因此，可以省略synchronized关键字
   * 问题：如果只是加载本类，而不是要调用getInstance（），甚至永远没有调用，则会造成资源浪费！

6. 懒汉式实现（单例对象延迟加载）

   ```java
   public class SingletonDemo01{
       private static  SingletonDemo01 s;
       
       private SingletonDemo01(){}  //私有化构造器
       
       public static synchronized SingletonDemo01 getInstance(){
           if(s == null){
               s = new SingletonDemo01();
           }
           return s;
       }
   }
   ```

   * 要点：

     ​		lazy load！		延迟加载，懒加载！真正用的时候才加载！

   * 问题：

     资源利用率高了。但是每次调用getInstance()方法都有同步，并发效率较低。

7. 双重检测锁实现

   * 这个模式将同步内容下放到if内部，提高了执行的效率，不必每次获取对象时都要进行同步，只有第一次才同步。
   * 问题：**由于编译器优化原因和JVM底层内部模型原因，偶尔会出问题。不建议使用。**

8. 静态内部类实现方式（也是一种懒加载方式）

   ```java
   public class SingletonDemo04{
       private static class SingletonClassInstance{
           private static final SingletonDemo04 instance = new SingletonDemo04();
       }
       public static SingletonDemo04 getInstance(){
           return SingletonClassIntance.instance;
       }
       private SingletonDemo04(){
       }
   }
   ```

   * 要点：
     * 外部类没有static属性，则不会像饿汉式那样立即加载对象。
     * 只有真正调用getInstance()，才会加载静态内部类。加载类时是线程安全的。instance是static final类型，保证了内存中只有这样一个实例存在，而且只能被赋值一次，从而保证了线程安全性。
     * 兼备了并发高效调用和延迟加载的优势！

9. **存在的问题**

   * 反射可以破解上面几种（不包含枚举式）实现方式！

     （可以在构造方法中手动抛出异常控制）

     ```java
     public class SingletonDemo01 {
         private static SingletonDemo01 s;
         private SingletonDemo01() throws Exception {
             if(s!=null){
                 throw new Exception("只能创建一个对象");
                 //通过手动抛出异常，避免反射跳过单例模式创建多个对象！
             }
         }//私有化构造器
     }
     ```

     

   * 反序列化可以破解上面几种（不包含枚举式）实现方式！

     * 可以通过定义readResolve()防止或者不同对象。
       
       * 反序列化时，如果对象所在类定义了readResolve()，（实际是一种回调），定义返回哪个对象。
       
         ```java
         private Object readResolve() throws ObjectStreamException {
             return s;
         }
         ```

10. 使用枚举实现单例模式

    ```java
    public enum SingletonDemo05{
        /**
         * 定义一个枚举的元素，它就代表了Singleton的一个实例。
         */
        INSTANCE;
        /**
         * 单例可以有自己的操作
         */
        public void singletonOperation(){
            //功能处理
        }
    }
    ```

    * 优点：
      * 实现简单
      * 枚举本身就是单例模式。由JVM从根本上提供保障！避免通过反射和反序列化的漏洞！
    * 缺点：
      * 无延时加载
    
11. **如何选用？**

    * 单例对象	占用资源少	不需要 延时加载
      * 枚举式	好于	饿汉式
    * 单例对象    占用资源大    需要 延时加载
      * 静态内部类式	好于	懒汉式

## 二、工厂模式

1. 工厂模式：

   * 实现了创建者和调用者的分离。
   * 详细分类：简单工厂模式；工厂方法模式；抽象工厂模式

2. 面向对象设计的基本原则：

   * OCP（开闭原则，Open-Closed Principle）：一个软件的实体应对对扩展开放，对修改关闭。
   * DIP（依赖倒转原则，Dependence Inversion Principle）：要针对接口编程，不要针对实现编程。
   * LoD（迪米特法则，Law of Demeter）：只与你直接的朋友通信，而避免和陌生人通信。

3. 核心本质：

   * 实例化对象，用工厂方法代替new操作。
   * 将选择实现类、创建对象统一管理和控制。从而将调用者跟我们的实现类解耦。

4. 工厂模式分类：

   * 简单工厂模式：用来生成同一等级结构中的任意产品。（对于增加新的产品，需要修改已有代码）
   * 工厂方法模式：用来生成同一等级结构中的固定产品（支持增加任意产品）
   * 抽象工厂模式：用来生成不同产品族的全部产品。（对于增加新的产品，无能为力；可以增加产品族）

5. 举例：不使用简单工厂模式的情况

   ```java
   public class Client01{  //调用者
       public static void main(String[] args){
           Car c1 = new Audi();
           Car c2 = new Byd();
           
           c1.run();
           c2.run();
       }
   }
   ```

6. **简单工厂模式**

   * 要点：

     * 简单工厂模式也叫静态工厂模式，就是工厂类一般使用静态方法，通过接收的参数的不同来返回不同的对象实例。
     * 对于增加新产品无能为力！不能修改代码的话，是无法扩展的。

     ```java
     public class CarFactory{
         public static Car createCar(String type){
             Car c = null;
             if("奥迪".equals(type)){
                 c = new Audi();
             }else if("比亚迪".equals(type)){
                 c = new Byd();
             }
             return c;
         }
     }
     
     
     public class CarFactory{
         public static Car creatAudi(){
             return new Audi();
         }
         public static Car creatByd(){
             return new Byd();
         }
     }
     ```

7. **工厂方法模式**

   * 要点：

     * 为了避免简单工厂模式的缺点，不完全满足OCP。
     * 工厂方法模式和简单工厂模式最大的不同在于，简单工厂模式只有一个（对于一个项目或者一个独立模块而言）工厂类，而工厂方法模式有一组实现了相同接口的工厂类。

     ```java
     //工厂接口
     public interface CarFactory{
         Car createCar();
     }
     
     //通过接口实现具体工厂类
     public class AudiFactory implements CarFactory{
         @Override
         public Car createCar(){
             return new Audi();
         }
     }
     public class BydFactory implements CarFactory{
         @Override
         public Car createCar(){
             return new Byd();
         }
     }
     //可以随意扩展添加具体工厂类
     public class BenzFactory implements CarFactory{
         ......
     }
     
     //调用
     public class Client{
         public static void main(String[] args){
             Car c1 = new AudiFactory().createCar();
             c1.run();
             
             Car c2 = new BydFactory().createCar();
             c2.run();
         }
     }
     ```

8. 简单工厂模式和工厂方法模式PK：

   * 结构复杂度

     简单工厂模式占优。简单工厂只有一个工厂类。工厂方法模式的工厂类随着产品类个数增加而增加。

   * 代码复杂度

     代码复杂度和结构复杂度是一对矛盾。

   * 客户端编程难度

     简单工厂占优

   * **根据设计理念建议：工厂方法模式。但实际上，一般都用简单工厂模式。**

9. **抽象工厂模式**

   