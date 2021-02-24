## Lambda表达式

### 1.1 函数式编程思想概述

在数学中,函数就是有输入量、输出量的一套计算方案,也就是“拿什么东西做什么事”,相对而言,面向对象过分强调“必须通过对象的形式来做事情”,而且函数式思想则尽量忽略面向对象的复杂语法————强调做什么,而不是以什么形式做.

面向对象的思考:

​	做一件事情,找一个能解决觉这个事情的对象,调用对象的方法,完成事情.

函数式编程思想:

​	只要能获取结果,谁去做的,怎么做的,都不重要,重视的是结果,不重视过程.

---

### 1.2 冗余的Runnable代码

#### 传统写法

当需要气定哦一个线程去完成任务时,通过会通过`java.lang.Runnable`接口来定义任务内容,并使用`java.lang.Thread`类来启动该线程.代码如下:

```java
public static void main(String[] args) {
  // 匿名内部类
  Runnable task = new Runable() {
    public void run() {
      System.out.println("ss");
    }
  };
  new Thread(task).start();
}
```

本着“一切皆对象‘的思想,这种做法无可厚非.受限创建一个Runnable接口的匿名内部类对象来指定任务内容,再将其交给一个线程来启动.

#### 代码分析

对于`Runnable`的匿名内部类用法,可以分析出几点内容:

- `Thread`类需要`Runnable`接口作为参数,其中的抽象`run`方法是用来指定线程任务内容的核心.
- 为了指定`run`的方法体,不得不需要`Runnable`接口的实现类;
- 为了省去定义一个`RunnableImpl`实现类的麻烦,不得不使用匿名内部类;
- 必须覆盖重写抽象`run`方法,所以方法名称、方法参数、方法返回值不得不再写一遍,且不能写错;
- 而实际上,**似乎只有方法体才是关键所在**.

---

### 1.3 编程思想转换

#### 做什么,而不是怎么做

我们不必创建一个匿名内部类对象,只是为了做这件事不得不创建一个对象,真正想做的事是:将`run`方法体内的代码传递给`Thread`类知晓.

**传递一段代码**——这才是我们真正的目的,而创建对象只是受限于面向对象语法而不得不采取的一种手段方式.拿,有没有更件简单的方法?如果我们将关注点从“怎么做”回归到“做什么”的本质上,就会发现只要能够更好地传达目的,过程与形式其实并不重要.

为了回归本质,2014年3月Oracle发布的java8中,加入了**Lambda表达式**的重量级新特性,为我们打开了新世界的大门.

### 1.4 体验Lambda的更优写法

借助java8的全新玩法,上述`Runnable`接口的匿名内部类写法可以通过更简单的Lambda表示式达到等效:

```java
public static void main(String[] args) {
  new Thread(() -> System.out.println("多线程任务执行")).start(); // 启动线程
}
```

执行效果完全一样,可以在1.8+的编译级别通过.从代码的语义可以看出启动了一个线程,而线程任务的内容以更加简洁的形式被指定.

不再有“不得不创建接口对象”的束缚,不再有“抽象方法覆盖重写”的负担.

### 1.5 回顾匿名内部类

#### 使用实现类

1. 创建RunnableImpl类,implements Runnable接口
2. 创建RunnableImpl类对象run,创建Thread对象
3. Thread(run).start();

#### 使用匿名内部类

这个`RuuanbleImpl`类只是为了实现Runnable接口而存在的,而且仅被使用了唯一一次,所有使用匿名内部类的语法即可省去该类的单独定义,即匿名内部类

```java
public static void main(Sitrng[] args) {
  new Thread(new Runnable() {
    public void run() {
      System.out.println("多线程任务执行!");
    }
  }).start();
}
```

#### 匿名内部类的好处与弊端

一方面省去实现类的定义:

另一方面,匿名内部类的语法,过于复杂.

#### 语义分析

仔细分析该代码中的语义,Runnable接口只有一个run方法的定义:

- `publid abstract void run();`

即制定了一种做事情的方法(就是一个函数):

