## 集合

集合长度可变。集合存储的都是对象。

---

## 集合框架

集合按照存储结构分为两大类，单列集合Collection和双列结合Map；

目标

1. 会使用集合存储数据

2. 会便利集合，把数据取出来

3. 掌握每种集合的特性。

   ![image-20200908214218372](Collection%E7%AC%94%E8%AE%B0.assets/image-20200908214218372.png)

---

**Collection接口**

- List接口 有序 允许重复元素
  - Vector实现类  同步的，一次只能有一个线程使用Vector 底层是数组
  - ArrayList实现类 底层是数组
  - LinkedList实现类  底层是链表
- Set接口 无序 不允许重复元素
  - TreeSet实现类
  - HashSet实现类 哈希表
  - LinkedHashSet实现类 哈希表

---

**Collection共性方法**

```java
public boolean add(E e);
public void clear();
public boolean remove(E e);
public contain(E e);
public boolean isEmpty();
public int size();
public Object[] toArray();
```

---

**iterator迭代器**

iterator接口

Collection接口和Map接口用于存储元素

Iterator主要用于迭代访问(遍历)Collection中的元素

想要遍历Collection集合，那么就要获取该集合迭代器完成迭代操作，下面介绍一下获取迭代器的方法：

- public Iterator iterator（）； 获取结合对应的迭代器，用来遍历集合中的元素。

迭代的概念

- 迭代：即Collection集合元素的通用获取方式。在取元素之前先要判断集合中有没有元素，有的话把元素去除，继续判断，直至全部取出。

Iterator接口的常用方法如下：

```java
public E next(); // 取出下一个元素
public boolean hasNext(); // 判断还有没有下一个元素
public void remove(); // 删除某个元素
// Itrtator迭代器，是一个接口，无法直接使用，需要用相应的实现类实现，获取实现类的方法很特殊。
// collection接口中有一个方法，iterator(),这个方法反悔的就是迭代器的实现类对象。
Iterator<E> iterator(); // 返回在此collection的元素上进行迭代的迭代器。
/* 迭代器使用步骤
		1. 使用集合中的方法iterator()获取对应的实现类，使用Iterator接口接受(多态)。
		2. 使用Iterator接口中的hasNext()方法判断还有没有下一个元素。
		3. 使用Iterator接口中的next()方法取出下一个元素。
*/
public class Demo{
  public static void main(String[] args) {
    Collection<String> coll = new ArrayList<>();
    coll.add("1");
    coll.add("2");
    coll.add("3");
    // 多态    接口           实现类对象
    Iterator<String> it = coll.iterator();
    while(it.hasNext()) {
      System.out.print(it.next());
    }
  }
}
```

---

**增强for循环**

底层使用的也是迭代器，是否for循环的格式，简化了iterator的书写

jdk1.5后出现的新特性

Collection<E> extends Iterable<E>; 所有单列结合都可以使用增强for

```java
public interface Interable<T> 实现这个接口允许对象成为foreach语句的对象
// 增强for循环 ： 用于遍历集合和数组
// 格式
  // for（集合/数组的数据类型 变量名 ：集合名/数组名）{}
ArrayList<String> list = new ArrayList<>();
for(String s:list) {
  System.out.print(s);
}
```



