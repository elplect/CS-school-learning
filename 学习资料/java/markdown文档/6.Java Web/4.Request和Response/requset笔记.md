## Request

servletRequset对应的就是请求消息数据



### 1.request和response原理

![image-20201126172539449](requset%E7%AC%94%E8%AE%B0.assets/image-20201126172539449.png)

注意 : 

1. request对象和response对象的原理
   1. request和response对象是由服务器创建的,我们来使用它
   2. request对象是获取请求信息,response对象是来设置响应消息.
2. request对象继承体系结构 : 
   1. ServletRequest-->HttpServletRequest   接口  继承关系
   2. Org.apache.catalina.connector.RequestFacade是实现类
3. request功能 :
   1. 获取请求消息数据
      1. 获取请求行
         1. GET /day/demo1?name=zhangsan HTTP/1.1
         2. 方法 : GET
            1. 获取请求方式 : GET
               1. String getMethod()
            2. **获取虚拟目录 : /day14**
               1. String getContextPath()
            3. 获取servlet路径 : /demo1
               1. String getServletPath()
            4. 获取get方式请求参数 : name=zhangsan
               1. String getQueryString()
            5. **获取请求的url : /day14/demo1**
               1. String getRequestURI() : /day14/demo1
               2. StringBuffer getRequestURL() : http://localhost/day14/demo1
            6. 获取协议及版本 : HTTP/1.1
               1. String getProtocal()
            7. 获取客户机的IP地址 :
               1. String getRemoteAddr()
      2. 获取请求头
         1. 方法
            1. **String getHeader(String name):通过请求头的名称来获取请求头的值**
            2. Enumeration<String> getHeaderNames():获取所有的请求头名称
      3. 获取请求体
         1. 只有POST请求方式,才有请求体,在请求体中封装了POST请求的请求参数
         2. 步骤:
            1. 获取流对象
               1. BufferedReader getReader() 获取字符输入流,只能操作字符数据
               2. ServletInputStream getInputStream() : 获取字节输入流,可以操作所有类型的数据
            2. 再从流对象中拿数据
   2. 其他功能
      1. 获取请求参数通用方式 不论get还是post都可以使用了
         1. `String getParameter(String name);` 根据参数名称获取参数值 常用
         2. `String[] getParameterValues(String name); `通常是复选框 获取复数值
         3. `Enumeration<String> getParameterNames()`:获取所有请求的参数名称
         4. `Map<String,String[]> getParameterMap();` 获取所有参数的Map集合 常用
         5. 中文乱码问题
            1. get方式 : tomcat8已经将get方式的乱码解决了
            2. post方式 : 要设置流的编码 setCharacterEncoding("UTF-8")
      2. 请求转发 : 一种在服务器内部资源跳转的方式
         1. 步骤 : 
            1. 通过request对象获取请求转发器对象 : RequestDispatcher getRequestDispatcher(String path)
            2. 使用 RequestDispatcher对象来进行转发 : forword(request,response);
         2. 特点 :
            1. 浏览器地址栏路径没有发生变化
            2. 不能访问服务器外部的资源
            3. 转发是一次请求
      3. 共享数据
         1. 域对象 : 一个有作用范围的对象,可以在范围内共享数据
         2. requset域 : 代表一次请求范围内,一般用于请求转发的多个资源中共享数据
         3. 方法 : 
            1. setAttribute(name,obj); // 存储数据
            2. getAttribute(name);
            3. removeAttribute(name);
      4. 获取ServletContext
         1. ServletContext getServletContext();

---

```java
// 登陆案例
// form表单的action路径的写法
  // * 虚拟目录/ + Servlet的资源路径 @WebServlet("/")
// BeanUtils工具类,简化数据封装
  // 用于封装javaBean
  // 方法
setProperty();
BeanUtils.setProperty(user,"username","zhangsna");
getProperty();
BeanUtils.getProperty(user,"username");
populate(); // 最重要
populate(Object obj,Map map); // 将map集合的键值对信息封装到对应的javaBean对象中
// 需要form表单的name 值与javaBean的属性名一一对应
```



