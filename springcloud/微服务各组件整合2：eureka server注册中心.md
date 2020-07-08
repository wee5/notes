# springcloud2：eureka server注册中心



## 依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
    <version>2.0.2.RELEASE</version>
</dependency>
```



## 配置文件

```yml
server:
  port: 8761 #当前Eureka Server服务端口
eureka:
  client:
    register-with-eureka: false #是否将当前的Eureka Server服务作为客户端进行注册
    fetch-registry: false #是否获取其他Eureka Server服务的数据
    service-url:
      defaultZone: http://localhost:8761/eureka/ #注册中心的访问地址
```



## 启动类

```java
@SpringBootApplication //声明该类是springboot服务端入口
@EnableEurekaServer //声明该类是Eureka Server微服务，提供服务注册和服务发现功能，即注册中心
public class EurekaServerApplication {
    public static void main(String[] args) {
        SpringApplication.run(EurekaServerApplication.class,args);
    }
}
```