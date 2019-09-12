# 九、容器

1. **泛型（Generics）**

   ![容器的接口层次结构图](https://www.sxt.cn/360shop/Public/admin/UEditor/20170524/1495613220648265.png)

   ​		泛型可以帮助我们建立类型安全的容器。在使用了泛型的容器中，遍历时不必进行强制类型转换。

   ​		泛型的本质就是“数据类型的参数化”。我们可以把“泛型”理解为数据类型的一个占位符（形式参数），即告诉编译器，在调用泛型时必须传入实际类型。

   * **自定义泛型**

     ​		可以在类的声明处增加泛型列表，如：<T,E,V>。

     ​		此处，字符可以试任何标识符，一般采用这3个字母。

     声明：

     ```java
     class MyCollection<E>{
         Object[] objs = new Object[5];
         
         public E get(int index){
             return (E)objs[index];
         }
         public void set(E e, int index){
             objs[index] = e;
         }
     }
     ```

     ​		泛型E像一个占位符一样表示“未知的某个数据类型”，我们在真正调用的时候传入这个“数据类型”。

     应用：

     ```java
     public class TestGenerics{
         public static void main(String[] args){
             //这里的“String”就是实际传入的数据类型；
             MyCollection<String> mc = new MyCollection<String>();
             mc.set("aaa",0);
             mc.set("bbb",1);
             String str = mc.get(1);  //加了泛型，直接返回String类型，不用强制转换。
             System.out.println(str);
         }
     }
     ```

   * **容器中使用泛型**

     ​		容器相关类都定义了泛型，使用容器类时都要使用泛型。这样，在容器的存储数据、读取数据时都避免了大量的类型判断，非常便捷。

     ```java
     public class Test{
         public static void main(String[] args){
             List<String> list new ArrayList<String>();
             Set<Man> mans = new HashSet<Man>();
             Map<Integer,Man> maps = new HashMap<Integer,Man>();
             Iterator<Man> iterator = man.iterator();
         }
     }
     ```

     **雷区**

     ​		只是强烈建议使用泛型。事实上，不使用编译器也不会报错！

2. **Collection接口**

   ​		Collection表示一组对象，它是集中、收集的意思。Collection接口的两个子接口是List、Set接口。

   ​					boolean add(Object element)		增加元素到容器中

   ​					boolean remove(Object element)		从容器中移除元素

   ​					boolean contains(Object element)		容器中是否包含该元素

   ​					int size()		容器中元素的数量

   ​					boolean isEmpty()		容器是否为空

   ​					void clear()		清空容器中所有元素

   ​					Iterator iterator()		获得迭代器，用于遍历所有元素

   ​					boolean containsAll(Collection c)		本容器是否包含c容器中的所有元素

   ​					boolean addAll(Collection c)		将容器c中所有元素增加到本容器

   ​					boolean removeAll(Collection c)		移除本容器和容器c中都包含的元素

   ​					boolean retainAll(Collection c)		取本容器和容器c中都包含的元素，移除非交集元素

   ​					Object[] toArray()		转化成Object数组

   ​		由于List、Set是Collection的子接口，意味着所有List、Set的实现类都有上面的方法。	

3. **List**

   * **List特点和常用方法**

     List是有序、可重复的容器。

     **有序**：List中每个元素都有索引标记。可以根据元素的索引标记（在List中的位置）访问元素，从而精确控			制这些元素。

     **可重复**：List允许加入重复的元素。List通常允许满足e1.equals(e2)的元素重复加入容器。

     除了Collection接口中的方法，List多了一些跟顺序（索引）有关的方法，如：

     ​		add(int index, Object element); set(int index, Object element); get(int index); remove(int index)

     List接口常用的实现类有3个：ArrayList、LinkedList和Vector
     
     ```java
     /**
      * 测试ArrayList的索引方法
      * @author zsk
      *
      */
     public class TestArrayList {
     	public static void main(String[] args) {
     		test02();
     	}
     	
     	public static void test02() {
     		List<String> list = new ArrayList<>();
     		list.add("A");
     		list.add("B");
     		list.add("C");
     		list.add("D");
     		System.out.println(list);
     		list.add(2, "zsk");
     		System.out.println(list);
     		list.remove(2);
     		System.out.println(list);
     		list.set(2, "c");
     		System.out.println(list);
     		System.out.println(list.get(1));
     		list.add("B");
     		System.out.println(list);
     		System.out.println(list.indexOf("B"));
     		System.out.println(list.lastIndexOf("B"));
     	}
     }
     ```
     
     **手工实现ArrayList底层原理**
     
     ​		ArrayList底层是用数组实现的存储。特点：查询效率高，增删效率低，线程不安全。
     
     ```java
     /**
      * 自定义一个ArrayList，体会底层原理
      * 增加泛型
      * 增加数组扩容
      * 增加set和get方法
      * 增加数组边界检查
      * 增加remove方法
      * @author zsk
      *
      */
     public class ZskArrayList <E>{
     	
     	private Object[] elementData;
     	private int size;
     	
     	private static final int DEFALT_CAPACITY = 10;
     	
     	public ZskArrayList() {
     		elementData = new Object[DEFALT_CAPACITY];
     	
     	}
     	
     	public ZskArrayList(int capacity) {
     		if(capacity < 0) {
     			throw new RuntimeException("容器容量不能为负！");
     		}else if(capacity == 0){
     			elementData = new Object[DEFALT_CAPACITY];
     		}else {
     			elementData = new Object[capacity];
     		}
     		
     	}
     	
     	@Override
     	public String toString() {
     		
     		StringBuilder sb = new StringBuilder();
     		sb.append("[");
     		for(int i = 0; i < size; i++) {
     			sb.append(elementData[i] + ",");
     		}
     		sb.setCharAt(sb.length() - 1, ']');
     		return sb.toString();
     		
     	}
     	
     	public void add(E e) {
     		//什么时候扩容
     		if(size == elementData.length) {
     			//怎么扩容
     			Object[] newArray = new Object[elementData.length + (elementData.length>>1)];
     			System.arraycopy(elementData, 0, newArray, 0, elementData.length);
     			elementData = newArray;
     		}
     		elementData[size++] = e;
     	}
     	
     	public E  get(int index) {
     		checkRange(index);
     		return (E) elementData[index];
     	}
     	
     	public void set(E e, int index) {
     		checkRange(index);
     		elementData[index] = e;
     	}
     	
     	public void checkRange(int index) {
     		//索引合法判断[0,size)
     		if(index < 0 || index > size - 1) {
     			//不合法
     			throw new RuntimeException("索引不合法：" + index);
     		}
     	}
     	
     	public void remove(int index) {
     		int numMoved = elementData.length - index - 1;
     		if(numMoved > 0) {
     			System.arraycopy(elementData, index+1, elementData, index, numMoved);
     		}
     		elementData[size - 1] = null;
     		size--;
     		
     	}
     	
     	public void remove(E e) {
     		//e 将它和所有元素挨个比较，获得第一个比较为true的
     		for(int i = 0; i < size; i++) {
     			if(e.equals(get(i))) {
     				remove(i);
     			}
     		}
     	}
     	
     	public static void main(String[] args) {
     		ZskArrayList<String> s1 = new ZskArrayList<>(20);
     		
     		for(int i = 0; i < 40; i++) {
     			s1.add("zsk" + i);
     		}
     		
     		System.out.println(s1.get(10));
     		System.out.println(s1);
     		s1.remove(5);
     		s1.remove("zsk10");
     		System.out.println(s1);
         }
     }
     ```
     
   * **LinkedList特点和底层实现**
   
     ​		LinkedList底层用双向链表实现的存储。特点：查询效率低，增删效率高，线程不安全。
   
     ​		双向链表也叫双链表，是链表的一种，它的每个数据节点中都有两个指针，分别指向前一个节点和后一个节点。所以，从链表中的任何一个节点开始，都可以很方便地找到所有节点。
   
     ```java
     class Node{
         Node previous;  //前一个节点
         Object element;  //节点的数据
         Node next;  //后一个节点
     }
     ```
   
     ​		**自定义实现LinkedList**
   
     ```java
     /**
      * 测试自写LinkedList,链表
      * 增加get方法
      * 增加remove方法
      * 增加插入节点的方法
      * 增加小封装，增加泛型
      * @author zsk
      *
      */
     public class ZskLinkedList<E> {
     	
     	private Node first;
     	private Node last;
     	private int size;
     	
     	public void add(int index, E e) {
     		checkRange(index);
     		
     		Node newNode = new Node(e);
     		Node temp = getNode(index);
     		
     		if(temp!=null) {
     			Node up = temp.previous;
     			
     			up.next = newNode;
     			newNode.previous = up;
     			
     			newNode.next = temp;
     			temp.previous = newNode;
     			
     		}
     		
     	}
     	
     	public void remove(int index) {
     		checkRange(index);
     		Node temp = getNode(index);
     		
     		if(temp != null) {
     			Node up = temp.previous;
     			Node down = temp.next;
     			
     			if(up != null) {
     				up.next = down;
     			}
     			
     			if(down != null) {
     				down.previous = up;
     			}
     			
     			if(index == 0) {
     				first = down;
     			}
     			
     			if(index == size-1) {
     				last = up;
     			}
     			
     			size--;
     		}
     		
     	}
     	
     	public E get(int index) {
     		checkRange(index);
     
     		Node temp = null;
     		temp = getNode(index);
     		return (E) temp.element;
     	}
     	
     	private void checkRange(int index) {
     		if(index <0 || index > size-1) {
     			throw new RuntimeException("索引不合法！");
     		}
     	}
     	
     	private Node getNode(int index) {
     		Node temp = null;
     		
     		if(index <=(size>>1)) {
     			temp = first;
     			for(int i = 0; i < index; i++) {
     				temp = temp.next;
     			}
     		}else {
     			temp = last;
     			for(int i = size-1; i > index; i--) {
     				temp = temp.previous;
     			}
     		}
     		return temp;
     	}
     	
     	public void add(E e) {
     		
     		Node node = new Node(e);
     		
     		if(first == null) {
     			first = node;
     			last = node;
     		}else {
     			node.previous = last;
     			node.next = null;
     			
     			last.next = node;
     			last = node;
     		}
     		size++;
     	}
     	
     	@Override
     		public String toString() {
     			StringBuilder sb = new StringBuilder("[");
     			
     			Node temp = first;
     			while(temp != null) {
     				sb.append(temp.element + ",");
     				temp = temp.next;
     			}
     			sb.setCharAt(sb.length()-1, ']');
     			return sb.toString();
     		}
     	
     	public static void main(String[] args) {
     		ZskLinkedList<String> list = new ZskLinkedList<>();
     		
     		list.add("a");
     		list.add("b");
     		list.add("c");
     		list.add("d");
     		list.add("e");
     		list.add("f");
     		
     		System.out.println(list);
     		
     		list.add(3, "zsk");
     		System.out.println(list);
     	}
     }
     ```
   
   * **Vector向量**
   
     ​		Vector底层是用数组实现的List，相关的方法都加了同步检查，因此“线程安全，效率低”。
   
     如何选用ArrayList、LinkedList、Vector？
   
     		* 需要线程安全时，用Vector。
     		* 不存在线程安全问题时，并且查找较多用ArrayList（一般用它）。
     		* 不存在线程安全问题时，增加或删除元素较多用LinkedList。