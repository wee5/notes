# 微服务各组件整合5：consumer服务消费者

让服务消费者通过RestTemplate调用其他微服务rest接口

## 依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    <version>2.0.2.RELEASE</version>
</dependency>
```



## 配置文件

```yml
server:
  port: 8020
spring:
  application:
    name: consumer
eureka:
  client:
    service-uri:
      defaultZone: http://localhost:8761/eureka/
  instance:
    prefer-ip-address: true
```



## 启动类

```java
@SpringBootApplication
public class ConsumerApplication {
    public static void main(String[] args) {
        SpringApplication.run(ConsumerApplication.class,args);
    }

    @Bean
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```



## 控制器

```java
@RestController
@RequestMapping("/consumer")
public class Controller {
    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("/findAll")
    public Collection<Student> findAll(){
        return restTemplate.getForObject("Http://localhost:8010/student/findAll",Collection.class);
    }
}
```



## 其它类

### 实体类

```java
/* 被调用的微服务会返回Collection<Student>，所以需要Student来接收参数 */
public class Student {

    private String name;
    private int age;

   /* 构造方法，get/set方法，toString方法 */
}
```

