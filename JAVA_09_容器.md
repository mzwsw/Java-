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
   
     * 需要线程安全时，用vector。
     * 不存在线程安全问题时，并且查找较多用ArrayList（一般用它）。
     * 不存在线程安全问题时，增加或删除元素较多用LinkedList。
   
4. **Map接口**

   ​		Map就是用来存储“键(key)-值(value)对”的。Map类中存储的“键值对”通过键来标识，所以“键对象”不能重复。

   ​		Map接口的实现类有HashMap、TreeMap、HashTable、Properties等。

   **常用方法**

   ​				Object put(Object key,Object value)		存放键值对

   ​				Object get(Object key)		通过键对象查找得到值对象

   ​				Object remove(Object key)		删除键对象对应的键值对

   ​				boolean containsKey(Object key)		Map容器中是否包含键对象对应的键值对

   ​				boolean containsValue(Object value)		Map容器中是否包含值对象对应的键值对

   ​				int size()		包含键值对的数量

   ​				boolean isEmpty()		Map是否为空

   ​				void putAll(Map t)		将t的所有键值对存放到本map对象

   ​				void clear()		清空本map对象所有键值对

   * **HashMap和HashTable**

     ​		HashMap采用哈希算法实现，是Map接口最常用的实现类。由于底层采用了哈希表存储数据，要求键不能重复，如果发生重复，新的键值对会替换旧的键值对。HashMap在查找、删除、修改方面都有非常高的效率。

     ```java
     /**
      * 测试HashMap的使用
      * @author zsk
      *
      */
     public class TestMap {
     	public static void main(String[] args) {
     		Map<Integer,String> m1 = new HashMap<>();
     		
     		m1.put(1, "one");
     		m1.put(2, "two");
     		m1.put(3, "three");
     		
     		System.out.println(m1.get(1));
     		
     		System.out.println(m1.size());
     		System.out.println(m1.isEmpty());
     		System.out.println(m1.containsKey(2));
     		System.out.println(m1.containsValue("four"));
     		
     		Map<Integer,String> m2 = new HashMap<>();
     		m2.put(4, "四");
     		m2.put(5, "五");
     		
     		m1.putAll(m2);
     		System.out.println(m1);
     	}
     }
     ```

     ​		HashTable类和HashMap用法几乎一样，底层实现几乎一样，只不过HashTable的方法添加了synchronized关键字确保线程同步检查，效率较低。

     **两者区别**

     * HashMap：线程不安全，效率高。允许key或value为null。
     * HashTable：线程安全，效率低。不允许key或value为null。

   * **HashMap底层实现详解**

     ​		HashMap底层实现采用了哈希表，这是一种非常重要的数据结构。

     ​		哈希表的本质就是“数组+链表”。

     * 基本结构讲解

       ​		哈希表的基本结构就是“数组+链表”。

       ​		其中的Entry[] table就是HashMap的核心数组结构，称之为“位桶数组”。

       ​		一个Entry对象存储了：

       ​		1.key：键对象 value：值对象  2.next：下一个节点  3.hash：键对象的hash值

       ​		显然**每一个Entry对象就是一个单向链表结构**。

     * 存储数据过程put(key,value)

       1. 获得key对象的hashcode

          首先调用key对象的hashcode()方法，获得hashcode。

       2. 根据hashcode计算出hash值（要求在[0,数组长度-1]区间）

          hashcode是一个整数，需要将它转化成[0,数组长度-1]的范围。要求转化后的hash值尽量均匀地分布在[0,数组长度-1]这个区间，减少“hash冲突”。

       3. 生成Entry对象

          一个Entry对象包含4部分：key对象、value对象、hash值、指向下一个Entry对象的引用。现在有了hash值。下一个Entry对象的引用为null。

       4. 将Entry对象放到table数组中

          如果本Entry对象对应的数组索引位置还没有放Entry对象，则直接将Entry对象存储进数组；如果对应索引位置已经有Entry对象，则将已有Entry对象的next指向本Entry对象，形成链表。

     * 取数据过程get(key)

       1. 获得key的hashcode，通过hash()散列算法得到hash值，进而定位到数组的位置。
       2. 在链表上挨个比较key对象。调用equals()方法，将key对象和链表上所有节点的key对象进行比较，知道碰到返回true的节点对象为止。
       3. 返回equals()为true的节点对象的value对象。

       **Java中规定，两个内容相同(equals()为true)的对象必须具有相等的hashCode**。

     * 扩容问题

       ​		HashMap的位桶数组，初始大小为16.实际使用时，显然大小可变。如果位桶数组中的元素达到（0.75*数组length），就重新调整数组大小为原来2倍大小。

       ​		扩容很耗时。扩容的本质是定义新的更到的数组，并将旧数组内容挨个拷贝到新数组中。

     * JDK8将链表在大于8情况下变为红黑二叉树

       ​		JDK8中，HashMap在存储一个元素时，当对应链表长度大于8时，链表就转换为红黑树。
     
     **自定义实现HashMap底层实现**
     
     ```java
     /**
      * 自定义HashMap
      * 增加put方法，解决了键重复时覆盖相应的value
      * 增加toString方法
      * 增加泛型
      * @author zsk
      * *
      */
     public class ZskHashMap<K, V> {
     	public static void main(String[] args) {
     		ZskHashMap<Integer, String> m = new ZskHashMap<>();
     		m.put(10, "aa");
     		m.put(20, "bb");
     		m.put(30, "cc");
     		m.put(20, "ssss");
     		
     		m.put(53,"zz");
     		m.put(69, "ss");
     		m.put(85, "kk");
     		
     		System.out.println(m);
     		
     		System.out.println(m.get(69));
     	}
     	
     	Node3[] table;   //位桶数组
     	int size;  //存放的键值对的个数
     	
     	public ZskHashMap() {
     		table = new Node3[16];  //长度一般定义为2的整数幂
     	}
     	
     	public V get(K key) {
     		
     		int hash = myHash(key.hashCode(), table.length);
     		V value = null;
     		
     		if(table[hash] != null) {
     			Node3 temp = table[hash];
     			
     			while(temp != null) {
     				if(temp.key.equals(key)) {
     					value = (V) temp.value;
     					break;
     				}else {
     					temp = temp.next;
     				}
     			}
     		}
     		
     		return value;
     	}
     	
     	public void put(K key, V value) {
     		//定义了新的节点对象
     		Node3 newNode = new Node3();
     		newNode.hash = myHash(key.hashCode(),table.length);
     		newNode.key = key;
     		newNode.value = value;
     		
     		Node3 temp = table[newNode.hash];
     		
     		Node3 iterLast = null;
     		
     		boolean keyRepeat = false;
     		
     		if(temp == null) {
     			//此处数组元素为空，则直接将新节点放进去
     			table[newNode.hash] = newNode;
     			size++;
     		}else {
     			//此处数组元素不为空，则遍历对应链表
     			while(temp != null) {
     				//判断key如果重复，覆盖
     				if(temp.key.equals(key)) {
     					keyRepeat = true;
     					temp.value = value;  //只是覆盖value，其他值保持不变
     					break;
     				}else {
     					//如果key不重复,则遍历下一个
     					iterLast = temp;
     					temp = temp.next;
     				}
     			}
     			if(!keyRepeat) {
     				iterLast.next = newNode;
     				size++;
     			}
     		}
     		
     	}
     		
     	@Override
     	public String toString() {
     		StringBuilder sb = new StringBuilder("{");
     		for(int i = 0; i < table.length; i++) {
     			Node3 temp = table[i];
     			while(temp!=null) {
     				sb.append(temp.key + ":" + temp.value + ",");
     				temp = temp.next;
     			}
     		}
     		sb.setCharAt(sb.length()-1, '}');
     		return sb.toString();
     	}
     	
     	public int myHash(int v, int length) {
     		return v&(length - 1);
     	}
     }
     ```
     
   * **TreeMap的使用和底层实现**
   
     ​		TreeMap是红黑二叉树的典型实现。
   
     ​		TreeMap和HashMap实现了同样的接口Map，因此，用法对于调用者来说没有区别。
   
     ​		Hashmap效率高于TreeMap；在需要排序的Map时才选用TreeMap。
     
     ```java
     /**
      * 测试TreeMap的使用
      * @author zsk
      *
      */
     public class TestTreeMap {
     	public static void main(String[] args) {
     		Map<Integer, String> treemap1 = new TreeMap<>();
     		
     		treemap1.put(20, "aa");
     		treemap1.put(3, "bb");
     		treemap1.put(6, "cc");
     		
     		for(Integer key : treemap1.keySet()) {
     			System.out.println(key + "---" + treemap1.get(key));
     		}
     		
     		Map<Emp,String> treemap2 = new TreeMap<>();
     		treemap2.put(new Emp(100, "张三", 50000	), "张三是个好小伙");
     		treemap2.put(new Emp(200, "李四", 5000), "李四工作不积极");
     		treemap2.put(new Emp(150, "王五", 6000), "王五还不错");
     		treemap2.put(new Emp(50, "赵六", 6000), "赵六也可以");
     		
     		for(Emp key : treemap2.keySet()) {
     			System.out.println(key + "---" + treemap2.get(key));
     		}
     	
     	}
     }
     
     
     class Emp implements Comparable<Emp>{
     	int id;
     	String name;
     	double salary;
     	
     	public Emp(int id, String name, double salary) {
     		super();
     		this.id = id;
     		this.name = name;
     		this.salary = salary;
     	}
     
     	@Override
     	public int compareTo(Emp o) {
     		if(this.salary > o.salary) {
     			return 1;
     		}else if(this.salary < o.salary){
     			return -1;
     		}else {
     			if(this.id > o.id) {
     				return 1;
     			}else if(this.id < o.id) {
     				return -1;
     			}else {
     				return 0;
     			}
     		}
     	}
     	
     	@Override
     		public String toString() {
     			return "id" + id + ",name" + name + ",salary" + salary;
     		}
     }
     ```
   
