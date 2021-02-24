## Set接口

不包含重复元素的集合。更正式地，集合不包含一对元素e1和e2 ，使得e1.equals(e2) ，并且最多一个空元素。

```java
	/*
		java.util.Set接口 extends Collection接口
		Set接口的特点:
			1. 不允许存储重复的元素
			2. 没有索引，没有带索引的方法，也不能使用普通的for循环
	*/
```

---

### HashSet

此类实现Set接口，由哈希表（实际为HashMap实例）支持。对集合的迭代次序不作任何保证;特别是，它不能保证订单在一段时间内保持不变。这个类允许null元素。

```java
/*
	HashSet特点：
		1. 不允许存储重复的元素
		2. 没有索引
		3. 是一个无序的集合，存储匀速和取出元素的顺序有可能不一致
		4. 底层是一个哈希表结构(查询速度非常快)
*/
public class HashSet {
  public static void main(String[] args) {
    Set<Integer> set = new Hash<>();
    // 添加
    set.add(1);
    set.add(2);
    set.add(1); // 不报错，但是不会添加成功
    // 遍历
    Iterator<Integer> it = set.iterator();
    while(it.hasNext()) {
      System.out.print(it.next());
    }
    // 使用增强for循环
    for(Integer i:set) {
      System.out.print(i);
    }
  }
}
```

### HashSet集合存储数据的结构(哈希表)

在JDK1.8之前，哈希表底层采用数组+链表实现。即使用链表处理冲突，同一hash值的链表都存在一个链表里。但是当位于一个桶中的元素较多，即hash值相等的元素较多时，通过key值一次查找的效率较低。而jdk1.8采用数组+链表+红黑树实现。当链表长度超过阈值(8)时,将链表转换为红黑树，大大减少了查找时间。

简单的说，哈希表是由数组+链表+红黑树实现的。

#### 哈希值

是一个十进制的整数，由系统随机给出(也就是对象的地址值，是一个逻辑地址，是模拟出来得到的地址，不是数据实际存储的物理地址)

在Object类中有一个方法，可以获取对象的哈希值。 int hashCode()

```java 
// native：代表该方法调用的是本地操作系统的方法
public native int hashCode();
public static void main(String[] args) {
  Person p1 = new Person();
  int h1 = i1.hashCode();
  System.out.println(h1);
  Person h2 = new Person();
  int h2 = p2.hashCode();
  System.out.println(h2);
  /*
  	toString方法源码：
 return getClass().getName()+"@"+Integer.toHexString(hashCode());
						 com.xxx.demo.hashCode.Persion@282bo1e
  */
  
}
```

> 哈希表的特点：查询速度快

哈希表存储过程：

- 数据结构：把元素进行了分组(相同哈希表的元素是一组)链表/红黑树把相同哈希值的元素连接到一起
- 两个元素不同，但是哈希值相同，哈希冲突。(挂到同一分组上)
- 当一个分组挂了8个元素后把链表转换为红黑树。为了提高查询速度。

```java
	/*
		Set集合不允许重复元素的原理
		Set集合在调用add方法的时候，add方法还会调用元素的hashCode方法和equlas方法，判断元素是否重复
			1. 先调用hashCode()计算出 s 哈希值
			2. 调用equals()，判断是否有重复元素
				2.1 没有，就把 s 存储到集合当中
				2.2 有，调用equals()和哈希值相同的元素进行比较,相同就不会存储
				2.3 有，调用equals()，判定不相同，存储到同一哈希值分组下。
	*/
public static void main(String[] args) {
  HashSet<String> set = new HashSet<>();
  String s1 = new String("abc");
  String s2 = new String("abc");
  set.add(s1);
  set.add(s2);
  set.add("abc");
}
/*
	HashSet存储自定义类型的元素
	set集合报错元素唯一：
		存储的元素(String，Interger,...Student,Person...)必须重写hashCode			方法和equals方法
		要求:
			同名同年龄的人，视为相同一个人，只能存储一次
*/
public class Person {
  Private String name;
  int age;
  public boolean equals(Object o) {
    if(this == 0) return true;
    if(o == null || getClass() != o.getClass()) return false;
    return age = persion.age && Objects.equals(name,persion.name);
  } 
}
public static void main(String[] args) {
  HashSet<Person> set = new HashSet<>();
  
}
```

### LinkedHashSet

LinkedHashSet是HashSet的一个子类。

```java
/*
	java.util.LinkedHashSet集合 extends HashSet集合
	LinkedHashSet集合特点:
		底层是一个哈希表+链表：多了一条链表记录元素的存储顺序，保证元素有序。
*/
```

### 可变参数

```java
/*
	可变参数 ： JDK1.5之后出现的新特性
	使用前提：
		当方法的参数列表数据类型已经确定，但是参数的个数不确定，就可以使用可变参数
	使用格式： 定义方法时使用
		修饰符 返回值类型 方法名(数据类型...变量名){}
	可变参数的原理
		可变参数底层就是一个数组，根据传递参数个数不同，会创建不同长度的数组传参
		传递的参数个数,可以是0、1...多个
	注意事项:
		1. 一个方法的参数列表，只能有一个可变参数
		2. 如果方法的参数有多个，可变参数必须写在列表的末尾
*/
public static int add(int... arr) {
  int sum = 0;
  for(int i:arr) {
    sum += i;
  }
  return sum;
}
// 可变参数的特殊(中间)写法
public static void method(Object...obj)
```

