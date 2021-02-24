## Map集合

Collection是单列集合

Map是双列集合

---

能够说出Map集合的特点

使用Map集合添加方法保存数据

使用“键找值”的方式遍历Map集合

使用“键值对”的方式遍历Map集合

能够使用HashMap存储自定义键值对的数据

能够使用HashMap编写斗地主洗牌发牌案例

---

### 第一章 Map集合

#### 概述

```java
/*
	java.util.Map<K,V>集合
	Map集合的特点:
		1.Map集合中的元素，key与values的类型无关系
		2.Map集合是一个双列集合，一个元素包含两个值
		3.Map集合中的元素，key是不允许重复的，value可以重复
		4.Map集合中的元素，key与value是一一对应
*/
```

---

### Map常用子类

#### HashMap

基于哈希表的Map接口的实现，具有可预知的迭代顺序

```java
/*
	HashMap集合的特点：
		1.HashMap底层是哈希表，查询速度非常快
			1.JDk1.8前：数组+单项链表
			2.JDK1.8后：数组+单项链表/红黑树
		2.hashMap是一个无序的集合，存储元素和取出元素的顺序可能不一致
*/
```

#### LinkedHashMap

```java
/*
	LinkedHashMap特点：
		1.LinkedHashMap集合底层是哈希表+链表
		2.LinkedHashMap集合是一个有序的集合，存储元素与取出元素的顺序一致
*/
```

---

### Map接口中的常用方法

```java
/*
public V put(K key,V value): 把指定的键与值添加到Map中
	返回值 V
		存储键值对的时候，key不重复，v为null
		存储键值对的时候，key重复，新value替换旧的，旧的返回
public V remove(Object key): 把指定的键所对应的键值对删除，返回被删除元素的值
	返回值 V
		key存在，v返回被删除的值
		key不存砸，v返回null
public V get(Object key) 根据指定的键，在Map中获取对应的值
	返回值
		key存在，返回对应value值
		key不存在，返回null
public boolean containsKey(Object key) 判断集合中是否包办指定的键
	返回值
		包含，返回true
		不包含，返回false

*/
Map<String,Integer> map = new HashMap<>();
```

---

### Map集合遍历键找值方式

Map集合的第一种遍历方式：通过键找值的方式

使用Map的keyset()

Set<String> set = map.ketSet();

```java
/*
	Map集合的第一种遍历方式：通过键找值的方式
		实现步骤：
			1.使用Map的keyset()，把Map集合所有的key取出来，存到一个Set集合
			2.通过Set集合获取Map集合的每一个key
			3.通过Map集合中的方法get(key),获取value
*/
Map<String,Integer> map = new HashMap<>();
map.put("赵丽颖",168);
map.put("杨颖",165);
// 1.使用Map的keyset()，把Map集合所有的key取出来，存到一个Set集合
Set<String> set = map.ketSet();
// 2.通过set集合获取Map集合的每一个key
Iterator<String> it = set.iterator();
while(it.hasNext()) {
  String key = it.next();
  // 3.通过Map集合中的方法get(key),获取value
  Integer value = map.get(key);
  System.out.println( key + "=" + value);
// 使用增强for循环
  for(String key:map.keySet()) {
    Integer value = map.get(key);
    System.out.println(key + "=" + value);
  }
}
```

---

### Entry键值对象

Map.Entry<L,V>:在Map接口中有一个内部接口Entry

作用：但Map集合一创建，那么就会在Map集合中创建一个Entry对象，用来记录键与值(键值对对象，键与值的映射关系) -->结婚证

![image-20200913190707572](Map%E9%9B%86%E5%90%88%E7%AC%94%E8%AE%B0.assets/image-20200913190707572.png)

---

### Map集合遍历键值对方式

Map集合遍历的第二种方式：使用Entry对象遍历

Set<Map.Entry<K,V>> entrySet 返回此映射中的映射关系的Set视图

  Set<Map.Entry<String,Integer>> set = map.entrySet();

```java
/*
	Map集合遍历的第二种方式：使用Entry对象遍历
	Mao集合中的方法：
		Set<Map.Entry<K,V>> entrySet 返回此映射中的映射关系的Set视图
	实现步骤
		1.使用Map集合中的方法entrySet(),把Map集合中多个Entry对象取出来，存到一个			Set集合中
		2.遍历Set集合，获取每一个Entry对象
		3.使用Entry对象中的getKey()和getValue()获取键与值
*/
public static void main(Sting[] args) {
  // 创建Map集合
  Map<String,Integer> map = new HashMap<>();
  map.put("赵丽颖",168);
  // 1.使用Map集合中的entrySet(),把Map集合中的多个Entry对象取出来，存到一个Set集合中
  Set<Map.Entry<String,Integer>> set = map.entrySet();
  // 2.遍历Set集合，获取每一个ENtry对象
  // 使用迭代器遍历set结合
  Iteraror<Map.Entry<String,Integer>> it = set.iterator();
  while(it.hasNext()) {
    Map.Entry<String,Integer> entry = it.next();
    // 3.使用Entrty对象中的方法getKey()和getValue()获取键与值
    String key = entry.getKey();
    String value = entry.getValue();
    System.out.println(key + "=" + value);
   // 增强for循环
    for(Map.Entry<String,Integer>entry:set) {
      String key = entry.getKey();
    	String value = entry.getValue();
   		System.out.println(key + "=" + value);
    }
  }
}
```

