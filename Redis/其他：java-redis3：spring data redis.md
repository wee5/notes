# 其他：java-redis3：spring data redis



参考：https://blog.csdn.net/weixin_39723544/article/details/80743074写的非常好

这篇有问题，会报错，解决不了

## 依赖        

```xml
    
<!-- Spring Boot Redis依赖 -->
<!-- 注意：1.5版本的依赖和2.0的依赖不一样，注意看哦 1.5我记得名字里面应该没有“data”, 2.0必须是“spring-boot-starter-data-redis” 这个才行-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
    <!-- 1.5的版本默认采用的连接池技术是jedis  2.0以上版本默认连接池是lettuce, 在这里采用jedis，所以需要排除lettuce的jar -->
    <exclusions>
        <exclusion>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
        </exclusion>
        <exclusion>
            <groupId>io.lettuce</groupId>
            <artifactId>lettuce-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<!-- 添加jedis客户端 -->
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
</dependency>

<!--spring2.0集成redis所需common-pool2-->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
    <version>2.5.0</version>
</dependency>

<!-- 将作为Redis对象序列化器 -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.47</version>
</dependency>
```


## 配置文件

```yml
spring:
	redis:
		###Redis数据库索引（默认为0）
		database: 0
		host: 127.0.0.1
		port: 6397
		password: 123456
		###连接超时时间（毫秒）
		timeout: 10000
		jedis:
			pool:
				###连接池最大连接数（使用负值表示没有限制）
				max-active: 8
				###连接池最大阻塞等待时间（使用负值表示没有限制）
				max-wait: -1
				###连接池中的最大空闲连接
				max-idle: 5
				###连接池中最小空闲连接
				min-idle: 0
				
```

## 配置类

```java
@Configuration
@EnableCaching	//必须加，使配置生效
public class RedisConfiguration extends CachingConfigurerSupport{

    //Logger
    private static final Logger lg = LoggerFactory.getLogger(RedisConfiguration.class);

    @Autowired
    private JedisConnectionFactory jedisConnectionFactory;

    @Bean
    @Override
    public KeyGenerator keyGenerator() {
        // 设置自动key的生成规则，配置spring boot的注解，进行方法级别的缓存
        // 使用：进行分割，可以很多显示出层级关系
        // 这里其实就是new了一个KeyGenerator对象，只是这是lambda表达式的写法，我感觉很好用，大家感兴趣可以去了解下
        return (target, method, params) -> {
            StringBuilder sb = new StringBuilder();
            sb.append(target.getClass().getName());
            sb.append(":");
            sb.append(method.getName());
            for (Object obj : params) {
                sb.append(":" + String.valueOf(obj));
            }
            String rsToUse = String.valueOf(sb);
            lg.info("自动生成Redis Key -> [{}]", rsToUse);
            return rsToUse;
        };
    }

    @Bean
    @Override
    public CacheManager cacheManager() {
        // 初始化缓存管理器，用来设置缓存的整体过期时间等等，这里默认没有配置
        lg.info("初始化 -> [{}]", "CacheManager RedisCacheManager Start");
        RedisCacheManager.RedisCacheManagerBuilder builder = RedisCacheManager
            .RedisCacheManagerBuilder
            .fromConnectionFactory(jedisConnectionFactory);
        return builder.build();
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplate(JedisConnectionFactory jedisConnectionFactory ) {
        //设置序列化
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        // 配置redisTemplate
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<String, Object>();
        redisTemplate.setConnectionFactory(jedisConnectionFactory);
        RedisSerializer stringSerializer = new StringRedisSerializer();
        redisTemplate.setKeySerializer(stringSerializer); // key序列化
        redisTemplate.setValueSerializer(jackson2JsonRedisSerializer); // value序列化
        redisTemplate.setHashKeySerializer(stringSerializer); // Hash key序列化
        redisTemplate.setHashValueSerializer(jackson2JsonRedisSerializer); // Hash value序列化
        redisTemplate.afterPropertiesSet();
        return redisTemplate;
    }

    @Override
    @Bean
    public CacheErrorHandler errorHandler() {
        // 异常处理，当Redis发生异常时，打印日志，但是程序正常走
        lg.info("初始化 -> [{}]", "Redis CacheErrorHandler");
        CacheErrorHandler cacheErrorHandler = new CacheErrorHandler() {
            @Override
            public void handleCacheGetError(RuntimeException e, Cache cache, Object key) {
                lg.error("Redis occur handleCacheGetError：key -> [{}]", key, e);
            }

            @Override
            public void handleCachePutError(RuntimeException e, Cache cache, Object key, Object value) {
                lg.error("Redis occur handleCachePutError：key -> [{}]；value -> [{}]", key, value, e);
            }

            @Override
            public void handleCacheEvictError(RuntimeException e, Cache cache, Object key)    {
                lg.error("Redis occur handleCacheEvictError：key -> [{}]", key, e);
            }

            @Override
            public void handleCacheClearError(RuntimeException e, Cache cache) {
                lg.error("Redis occur handleCacheClearError：", e);
            }
        };
        return cacheErrorHandler;
    }

	//此内部类就是把yml的配置数据，进行读取，创建JedisConnectionFactory和JedisPool，以供外部类初始化缓存管理器使用
    @ConfigurationProperties	//这个注解总是报错；添加@EnableConfigurationProperties也没用
    class DataJedisProperties{
        @Value("${spring.redis.host}")
        private  String host;
        @Value("${spring.redis.password}")
        private  String password;
        @Value("${spring.redis.port}")
        private  int port;
        @Value("${spring.redis.timeout}")
        private  int timeout;
        @Value("${spring.redis.jedis.pool.max-idle}")
        private int maxIdle;
        @Value("${spring.redis.jedis.pool.max-wait}")
        private long maxWaitMillis;

        @Bean
        JedisConnectionFactory jedisConnectionFactory() {
            lg.info("Create JedisConnectionFactory successful");
            JedisConnectionFactory factory = new JedisConnectionFactory();
            factory.setHostName(host);
            factory.setPort(port);
            factory.setTimeout(timeout);
            factory.setPassword(password);
            return factory;
        }
        @Bean
        public JedisPool redisPoolFactory() {
            lg.info("JedisPool init successful，host -> [{}]；port -> [{}]", host, port);
            JedisPoolConfig jedisPoolConfig = new JedisPoolConfig();
            jedisPoolConfig.setMaxIdle(maxIdle);
            jedisPoolConfig.setMaxWaitMillis(maxWaitMillis);

            JedisPool jedisPool = new JedisPool(jedisPoolConfig, host, port, timeout, password);
            return jedisPool;
        }
    }
}
```



