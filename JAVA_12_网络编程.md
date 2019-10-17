# 第十二章、网络编程

1. **基本概念**

   ​		计算机网络实现了不同计算机之间的通信，这必须依靠编写网络程序来实现。

   * **什么是计算机网络**

     ​		计算机网络是指将地理位置不同的具有独立功能的多台计算机及其外部设备，通过通信线路连接起来，在网络操作系统，网络管理软件及网络通信协议的管理和协调下，实现资源共享和信息传递的计算机系统。

     1. 计算机网络的作用：资源共享和信息传递
     2. 计算机网络的组成：
        * 计算机硬件：计算机（大中小型服务器，台式机、笔记本等）、外部设备、通信线路。
        * 计算机软件：网络操作系统（windows2000 Server、Unix、Linux等）、网络管理软件(WorkWin、SugarNMS等)、网络通信协议（如TCP/IP协议栈等）。
     3. 计算机网络的多台计算机是具有独立功能的，而不是脱离网络就无法存在的。

   * **什么是网络通信协议？**

     ​		通过计算机网络可以实现不同计算机之间的连接与通信，但是计算机网络中实现通信必须有一些约定即通信协议，对速率、传输代码、代码结构、传输控制步骤、出错控制等制定标准。

     ​		OSI（Open System Interconnect，即开放系统互联）模型。OSI模型制定的七层标准模型，分别是：**应用层，表示层，会话层，传输层，网络层，数据链路层，物理层。**

     ​		![七层协议模型](https://www.sxt.cn/360shop/Public/admin/UEditor/20170528/1495936432794161.png)

     ​		而TCP/IP是一个协议族，也是按照层次划分，共四层：应用层，传输层，互连网络层，网络接口层（物理+数据链路层）。TCP/IP协议参考了OSI模型。

   * **网络协议的分层**

     ​		由于网络结点之间联系很复杂，在制定协议时，把复杂成份分解成一些简单的成份，再将它们复合起来。最常用的复合方式是层次方式，即同层间可以通信、上一层可以调用下一层，而与再下一层不发生关系。

     ​		把用户应用程序作为最高层，把物理通信线路作为最低层，将其他的协议处理分为若干层，规定每层处理的任务，也规定每层的接口标准。

     ​		开放系统互连参考模型与TCP/IP参考模型对比：

     ​												![](https://www.sxt.cn/360shop/Public/admin/UEditor/20170528/1495936545384683.png)

   * **数据封装与解封**

     ​		由于用户传输的数据一般都比较大，一次性发送十分困难，于是就需要把数据分成许多片段，再按照一定的次序发送出去。这个过程就需要对数据进行封装。

     ​		数据封装（Data Encapsulation）是指将协议数据单元（PDU）封装在一组协议头和协议尾中的过程。在OSI七层参考模型中，每层主要负责与其他机器上的对等层进行通信。该过程是在协议数据单元（PDU）中实现的，其中每层的PDU一般由本层的协议头、协议尾和数据封装构成。

       1. 数据发送处理过程

          (1)、应用层将数据交给传输层，传输层添加上TCP的控制信息（成为TCP头部），这个数据单元成为段（Segment），加入控制信息的过程叫做封装。然后将段交给网络层。

          (2)、网络层接收到段，再添加上IP头部，这个数据单元成为包（Packet）。然后，将包交给数据链路层。

          (3)、数据链路层接收到包，再添加上MAC头部和尾部，这个数据单元成为帧（Frame）。然后，将帧交给物理层。

          (4)、物理层将接收到的数据转化为比特流，然后在网线中传送。

     		2. 数据接收处理过程

          (1)、物理层接收到比特流，经过处理后将数据交给数据链路层。

          (2)、数据链路层将接收到的数据转化为数据帧，再除去MAC头部和尾部，这个除去控制信息的过程称为解封，然后将包交给网络层。

          (3)、网络层接收到包，再除去IP头部，然后将段交给传输层。

          (4)、传输层接收到段，再除去TCP头部，然后将数据交给应用层。

        从以上传输过程中，可以总结出以下规则：

     ​		(1)、发送方数据处理的方式是从高层到底层，逐层进行数据封装。

     ​		(2)、接收方数据处理的方式是从底层到高层，逐层进行数据解封装。

   * **IP地址**

     ​		用来标识网络中的一个通信实体的地址。通信实体可以是计算机、路由器等。

     ​		目前主流使用的IP地址是IPV4，但是随着网络规模的不断扩大，IPV4面临着枯竭的危险，所有推出了IPV6.

     ​		IPV4:32位地址，并以8位为一个单位，分成四部分，以点分十进制表示，如192.168.0.1。因为8位二进制的计数范围是00000000--11111111，对应十进制的0-255，所以-4.278.4.1是错误的IPV4地址。

     ​		IPV6:128位（16字节）写成8个16位的无符号整数，每个整数用四个十六进制位表示，每个数之间用冒号分开，如：3ffe:3201:1401:1280:c8ff:fe4d:db39:1984

     **注意事项**

     		1. 127.0.0.1	本机地址

       		2. 192.168.0.0-192.168.255.255为私有地址，属于非注册地址，专门为组织机构内部使用。

   * **端口**

     ​		IP地址用来标识一台计算机，但是一台计算机上可能提供多种网络应用程序，如何来区分这些不同的程序？就要用到端口。

     ​		端口虚拟的概念，并不是说在主机上真的有若干个端口。通过端口，可以在一个主机上运行多个网络应用程序。端口的表示是一个16位的 二进制整数，对应十进制的0-65535.

     ​		Oracle、MySQL、Tomcat、QQ、msn、迅雷、电驴等网络程序都有自己的端口。

   * **URL**

     ​		在www上，每一个信息资源都有统一且唯一的地址，该地址就叫URL（Uniform Resource Locator），它是www的统一资源定位符。URL由4部分组成：协议、存放资源的主机域名、资源文件名和端口号。如果未指定该端口号，则使用协议默认的端口。例如http协议的默认端口为80。

     ​		在java.net包中提供了URL类，该类封装了大量复杂的涉及从远程站点获取信息的细节。

   * **Socket**

     ​		开发的网络应用程序位于应用层，TCP和UDP属于传输层协议，在应用层和传输层之间，使用套接Socket来进行分离。

     ​		Socket实际是传输层供给应用层的编程接口。Socket就是应用层与传输层之间的桥梁。使用Socket编程可以开发客户机和服务器应用程序，可以在本地网络上进行通信，也可通过Internet在全球范围内通信。

2. **TCP和UDP**

   * **两者的联系和区别**

     ​		TCP协议和UDP协议时传输层的两种协议。Socket是传输层供给应用层的编程接口，所以Socket编程就分为TCP编程和UDP编程两类。

     ​		在网络通讯中，TCP方式就类似于拨打电话，使用该种方式进行网络通讯时，需要建立专门的虚拟连接，然后进行可靠的数据传输，如果数据发送失败，则客户端会自动重发该数据。而UDP方式就类似于发送短息你，使用这种方式进行网络通讯时，不需要建立专门的虚拟连接，传输也不是很可靠，如果发送失败则客户端无法获得。

     ​		 这两种传输方式都在实际的网络编程中使用，重要的数据一般使用TCP方式进行数据传输，而大量的非核心数据则可以通过UDP方式进行传递，在一些程序中甚至结合使用这两种方式进行数据传递。 

     ​		 由于TCP需要建立专用的虚拟连接以及确认传输是否正确，所以使用TCP方式的速度稍微慢一些，而且传输时产生的数据量要比UDP稍微大一些。 

     **总结**

     1. TCP是面向连接的，传输数据安全，稳定，效率相对较低
     2. UDP是面向无连接的，传输数据不安全，效率极高。

   * **TCP协议**

     ​		TCP（Transfer Control Protocol）是面向连接的，所谓面向连接，就是当计算机双方通信时，必须经过先建立连接，然后传送数据，最后拆除连接三个过程。

     **TCP咋建立连接时又分三步走：**

     ​		第一步，是请求端（客户端）发送一个包含SYN即同步(Synchronize)标志的TCP报文，SYN同步报文会指明客户端使用的端口以及TCP连接的初始序号。

     ​		第二步，服务器在接收到客户端的SYN报文后，将返回一个SYN+ACK的报文，表示客户端的请求被接受，同时TCP序号被加一，ACK即确认（Acknowledgement）。

     ​		第三步，客户端也返回一个确认报文ACK给服务器端，同样TCP序列号被加一，到此一个TCP连接完成。然后才开始通信的第二步：数据处理。

     ​		这就是TCP的三次握手（Three-way Handshake）。

   * **UDP协议**

     ​		基于TCP协议可以建立稳定连接的点对点的通信。这种通信方式实时、快速、安全性高，但是很占用系统的资源。

     ​		在网络传输方式上，还有另一种基于UDP协议的通信方式，成为数据报通信方式。在这种方式中，每个数据发送单元被统一封装成数据报包的方式，发送发将数据报包发送到网络中，数据报包在网络中去寻找它的目的地。

3. **Java网络编程**

   ​		Java为了可移植性，不允许直接调用操作系统，而是由java.net包来提供网络功能。Java虚拟机负责提供与操作系统的实际连接。下面介绍常用类：

   * **InetAddress**

     **作用：**封装计算机的IP地址和DNS（没有端口信息）。

     ​			注：DNS是Domain Name System，域名系统。

     **特点：**这个类没有构造方法。如果要得到对象，只能通过静态方法：getLocalHost()、getByName()、getAllByName()、getAddress()、getHostName()。

     **示例：使用getLocalHost方法创建InetAddress对象**

     ```java
     import java.net.InetAddress;
     import java.net.UnknownHostException;
     public class Test1{
         public static void main(String[] args) throws UnknownHostException{
             InetAddress addr = InetAddress.getLocalHost();
             //返回IP地址：192.168.1.110
             System.out.println(addr.getHostAddress());
             //输出计算机名
             System.out.println(addr.getHostName());
         }
     }
     ```

     **示例：根据域名得到InetAddress对象**

     ```java
     import java.net.InetAddress;
     import java.net.UnknownHostException;
     public class Test2{
         public static void main(String[] args) throws UnknownHostException {
             InetAddress addr = InetAddress.getByName("www.sxt.cn");
             //返回sxt服务器的IP
             System.out.println(addr.getHostAddress());
             //输出：www.sxt.cn
             System.out.println(addr.getHostName());
         }
     }
     ```

     **示例：根据IP得到InetAddress对象**

     ```java
     import java.net.InetAddress;
     import java.net.UnknownHostException;
     public class Test3{
         public static void main(String[] args) throws UnknownHostException {
             InetAddress addr = InetAddress.getByName("59.110.14.7");
             //返回sxt服务器的IP
             System.out.println(addr.getHostAddress());
             /*
              *输出ip而不是域名。如果这个ip地址不存在或DNS服务器不允许进行IP地址和域名
              *的映射，getHostName方法就直接返回这个IP地址。
              */
             System.out.println(addr.getHostName());
         }
     }
     ```

   * **InetSocketAddress**
   
     **作用：**包含IP和端口信息，常用于Socket通信。此类实现IP套接字地址（IP地址+端口号），不依赖任何协议。
   
     **示例：InetSocketAddress的使用**
   
     ```java
     import java.net.InetSocketAddress;
     public class Test4{
         public static void main(String[] args){
             InetSocketAddress socketAddress = new InetSocketAddress("127.0.0.1",8080);
             InetSocketAddress socketAddress2 = new InetSocketAddress("localhost",9090);
             System.out.println(socketAddress.getHosetName());
             System.out.println(socketAddress2.getAddress());
         }
     }
     ```
   
   * **URL类**
   
     ​		IP地址唯一标识了Internet上的计算机，而URL则标识了这些计算机上的资源。类URL代表一个统一资源定位符，它是指向互联网“资源”的指针。资源可以是简单的文件或目录，也可以是对更复杂对象的引用，例如对数据库或搜索引擎的查询。
   
     ​		为了方便程序员编程，JDK中提供了URL类，该类的全名是java.net.URL，有了这样一个类，就可以使用它的各种方法来对URL对象进行分割、合并等处理。
   
     **示例：URL类的使用**
   
     ```java
     import java.net.MalformedURLException;
     import java.net.URL;
     public class Test5{
         public static void main(String[] args) throws MalformedURLException{
             URL u = new URL("http://www.google.cn:80/webhp#aa?canhu=33");
             System.out.println("获取与此url关联的协议的默认端口：" + u.getDefaultPort());
             System.out.println("getFile:" + u.getFile()); //端口号后面的内容
             System.out.println("主机名：" + u.getHost());  //www.google.cn
             System.out.println("路径：" + u.getPath()); //端口号后，参数前的内容
             //如果www.google.cn:80 则返回80。否则返回-1
             System.out.println("端口：" + u.getPort());
             System.out.println("协议：" + u.getProtocol());
             System.out.println("参数部分：" + u.getQuery());
             System.out.println("锚点：" + u.getRef());
             
             URL u1 = new URL("http://www.abc/com/aa/");
             URL u2 = new URL(u,"2.html"); //相对路径构建url对象
             System.out.println(u2.toString()); //http://www.abc.com/aa/2.thml
         }
     }
     ```
   
     **最简单的网络爬虫**
   
     ```java
     import java.io.BufferedReader;
     import java.io.IOException;
     import java.io.InputStream;
     import java.io.InputStreamReader;
     import java.net.MalformedURLException;
     import java.net.URL;
     
     public class TestSpider {
     	public static void main(String[] args) {
     		
     	}
     	
     	//网络爬虫
     	static void basicSpider() {
     		URL url = null;
     		InputStream is = null;
     		BufferedReader br = null;
     		StringBuilder sb = new StringBuilder();
     		String temp = "";
     		try {
     			url = new URL("http://www.baidu.com");
     			is = url.openStream();
     			br = new BufferedReader(new InputStreamReader(is));
     			/*
     			 * 这样就可以将网络内容下载到本地机器。
     			 * 然后进行数据分析，建立索引。这也是搜索引擎的第一步。
     			 */
     			while((temp = br.readLine()) != null) {
     				sb.append(temp);
     			}
     			System.out.println(sb);
     		}catch(MalformedURLException e) {
     			e.printStackTrace();
     		}catch(IOException e) {
     			e.printStackTrace();
     		}finally {
     			try {
     				br.close();
     			}catch(IOException e){
     				e.printStackTrace();
     			}
     			try {
     				is.close();
     			}catch(IOException e) {
     				e.printStackTrace();
     			}
     		}
     	}
     }
     ```
   
   * **基于TCP协议的Socket编程和通信**
   
     ​		在网络通讯中，第一次主动发起通讯的程序被称为客户端（Client）程序，简称客户端，而在第一次通讯中等待连接的程序被称作服务器端（Server）程序，简称服务器。一旦通讯建立，则客户端和服务器端完全一样，没有本质区别。
   
     **"请求-响应"模式**
   
     1. Socket类：发送TCP消息。
     2. ServerSocket类：创建服务器
   
     ***
   
     ​		套接字是一种进程间的数据交换机制。这些进程既可以在同一机器上，也可以在通过网络连接的不同机器上。套接字起到通信端点的作用。单个套接字是一个端点，而一对套接字则构成一个双向通信信道，使非关联进程可以在本地或通过网络进行数据交换。一旦建立套接字连接，数据即可在相同或不同的系统中双向或单向发送，直到其中一个端点关闭连接。套接字与主机地址和端口地址相关联。主机地址就是客户端或服务器程序所在的主机的IP地址。端口地址是指客户端或服务器程序使用的主机的通信端口。
   
     ​		在客户端和服务器中，分别创建独立的Socket，并通过Socket的属性，将两个Socket进行连接，这样，客户端和服务器通过套接字所建立的连接使用输入输出流进行通信。
   
     ​		TCP/IP套接字是最可靠的双向流协议，使用TCP/IP可以发送任意数量的数据。
   
     ​		套接字只是计算机上已编号的端口。如果发送方和接收方计算机确定好端口，他们就可以通信了。
   
     ​		客户端与服务器端的通信关系图
   
     ​																	![](https://www.sxt.cn/360shop/Public/admin/UEditor/20170528/1495940019482522.png)
   
     **TCP/IP通信连接的简单过程：**
   
     ​		位于A计算机上的TCP/IP软件向B计算机发送包含端口号的消息，B计算机的TCP/IP软件接收该消息，并进行检查，查看是否有它知道的程序正在该端口上接收消息。如果有，他就将该消息交给这个程序。
   
     ​		要使程序有效地运行，就必须有一个客户端和一个服务器端。
   
     **通过Socket的编程顺序**
   
     1. 创建服务器ServerSocket，在创建时，定义ServerSocket的监听端口（在这个端口接收客户端发来的消息）。
     2. ServerSocket调用accept()方法，使之处于阻塞状态。
     3. 创建客户端Socket，并设置服务器的IP及端口。
     4. 客户端发出连接请求，建立连接。
     5. 分别取得服务器和客户端Socket的InputStream和OutputStream。
     6. 利用Socket和ServerSocket进行数据传输。
     7. 关闭流及Socket。
   
     **示例：TCP：单向通信Socket之服务器端**
   
     ```java
     import java.io.BufferedWriter;
     import java.io.IOException;
     import java.io.OutputStreamWriter;
     import java.net.ServerSocket;
     import java.net.Socket;
     
     public class BasicSocketServer {
     	public static void main(String[] args) {
     		Socket socket = null;
     		BufferedWriter bw = null;
     		try {
     			//建立服务器端套接字：指定监听的接口
     			ServerSocket serverSocket = new ServerSocket(8888);
     			System.out.println("服务端建立监听");
     			//监听，等待客户端请求，并愿意建立连接
     			socket = serverSocket.accept();
     			//获取socket的输出流，并使用缓冲流进行包装
     			bw = new BufferedWriter(new
     															OutputStreamWriter(socket.getOutputStream()));
     			//向客户端发送反馈信息
     			bw.write("hhhh");
     		}catch(IOException e) {
     			e.printStackTrace();
     		}finally {
     			//关闭流及socket连接
     			if(bw != null) {
     				try {
     					bw.close();
     				}catch(IOException e) {
     					e.printStackTrace();
     				}
     			}
     			if(socket != null) {
     				try {
     					socket.close();
     				}catch(IOException e) {
     					e.printStackTrace();
     				}
     			}
     		}
     	}
     }
     ```
   
     **TCP:单向通信Socket之客户端**
   
     ```java
     public class BasicSocketClient {
     	public static void main(String[] args) {
     		Socket socket = null;
     		BufferedReader br = null;
     		try {
     			/*
     			 * 创建Socket对象：指定要连接的服务器的IP和端口而不是自己机器的端口
     			 * 发送端口是随机的。
     			 */
     			socket = new Socket(InetAddress.getLocalHost(),8888);
     			//获取socket的输入流，并使用缓冲流进行包装
     			br = new BufferedReader(new
     														InputStreamReader(socket.getInputStream()));
     			//接收服务器发送的消息
     			System.out.println(br.readLine());
     		}catch(Exception e) {
     			e.printStackTrace();
     		}finally {
     			//关闭流及socket连接
     			if(br != null) {
     				try {
     					br.close();
     				}catch(IOException e) {
     					e.printStackTrace();
     				}
     			}
     			if(socket != null) {
     				try {
     					socket.close();
     				}catch(IOException e) {
     					e.printStackTrace();
     				}
     			}
     		}
     	}
     }
     ```
   
     **TCP:双向通信Socket之服务器端**
   
     ```java
     public class Double_Server {
     	public static void main(String[] args) {
     		Socket socket = null;
     		BufferedReader in = null;
     		BufferedWriter out = null;
     		BufferedReader br = null;
     		try {
     			//建立服务器端套接字：指定监听的接口
     			ServerSocket server = new ServerSocket(8888);
     			//监听客户端的连接
     			socket = server.accept();
     			//获取socket的输入输出流和发送消息
     			in = new BufferedReader(new
     															InputStreamReader(socket.getInputStream()));
     			out = new BufferedWriter(new
     															OutputStreamWriter(socket.getOutputStream()));
     			br = new BufferedReader(new InputStreamReader(System.in));
     			while(true) {
     				//接收客户端发送的消息
     				String str = in.readLine();
     				System.out.println("客户端说：" + str);
     				String str2 = "";
     				//如果客户端发送的是“end”则终止连接
     				if(str.equals("end")) {
     					break;
     				}
     				//否则，发送反馈消息
     				str2 = br.readLine(); //读到\n为止，因此一定要输入换行符
     				out.write(str2 + "\n");
     				out.flush();
     			}
     		}catch(IOException e) {
     			e.printStackTrace();
     		}finally {
     			//关闭资源
     			if(in != null) {
     				try {
     					in.close();
     				}catch(IOException e) {
     					e.printStackTrace();
     				}
     			}
     			if(out != null) {
     				try {
     					out.close();
     				}catch(IOException e) {
     					e.printStackTrace();
     				}
     			}
     			if(br != null) {
     				try {
     					br.close();
     				}catch(IOException e) {
     					e.printStackTrace();
     				}
     			}
     			if(socket != null) {
     				try {
     					socket.close();
     				}catch(IOException e) {
     					e.printStackTrace();
     				}
     			}
     		}
     	}
     }
     ```
   
     **TCP:双向通信Socket之客户端**
   
     ```java
     public class Double_Client {
     	public static void main(String[] args) {
     		Socket socket = null;
     		BufferedReader in = null;
     		BufferedWriter out = null;
     		BufferedReader wt = null;
     		try {
     			//创建Socket对象，指定服务器端的IP与端口
     			socket = new Socket(InetAddress.getLocalHost(),8888);
     			//获取socket的输入输出流和发送消息
     			in = new BufferedReader(new
     															InputStreamReader(socket.getInputStream()));
     			out = new BufferedWriter(new
     															OutputStreamWriter(socket.getOutputStream()));
     			wt = new BufferedReader(new InputStreamReader(System.in));
     			while(true) {
     				//接收客户端发送的消息
     				String str = wt.readLine();
     				out.write(str + "\n");
     				out.flush();
     				//如果输入的信息是“end”则终止连接
     				if(str.equals("end")) {
     					break;
     				}
     				//否则，接收并输出服务器端信息
     				System.out.println("服务器端说：" + in.readLine());
     			}
     		}catch(UnknownHostException e) {
     			e.printStackTrace();
     		}catch(IOException e) {
     			e.printStackTrace();
     		}finally {
     			//关闭资源
     			if(in != null) {
     				try {
     					in.close();
     				}catch(IOException e) {
     					e.printStackTrace();
     				}
     			}
     			if(out != null) {
     				try {
     					out.close();
     				}catch(IOException e) {
     					e.printStackTrace();
     				}
     			}
     			if(wt != null) {
     				try {
     					wt.close();
     				}catch(IOException e) {
     					e.printStackTrace();
     				}
     			}
     			if(socket != null) {
     				try {
     					socket.close();
     				}catch(IOException e) {
     					e.printStackTrace();
     				}
     			}
     		}
     	}
     }
     ```
   
     **菜鸟雷区**
   
     ​		运行时，要先启动服务器端，再启动客户端，才能得到正常的运行效果。
   
     ​		但是，上面这个程序，必须按照安排好的顺序，服务器和客户端一问一答！不够灵活！！可以使用多线程更加灵活的双向通讯！！
   
     ​		**服务器端**：一个线程专门发送消息，一个线程专门接收消息。
   
     ​		**客户端：**一个线程专门发送消息，一个线程专门接收消息。
   
     