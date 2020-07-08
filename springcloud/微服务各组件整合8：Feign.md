# 微服务各组件整合8：Feign



与ribbon一样，feign也是netflix提供的feign是一个声明式，模板化的web service客户端，它简化了开发者编写web服务客户端的操作，开发者可以通过简单的接口和注解来调用http api，springcloud feign，它整合了ribbon和hystrix，具有可插拔，基于注解，负载均衡，服务熔断等一系列便捷功能

将较于ribbon——RestTemplate的方式，frign大大简化了代码的开发，feign支持多种注解，包括feig注解，JSX-RS注解，springmvc注解等，springcloud对feign进行了优化，整合ribbon和eureka，从而让feign的使用更加方便

ribbon和feign的区别：

ribbon是一个通用的http客户端工具，feign是基于ribbon实现的

ribbon的特点：

**feign是一个声明式的web service客户端**

**支持feign注解，springmvc注解，JAX-RS注解**

**feign基于ribbon实现，使用起来更加简单**

**feign集成了hystrix，具备服务熔断的功能**



## 依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
    <version>2.0.2.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframeword.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
    <version>2.0.2.RELEASE</version><!--依赖报错-->
</dependency>
```



## 配置文件

```yml
server:
  port: 8050
spring:
  application:
    name: feign
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
@EnableFeignClients
public class FeignApplication {
    public static void main(String[] args) {
        SpringApplication.run(FeignApplication.class,args);
    }
}
```



## 声明式接口

```java
//可以新建一个包，包含这个接口；因为是声明式接口，所以不需要实现类
@FeignClient(value="provider")
public interface FeignProviderClient {
    @GetMapping("/student/findAll")
    public Collection<Student> findAll();
    @GetMapping("/student/index")
    public String index;
}
```



## 控制器

```java
@RestController
@RequestMapping("/feign")
public class Controller {

    @Autowired
    private FeignProviderClient feignProviderClient;

    @GetMapping("/findAll")
    public Collection<Student> findAll(){
        return feignProviderClient.findAll();
    }

    @GetMapping("/index")
    public String index(){
        return feignProviderClient.index();
    }
}
```



## 其它类

### 实体类

略



## 熔断机制

### 配置文件

```yml
#添加熔断机制
feign:
  hystrix:
    enable:true #是否开始熔断器
```

### FeignProviderClient实现类

```java
public class FeignError implements FeignProviderClient {
    @Override
    public Collection<Student> findAll() {
        return null;
    }

    @Override
    public String index() {
        return "服务器维护中。。。";
    }
}
```

### 添加注解

```java
@FeignClient(value="provider",fallback=FeignError.class)//添加降级处理
public interface FeignProviderClient {
    @GetMapping("/student/findAll")
    public Collection<Student> findAll();
    @GetMapping("/student/index")
    public String index();
}
```



## 测试

访问localhost:8050/feign/index，页面返回端口号，刷新页面，端口号循环出现

实际上是通过feign服务调用provider服务，同时实现负载均衡

### 熔断机制测试

启动注册中心，启动feign，不启动provider

访问localhost:8050/feign/index，会报错并返回错误页面，此时熔断机制起作用，返回的页面显示“服务器维护中。。。”，而不是默认的错误页面