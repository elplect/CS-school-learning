## 第一章 Stream流

说到Stream就容易想到I/O Stream,实际上,没人规定“流”一定就是“IO”流.在java8中,得益于Lambda所带来的函数式编程,引入了一种全新的Stream概念,用于解决已由集合库既有的弊端.

### 1.1 引言

#### 传统集合的多步遍历代码

几乎所有的集合(如`Collection`接口或`Map`接口等)都支持直接或间接的便利操作.而当我们需要对集合中的元素进行操作的时候,除了必须的添加、删除、获取外,最典型的就是集合遍历.例如

```java
public static void main(String[] args) {
  List<String> list = new ArrayList<>();
  list.add("张无忌");
  list.add("周芷若");
  list.add("赵敏");
  for(String name : list) {
    System.out.println(name);
  }
}
```

这是一段非常简单的集合便利操作;对集合中的每一个祖父穿都进行打印输出操作

#### 循环遍历的弊端

java 8的Lambda让我们可以更加专注于**做什么**而不是**怎么做**,这点此前已经结合内部类进行了对比说明.现在,我们仔细体会一下上述代码,可以发现:

- for循环的语法就是“**怎么做**”
- for循环的循环体才是“**做什么**”

为什么使用循环?因为要进行遍历.但循环是遍历的唯一方式吗?遍历是指每一个元素逐一进行处理,而**不是从第一个到最后一个顺次处理的循环**.前者是目的,后者是方式

试想一下,如果希望对集合中的元素进行筛选过滤:

1. 将集合A根据条件一过滤为子集B
2. 然后再根据条件二过滤为子集C

那怎么办?在java 8之前的做法为:

```java
public static void main(String[] args) {
  List<String> list = new ArrayList<>();
  list.add("张无忌");
  list.add("周芷若");
  list.add("赵敏");
  List<String> zhangList = new ArrayList<>();
  for(String name : list) {
    if(name.startsWith("张")) {
      zhangList.add(name);
    }
  }
  List<String> shortList = new ArrayList<>();
  for(String name : zhangList) {
    if(name.length() == 3) {
      shortList.add(name);
    }
  }
  for(String name:shortList) {
    System.out.println(name);
  }
}
```

这段代码中含有三个循环,每一个作用不同:

1. 首先筛选所有姓张的人;
2. 然后筛选名字有三个字的人;
3. 最后进行对结果进行打印输出.

每当我们需要对集合中的元素进行操作的时候,总是需要进行循环、循环、再循环.这是理所当然的么?不是,循环是做事情的方式,而不是目的.另一方面,使用线性循环就意味着只能遍历一次.如果希望再次比那里,只能再使用另一个循环从头开始.

那么,Lambda的衍生物Stream能给我们带来更加优雅的写法呢?

#### Stream的更优写法

下面来看一下借助java8的Stream API,什么才叫优雅:

```java
public static void main(String[] args) {
	List<String> list = new ArrayList<>();
  list.add("张无忌");
  list.add("周芷若");
  list.add("赵敏");
  list.add("张三丰");
  list.stream()
               .filter(name->name.startsWith("张"))
               .filter(name->name.length()==3)
               .forEach(name->System.out.println(name));
} 
```

直接阅读代码的字面意思即可完美展示无关逻辑方式的语义:获取流、**过滤姓张**、**过滤长度为3**、**注意打印**.代码中并没有体现使用线性循环或是其他任何算法进行遍历,我们真正要做的事情内容被更好地体现在代码中.

### 1.2 流式思想概述

**注意:请暂时忘记对传统IO流的固有印象!**

整体来看,流式思想类似于工厂车间的**生产流水线**.

当需要对多个元素进行操作(特别是多不操作的时候),考虑到性能及便利性,我们应该首先拼接好一个“模型”步骤方案,然后再按照方案去执行它.

![image-20200926212557128](Stream%E6%B5%81%E5%BC%8F%E6%80%9D%E6%83%B3%E7%AC%94%E8%AE%B0.assets/image-20200926212557128.png)

这张图中展示了过滤、映射、跳过、计数等多步操作,这是一种集合元素的处理方案,而方案就是一种“函数模型”.图中的每一个方框都是一个“流”,调用指定的方法,可以从一个流模型转换为另一个流模型.而最右侧的数字3时最终结果.

这里的`filter`、`map`、`skip`都是在对函数模型急性操作,集合元素并没有被真正处理.只有当终结方法`count`执行的时候,整个模型才会按照指定策略执行操作.而这得益于Lambda的延迟执行特性.

> 备注:“Stream”流其实是一个集合元素的函数模型,它并不是集合,也不是数据结构,其本身并不存储任何元素(或其地址值)

