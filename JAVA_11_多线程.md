# 第十一章 多线程

1. **基本概念**

   * **程序**

     “程序（Program）”是一个静态的概念，一般对应于操作系统中的一个可执行文件。

   * **进程**

     ​		执行中的程序叫做进行（Process），是一个动态的概念。现代操作系统都可以同时启动多个进程。进程具有以下特点：

     1. 进程是程序的一次动态执行过程，占用特定的地址空间。
     2. 每个进程由3部分组成：cpu、data、code。每个进程都是独立的，保有自己的cpu时间，代码和数据，即便是同一份程序产生好几个进程，它们之间还是拥有自己的这3样。缺点是：浪费内存，cpu的负担较重。
     3. 多任务（Multitasking）操作系统将cpu时间动态划分给每个进程，操作系统同时执行多个进程，每个进程独立运行。以进程的观点来看，它会认为自己独占cpu的使用权。

   * **线程**

     ​		一个进程可以产生多个线程。同多个进程可以共享操作系统的某些资源一样，同一进程的多个线程也可以共享此进程的某些资源（比如：代码、数据），所以线程又被称为轻量级进程（lightweight process）。

     1. 一个进程内部的一个执行单元，它是程序中的一个单一的顺序控制流程。
     2. 一个进程可拥有多个并行的（concurrent）线程。
     3. 一个进程中的多个线程共享相同的内存单元/内存地址空间，可以访问相同的变量和对象，而且它们从同一堆中分配对象并进行通信、数据交换和同步操作。
     4. 由于线程间的通信是在同一地址空间上进行的，所以不需要额外的通信机制，使得通信更简便而且信息传递的速度也更快。
     5. 线程的启动、终端、消亡，消耗的资源非常少。

   * **线程和进程的区别**

     1.  每个进程都有独立的代码和数据空间(进程上下文)，进程间的切换会有较大的开销。 
     2.  线程可以看成是轻量级的进程，属于同一进程的线程共享代码和数据空间，每个线程有独立的运行栈     和程序计数器(PC)，线程切换的开销小。 
     3.  **线程和进程最根本的区别在于：进程是资源分配的单位，线程是调度和执行的单位。**
     4.  多进程: 在操作系统中能同时运行多个任务(程序)。 
     5.  多线程: 在同一应用程序中有多个顺序流同时执行。 
     6.  线程是进程的一部分，所以线程有的时候被称为轻量级进程。 
     7.  一个没有线程的进程是可以被看作单线程的，如果一个进程内拥有多个线程，进程的执行过程不是一条线(线程)的，而是多条线(线程)共同完成的。 
     8.  系统在运行的时候会为每个进程分配不同的内存区域，但是不会为线程分配内存(线程所使用的资源是它所属的进程的资源)，线程组只能共享资源。那就是说，除了CPU之外(线程在运行的时候要占用CPU资源)，计算机内部的软硬件资源的分配与线程无关，线程只能共享它所属进程的资源。 

   * **进程与程序的区别**

     ​		程序是一组指令的集合，它是静态的实体，没有执行的含义。而进程是一个动态的实体，有自己的生命周期。一般说来，一个进程肯定与一个程序相对应，并且只有一个，但是一个程序可以有多个进程，或者一个进程都没有。除此之外，进程还有并发性和交往性。简单说，**进程是程序的一部分，程序运行的时候会产生进程。**

2. **Java中如何实现多线程**

   * **通过继承Thread类实现多线程**

     继承Thread类实现多线程的步骤：

     1. 在Java中负责实现线程功能的类是java.lang.Thread类。
     2. 可以通过创建Thread的实例来创建新的线程。
     3. 每个线程都是通过某个特定的Thread对象所对应的方法run()来完成其操作的，方法run()成为线程体。
     4. 通过调用Thread类的start()方法来启动一个线程。

     ```java
     public class TestThread extends Thread{ //自定义类继承Thread类
         //run()方法是线程体
         public void run(){
             for(int i = 0; i < 10; i++){
                 System.out.println(this.getName() + ":" + i); //getName()方法返回线程名称
             }
         }
         
         public static void main(String[] args){
             TestThread thread1 = new TestThread(); //创建线程对象
             thread1.start();
             TestThread thread2 = new TestThread();
             thread2.start();
         }
     }
     ```

     **此种方法的缺点：**如果我们的类已经继承了一个类（如小程序必须继承自Applet类），则无法再继承Thread类。

   * **通过Runnable接口实现多线程**

     ​		在开发中，应用更多的是通过Runnable接口实现多线程。即在实现Runnable接口的同时还可以继承某个类。

     ```java
     public calss TestThread2 implements Runnable { //自定义类实现Runnable接口
         //run()方法里是线程体
         public void run(){
             for(int i = 0; i < 10; i++){
                 System.out.println(Thread.currentThread().getName() + ":" +i);
             }
         }
         
         public static void main(String[] args){
             //创建线程对象，把实现了Runnable接口的对象作为参数传入；
             Thread thread1 = new Thread(new TestThread2());
             thread1.start(); //启动线程
             Thread thread2 = new Thread(new TestThread2());
             thread2.start();
         }
     }
     ```

