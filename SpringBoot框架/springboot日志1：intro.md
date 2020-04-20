# springboot日志1：intro



## 日志框架

* 小张：开发一个大型系统
  * System.out.println("")：将关键数据打印在控制台
  * 框架来记录系统的一些运行时信息：日志框架：zhanglogging.jar
  * 高大上的几个功能：异步模式，自动归档。。zhanglogging-good.jar
  * 将以前的框架卸下来，换上新的框架，重新修改之前相关的API：zhanglogging.prefect.jar
  * JDBC--数据库驱动
    * 写一个统一的接口层：日志门面（日志的抽象层）：logging-abstract.jar
    * 给项目中导入具体的日志实现就行了 ，之前的日志框架都是实现的抽象层
* 市面上的日志框架：JUL,JCL,Jboss-loggin,logback,log4j,lgo4j2,slf4j

| 日志门面（日志的抽象层）                                     | 日志实现                                    |
| ------------------------------------------------------------ | ------------------------------------------- |
| JCL(jakarta Commons Logging) **SLF4J**(Simple Logging Facade for java) jboss-logging | Log4j JUL(java.util.logging) Log4j2 Logback |

* 一个门面（抽象层），加一个实现
  * 日志门面：SLF4J
  * 日志实现：Logback
  * springboot：底层是spring框架，spring框架默认是用JCL；**springboot选用SLF4J和Logback**



## SLF4J使用

* slf4j官网：https://www.slf4j.org/manual.html

### 如何在系统中使用SLF4J

* 开发时，日志记录方法调用，应该调用日志抽象层的方法，而不是直接调用日志的实现类

  * 依赖：slf4j的jar包，logback的实现jar

  * ```java
    import org.slf4j.Logger;
    import org.slf4j.LoggerFactory;
    
    public class HelloWorld {
      public static void main(String[] args) {
        Logger logger = LoggerFactory.getLogger(HelloWorld.class);
        logger.info("Hello World");
      }
    }
    ```

* 配置文件是日志**实现**框架的配置文件

### 遗留问题

* 一个项目：slf4j+logback,依赖Spring(commons-logging),Hibernate(jboss-logging),Mybatis,xxx
* 同一日志记录，
* ![avatar](图片引用\legacy.png)

* **如何让系统中所有的日志都统一到slf4j：**
  * 将系统中其他日志框架先排除
  * 用中间包来替换原有的日志框架
  * 再导入slf4j其他实现



## springboot日志关系

* ```xml
  <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-web</artifactId>
  </dependency>
  
  <!-- 日志功能依赖 -->
  <dependency>
  	<groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-logging</artifactId>
  </dependency>
  
  ```

* 总结：

  * springboot底层使用**slf4j+logback**方式进行日志记录
  * springboot将其他日志都替换成slf4j
  * 中间替换包：...
  * 若需要引入其他框架，需要将该框架默认日志依赖移除掉

