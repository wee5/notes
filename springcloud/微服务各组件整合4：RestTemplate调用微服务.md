# 微服务各组件整合4：RestTemplate



RestTemplate是spring框架提供基于REST的服务组件，底层是对http请求及响应进行了封装，提供了很多访问REST服务的方法，简化开发

restTemplate并未注册到eurekaServer注册中心，它直接作为springboot应用调用eurekaClient中暴露的接口，并不算严格的服务消费者

这只是演示RestTemplate使用方法



## 依赖

```xml
<!--需要springboot依赖，因为该项目就是基于springboot开发的，所以不需要额外的依赖-->
```



## 配置文件

未做任何配置，使用默认端口8080



## 启动类

```java
@SpringBootApplication
public class RestTemplateApplication {
    public static void main(String[] args) {
        SpringApplication.run(RestTemplateApplication.class,args);
    }

    @Bean
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```



## 其它类

```java
/* 被调用的微服务会返回Collection<Student>，所以需要Student来接收参数 */
public class Student {

    private String name;
    private int age;

   /* 构造方法，get/set方法，toString方法 */
}
```





## 使用

```java
/*
* 这里作为web请求接口，而eurekaServer作为被调用的微服务接口
* */
@RestController
@RequestMapping("/rest")
public class RestHandler {

    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("/findAll")
    public Collection<Student> findAll(){
        //通过RestTemplate调用了微服务eurekaServer的rest接口-student/findAll
        return restTemplate.getForEntity("Http://localhost:8010/student/findAll",Collection.class).getBody();
    }
}
```



## 测试

访问http://localhost:8080/rest/findAll，页面显示包含Collection<Student>的数据

实际是访问了restTemplate微服务，restTemplate微服务又调用eurekaServer微服务的rest接口来返回的数据