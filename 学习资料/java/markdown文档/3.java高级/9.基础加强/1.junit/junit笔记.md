## 第一章 junit

junit单元测试

测试类和测试代码

### junit单元测试

测试分类

- 黑盒测试
  - 输入,测试,不需要写代码,输出是否符合预期结果
- 白盒测试
  - 输入,能看到怎么执行,需要写代码,关注代码具体执行流程

#### junit使用 : 白盒测试

步骤

1. 定义一个测试类(测试用例)
   1. 建议
      1. 测试类名 : 被测试的类名Test  CalculatorTest
      2. 包名 : xxx.xxx.xx.test   cn.itcast.test
2. 定义测试方法 : 可以独立运行
   1. 建议
      1. 方法名 : test测试的方法名  testAdd
      2. 返回值 : void
      3. 参数类表 : 空参
3. 给方法加注解 @Test
4. 导入junit依赖环境

判定结果:

​	红色 : 失败

​	绿色 : 成功

​	一般我们会使用断言操作来处理结果(期望的结果,运算的结果);

### @before

```java
/**
* 初始化方法:
*  用于资源申请,所有测试方法在执行之前都会先执行该方法
*/
@Before
public void init() {
  System.out.println("da");
}
```

### @after

```java
/**
* 释放资源方法
*  在所有测试方法执行完后,都会自动执行该方法
*/
@After
public void close() {
  System.out.println("close...");
}
```





