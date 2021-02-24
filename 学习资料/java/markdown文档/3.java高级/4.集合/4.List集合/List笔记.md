## List集合

有序的Collections ：允许存储重复的元素。

java.util.list

---

```java
/*
	list接口的特点：
		1. 有序的集合，存储元素和取出元素的顺序是一致的(存储123 取出123)
		2. 有索引，包含了一些带索引的方法
		3. 允许存储重复的元素
	list接口中带索引的方法(特有)
		- public void add(int index,E element) // 添加
		- public E get(int index) // 获取
		- public E remove(int index) // 删除
		- public E set(int index,E element) // 替换
	注意：
		操作索引一定要防止索引越界。
		IndexOutOfBoundException 索引越界异常
		ArrayIndexOutOfBoundException 数组索引越界异常
		StringIndexOutOfBoundException 字符串索引越界异常
*/

/*
	list结合循环遍历
*/
	// 普通的for循环
			for(int i=0;i<list.size();i++) {
				list.get(i);
			}
	// 迭代器编译
			Iterator<String> it = list.iterator();
			while(it.hasNext()) {
        String s = it.next();
      }
	// 增强for循环
			for(String s:list) {
        
      }

```

---

### List集合子类

#### ArrayList集合

Java.util.ArrayList集合数据存储的是数组结构，元素增删慢，查找快。由于日常开发中最常用的功能为查询数据，遍历数据，所以ArrayList是最常用的集合。

但是不能随意使用它满足任何需求。增删太慢

#### LinkedList集合

java.util.LinkedList集合数据存储的结构是链表结构，方便元素添加、删除的集合

> LinkedList是一个双向链表。

实际开发对一个集合元素的添加与删除经常涉及守卫操作，而LinkedList提供了大量首位操作的方法。

```java
/*
	LinkedList集合的特点：
		1. 底层是链表结构，查询慢，增删快
		2. 里面有大量操作首位元素的方法
		注意:使用linkedList集合特有的方法，不能使用多态
		
		- public void addFirst(E e);
		- public void addLast(E e);
		- public void push(E e);
		
		- public E getFirst();
		- public E getLast();
		
		- public E removeFirst();
		- public E removeLast();
		- public E pop();
		
		- public boolean isEmpty();
*/
private static void show(){
  LinkedList<String> linked = new LinkedList<>();
  linked.add("a");
  linked.add("b");
  linked.push("www");
  
}
```

#### Vector集合

Vector可以实现可增长的对象数组。Vector的大小可以根据需要增大或缩小，以适应创建Vector后进行添加或移除项的操作。

与ArrayList相比，Vector是线程同步的，所以也是线程安全。即一个时刻只有一个线程可以使用Vector，但是实现同步需要更高的时间，所以访问它需要更多的时间。用大数据量的数据时，Vector比较有优势，因为vector的增长率是100%，arrayList是50%



