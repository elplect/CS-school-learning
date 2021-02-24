### Spring JDBC

Spring框架对JDBC的简单封装,提供了一个JDBCTemplate对象简化JDBC的开发

步骤:

1. 导入jar包
2. 创建JdbcTemplate对象,依赖于数据源DataSource(连接池技术)
   1. `JdbcTemplate template = new JdbcTemplate(ds);`
3. 调用JdbcTemplate的方法来完成CRUD操作
   1. update() : 执行DML语句.增删改语句
   2. queryForMap() : 查询结果将结果集封装为map集合
   3. qurryForList() : 封装为List集合 List<Map<String,Object>>
   4. query() : 封装为javaBean对象
      1. list<Emp> list = template.query(sql,new BeanPropertyRowMapper<Emp>(Amp.class));
   5. queryForObject() : 封装为Object对象
      1. Long total = template.queryForObject(sql,Long.class);

---

```java
public static void main(String[] args) {
  // 1. 导入jar包
  // 2. 创建JDBCTemplate对象
  JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
  // 3. 调用方法
  String sql = "update account set balance = 5000 where id = ?";
  template.update(sql,3); // 不需要释放连接,不需要申请资源
  // 原先
  PreparedStatement pstmt = conn.prepareStatement(sql); // 申请资源
  pstmt.setInt(1,uid);
  pstmt.executeQuery();
  pstmt.close(); // 释放资源
}
```

