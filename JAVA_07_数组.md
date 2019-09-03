# 七、数组

1. **概述和特点**

   * 定义

     ​    	数组是相同类型数据的有序集合。数组描述的是相同类型的若干个数据，按照一定的先后次序排列组合而成。其中，每一个数据称为一个元素，每个元素可以通过一个索引（下标）来访问它们。数组的三个特点：
     
     		* 长度是确定的。数组一旦被创建，它的大小就是不可改变的。
     		* 元素必须是相同类型，不允许出现混合类型。
     		* 数组类型可以是任何数据类型，包括基本类型和引用类型。
     
   * 建议：
   
     ​		数组变量属引用类型，数组也是对象，数组中的每个元素相当于该对象的成员变量。Java中对象是在堆中的，因此数组无论保存原始类型还是其他对象类型，数组对象本身是在堆中保存的。
   
2. **声明和初始化**

   * 声明

     数组的声明有两种方式

     ```java
     type[] arr_name; //推荐使用这种
     type arr_name[];
     ```

     **注意事项**

     		* 声明的时候并没有实例化任何对象，只有在实例化数组对象时，JVM才分配空间，这是才与长度有关。
     		* 声明一个数组的时候并没有数组被真正创建。
     		* 构造一个数组，必须制定长度。

     ```java
     public class TestArray{
         public static void main(String args[]){
             int[] s = null; //声明数组；
             s = new int[10];  //给数组分配空间
             for(int i = 0; i < 10; i++){
                 s[i] = 2*i+1; //给数组元素赋值
                 System.out.println(s[i]);
             }
         }
     }
     ```

     ![基本类型数组内存分配](https://www.sxt.cn/360shop/Public/admin/UEditor/20170522/1495418560857133.png)

     ```java
     class Man{
         private int age;
         private int id;
         public Man(int id,int age) {
             super();
             this.age = age;
             this.id = id;
         }
     }
     public class AppMain {
         public static void main(String[] args) {
             Man[] mans;  //声明引用类型数组； 
             mans = new Man[10];  //给引用类型数组分配空间；
              
             Man m1 = new Man(1,11);
             Man m2 = new Man(2,22);  
              
             mans[0]=m1;//给引用类型数组元素赋值；
             mans[1]=m2;//给引用类型数组元素赋值；
         }
     }
     ```

     ![引用类型数组内存分配图](https://www.sxt.cn/360shop/Public/admin/UEditor/20170522/1495418626975934.png)

   * 初始化

     ​		数组的初始化有三种：静态初始化、动态初始化、默认初始化。

     ```java
     /**
      * 测试数组的三种初始化方式
      * @author zsk
      *
      */
     
     public class Test02 {
     	public static void main(String[] args) {
     		int[] a = {2,4,5};   //静态初始化
     		
     		int[] b = new int[3];   //默认给数组的元素赋值。赋值规则和成员变量默认赋值规则一样
     		
     		int[] c = new int[2];
     		c[1] = 1;
     		c[2] = 2;    //动态初始化
     		
     	}
     }
     ```

3. **数组的遍历、循环、拷贝等**

   * 数组的遍历

     ​		数组元素下标的合法区间：[0,length-1]。可以通过下标来遍历数组中的元素。for循环

   * for-each循环（只读不能修改）

     ​		增强for循环for-each专门用于**读取数组或集合**中所有的元素，即对数组进行遍历。

     ```java
     public class Test {
         public static void main(String[] args) {
             String[] ss = { "aa", "bbb", "ccc", "ddd" }; //静态初始化
             for (String temp : ss) {   //foreach循环，只能读取，不能修改
                 System.out.println(temp);
             }
         }
     }
     ```

   * 拷贝
   
     ​		System类里包含了一个Static void arraycopy(object src, int srcpos, object dest, int destpos, int length)方法，该方法可以将src数组里的元素赋值给dest数组，其中srcpos指定从src数组的某一个索引位置开始，length指定将src数组的多少个元素赋值给dest数组。
   
     ```java
     /**
      * 测试数组拷贝
      * @author zsk
      *
      */
     public class TestArrayCopy {
     	public static void main(String[] args) {
     //		testBasicCopy();
     		String[] s = {"aa", "bb", "ee"};
     		String[] d = { "cc", "dd"};
     		String[] ss = insertElement(s, 2, d);
     		for(String temp : ss) {
     			System.out.println(temp);
     		}
     //		String[] s1 = removeElement(s, 2);
     //		for(String temp:s1) {
     //			System.out.println(temp);
     //		}
     //		extendRange();
     	}
     	
     	public static void  testBasicCopy() {
     		String[] s1 = {"aa", "bb", "cc", "dd", "ee"};
     		String[] s2 = new String[10];
     		System.arraycopy(s1, 2, s2, 6, 3);
     		
     		for(int i = 0; i < s2.length; i++) {
     			System.out.println(i + "--" +s2[i]);
     		}
     	}
     	
     	//测试从数组中删除某个元素（本质上还是数组的拷贝）
     	public static String[] removeElement(String[] s, int index) {
     		System.arraycopy(s, index+1, s, index, s.length-index-1);
     		s[s.length-1] = null;		
     		return s;
     	}
     	
     	//数组的扩容（本质上：先定义一个更大的数组，然后将原数组内容原封不动拷贝到新数组中）
     	public static void extendRange() {
     		String[] s1 = {"aa", "bb", "cc"};
     		String[] s2 = new String[s1.length + 10];
     		System.arraycopy(s1, 0, s2, 0, s1.length);
     		for(String temp:s2) {
     			System.out.println(temp);
     		}
     	}
     	
     	//数组的插入（相互拷贝）
     	public static String[] insertElement(String[] s,int index, String[] d) {
     		String[] ss = new String[s.length+d.length];
     		System.arraycopy(s, 0, ss, 0, s.length);
     		System.arraycopy(ss, index, ss, index+d.length, s.length-index);
     		System.arraycopy(d, 0, ss, index, d.length);
     		return ss;
     	}
     }
     ```
   
   * java.util.Arrays类
   
     ​		包含了常用的数组操作：排序、查找、填充、打印等。
   
     * 打印
   
       ```java
       Arrays.toString(a)
       ```
   
     * 排序
   
       ```
       Arrays.sort(a)  //从小到大排序
       ```
   
     * 二分查找
   
       ```
       Arrays.binarySearch(String[] a, key)
       ```
   
     * 填充
   
       ```
       Arrays.fill(String[] a,index1,index2,key)//将数组a中索引[index，index2）的元素替换为key
       ```
   
     ```java
     /**
      * 测试java.util.Arrays类
      * @author lenovo
      *
      */
     public class TestArrays {
     	public static void main(String[] args) {
     		int[] a = {100, 20 ,3, 50, 200, 150};
     		System.out.println(Arrays.toString(a));
     		Arrays.sort(a);
     		System.out.println(Arrays.toString(a));
     		System.out.println("二分查找："+ Arrays.binarySearch(a, 3));
     	}
     	
     }
     ```
   
4. **多维数组**

   ​		多维数组可以看成以数组为元素的数组。
   
   声明
   
   ```java
   //Java中多维数组的声明和初始化应该按从低维到高维的顺序进行
   int[][] a = new int[3][];
   a[0] = new int[2];
   a[1] = new int[4];
   a[2] = new int[3];
   ```
   
   静态初始化
   
   ```java
   int[][] a = {{1,2,3},{3,4},{3,4,5,6}};
   ```
   
5. **冒泡排序**

   ​		冒泡算法重复的走过要排序的数列，一次比较两个元素，如果他们的顺序错误就把他们交换过来，这样越大的元素会经过交换慢慢“浮”到数列的顶端。

   ```java
   /**
    * 测试冒泡排序及优化算法
    * @author zzssk
    *
    */
   public class TestBubbleSort {
   	public static void main(String[] args) {
   		int[] values = { 3, 1, 6, 2, 9, 0, 7, 4, 5, 8 };
   		int temp = 0;
   		
   		for(int i = 0; i < values.length-1; i++) {
   			boolean flag = true;
   			for(int j = 0; j < values.length-i-1; j++) {
   				if(values[j] > values[j+1]) {
   					temp = values[j];
   					values[j] = values[j+1];
   					values[j+1] = temp;
   					flag = false;
   				}
   				System.out.println(Arrays.toString(values));
   			}
   			if(flag) {
   				System.out.println("结束！！！");
   				break;
   			}
   			System.out.println("#############");
   		}
   	}
   }
   ```

6. **二分法查找**

   ​		二分法检索（binary search）又称折半检索，基本思想是设数组中的元素从小到大有序地排放在数组中，首先将给定key值与数组中间位置上元素的关键码（key）比较，如果相等，则检索成功， 否则，若key小，则再数组前半部分中继续进行二分法检索；若key大，则在数组后半部分中继续进行二分法检索。

   ```java
   /**
    * 测试二分检索
    * @author zsk
    *
    */
   public class TestBinarySearch {
   	public static void main(String[] args) {
   		int[] arr = { 30,20,50,10,80,9,7,12,100,40,8};
           int searchWord = 10; // 所要查找的数
           Arrays.sort(arr); //二分法查找之前，一定要对数组元素排序
           System.out.println(Arrays.toString(arr));
           System.out.println(searchWord+"元素的索引："+binarySearch(arr,searchWord));
   	}
   	
   	public static int binarySearch(int[] arr, int value) {
   		int low = 0;
   		int high = arr.length-1;
   		
   		while(low <= high) {
   			int mid = (low + high)/2;
   			if (value == arr[mid]) {
   				return mid;
   			}
   			if(value < arr[mid]) {
   				high = mid-1;
   			}
   			if(value > arr[mid]) {
   				low = mid+1;
   			}
   		}
   		return -1;
   	}
   }
   ```

   