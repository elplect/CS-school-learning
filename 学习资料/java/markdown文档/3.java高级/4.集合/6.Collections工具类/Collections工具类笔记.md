## Collections工具类

```java
/*
	java.util.Collections是集合工具类，用来对集合进行操作
		public static<T> boolean addAll(Collection<T> C,T...elements)
		public static void shuffle(List<T> list) 打乱顺序
		public static<T> void sort(List<T> list);
		public static<T> void sort(List<T> list,Comparator<? super T>);
			Comparable和Comparator的区别
				Comparable：自己和别人比较，自己需要实现Comparable接口
				Comparator：相当于找了一个裁判，进行两者比较
*/
public static void main(String[] args) {
  ArrayList<String> list = new ArrayList<>();
  list.add("a");
  list.add("b")
  list.add("c");
  Collections.addAll(list,"a","c","b");
  Collections.shuffle(list);
  Collections.sort(list);
  ArrayList<Person> pList = new ArrayList<>();
  Collections.sort(pList);
  Collections.sort(pList,new Comparator<Person>() {
    public int compare(Person o1,Person o2) {
      return o1.age-o2,age;
    }
  })
}
public class Person implements Comparable<Person>{
  String name;
  int age;
  // 重写排序的规则
  public int compareTo(Person o) {
    return this.age - o.age; // 升序规则
  }
}

```