---

### HashMap存储自定义

使用keySet和增强for遍历Map集合

  Set<String> set = map.ketSet();

```java
public class Person {
  private String name;
  private int age;
  // show02()，必须重写
  public String toString() {}
  public boolean equals(Object o) {
    if(this == o) return true;
    if(o==null || getClass() != o.getClass()) return false;
    Person person = (Person) person;
    return age==Person.age &&
      Objects.equals(name,person.name);
  }
}
/*
	HashMap存储自定义类型值
	Map集合保证key是唯一的
		作为key的元素，必须重写hashCode()方法和equals()方法，以保证key唯一
*/
public static void main(String[] args) {
  show1();
  show2();
}
/*
	HashMap存储自定义类型键值
	key:String类型
		String类重写hashCode()方法和equals()方法，可以保证key唯一
	value:Person类型
		value可以重复
*/
private static void show1{
  //创建HashMap集合
  HashMap<String,Person> map = new HashMap<>();
  map.put("北京",new Person("张三",18));
  //使用keySet和增强for遍历Map集合
  Set<String> set = map.ketSet();
  for(String key:set) {
    Person value = map.get(key);
    System.out.println(key + "=" + value);
  }
}
/*
	HashMap存储自定义类型键值
	Key：Person类型
		Person类型就必须重写hashCode方法和equals方法，以保证key唯一
*/
private static void show2{
  //创建hashMap集合
  HashMap<Person,String> map = new HashMap<>();
  //往集合中添加元素
  map.put(new Person("女王",18),"英国");
  map.put(new Person("女王",18),"毛里求斯");
  //使用entrySet和增强for循环遍历Map集合
  Set<Map.Entry<Person,String>> set = map.entrySet();
  for(Map.Entry<Person,String> entry:set) {
    Person key = entry.getKey();
    String value = entry.getValue();
    System.out.println(key + "---->" + value);
  }
}
```

---

### LinkedHashMap集合

我们知道HaspMap保证成对元素唯一，并且查询速度很快，可是成对元素存放进去是没有顺序的，那么我们要保证有序，还要速度快怎么办呢？

在HashMap下面有一个子类LinkedHashMap，它是链表+哈希表组合的一个数据存储结构。

```java
public class LinkedHasMapDemo {
  public static void main(String[] args) {
    LinkedHashMap<String,String> map = new LinkedHashMap<>();
    map.put("邓超","孙膑");
    Set<Entry<String,String>> entrySet = map.entrySet();
    for(Entry<String,String>entry:entrySet) {
      System.out.print(entry.geyKey() + " " + entry.getValue());
    }
  }
}
```

---

### Hashtable集合

等同于HashMap集合，但是不允许存储null对象。

最早期的双列集合，jdk1.0开始的。是同步的，线程安全的，意味着慢。

```java
/*
	java.util.Hashtable<K,V> implements Map<K,V>
	Hashtable：底层也是个哈希表，是一个线程安全的集合，是单线程集合，速度慢
	HashMap：底层是哈希表，是一个线程不安全的多线程的集合，速度快
	HashMap：可以存储null值，null键
	Hashtable集合：不能存储null值、null键
	Hashtable和Vector集合，在jdk1.2之后被(HashMap，ArrayList)取代了
	Hashtable的子类Properties依旧活跃在历史舞台
		Properties集合是一个唯一和IO流相结合的集合
*/
public static void main(String[] args) {
  HashMap<String,String> map = new Hashtable<>();
  // 不允许
  map.put(null,"a");
  map.put("b",null);
  map.put(null,null);
}
```

---

## 补充知识点

### Jdk9对集合添加的优化_of方法

通常，我们在代码中创建一个集合(例如，List或Set)，并直接使用一些元素填充它，实例化集合，几个add方法调用，使得代码重复

```java
public static void main(String[] args) {
  List<String> list = new ArrayList<>();
  list.add("abc");
}
```

JDK9，添加了集中集合工厂方法，更方便创建少量元素的集合，map实例。新的List、Set、Map的静态工厂方法，可以更方便地创建集合的不可变实例。

```java
/*
	JDK9的新特性
		of方法
			static <E> list<E> of(E... elements)
			使用前提，集合存储的元素个数已经确定，不在改变
	注意
		1.of只适用于list、set和Map接口，不适用于接口的实现类
		2.of方法的返回值是一个不能改变的集合，集合不能再用add和put
		3.Set和Map接口调用of方法时，不允许重复元素
*/
public static void main(String[] args) {
  Set<String> str1 = Set.of("a","b","c");
  // str1.add("c");编译不会报错，但是执行会报错，因为这是不可变的集合
  System.out.println(str1);
  Map<String,Integer>str2 = Map.of("a",1,"b",2);
  System.out.println(str2);
  List<String> str3 = List.of("a","b");
  System.out.println(str3);
}
```

