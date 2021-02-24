## XML

干不过html,开始干properties.....

甚至后期还被JSON干

### 1、概念

概念 : Extensible Markup Language 可扩展标记语言

- 可扩展 : 标签都是自定义的.. HTML的标签都是规定好的,
- 功能
  - 存储数据
    1. 配置文件
    2. 在网络中传输
- xml和html的区别
  - xml标签都是自定义的,html是预定义的
  - xml的语法非常严格,html语法宽松
  - xml是存储数据的,html是展示数据的

### 2、语法

基本语法 :

1. xml文档的后缀名 .xml
2. xml第一行必须定义为文档声明 <?xml version='1.0' ?>
3. xml文档中有且仅有一个根标签,必须有一个
4. 属性值必须使用引号(单双都可以)引起来
5. 标签必须正确关闭
6. xml标签名称区分大小写

快速入门

```xml
<?xml version='1.0' ?>
<users>
  <user id='1'>
    <name>zhangsan</name>
    <age>23</age>
    <gender>male</gender>
  </user>
  <user id='2'>
    <name>zsan</name>
    <age>23</age>
    <gender>female</gender>
  </user>
</users>
```

组成部分 : 

1. 文档声明
   1. 必须定义第一行
   2. <?xml 属性列表 ?>
      1. version : 版本号 必须的属性 一般1.0
      2. encoding : 编码方式
         1. 告知解析引擎当前文档使用的字符集 默认 : ISO-8859-1
         2. 编码格式和解码格式必须统一 高级开发工具会自动识别
      3. standalone : 是否独立
         1. yes : 独立 不依赖于其他文件
         2. no : 依赖于约束文件
2. 指令(了解) 结合css,然而竞争不过html.
   1. <?xml-stylesheet type="text/css" href="a.css" ?>
3. 标签(自定义的)
   1. 规则
      1. 不能以xml开始
      2. 不能以数字、标点开始
      3. 不能包含空格
4. 属性
   1. id属性值唯一
5. 文本
   1. CDATA区 : 在该区域的会被原样展示
      1. 格式 <![CDATA[ 数据 ]]>

### 3、解析

谁编写xml? ———— 用户

谁解析xml? ———— 软件

![image-20201019001322444](XML%E7%AC%94%E8%AE%B0.assets/image-20201019001322444.png)

约束 : 规定xml文档的书写规则

- 作为框架使用者:

  - 能够在xml中引入约束文档
  - 能够简单的读懂约束文档

- 分类

  - DTD : 一种简单的约束技术
    - 内部dtd : 将约束规则定义在xml文档中
    - 外部dtd : 将约束的规则定义在外部文件中
      - 本地 :  <!DOCTYPE 根标签名 SYSTEM "dtd文件的位置">
      - 网络 :  <!DOCTYPE 根标签名 PUBLIC "dtd文件的名字" "dtd文件的位置URL">
    - 缺陷 : 只能约束格式,约束不了 内容
  - Schema : 一种复杂的约束技术
    - 

  

