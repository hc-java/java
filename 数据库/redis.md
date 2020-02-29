官网：<https://redis.io/>

学习：<https://www.runoob.com/redis/redis-tutorial.html>

# 下载

下载详细介绍：<https://blog.csdn.net/qq_29291085/article/details/77489342>

下载地址：<https://github.com/microsoftarchive/redis/releases>

# 安装

安装成功：

![1582682071786](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1582682071786.png)



# 运行测试

- 打开cmd窗口，进入redis安装目录

- 输入 redis-server.exe redis.windows.conf，显示上图则安装成功

- 重新打开一个cmd窗口

- 输入 redis-cli.exe -h 127.0.0.1 -p 6379

- ```
  set myKey abc
  get myKey
  
  ```

![1582683256741](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1582683256741.png)



把redis 的路径加到系统的环境变量里；所以使用 redis-server.exe 可以开启redis服务器

# redis桌面工具

安装:redis Desktop Manager

![1582772628627](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1582772628627.png)

# 简介

高性能的key-value数据库,经常用来缓存数据

- 数据库持久化：可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
- 支持多种数据结构存储：Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储。
- 可以备份：Redis支持数据的备份，即master-slave模式的数据备份。



# 数据类型

Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。

![1582726163490](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1582726163490.png)



# 语法

基本语法：命令 键名

redis 127.0.0.1:6379> COMMAND KEY_NAME

redis key 命令：

<https://www.runoob.com/redis/redis-keys.html>

![1582761166860](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1582761166860.png)

![1582761189555](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1582761189555.png)



## 字符串(String)



<https://www.runoob.com/redis/redis-strings.html

>

## 哈希(Hash)



hash 是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于存储对象

<https://www.runoob.com/redis/redis-hashes.html>



## 列表(List)



Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）

<https://www.runoob.com/redis/redis-lists.html>



## 集合(Set)



Redis 的 Set 是 String 类型的无序集合。集合成员是唯一的，这就意味着集合中不能出现重复的数据。

Redis 中集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)。

<https://www.runoob.com/redis/redis-sets.html>



## 有序集合(sorted set)



Redis 有序集合和集合一样也是string类型元素的集合,且不允许重复的成员。

不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。

有序集合的成员是唯一的,但分数(score)却可以重复。

集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)。



<https://www.runoob.com/redis/redis-sorted-sets.html>



# springboot整合redis

## pom.xml

导入xml

~~~xml
    <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-redis</artifactId>
            <version>1.3.8.RELEASE</version>
        </dependency>
~~~



## application.yml



报错：缺少jedis

![1582812905729](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1582812905729.png)

正确的redis配置：

~~~yml
spring:
  # Redis
  redis:
    host: localhost # Redis服务器地址
    database: 0 # Redis数据库索引（默认为0）
    port: 6379 # Redis服务器连接端口
    password: # Redis服务器连接密码（默认为空）
    jedis:
      pool:
        max-active: 8 # 连接池最大连接数（使用负值表示没有限制）
        max-wait: -1ms # 连接池最大阻塞等待时间（使用负值表示没有限制）
        max-idle: 8 # 连接池中的最大空闲连接
        min-idle: 0 # 连接池中的最小空闲连接
    timeout: 3000ms # 连接超时时间（毫秒）
~~~



## 实现(add)

~~~java
	 @Resource
    private RedisTemplate<Object, Object> redisTemplate;

  /**
     * 设置保存string类型到redis
     */
    @RequestMapping(value = "/setAndGetString")
    public void setAndGetString(){
        
        redisTemplate.opsForValue().set("key1","1");
        redisTemplate.opsForValue().set("key2","2");
        redisTemplate.opsForValue().set("key3","3");

        System.out.println(redisTemplate.opsForValue().get("key1"));
    }

    /**
     * 设置保存map类型到redis
     */
    @RequestMapping(value = "setAndGetMap")
    public void setAndGetMap(){
        Map<String,Object> map = new HashMap<>();
        map.put("name","张三");
        map.put("age","18");
        map.put("gender","男");

        redisTemplate.opsForHash().putAll("userInfo",map);
        redisTemplate.opsForHash().put("userInfo","sel","111");

        System.out.println(redisTemplate.opsForHash().entries("userInfo"));
        System.out.println(redisTemplate.opsForHash().keys("userInfo"));
        System.out.println(redisTemplate.opsForHash().values("userInfo"));
        System.out.println(redisTemplate.opsForHash().get("userInfo","name"));
    }

    /**
     * 设置保存list类型到redis
     */
    @RequestMapping(value = "setAndGetList")
    public void setAndGetList(){

        List<String > list1 = new ArrayList<>();
        list1.add("hello1");
        list1.add("world1");

        List<String > list2 = new ArrayList<>();
        list2.add("hello2");
        list2.add("world2");

        //会报转换异常
        redisTemplate.opsForList().leftPush("leftList",list1);
        redisTemplate.opsForList().rightPush("rightList",list2);
        System.out.println(redisTemplate.opsForList().leftPop("leftList"));
        System.out.println(redisTemplate.opsForList().rightPop("rightList"));
    }
    /**
     * 设置保存set类型到redis
     */
    @RequestMapping(value = "setAndGetSet")
    public void setAndGetSet(){

        SetOperations<Object, Object> set = redisTemplate.opsForSet();
        set.add("numberSet","1");
        set.add("numberSet","2");
        set.add("numberSet","3");
        Set<Object> resultSet =redisTemplate.opsForSet().members("numberSet");
        System.out.println("resultSet:"+resultSet);

    }


    /**
     * 设置保存zset类型到redis
     */
    @RequestMapping(value = "setAndGetZSet")
    public void setAndGetZSet(){

        ZSetOperations<Object,Object> zSetOperations = redisTemplate.opsForZSet();
        zSetOperations.add("zSet","one",1);
        zSetOperations.add("zSet","two",2);
        zSetOperations.add("zSet","three",3);

        System.out.println(redisTemplate.opsForZSet().range("zSet",1,3));
        System.out.println(redisTemplate.opsForZSet().range("zSet",0,zSetOperations.size("zSet")));


    }
~~~

## redis乱码

redis解决乱码配置

~~~java
@Component
public class RedisTemplateUtil {

    @Autowired
    private RedisTemplate redisTemplate;
    @Bean
    public RedisTemplate<String, Object> redisTemplate() {
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(new StringRedisSerializer());
        redisTemplate.setHashValueSerializer(new StringRedisSerializer());
        return redisTemplate;
    }
}
~~~



## redis其它用处

<https://www.cnblogs.com/jing1617/p/9075105.html>

可以发布、订阅的实时消息系统

队列应用

计数器应用

数据排重