Stream(流)是一个来自数据源的元素队列

- 元素是特定类型的对象,形成一个队列,java中的Stream并不会存储元素,而是按需计算
- 数据源 流的来源,可以是集合、数组等

和以前的Collection操作不同,Stream操作还有两个基础的特征:

- **pipelining** : 中国南京爱你操作都会返回流对象本身.这样多个操作可以串联成一个管道,如同流式风格(fluent style).这样做可以对操作进行优化,比如延迟执行(laziness)和短路(short-circulting).
- **内部迭代** : 以前对集合便利都是通过iterator或者增强for的方式.显式的再集合外部进行迭代.这叫做外部迭代.Stream提供了内部迭代的方式,流可以直接调用遍历方式.

当使用一个流的时候,通常包括三个基本步骤: 获取一个数据源->数据转换->执行操作获取想要的结果,每次转换原有Stream对象不改变.返回一个Stream对象(可以有多次转换),这就源旭对其操作可以像链条一样排列,变成一个管道.

### 1.3 获取流

`java.util.stream.Stream<T>`是java8新加入的最常用的流对象.(并不是函数式接口)

获取一个流非常简单,有以下几种常用的方式:

- 所有的`Collection`集合都可以通过`Stream`默认方法获取流
- `Stream`接口的静态方法`of`可以获取数组对应的流

#### 根据Collection获取流

首先,`java.util.Collection`接口中加入了default方法`stream`用来获取流,所以其所有实现类均可获取流.

```java
/*
    java.util.stream.Stream<T> 是java 8新加入的最常用的流对象。
    获取一个流非常简单，有以下几种常用的方式：
        - 所有的`Collection`集合都可以通过`Stream`默认方法获取流
            default Stream<E> stream()
        - `Stream`接口的静态方法`of`可以获取数组对应的流
            static <T> Stream<T> of(T...values)
            参数是一个可变参数，那么我们就可以传递一个数组
 */
public class demoGetStream {
    public static void main(String[] args) {
        // 把集合转换为Stream流
        List<String> list = new ArrayList<>();
        Stream<String> stream1 = list.stream();

        Set<String> set = new HashSet<>();
        Stream<String> stream2 = set.stream();

        Map<String,String> map = new HashMap<>();
        // 获取键，存储到一个set集合中
        Set<String> ketSet = map.keySet();
        Stream<String> stream3 = ketSet.stream();

        // 获取值，存储到一个Collection集合中
        Collection<String> values = map.values();
        Stream<String> stream4 = values.stream();
        // 获取键值对 (键与值的映射关系，entrySet)
        Set<Map.Entry<String,String>> entries = map.entrySet();
        Stream<Map.Entry<String,String>> stream5 = entries.stream();

        // 把数组转换为Stream流
        Stream<Integer> steam6 = Stream.of(1,2,3,4,5);
        // 可变参数可以传递数组
        Integer[] arr = {1,2,3,4,5};
        Stream<Integer> stream7 = Stream.of(arr);
    }
}
```

> 备注: of方法的参数其实是一个可变参数,所以支持数组

### 1.4常用方法

![image-20200926231835472](Stream%E6%B5%81%E5%BC%8F%E6%80%9D%E6%83%B3%E7%AC%94%E8%AE%B0.assets/image-20200926231835472.png)

流模型的操作很丰富,这里介绍一些常用的API.这些方法可以被分成两种:

- **延迟方法:** 返回值类型仍然是`Stream`接口自身类型的方法,因此支持链式调用.(除了终结方法外,其余方法均为延迟方法)
- **终结方法:**返回值类型不再是`Stream`接口自身类型的方法,因此不再支持类似`StringBuilder`那样的链式调用.本小结中,终结方法包括`count`和`forEach`方法

> 备注 : 本小结之外的更多方法,自行参考API文档

#### 逐一处理 : forEach

虽然方法名字叫`forEach`,但是与for循环体的for-each昵称不同

```java
void forEach(Consumer<? super T> action);
```

该方法接受一个`Consumer`接口函数,会将每一个流元素交给该函数进行处理

**复习Consumer接口**

```java
java.util.function.Consumer<T> 是一个消费性接口
  Consumer接口中包含抽象方法void accept(T t),意味消费一个执行泛型的数据
```

**基本使用** :

```java
public static void main(String[] args) {
  Stream<String> stream = Stream.of("张无忌","张三丰","周芷若");
  stream.forEach(name->System.out.println(name));
}
```

#### 过滤 : filter

可以通过`filter`方法将一个流转成另一个子集流.方法签名 : 

```java
Stream<T> filter(Predicate<? super T> predicate);
```

该接口接受一个`Predicate`函数式接口参数(可以是一个Lambda或方法引用)作为筛选条件,

