## Redis

概念

​	高性能的nosql系列的非关系型数据库 

下载安装

命令操作

​	数据结构

​		redis 存储的是 : key/value 

​			key是字符串

​			value ： string、hash(map)、列表list、集合set、有序集合sortedset

![image-20210224153722380](https://gitee.com/elplect/personal-image-bed/raw/master/beautyImg/image-20210224153722380.png)

​	string

​		set key value

​		get key value

​		del key

​	Hash

​		hset key field value.  Hset myhash name lisi hset myhash pad 123

​		hget key field.      Hget myhash name. 

​		hgetall myhash  获取整个hash列表

​		hdel key field.      

​	list  左边为头

​		lpush key value 左边加入

​		rpush key value 右边加入

​		lrange key start end

​		lpop key 从列表左边删除，并返回

​		rpop key 从列表右边删除，并返回

​	set 不允许重复元素

​		sadd key value….value

​		smembers key 获取所有元素

​		srem key value 删除set集合中的某个元素

​	sorted set 从小到大

​		zadd key score value

​		zrange key start end

​		zrange key start end withscores // 带分数

​		zren key value

​	通用命令：

​		keys * 获取所有的键

​		type key 获取键对应的value类型

​		del key 删除指定key的value

持久化操作

​	redis是一个内存数据库

​	当redis服务器重启或电脑重启，数据会丢失，

​	可以将redis内存中的数据持久化保存到硬盘的文件中

​	redis持久化机制:

​		1. RDB: 默认方式，不需要进行配置，默认

​			在一定的时间间隔中，检测key的变化情况，然后持久化数据

​			编辑redis.windows.conf文件

​				save 900 1.    15分钟改变一个key，持久化

​				save 300 10	 6分钟改变10个key，持久化

​				save 60 10000	 1分钟改变10000个key，持久化

​			重新重启redis服务器，并指定配置文件的名称

​				redis-server.exe redis.windows.conf

​		2. AOF:日志记录的方式，可以记录每一条命令的操作，

​				在每一次操作后持久化数据

​			性能影响很大

​			编辑redis.windows.conf文件

​				appendonly no 改为 appendonly yes

​				appendfsync always : 每一次操作持久化

​				appendfsync everysec : 每隔1s持久化1次

​				appendfsync no： 不进行持久化

使用java客户端操作redis

​	jedis： 一款java操作redis数据库的工具

​	使用步骤:

​		1. 下载jedis的jar包

​		2. 导包

​			new Jedis(“localhost”,6379);

​			jedis.set(“username”,”lisi”)

​			jedis.close()

​		特殊方法

​			jedis.setex(“activecode”,20,”hehe”); // 20s后自动删除



##  **jedis连接池**

jedisPool

获取客户端连接，直接从连接池获取，更好的复用和管理

​	使用

​		0. 创建配置对象

​		1. 创建JedisPool连接池对象

​		2. 调用方法 getResource() 获取链接

​	JedisPoolConfig config = new JedisPoolConfig()

​	config.setMaxTotal(50);

​	config.setMaxle(10);

​	JedisPool jedisPool = new JedisPool()

​	Jedis jedis = jedisPool.getResource()

​	然后直接用jedis

​	jedis.close(); // 直接归还