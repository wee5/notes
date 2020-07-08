# 微服务各组件整合6：Zuul服务网关



springcloud继承了Zuul组件，实现服务网关

Zuul是Netflix提供的一个开源的api网关服务器，是客户端和网站后端所有请求的中间层，对外开放一个api，将所有请求导入统一的入口，屏蔽了服务端的具体的实现逻辑，Zuul可以实现反向代理的功能，在网关内部实现动态路由，身份认证，ip过滤，数据监控等

同时，Zuul自带负载均衡功能

## 依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        <version>2.0.2.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
        <version>2.0.2.RELEASE</version>  <--该依赖导入报错-->
    </dependency>
</dependencies>
```



## 配置文件

```yml
server:
  port: 8030
spring:
  application:
    name: gateway
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/
zuul:
  routes:
    provider: /p/** #给服务提供者设置请求映射
```



## 启动类

```java
@EnableZuulProxy //包含了@EnableZuulServer，设置该类是网关的启动类
@EnableAutoConfiguration //可以帮助springboot应用将所有符合条件的@Configuration配置加载到当前springboot创建并使用IoC容器中
public class ZuulApplication {
    public static void main(String[] args) {
        SpringApplication.run(ZuulApplication.class,args);
    }
}
```



## 测试

### 测试网关

启动注册中心，服务提供者，网关

访问localhost:8030/p/student/findAll，页面返回数据

实际是通过网关的请求映射来访问eurekaServer的rest接口

### 测试Zuul负载均衡

启动注册中心，服务提供者，修改端口号，再启动服务提供者（另一个启动类），网关

访问locahost:8010/student/index和locahost:8011/student/index，页面返回两个端口号8010和8011，证明两个服务同时存在

访问localhost:8030/p/student/index，页面返回端口号，刷新页面，端口号8010和8011循环出现，证明Zuul自带的负载均衡功能起作用