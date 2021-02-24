## 第一章File类

### 1.1 概述

`java.io.FIle`类是文件和目录路径名的抽象表示,主要用于文件和目录的创建、查找和删除等操作.

```java
static String pathSeparator 与系统有关的路径分隔符，为了方便，它被表示为一个字符串
static char pathSeparatorChar 与系统有关的路径分隔符
static String separator 与系统有关的默认名称分隔符，为了方便，它被表示为一个字符串
static char separatorChar 与系统有关的默认名称分隔符
```

### 1.2 构造方法

- `public File(String pathname)` : 通过给定的路径名字符串转换为抽象路径名来创建新的File实例
- `public FIle(String parent,String child)`: 从父路径名字符串和子路径名字符串创建新的FIle实例
- `public File(FIle parent,String child)`: 从父抽象路径名和子路径名字符串创建新的FIle实例

构造举例:

```java
// public File(String pathname)
// 路径可以是存在的,也可以是不存在的.
// 创建File对象,只是把字符串路径封装为File对象,不考虑路径的真假情况
String pathname = "D:\\aaa.txt";
File file1 = new File(pathname);
String pathname2 = "D\\aaa\\bbb.txt";
File file2 = new File(pathname);
File file3 = new File("b:txt"); // 相对路径
// public FIle(String parent,String child)
File file4 = new File("c:\\","a.txt");
// public File(FIle parent,String child)
File fu = new FIle("c:\\");
File zi = new File(fu,"a.txt");
```

### 1.3 常用方法

#### 获取方法的功能

- `public String getAbsolutePath()`: 返回此File的绝对路径名字符串
- `public Strign getPath()` : 将此File转换为路径名字符串
- `public String getName()` : 返回由此File表示的文件或目录的名称
- `public long length()` : 返回由此File表示的文件的长度
  - 文件夹是没有大小概念的,不能获取文件夹的大小
  - 如果构造方法中的给出的路径不存在,返回0

#### 判断功能的方法

- `public boolean exists()` : 此File表示的文件或目录是否实际存在
- `public boolean isDirectory()`: 此File表示的是否为目录
- `pubic boolean isFile()`: 此File表示的是否为文件

#### 创建删除功能的方法

- `public boolean createNewFile()` : 当且仅当具有该名称的摁键上不存在时,常见一个新的空文件
- `public boolean delete()` : 删除由此File表示的文件或目录
- `public boolean mkdir()` : 创建由此File表示的目录
- `public boolean mkdirs()` : 创建由此File表示的目录,包括任何必须但不存在的父目录

### 1.4 目录的遍历

- `public String[] list()`: 返回一个String数组,表示该FIle目录中的所有子文件或目录
- `public File[] listFiles()` : 返回一个File数组,表示该File目录中的所有的字文件或目录.

> 注意:
>
> 调用listFiles方法的File对象,表示的必须是实际存在的目录,不然返回null,无法进行遍历
>
> 如果构造方法中给出的目录路径不存在,空指针异常
>
> 如果构造方法中的路径不是一个目录,空指针异常

---

## 第二章 综合案例

### 2.1 文件搜索

搜索`D:/aaa`目录下的`.java`文件

**分析:**

1. 目录搜索,无法判断多少级目录,搜一使用递归,遍历所有目录
2. 遍历目录时,获取的字文件,通过文件名称,判断是否符合条件

### 2.2文件过滤器优化

`java.io.FileFilter`是一个接口,是File的过滤器,该接口的对象可以传递给File类的`listFiles(FileFilter)`作为参数,接口中只有一个方法

`boolean accept(File pathname)` : 吃的是pathname是否应该包含在当前FIle目录中,符合则返回true

**分析:**

1. 接口作为参数,需要传递子类对象,重写其中方法,我们选择匿名内部类的形式

2. accept方法,参数为File,表示当前File下的所有子文件和子目录,保留则返回true,过滤掉则返回false

   保留规则:

   	1. 要么是.java文件
    	2. 要么是目录,用于继续遍历

```java
    文件过滤器
    需求：
        遍历c:\\abc文件夹，及abc文件夹的子文件夹
        只有.java结尾的文件
        我们可以使用过滤器来实现
        在File类中有两个和ListFiles重做的方法，方法的参数传递的就是过滤器
-------------------------------------------------------------------------------------------------
File[] listFiles(FileFilter filter)
java.io.FileFilter接口：用于抽象路径名(File对象)的过滤器
    作用：用来过滤文件(File对象)
    抽象方法：用来过滤文件的方法
        boolean accept(File pathname) 测试指定抽象路径名是否应该包含在某个路径名列表中
        参数：
            File pathname 使用ListFiles方法遍历目录，得到的每一个文件对象
File[] listFIles(FilenameFilter filter)
java.io.FilenameFilter接口 实现此接口的类实例可用于过滤器文件名
    作用：用于过滤文件名称
    抽象方法：用来过滤文件的方法
        boolean accept(File dir,String name) 测试指定文件是否应该包含在某一文件列表中
        参数
            File dir 构造方法中传递的被遍历的目录
            String name 使用listFiles方法遍历目录，获取的每一个文件/文件夹的名称
注意：
    两个过滤器接口是没有实现类的，需要我们自己写实现类，重写过滤的方法accept，在方法中自己定义过滤的规则
```

```java
FileFilter
    public static void main(String[] args) {
        File file = new File("src");
        getAllFile(file);
    }
    public static void getAllFile(File dir) {
        System.out.println(dir);//打印被遍历的目录名称
//        File[] files = dir.listFiles(new FileFilterImpl());
        // 匿名内部类
        File[] files = dir.listFiles(new FileFilter() {
            @Override
            public boolean accept(File pathname) {
                if (pathname.isDirectory()) {
                    return true;
                }
                return pathname.getName().toLowerCase().endsWith(".java");
            }
        });
        for (File f : files) {
            if (f.isDirectory()) {
                getAllFile(f);
            }else {
                System.out.println(f);
            }
        }
    }
FilenameFileter filter
    public static void main(String[] args) {
        File file = new File("src");
        getAllFile(file);
    }
    public static void getAllFile(File dir) {
        System.out.println(dir);//打印被遍历的目录名称
//        File[] files = dir.listFiles(new FileFilterImpl());
        // 匿名内部类
        File[] files = dir.listFiles(new FilenameFilter() {
            @Override
            public boolean accept(File dir, String name) {
                return new File(dir,name).isDirectory() || name.toLowerCase().endsWith(".java");
            }
        });
//        lambda简化
        File[] files1 = dir.listFiles((dir1, name) -> new File(dir1,name).isDirectory() || name.toLowerCase().endsWith(".java"));
        for (File f : files) {
            if (f.isDirectory()) {
                getAllFile(f);
            }else {
                System.out.println(f);
            }
        }
    }
```