![image-20200926234834169](Stream%E6%B5%81%E5%BC%8F%E6%80%9D%E6%83%B3%E7%AC%94%E8%AE%B0.assets/image-20200926234834169.png)

**复习Predicate接口**

此前我们已经学习过`java.util.stream.predicate`函数式接口,其中唯一的抽象方法为:

```java
boolean test(T t);
```

#### 基本使用

Stream流中的`filter`方法基本使用的代码如下:

```java
/*
    Stream流中的常用方法 filter：用于对Stream流中的数据进行过滤
    Stream<T> filter(Predicate<? super T> predicate);
    filter方法的参数Predicate是一个函数式接口，所以可以传递Lambda表达式，对数据进行过滤
    Predicate中的抽象方法：
        boolean test(T t);
 */
public class demoStreamFilter {
    public static void main(String[] args) {
        Stream<String> stream = Stream.of("张无极","张三丰","赵敏","周芷若","张翠山1");
        Stream<String> stream1 = stream
                .filter(name->name.startsWith("张"))
                .filter(name->name.length()==3);
        stream1.forEach(name->System.out.println(name));
    }
}
```

#### 映射 : map

如果将流中的元素映射到另一个流中,可以使用map方法.方法签名:

```java
<R> Stream<R> map(Function<? super T,? extends R> mapper);
```

该接口需要一个`Function`函数式接口参数,可以将当前流中的T类型数据转换为另一种R类型的流.

![image-20200926235749894](Stream%E6%B5%81%E5%BC%8F%E6%80%9D%E6%83%B3%E7%AC%94%E8%AE%B0.assets/image-20200926235749894.png)

**复习Function接口**

此前我们已经学习过`java.util.stream.Function`函数式接口,其中唯一的抽象方法为:

```java
R apply(T t);
```

这可以将一种T类型转换成为R类型,而这种转换的动作,成为“映射”

**基本使用**

Stream流中的map方法基本使用如下

```java
/*
    如果将流中的元素映射到另一个流中,可以使用map方法.方法签名:
    <R> Stream<R> map(Function<? super T,? extends R> mapper);
    该接口需要一个`Function`函数式接口参数,可以将当前流中的T类型数据转换为另一种R类型的流.
    Function中的抽象方法：
        R apply(T t);
 */
public class demoStreamMap {
    public static void main(String[] args) {
        //获取一个String类型的Stream流
        Stream<String> stream = Stream.of("1","2","3","4");
        //使用map方法，把字符串类型的整数，转换(映射)为Integer类型的整数
        Stream<Integer> stream1 = stream.map(s->Integer.parseInt(s));
        stream1.forEach(i->System.out.println(i));
    }
}
```

#### 统计个数 : count

**是一个终结方法**

正如旧集合`Collection`当中的`size`方法已由,流提供`count`方法来数一数其中的元素个数:

```java
long count();
```

该方法返回一个long值代表元素个数(不再像旧集合那样是int值).基本使用 : 

```java
public static void main(String[] args) {
  Stream<String> original = Stream.of("张无忌","张三丰","周芷若");
  Stream<String> result = original.filter(s->s.startWith("张"));
  System.out.println(result.count());
}
```

#### 取用前几个 : limit

是一个延迟方法

`limit`方法可以对流进行截取,只取用前n个,方法签名 :

```java
Stream<T> limit(long maxSize);
```

参数是一个long型,如果集合当前长度大于参数则进行街区,否则不进行操作.基本使用 :

![image-20200927104137864](Stream%E6%B5%81%E5%BC%8F%E6%80%9D%E6%83%B3%E7%AC%94%E8%AE%B0.assets/image-20200927104137864.png)

```java
Stream<String> stream2 = stream1.limit(3);
```

#### 跳过前几个 : skip

如果希望跳过前几个元素,可以使用`skip`方法获取一个截取之后的新流 :

```java
Stream<T> skip(long n);
```

如果流的当前长度大于n,则跳过前n个.否则将会得到一个长度为0的空流.基本使用 : 

![image-20200927104452628](Stream%E6%B5%81%E5%BC%8F%E6%80%9D%E6%83%B3%E7%AC%94%E8%AE%B0.assets/image-20200927104452628.png)

```java
Stream<String> stream2 = stream1.skip(4);
```

#### 组合 : concat

如果有两个流,希望合并成为一个流,那么可以使用`Stream`接口的静态方法`concat`:

```java
static <T> Stream<T> concat(Stream<? extends T> a,Stream<? extends T> b)
```

> 备注 : 这是一个静态方法,与`java.lang.String`当中的`concat`方法是不同的

该方法的基本使用代码如:

```java
Stream<String> result = Stream.concat(streamA,streamB);
```

