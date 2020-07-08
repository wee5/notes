# springcloud3:eureka client



改子模块作为一个服务提供者



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
  port: 8010
spring:
  application:
    name: provider #当前服务注册在eureka server上的名称
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka #注册中心的访问地址
  instance:
    prefer-ip-address: true #是否将当前服务的ip注册到eureka server
```



## 启动类

```java
@SpringBootApplication
public class ProviderApplication {
    public static void main(String[] args) {
        SpringApplication.run(ProviderApplication.class,args);
    }
}

/*这个启动类是用来测试负载均衡的，启动第一个启动类后，并保持启动状态
* 将端口号改为8011，再启动这个启动类
* 两个服务会同时存在
* 若不测试Zuul的负载均衡功能，可以不写这个启动类
*/
@SpringBootApplication
public class ProviderApplication2 {
    public static void main(String[] args) {
        SpringApplication.run(ProviderApplication.class,args);
    }
}
```



## 其它类

### 控制器

```java
/* 写一个rest接口，作为被调用的服务 */
@RestController
@RequestMapping("/student")
public class Controller {

    @GetMapping("/findAll")
    public Collection<Student> findAll(){
        ArrayList<Student> students = new ArrayList<>();
        students.add(new Student("lilwayne",37));
        students.add(new Student("hov",45));
        return students;
    }
}
```

### 实体类

```java
/* 封装参数 */
public class Student {

    private String name;
    private int age;

   /* 构造方法，get/set方法，toString方法 */
}
```