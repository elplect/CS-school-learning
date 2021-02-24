## IO字节流

### 第一章 IO概述

#### 1.1 什么是IO

生活中,你肯定经历过这样的场景,当你编辑一个文本文件,忘记了`ctrl+s`,可能文件就白白编辑了.当你电脑上插入一个U盘,可以把一个视频,拷贝到你的电脑硬盘里,那么数据都是在哪些设备上的呢?键盘、内存、硬盘、外接设备等等

我们把这种数据的传输,可以看作是一种数据的流动,按照流动的方向,以内存为基准,分为`输入input`和`输出output`,即流向内存是输入流,流出内存的输出流.

java中I/O操作主要是指使用`java.io`包下的内容,进行输入、输出操作.输入也叫做读取数据,输出也叫做写出数据.

#### 1.2 IO的分类

根据数据的流向分为:输入流和输出流

- 输入流: 把数据从`其他设备`上读取到`内存`中的流
- 输出流: 把数据从`内存`中写出到`其他设备`上的流

格局数据的类型分为:**字节流**和**字符流**

- **字节流** : 以字节为单位,读写数据的流
- **字符流** : 以字符为单位,读写数据的流

#### 1.3 顶级父类们

|        | 输入流                        | 输出流                         |
| ------ | ----------------------------- | ------------------------------ |
| 字节流 | 字节输入流<br>**InputStream** | 字节输出流<br>**OutputStream** |
| 字符流 | 字符输入流<br>Reader          | 字符输出流<br>Writer           |

## 第二章 字节流

### 2.1 一切皆为字节

一切文件数据(文本、图片、视频等)在存储时,都是以二进制数字的形式保存,都一个一个的字节,那么传输时一样如此.所以,字节流可以传输任意文件数据.在操作流的时候,我们要时刻明确,无论使用什么样的流对象,底层传输的始终为二进制数据.

### 2.2 字节输出流[OutputStream]

`java.io.OutputStream`抽象类是表示字节输出流的所有类的超类,将指定的字节信息写出到目的地.它定义了字节输出流的基本共性功能方法.

- `public void close()` : 关闭此输出流并释放与此流相关联的任何系统资源
- `public void flush()` : 刷新此输出流并强制任何缓冲的输出字节被写出
- `public void write(byte[] b)` : 将b.length字节从指定的字节数组写入次输出流
- `public void write(byte[] b,int off,int len)` : 从指定的字节数组写入len字节,从偏移量off开始输出到此输出流
- `public abstract void write(int b)` : 将指定的字节输出流

> 小贴士:
>
> close方法,当完成流的操作时,必须调用此方法,释放系统资源

### 2.3 FIleOutputStream类

`OutputStream`有很多子类,我们从最简单的子类开始

`java.io.FileOutStream`类是文件输出流,用于将数据写出到文件

#### 构造方法

- `FileOutputStream(String name)` 创建一个 可以向具有指定名称的文件中写入数据 的输出文件流。
- `FilePutputStrean(File file) `创建一个 可以向指定File对象表示的文件中写入数据 的文件输出流。

```
构造方法的作用 ：
    1.创建一个FileOutputStrean对象
    2.会根据构造方法中传递的文件/文件对象，创建一个空的文件
    3.会把FileOutputStream对象指向创建好的文件
```

#### 写出字节数据

> 原理(内存-->硬盘)
>
> ​	java-->JVM(java虚拟机)-->os(操作系统)-->os调用写数据的方法-->把数据写入到文件中

字节输出流的使用步骤(重点):

	1. 创建一个FileOutputStream对象,构造方法中传递写入数据的目的地
 	2. 调用FIleOutputStream对象中的write方法,把数据写入到文件中
 	3. 释放资源(流回占用一定的内存,使用完毕,清空内存,提高程序效率)

写出字节:`write(int b)`方法,每次可以写出一个字节数据,代码使用演示

```java
public static void main(String[] args) {
  FileOutput fos = new FIleOutputStream("fos.txt");
  fos.write(97);
  fos.close();
}
```