## 测试使用

```java
//在UserService的实现类中（业务层）进行缓存测试，注入RedisTemplate或StringRedisTemplate都可以
@Service
public class UserServiceImpl implements UserService {

    @Autowired
    UserEntityMapper userEntityMapper;
    @Autowired
    RedisTemplate redisTemplate;
    @Autowired
    StringRedisTemplate stringRedisTemplate;

    //新增
    @Override
    @Transactional
    public int save(UserEntity userEntity) {
        userEntityMapper.insert(userEntity);
        return userEntity.getUserId();
    }

    //查询所有
    @Override
    public PageInfo<UserEntity> findAllUserList(int pageNum, int pageSize) {
        PageHelper.startPage(pageNum, pageSize);
        List<UserEntity> list = userEntityMapper.selectAll();
        PageInfo<UserEntity> pageInfo = new PageInfo<>(list);
        // 具体使用
        redisTemplate.opsForList().leftPush("user:list", JSON.toJSONString(list));
        stringRedisTemplate.opsForValue().set("user:name", "张三");
        return pageInfo;
    }
}
```



## 其他

```java
/*
redisTemplate 中存取数据都是字节数组。当redis中存入的数据是可读形式而非字节数组时，使用redisTemplate取值的时候会无法获取导出数据，获得的值为null。可以使用StringRedisTemplate 试试
*/
redisTemplate.opsForValue();//操作字符串

redisTemplate.opsForHash();//操作hash

redisTemplate.opsForList();//操作list

redisTemplate.opsForSet();//操作set

redisTemplate.opsForZSet();//操作有序set

/*
当你的redis数据库里面本来存的是字符串数据或者你要存取的数据就是字符串类型数据的时候，那么你就使用StringRedisTemplate即可
*/
StringRedisTemplate.opsForValue().* //操作String字符串类型
    
StringRedisTemplate.delete(key/collection) //根据key/keys删除
    
StringRedisTemplate.opsForList().*  //操作List类型
    
StringRedisTemplate.opsForHash().*  //操作Hash类型
    
StringRedisTemplate.opsForSet().*  //操作set类型
    
StringRedisTemplate.opsForZSet().*  //操作有序set
```

