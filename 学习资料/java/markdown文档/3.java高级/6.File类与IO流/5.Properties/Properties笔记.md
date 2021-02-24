## Properties

### 1.1 概述

`java.util.Properties`继承于`Hashtable`,来表示一个吃酒的属性集,它使用键值结构存储数据,每个键及其对应值都是一个字符串,高发i类也被许多java类使用,比如获取系统属性时,System.getProperties方法就是返回一个Properties对象

```java
java.uitl.Properties集合 extends Hashtable<> implements Map<K,V>
Properties类表示了一个持久的属性集，Properties可保存在流中，或从流中加载
Properties集合是一个唯一和IO流相结合的集合
    可以使用Properties集合中的方法store，把集合中的临时数据，持久化写入到硬盘中存储
    可以使用Properties集合中的方法load，把硬盘中保存的文件(键值对),读取到集合中使用
属性列表中每个键及其对应值都是一个字符串
    Properties集合是一个双列集合，key和value默认都是字符串
```

### 1.2 Properties类

#### 构造方法

- `public Properties()` : 创建一个空的属性列表

#### 基本的存储方法

- `public Object setProperty(String key,String value)` : 保存一对属性
- `public String getProperty(String key)` : 使用此属性列表中指定的键搜索属性值
- `public Set<String> stringPropertyNames()` : 所有键的名称的集合

```java
public static void main(String[] args) {
    /*
        使用Properties集合存储数据，遍历取出Properties集合中的数据
        Properties集合有一些操作字符串的特有方法
            Object setProperty(String key,String value) 调用Hashtable的方法put
            String gtProperty(String key) 通过键找到value的方法，此方法相当于Map集合中的get(key)方法
            Set<String> stringPropertyNames() 返回此属性列表中的键集，其中该键及其对应值都是字符串，此方法相当于Map集合中的ketSet方法
     */
    // 创建Properties集合对象
    Properties prop = new Properties();
    // 使用 setProperty往集合中添加数据
    prop.setProperty("赵丽颖","168");
}
```

#### 写入硬盘

```java
   可以使用Properties集合中的方法store，把集合中的临时数据，持久化写入到硬盘中存储
   void store(OutputStream out,String comments)
   void store(Writer writer,String comments)
   参数
       OutputStream out：字节输出流，不能写入中文
       writer writer : 字符输出流 能写入中文
       String comments : 注释，用来解释说明保存的文件是做什么的
               不能使用中文，会产生乱码，默认是Unicode编码
               一般使用""空字符串
   使用步骤：
       1. 创建Properties集合对象，添加数据
       2. 创建字节输出流/字符输出流对象，构造方法中绑定要输出的目的地
       3. 使用Properties集合中的方法store，把集合中的数据持久化的存储到硬盘中
       4. 释放资源
         
public class demo2 {
    public static void main(String[] args) throws IOException {
        // 1. 创建Properties集合对象，添加数据
        Properties prop = new Properties();
        prop.setProperty("赵丽颖","168");
        prop.setProperty("古力娜扎","173");
        prop.setProperty("迪丽热巴","175");
        // 2. 创建字节输出流/字符输出流对象，构造方法中绑定要输出的目的地
        FileWriter fw = new FileWriter("b.txt");
        // 3. 使用Properties的方法store，把集合中的临时数据，持久化写入硬盘中存储
        prop.store(fw,"save data"); // "save data"是一个注释，实际内容还是fw里的
        prop.store(new FileOutputStream("b1.txt"),"save_data"); // 乱码
        // 4. 释放资源
        fw.close();
    }
}
```

#### 从硬盘中读取属性列

```java
    可以使用Properties集合中的方法load，把硬盘中保存的文件(键值对),读取到集合中使用
        void load(InputStream inStream)
        void load(Reader reader)
    使用步骤
        1. 创建Properties集合对象
        2. 使用load方法读取
        3. 遍历Properties集合
    注意
        1. 存储键值对的文件中，键与值默认的连接符号可以使用=，空格(其他符号)
        2. 存储键值对的文件中，可以使用#进行注释，被注释的键值对不会再被读取
        3. 存储键值对的文件中，键与值默认都是祖父穿，不用再加引号

public class demo3 {
    public static void main(String[] args) throws IOException {
        // 1. 创建Properties集合对象
        Properties prop = new Properties();
        // 2. 使用load方法读取
        prop.load(new FileReader("b.txt"));
        // 3. 遍历Properties集合
        Set<String> set = prop.stringPropertyNames();
        for (String s : set) {
            System.out.println(s + ":" + prop.get(s));
        }
    }
}
```

