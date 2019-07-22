# JAVA基础

1. 基本概念
      * JVM(Java Virtual Machine)就是一个虚拟的用于执行bytecode字节码的”虚拟计算机”。他也定义了指令集、寄存器集、结构栈、垃圾收集堆、内存区域。JVM负责将Java字节码解释运行，边解释边运行，这样，速度就会受到一定的影响
   * java Runtime Environment(JRE)包含：Java虚拟机、库函数、运行Java应用程序所必须的文件
   * Java Development Kit(JDK)包含：JRE，以及增加编译器和调试器等用于程序开发的文件
2. 简单桌球

```java
import java.awt.*;
import javax.swing.*;

public class BallGame extends JFrame {
	
	Image ball = Toolkit.getDefaultToolkit().getImage("images/ball.png");
	Image desk = Toolkit.getDefaultToolkit().getImage("images/desk.jpg");
	
	double x=100;  //小球的横坐标
	double y=100;  //小球的纵坐标
	
	boolean right = true;  //方向
	
	//画窗口的方法
	public void paint(Graphics g) {
		g.drawImage(desk, 0, 0, null);
		g.drawImage(ball, (int)x, (int)y, null);
		
		if (right) {
			x = x + 7;
		}else {
			x = x - 7;
		}
		
		if(x > 856-40-30) {    //856是窗口宽度，40是桌子边框的宽度，30是小球的直径
			right = false;
		}
		
		if(x < 40) {    //40是桌子边框的宽度
			right = true;
		}
		
	}
	
	//窗口加载
	void launchFrame() {
		setSize(856,500);
		setLocation(50,50);
		setVisible(true);
		
		
		//重画窗口，每秒25次
		while(true) {
			repaint();
			try {
				Thread.sleep(40);   //40ms
			}catch(Exception e) {
				e.printStackTrace();
			}
			
		}
			
	}

	//main方法是程序执行的入口
	public static void main(String[] args) {
		BallGame game = new BallGame();
		game.launchFrame();
	}

}

```

   

​    

​    

​    

​    

​    

​    


​    
​    
​    