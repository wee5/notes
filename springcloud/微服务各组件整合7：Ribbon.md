# 微服务各组件整合7：Ribbon



springcloud Ribbon是一个负载均衡解决方案，Ribbon是Netflix发布的负载均衡器，springcloud Ribbon是基于Netflix Ribbon实现的，是一个用于对Http请求进行控制的负载均衡客户端

在注册中心对Ribbon进行注册之后，Ribbon就可以基于某种负载均衡算法，如轮询，随机，加权轮询，集全随即等自动帮助服务消费者调用接口，开发者也可以根据具体需求自定义Ribbon负载均衡算法，实际开发中，springcloud Ribbon需要结合springcloud Eureka来使用，**eurekaServer提供所有可以调用的服务提供者列表，Ribbon基于特定的负载均衡算法从这些服务提供者中选择要调用的具体实例**



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
  port: 8040
spring:
  application:
    name: ribbon
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/
  instance:
    prefer-ip-address: true
```



## 启动类

```java
@SpringBootApplication
public class RibbonApplication {
    public static void main(String[] args) {
        SpringApplication.run(RibbonApplication.class,args);
    }

    @Bean
    @LoadBalanced //声明一个基于Ribbon的负载均衡
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```



## 控制器

```java
@RestController
@RequestMapping("ribbon")
public class Controller {

    @Autowired
    private RestTemplate restTemplate;

    @GetMapping("findAll")
    public Collection<Student> findAll(){
        return restTemplate.getForObject("http://provider/student/findAll",Collection.class);
    }

    @GetMapping("index")
    public String index(){
        return restTemplate.getForObject("http://provider/student/index",String.class);
    }
}
```



## 其它类

### 实体类

略



## 测试

启动注册中心，服务提供者，修改端口号，再启动服务提供者（另一个启动类），ribbon

访问locahost:8010/student/index和locahost:8011/student/index，页面返回两个端口号8010和8011，证明两个服务同时存在

访问localhost:8040/ribbon/index，页面返回端口号，刷新页面，端口号8010和8011循环出现，证明ribbon的负载均衡功能起作用

实际上是通过ribbon服务调用provider服务，通过RestTemplate实现服务间的调用，并在注入bean——RestTemplate时，注释了@LoadBalanced，实现负载均衡