5. **Set接口**

   ​		Set接口继承自Collection，Set接口中没有新增方法，方法和Collection保持完全一致。

   ​		Set容器特点：无序、不可重复。无序指Set中的元素没有索引，只能遍历查找；不可重复指不允许加入重复的元素。更确切的讲，新元素如果和Set中某个元素通过equals()方法对比为true，则不能加入；甚至，Set中只能放入一个null元素。

   ​		Set常用的实现类有：HashSet、TreeSet等。一般用HashSet。

   * **HashSet基本使用**

     ```java
     public class test{
         public static void main(String[] args){
             Set<String> s = new HashSet<>();
             s.add("hello");
             s.add("world");
             System.out.println(s);  //[world,hello]
             s.add("hello");  //相同元素不会被加入
             System.out.println(s);  //[world,hello]
             s.add(null);
             System.out.println(s);  //[null,world,hello]
             s.add(null);
             System.out.println(s);  //[null,world,hello]
         }
     }
     ```

   * **HashSet底层实现**

     ​		HashSet是采用哈希算法实现，底层实际是用HashMap实现的（HashSet本质是一个简化版的HashMap），因此，查询效率和增删效率都比较高。

     ​		**往set中加入元素，本质就是把这个元素作为key加入到了内部的map中**，由于map中key都是不可重复的，因此，Set天然具有“不可重复”特性。

     ```java
     /**
      * 自定义一个HashSet
      * @author zsk
      *
      */
     public class ZskHashSet {
     	
     	HashMap map;
     	
     	private static final Object PRESENT = new Object();
     	
     	public ZskHashSet() {
     		map = new HashMap();
     	}
     	
     	public int size() {
     		return map.size();
     	}
     	
     	@Override
     		public String toString() {
     			StringBuilder sb = new StringBuilder();
     			sb.append("[");
     			for(Object key:map.keySet()) {
     				sb.append(key + ",");
     			}
     			sb.setCharAt(sb.length()-1, ']');
     			return sb.toString();
     		}
     	
     	public void add(Object o) {
     		map.put(o, PRESENT);
     	}
     	
     	public static void main(String[] args) {
     		ZskHashSet set = new ZskHashSet();
     		set.add("aaa");
     		set.add("bbb");
     		set.add("ccc");
     		
     		System.out.println(set);
         }
     }
     ```

   * **TreeSet的使用和底层实现**
   
     ​		TreeSet底层实际是用TreeMap实现的，内部维持了一个简化版的TreeMap，通过key来存储Set的元素。TreeSet内部需要对存储的元素进行排序，因此，我们对应的类需要实现Comparable接口。这样，才能根据compareTo()方法比较对象之间的大小，才能进行内部排序。
   
     **要点**
   
     	* 由于是二叉树，需要对元素做内部排序。如果要放入TreeSet中的类没有实现Comparable接口，则会抛出异常：java.lang.ClassCastException.
     	* TreeSet中不能放入null元素。
   
6. **迭代器**

   ​		