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

     除了Collection接口中的方法，List多了一些跟顺序（索引）有关的方法

     List接口常用的实现类有3个：ArrayList、LinkedList和Vector

     