3. **线程**

   * **线程状态**

     一个线程对象在它的生命周期内，需要经历5个状态。

     <img src="https://www.sxt.cn/360shop/Public/admin/UEditor/20170526/1495787690411518.png" alt="线程生命周期图"  />

     * **新生状态（New）**

       ​		用new关键字建立一个线程对象后，该线程对象就处于新生状态。处于新生状态的线程有自己的内存空间，通过调用start方法进入就绪状态。

     * **就绪状态（Runnable）**

       ​		处于就绪状态的线程已经具备了运行条件，但是还没有被分配到cpu，处于“线程就绪队列”，等待系统为其分配CPU。就绪状态并不是执行状态，当系统选定一个等待执行的Thread对象后，它就会进入执行状态。一旦获得CPU，线程就进入运行状态并自动调用自己的run方法。

       ​		有四种原因会导致线程进入就绪状态：

       1. 新建线程：调用start方法，进入就绪状态；
       2. 阻塞线程：阻塞解除，进入就绪状态；
       3. 运行线程：调用yield()方法，直接进入就绪状态;
       4. 运行线程：JVM将CPU资源从本线程切换到其他线程。

     * **运行状态（Running）**

       ​		在运行状态的线程执行自己run方法中的代码，直到调用其他方法而终止或等待某资源而阻塞或完成任务而死亡。如果在给定的时间片内没有执行结束，就会被系统给换下来回到就绪状态。也可能由某些“导致阻塞的事件”而进入阻塞状态。

     * **阻塞状态（Blocked）**

       ​		阻塞指的是暂停一个线程的执行以等待某个条件发生（如某资源就绪）。有四种原因会导致阻塞：

       1. 执行sleep(int millsecond)方法，使当前线程休眠，进入阻塞状态。当指定的时间到了后，线程进入就绪状态。
       2. 执行wait()方法，使当前线程进入阻塞状态。当使用nofity()方法唤醒这个线程后，它进入就绪状态。
       3. 线程运行时，某个操作进入阻塞状态，比如执行IO流操作（read()/write()方法本身就是阻塞的方法）。只有当引起该操作阻塞的原因消失后，线程进入就绪状态。
       4. join()线程联合：当某个线程等待另一个线程执行结束后，才能继续执行时，使用join()方法。

     * **死亡状态（Terminated）**

       ​		死亡状态是线程生命周期中的最后一个阶段。线程死亡的原因有两个。一个是正常运行的线程完成了它run()方法内的全部工作；另一个是线程被强制终止，如通过执行stop()或destroy()方法来终止一个线程（注：stop()/destory()方法已经被JDK废弃，不推荐使用）。

       ​		当一个线程进入死亡状态以后，就不能再回到其他状态了。

   * **终止线程的典型方式**

     ​		终止线程一般不使用JDK提供的stop()/destroy()方法。通常做法是提供一个boolean型的终止变量，当这个变量置为false，则终止线程的运行。

     **例：终止线程（重要）**

     ```java
     public class TestThreadCiycle implements Runnable{
         String name;
         boolean live = true; //标记变量，表示线程是否可终止；
         public TestThreadCiycle(String name){
             super();
             this.name = name;
         }
         public void run(){
             int i = 0;
             //当live的值时true时，继续执行线程体；false则结束循环，终止线程体。
             while(live){
                 System.out.println(name + (i++));
             }
         }
         public void terminate(){
             live = false;
         }
         
         public static void main(String[] args){
             TestThreadCiycle ttc = new TestThreadCiycle("线程A:");
             Thread t1 = new Thread(tcc);//新生状态
             t1.start(); //就绪状态
             for(int i = 0; i < 100; i++){
                 System.out.println("主线程" + i);
             }
             ttc.terminate();
             System.out.println("ttc stop!");
         }
     }
     ```

   * **暂停线程执行sleep/yield**

     ​		暂停线程执行常用的方法有**sleep()和yield()**方法，两个方法的区别是：

     		1. sleep()方法：可以让正在运行的线程进入阻塞状态，直到休眠时间满了，进入就绪状态。
       		2. yield()方法：可以让正在运行的状态直接进入就绪状态，让出CPU的使用权。

     **暂停线程的方法--sleep()**

     ```java
     public class TestThreadState {
         public static void main(String[] args){
             StateThread thread1 = new StateThread();
             thread1.start();
             StateThread thread2 = new StateThread();
             thread2.start();
         }
     }
     //使用继承方式实现多线程
     class StateThread extends Thread{
         public void run(){
             for(int i = 0; i < 100; i++){
                 System.out.println(this.getName() + ":" + i);
                 try{
                     Thread.sleep(2000);//调用线程的sleep()方法；
                 }catch(InterruptedException e){
                     e.printStackTrace();
                 }
             }
         }
     }
     ```

      **暂停线程的方法--yield()**

     ```java
     public class TestThreadState{
         public static void main(String[] args){
             StateThread thread1 = new StateThread();
             thread1.start();
             StateThread thread2 = new StateThread();
             thread2.start();
         }
     }
     //使用继承方法实现多线程
     class StateThread extends Thread{
         public void run(){
             for(int i = 0; i < 100; i++){
                 System.out.println(this.getName() + ":" + i);
                 Thread.yield(); //调用线程的 yield（）方法；
             }
         }
     }
     ```

   * **线程的联合join()**

     ​		线程A在运行期间，可以调用线程B的join()方法，让线程A和线程B联合。这样，线程A就必须等待线程B执行完毕后，才能继续执行。

     **示例**

     ```java
     public class TestThreadState{
         public static void main(String[] args){
             System.out.println("爸爸和儿子买烟故事");
             Thread father = new Thread(new FatherThread());
             father.start();
         }
     }
     
     class FatherThread implements Runnable{
         public void run(){
             System.out.println("爸爸想抽烟，发现烟抽完了");
             System.out.println("爸爸让儿子去买烟");
             Thread son = new Thread(new SonThread());
             son.start();
             System.out.println("爸爸等儿子买烟回来");
             try{
                 son.join();
             }catch(InterruptedException e){
                 e.printStackTrace();
                 System.out.println("爸爸出门找儿子了");
                 //结束JVM。如果是0则表示正常结束；如果非0，则表示非正常结束
                 System.exit(1);
             }
             System.out.println("爸爸高兴的开始抽烟。");
         }
     }
     
     class SonThread implements Runnable{
         public void run(){
             System.out.println("儿子出门买烟");
             System.out.println("儿子买烟需要10分钟");
             try{
                 for(int i = 0; i <= 10; i++){
                     System.out.println("第" + i + "分钟");
                     Thread.sleep(1000);
                 }
             }catch(InterruptedException e){
                 e.printStackTrace();
             }
             System.out.println("儿子买烟回来了");
         }
     }
     ```

