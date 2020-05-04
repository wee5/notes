# redis5：示例



* 在spring中整合redis，用redis连接池创建连接，而不直接创建Jedis

  * redis.clients.jedis.JedisPool：单机环境适用的redis连接池
  * redis.clients.jedis.ShardedJedisPool：基于hash算法的一种分布式集群redis客户端连接池

* 依赖

  * spring的相关依赖

  * ```xml
    <!-- redis依赖包 -->
    <dependency>
     <groupId>redis.clients</groupId>
     <artifactId>jedis</artifactId>
     <version>2.9.0</version>
    </dependency>
    ```



## JedisPool

* 配置文件

  * ```properties
    #redis.properties文件
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
    redis.password = 12345
    redis.database = 0
    ```

  * ```xml
    <!-- spring-redis.xml文件 -->
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

* 使用

  * ```java
    @Autowired
    private JedisPool jedisPool;//注入JedisPool
    @RequestMapping(value = "/demo_set",method = RequestMethod.GET)
    @ResponseBody
    public String demo_set(){
      //获取ShardedJedis对象
      Jedis jedis = jedisPool.getResource();
      //存入键值对
      jedis.set("key2","hello jedis one");
      //回收ShardedJedis实例
      jedis.close();
      return "set";
    }
    @RequestMapping(value = "/demo_get",method = RequestMethod.GET)
    @ResponseBody
    public String demo_get(){
      Jedis jedis = jedisPool.getResource();
      //根据键值获得数据
      String result = jedis.get("key2");
      jedis.close();
      return result;
    }
    ```



## ShardedJedis

* 配置文件

  * ```properties
    #redis.properties文件
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
    redis.password = 12345
    redis.database = 0
    ```

  * ```xml
    <!-- spring-jedis -->
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
                               http://www.springframework.org/schema/beans/spring-beans.xsd
                               http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
        <!-- 引入jedis的properties配置文件 -->
        <!--如果你有多个数据源需要通过<context:property-placeholder管理，且不愿意放在一个配置文件里，那么一定要加上ignore-unresolvable=“true"-->
        <context:property-placeholder location="classpath:properties/redis.properties" ignore-unresolvable="true" />
        <!--shardedJedisPool的相关配置-->
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
        <bean id="shardedJedisPool" class="redis.clients.jedis.ShardedJedisPool" scope="singleton">
            <constructor-arg index="0" ref="jedisPoolConfig" />
            <constructor-arg index="1">
                <list>
                    <bean class="redis.clients.jedis.JedisShardInfo">
                        <constructor-arg name="host" value="${redis.uri}" />
                    </bean>
                </list>
            </constructor-arg>
        </bean>
    </beans>
    ```



## 序列化工具

* ```java
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
              throw new CacheException(e);//该异常对象依赖mybatis
          }
      }
  }
  
  ```

* 使用jedis.hset()序列化参数时，报过空指针错误，没有找出原因