#### 数据追加续写

经过以上的演示,每次程序运行,创建输出流对象,都会清空目标文件中的数据.如何保留目标文件中数据,还能继续添加新数据呢?

- `public FileOutputStream(File file,boolean append)`

  创建文件输出流以写入由指定的File对象表示的文件.

- `public FileOutputStream(String name,boolean append)`

  创建文件输出流以指定的名称写入文件

这两个构造方法,参数重都需要传入一个boolean类型的值,`true`表示追加数据,false表示清空原油数据.这样创建的输出流对象,就可以指定是否追加续写了,代码使用演示:

```java
   public static void main(String[] args) throws IOException {
        FileOutputStream fos = new FileOutputStream("a.txt",true);
        fos.write("我是你爸爸".getBytes());
        fos.close();
    }
```

### 2.4 字节输入流

`java.io.InputStream`抽象类是表示字节输入流的所有类的超类,可以读取字节信息到内存里,它定义了字节输入流的基本共性功能方法.

- `public void close` : 关闭此输入流并释放与此流相关联的任何系统资源
- `public abstract int read()` : 从输入流读取数据的下一字节
- `public int read(byte[] b)` : 从输入流中读取一些字节数,并将它们存储到字节数组b中

> 小贴士
>
> close方法,当完成流的操作时,必须调用此方法,释放系统资源

### 2.5 FileInputStream类

`java.io.FileInputStream`类是文件输入流,从文件中读取字节

#### 构造方法

- `FileInputStream(File file)` : 通过打开与实际文件的链接来创建一个FIleInputStream,该文件由文件系统中的File对象file命名.
- `FileInputStream(String name)` : 通过打开与实际文件的链接来创建一个FIleInputStream,该文件由文件系统的路径名name命名.

当你创建一个流对象,必须传入一个文件路径,该路径下,如果没有该文件,会抛出`FileNotFoundException`

#### 读取字节数据

1. **读取字符**: read方法,每次可以读取一个字节的数据,提升为int类型,读取到文件末尾,返回-1
2. **读取多个字节**:read方法,每次可以读取多个字节的数据,并将其存储在缓冲区数组b中

## 第三章 字符流

当使用字节流读取文本文件时,会出一个小问题,遇到中文字符,可能不会显示完整店的字符,那是因为一个中文字符可能占用多个字节存储.所以java提供一些字符流类,以字符为单位读写数据,专门用于处理文本文件

### 3.1 字符输入流 Reader

`java.io.Reader`抽象类是表示用于读取字符流的所有类的超类,可以读取字符信息到内存,它定义了字符输入流的基本共性方法

- `public void close()` : 关闭此流以及相关联的任何系统资源
- `public int read()` :  从输入流读取一个字符
- `public int read(char[] cbuf)` : 从输入流中读取一些字符,并将他们存储到字符数组cbuf中

### 3.2 FileReader类

`java.io.FileReader`类是读取字符文件的便利类,构造时使用系统默认的字符编码和默认的字节缓冲区

### 3.3字符输出流 [Writer]

Writer抽象类是所有写出字符流的类的超类,将指定字符信息写出到目的地.定义了输出流的基本共性功能方法.

- void write(int c);
- void write(char[] cbuf);
- abstract void write(char[] cbuf,int off,int len)
- void write(String str)
- void write(String str,int off ,int len)
- void flush(); // 舒心该流的缓冲
- void close();

### 3.4 FIleWriter类

FileWriter fw = new FileWriter("aaa.txt",true);

是写出字符到文件的便利类,构造时使用系统默认的字符编码和默认字节缓冲区.

使用步骤

1. 创建FIlterWriter对象,构造方法中要写入数据的目的地
2. 使用write()方法,把数据写入到内存缓冲区中(字符转换为字节的过程)
3. 使用flush()方法,把内存缓冲区的数据,刷新到文件中
4. 释放资源

## 第四章 IO异常的处理

### JDK7前处理

之前的入门练习,我们一直把异常抛出,而实际开发中并不能这样处理,建议使用`try...catch...finally`代码块,处理异常部分,代码使用演示