4. **线程操作**

   * **获取线程基本信息的方法**

     isAlive()		判断线程是否还“活着”，即线程是否还未终止；

     getPriority()		获得线程的优先级数值；

     setPriority()		设置线程的优先级数值；

     setName()		给线程一个名字；

     getName()		获得线程的名字

     currentThread()		获取当前正在运行的线程对象，也就是获取自己本身

     **线程的常用方法一**

     ```java
     public class TestThread{
         public static void main(String[] args){
             Runnable r = new MyThread();
             Thread t = new Thread(r,"Name test"); //定义线程对象，并传入参数；
             t.start(); //启动线程
             System.out.println("name is:" + t.getName()); //输出线程名字；
             Thread.currentThread().sleep(5000); //线程暂停5分钟；
             System.out.println(t.isAlive()); //判断线程是否还在运行；
             System.out.println("over!");
         }
     }
     class MyThread implements Runnable{
         //线程体；
         public void run(){
             for(int i = 0; i < 10; i++){
                 System.out.println(i);
             }
         }
     }
     ```

   * **线程的优先级**

     1. 处于就绪状态的线程，会进入“就绪队列”等待JVM来挑选。

     2. 线程的优先级用数字表示，范围从1到10，一个线程的缺省优先级是5。

     3. 使用下列方法或者或设置线程对象的优先级。

        int getPriority();

        void setPriority(int newPriority);

     ​        注意：优先级低只是意味着获得调度的概率低。并不是绝对先调用优先级高的线程后调用优先级低的线程。

5. **线程同步**

   * **线程同步的概念**

     ​		处理多线程问题时，多个线程访问同一个对象，并且某些线程还想修改这个对象。这时候，我们就需要用到“线程同步”。线程同步其实就是一种等待机制，多个需要同时访问此对象的线程进入这个对象的等待池形成队列，等待前面的线程使用完毕后，下一个线程再使用。