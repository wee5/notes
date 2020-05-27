# 其他：java-redis1：jedis



该演示不是springboot的注解实现的配置方式，ssm框架项目可以使用这种配置

## redis，jedis，spring data redis

redis是一种缓存工具

jedis是redis的原生java jar包

spring是spring推出的封装了jedis的jar包



## 依赖

```xml
<!-- jedis -->
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>3.2.0</version>
</dependency>
```



## 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd
    http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <!-- 引入jedis的properties配置文件 -->
    <!--如果你有多个数据源需要通过<context:property-placeholder管理，且不愿意放在一个配置文件里，那么一定要加上ignore-unresolvable=“true"-->
    <context:property-placeholder location="classpath:redis.properties" ignore-unresolvable="true" />
    <!--Jedis连接池的相关配置-->
    <bean id="jedisPoolConfig" class="redis.clients.jedis.JedisPoolConfig">
        <!--新版是maxTotal，旧版是maxActive-->
        <property name="maxTotal">
            <value>${redis.pool.maxActive}</value>
        </property>
        <property name="maxIdle">
            <value>${redis.pool.maxIdle}</value>
        </property>
        <property name="testOnBorrow" value="true"/>
        <property name="testOnReturn" value="true"/>
    </bean>
    <bean id="jedisPool" class="redis.clients.jedis.JedisPool">
        <constructor-arg name="poolConfig" ref="jedisPoolConfig" />
        <constructor-arg name="host" value="${redis.host}" />
        <constructor-arg name="port" value="${redis.port}" type="int" />
        <constructor-arg name="timeout" value="${redis.timeout}" type="int" />
        <constructor-arg name="password" value="${redis.password}" />
        <constructor-arg name="database" value="${redis.database}" type="int" />
    </bean>
</beans>
```

```properties
#最大分配的对象数
redis.pool.maxActive=200
#最大能够保持idel状态的对象数
redis.pool.maxIdle=50
redis.pool.minIdle=10
redis.pool.maxWaitMillis=20000
#当池内没有返回对象时，最大等待时间
redis.pool.maxWait=300
#格式：redis://:[密码]@[服务器地址]:[端口]/[db index]
redis.uri = redis://:12345@127.0.0.1:6379/0
redis.host = 127.0.0.1
redis.port = 6379
redis.timeout=30000
redis.password = 123456
redis.database = 0
```



## 工具类

```java
//序列化工具
@Component
public class SerializeUtil {
    /**
     * 序列化
     *
     * @param object
     * @return
     */
    public static byte[] serialize(Object object) {
        ObjectOutputStream oos = null;
        ByteArrayOutputStream baos = null;
        try {
            // 序列化
            baos = new ByteArrayOutputStream();
            oos = new ObjectOutputStream(baos);
            oos.writeObject(object);
            byte[] bytes = baos.toByteArray();
            return bytes;
        } catch (Exception e) {

        }
        return null;
    }
    public static Object deserialize(byte[] bytes) {
        if (bytes == null) {
            return null;
        }
        ByteArrayInputStream bais = null;
        try {
            bais = new ByteArrayInputStream(bytes);
            ObjectInputStream ois = new ObjectInputStream(bais);
            return ois.readObject();
        } catch (Exception e) {
            throw new CacheException(e);
        }
    }
}
```

```java
//redis连接池
public class MyJedisPool {
    //Redis服务器IP
    private static String ADDR = "127.0.0.1";
    //Redis的端口号
    private static int PORT = 6379;
    //访问密码
    private static String AUTH = "123456";
    //可用连接实例的最大数目，默认值为8；
    //如果赋值为-1，则表示不限制；如果pool已经分配了maxActive个jedis实例，则此时pool的状态为exhausted(耗尽)。
    private static int MAX_ACTIVE = 1024;
    //控制一个pool最多有多少个状态为idle(空闲的)的jedis实例，默认值也是8。
    private static int MAX_IDLE = 200;
    //等待可用连接的最大时间，单位毫秒，默认值为-1，表示永不超时。如果超过等待时间，则直接抛出JedisConnectionException；
    private static int MAX_WAIT = 10000;
    private static int TIMEOUT = 10000;
    //在borrow一个jedis实例时，是否提前进行validate操作；如果为true，则得到的jedis实例均是可用的；
    private static boolean TEST_ON_BORROW = true;
    private static redis.clients.jedis.JedisPool jedisPool = null;
    /**
     * 初始化Redis连接池
     */
    static {
        try {
            JedisPoolConfig config = new JedisPoolConfig();
//            config.setMaxActive(MAX_ACTIVE);
            config.setMaxIdle(MAX_IDLE);
//            config.setMaxWait(MAX_WAIT);
            config.setTestOnBorrow(TEST_ON_BORROW);
            jedisPool = new redis.clients.jedis.JedisPool(config, ADDR, PORT, TIMEOUT, AUTH);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取Jedis实例
     * @return
     */
    public synchronized static Jedis getJedis() {
        try {
            if (jedisPool != null) {
                Jedis resource = jedisPool.getResource();
                return resource;
            } else {
                return null;
            }
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }

}
```



## 使用

```java
//直接使用jedis
public class JedisTest {

    public static void main(String[] args) {
        //连接本地的 Redis 服务
        Jedis jedis = new Jedis("localhost");
        System.out.println("连接成功");
        //查看服务是否运行
        System.out.println("服务正在运行: "+jedis.ping());

        Man man = new Man("weezy",38);

        jedis.set("string1","set-value1");
        jedis.hset("hash1","name",man.getName());
        jedis.hset("hash1","age",String.valueOf(man.getAge()));

        jedis.lpush("list1","list-value1");
        /*jedis.sadd("set1","set-value1");*/
        jedis.zadd("zadd",1,"zadd-value1");
    }
}
```

```java
//通过连接池获取jedis，再使用jedis
public class MyJedisPoolTest {

    public static void main(String[] args) {
        Jedis jedis = MyJedisPool.getJedis();
        System.out.println(jedis.getClient().getPort());
        System.out.println(jedis.getClient().getHost());

        jedis.set("allie","peoplewell");
        jedis.hset("person","name","fit");
        jedis.hset("person","age","37");

    }

}
```