```java

/*
    在jdk1.7之前使用try...catch....finally 处理流中的异常
    格式
        try{
            可能会产生异常的代码
        }catch(异常类变量 变量名) {
            异常的处理逻辑
        }finally{
            一定会执行的带啊么
            资源释放
        }
 */
public class Demo2 {
    public static void main(String[] args) throws IOException {
        // 提高变量fw的作用域，让finally可以使用
        // 变量在定义地1时候，可以没有值，但是使用的时候必须有值
        // fw = new FileWreiter("a.txt",true); 执行失败，fw没有值，fw.close会报错
        FileWriter fw = null;
        try {
            // 可能会出现异常的代码
            fw = new FileWriter("a.txt",true);
            for (int i=0;i<10;i++) {
                fw.write("hello,world"+i + "\r\n");
            }
        } catch (IOException e) {
            // 异常的处理逻辑
            System.out.println(e);
        }finally {
            // 一定会执行的代码
            // 创建对象失败了，fd的默认值就是null，null是不能调用方法的，会抛出NullPointerException，需要增加一个大判断，不是null就把资源都释放
            if (fw!=null) {
                try{

                    // fw.close方法声明抛出了IOException异常对象，所以我们就要处理这个异常对象.要么throw要么try...catch
                    fw.close();
                }catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

### JDK7后的新特性

```java
/*
    JDK7的新特性
    在try的后边可以增加一个(),在括号中可以定义流对象
    要么这个流对象的作用域就在try中有效
    try中的代码执行完，会自动把流对象释放，不用谢finally
    格式
        try(定义流对象;定义流对象...) {
            可能会产生异常的代码
        }catch(异常类变量 变量名) {
            异常的处理逻辑
        }
 */
public class demo3 {
    public static void main(String[] args) {
        try(    // 1.创建一个字节输入流对象，构造方法中绑定要读取的数据源
                FileInputStream fis = new FileInputStream("src/com/elplect/第三章/File类与IO流/IO字符流/a/a");
                // 2.创建一个字节输出流对象，构造方法中国绑定要写入的目的地
                FileOutputStream fos = new FileOutputStream("src/com/elplect/第三章/File类与IO流/IO字符流/b/b");
                )
        {
            // 一次读取一个字节写入一个字街的方式
            // 3. 使用字节输入流对象中的防脱发read读取文件
            int len =0;
            while ((len = fis.read()) != -1) {
                // 4. 使用字节输出流中的方法write，把读取到的字节流传入到目的地文件中
                fos.write(len);
            }
        }catch (IOException e) {
            System.out.println(e);
        }
    }
}
```

### JDK9的新特性

```java
/*
    JDK9的新特性
    try的前面可以定义流对象
    在try后面的()可以直接引入流对象的名称(变量名)
    在try代码执行完毕之后，流对象也可以释放掉，不用写finally
    格式
        A a = new A();
        B b = new B();
        try(a,b) {
            可能会产生异常的代码
        }catch(异常类变量 变量名) {
            异常的处理逻辑
        }
 */
public class demo4 {
    public static void main(String[] args) throws FileNotFoundException {
        // 1.创建一个字节输入流对象，构造方法中绑定要读取的数据源
        FileInputStream fis = new FileInputStream("src/com/elplect/第三章/File类与IO流/IO字符流/a/a");
        // 2.创建一个字节输出流对象，构造方法中国绑定要写入的目的地
        FileOutputStream fos = new FileOutputStream("src/com/elplect/第三章/File类与IO流/IO字符流/b/b");
        try(fis;fos) {
            // 一次读取一个字节写入一个字街的方式
            // 3. 使用字节输入流对象中的防脱发read读取文件
            int len =0;
            while ((len = fis.read()) != -1) {
                // 4. 使用字节输出流中的方法write，把读取到的字节流传入到目的地文件中
                fos.write(len);
            }
        }catch (IOException e) {
            System.out.println(e);
        }
    }
}
```