- **无参数**:不需要任何条件即可执行该方案

- **无返回值:** 该方案不产生任何结果.
- **代码块**(方法体): 该方案的具体执行步骤

同样的语义体在Lambda语法中,要更加简单:

`() -> System.out.println("多线程任务执行!");`

- 前面一对小括号即run方法的参数(无),代表不需要任何条件
- 中间的一个箭头代表将前面的参数传递给后面的代码
- 后面的输出语句即业务逻辑代码

### 1.6 Lambda标准格式

Lambda省去面向对象的条条框框,格式由3个部分组成:

- 一些参数
- 一个箭头
- 一段代码

Lambda表达式的**标准格式**为:

```markdown
(参数类型 参数名称) -> {代码语句}
```

格式说明:

- 小括号内的语法与传统方法参数列表一致:无参数则留空;多个参数则用逗号分隔.
- -> 是新引入的语法格式,代表指向动作
- 大括号内的语法与传统方法体要求基本一致

### 1.7 练习 : 使用Lambda标准格式(无参无返回)

**题目**

给定一个厨子Cook接口,内含唯一的抽象方法makeFood,且无参数,无返回值.如下:

```java
public interface Cook {
  void makeFood();
}
```

在下面的代码中,请使用Lambda的标准格式调用invoikeCook方法,打印输出“吃饭啦!”字样:

```java
public static void main(String[] args) {
  
}
public static void invokeCook(Cook cook) {
  cook.makeFood();
}
```

### 1.8 Lambda标准格式(有参数有返回值)

```java
 public static void main(String[] args) {
        Person[] arr = {
                new Person("柳岩",30),
                new Person("迪丽热巴",28),
                new Person("古力娜扎",20)
        };
   			// 传统写法
        Arrays.sort(arr, new Comparator<Person>() {
            @Override
            public int compare(Person o1, Person o2) {
                return o1.getAge() - o2.getAge();
            }
        });
        System.out.println(Arrays.toString(arr));
   			// Lambda
        Arrays.sort(arr,(Person o1,Person o2) -> {return o2.getAge() - o1.getAge();});
        System.out.println(Arrays.toString(arr));
 }
```

### 1.9 练习 Lambda标准格式(有参有返回)

```java
    public static void main(String[] args) {
        // 传统的
        invokeCalc(10, 11, new Calculator() {
            @Override
            public int calc(int a, int b) {
                return a+b;
            }
        });
        invokeCalc(10,11,(a,b)->{return a+b;});
    }
    private static void invokeCalc(int a,int b,Calculator calculator) {
        int result = calculator.calc(a,b);
        System.out.println("结果是：" + result);
    }
```

### 1.10 Lambda省略格式

#### 可推导即可省略

Lambda强调的是“做什么”而不是“怎么做”,所以凡事可以通过上下文推导得知的信息,都可以省略.例如上例还可以使用Lambda的省略写法:

```java
public static void main(String[] args) {
  invokeCalc(120,130,(a,b)->a+b);
}
```

#### 省略规则

在Lambda标准格式的基础上,使用省略写法的规则为:

1. 小括号内参数的类型可以省略

2. 如果小括号内有且仅有一个参数,则小括号可以省略

3. 如果大括号内有且仅有一个语句,则无论是否有返回值,都可以省略大括号,return关键字及语句分号

   > 如果要省略第三条,就要全部一起省略

###  Lambda的使用前提

Lambda的语法非常简洁,完全没有面向对象的复杂的约束,但是使用时有几个问题需要特别注意:

1. 使用Lambda必须具有接口,且要求接口中有且仅有一个抽象方法.

   无论是JDK内置的Runnable、Comparator接口还是自定义的接口,只有当接口中的抽象方法存在且唯一时,才可以使用Lambda.

2. 使用Lambda必须具有上下文推断

   也就是方法的参数或局部变量类型必须为Lambda对应的接口类型,才能使用Lambda座位该接口的实例.

> 备注:有且仅有一个抽象方法的接口,称为“函数式接口”

