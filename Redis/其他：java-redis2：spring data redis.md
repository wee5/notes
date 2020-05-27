# 其他：java-redis2：spring data redis



springboot中配置spring data redis

推荐：https://www.cnblogs.com/songanwei/p/9274348.html（spring data redis详细配置及使用）

## 依赖

```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-redis</artifactId>
    </dependency>
    <!--spring2.0集成redis所需common-pool2-->
    <!--<dependency>-->
        <!--<groupId>org.apache.commons</groupId>-->
        <!--<artifactId>commons-pool2</artifactId>-->
        <!--<version>2.4.2</version>-->
    <!--</dependency>-->
```


## 配置文件

```yml
spring:
  redis:
    jedis:
      pool:
        max-active: 10
        max-wait: -1
    timeout: 1000
    host: 127.0.0.1
    port: 6379
    password: 123456
```



## 配置类


```java
@Configuration
public class RedisConfig {
    
    /**
     * @param redisConnectionFactory
     * @return 自定义redisTemplate，自带的bean没有序列化器
     */
    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(redisConnectionFactory);
        redisTemplate.setKeySerializer(new StringRedisSerializer());//设置key的序列化器
        redisTemplate.setValueSerializer(new RedisConverter());//设置值的序列化器
        return redisTemplate;
    }
}

```
## 其他类

```java

public class RedisConverter implements RedisSerializer<Object> {
    
    private Converter<Object, byte[]> serializer = new SerializingConverter();//序列化器
    private Converter<byte[], Object> deserializer = new DeserializingConverter();//反序列化器@Override
    
    public byte[] serialize(Object o) throws SerializationException {//将对象序列化成字节数组
        if (o == null) return new byte[0];
        try {
            return serializer.convert(o);
        } catch (Exception e) {
            e.printStackTrace();
            return new byte[0];
        }
    }

    @Override
    public Object deserialize(byte[] bytes) throws SerializationException {//将字节数组反序列化成对象
        if (bytes == null || bytes.length == 0) return null;
        try {
            return deserializer.convert(bytes);
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }
}
```


## 测试使用

```java
@RestController
@RequestMapping("/redis")
public class RedisController {
    
    private String testString = "testString";
    private String userKey = "userKey";
    
    @Autowired
    private RedisTemplate<String, Object> redisTemplate;
    @Autowired
    private StringRedisTemplate stringRedisTemplate;

    @GetMapping("/add")
    public String add() {
        //1,添加一个Value为String
        stringRedisTemplate.opsForValue().set(testString, "测试存储字符串类型");
        //2,添加一个Value为对象
        User user = new User();
        user.setId(1);
        user.setUsername("张三");
        user.setPassword("1111");
        user.setRediskey(userKey);
        redisTemplate.opsForValue().set(user.getRediskey(), user);
        return "成功";
    }

    @GetMapping("/getUser")
    public User findUserByKey() {
        User user = (User) redisTemplate.opsForValue().get(userKey);
        return user;
    }

    @GetMapping("/getString")
    public String findString() {
        String s = stringRedisTemplate.opsForValue().get(testString);
        return s;
    }
    
    @GetMapping("/delete")
    public String deleteByKey(){
        //1,删除string类型
        stringRedisTemplate.delete(testString);
        //2,删除user对象
        redisTemplate.delete(userKey);
        return "删除成功";
    }
}
```