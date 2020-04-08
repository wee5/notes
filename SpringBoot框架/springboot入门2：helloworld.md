# springboot入门2：helloworld

* 一个功能：
  * 浏览器发送hello请求，服务器接收请求并处理，响应Hello World字符串



## 创建一个maven工程

* **导入springboot相关依赖**

  * ```
    <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>2.2.2.RELEASE</version>
            <relativePath/> <!-- lookup parent from repository -->
        </parent>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-starter-web</artifactId>
            </dependency>
        </dependencies>
    ```

* **编写一个主程序：启动spring应用**

  * ```
    /*@SpringBootApplication来标注一个主程序类，说明这是一个springboot应用*/
    @SpringBootApplication
    public class HelloWorldMainApplication {
        public static void main(String[] args) {
            //spring应用启动起来
            SpringApplication.run(HelloWorldMainApplication.class,args);
        }
    }
    ```

* **编写相关Controller，Service**

  * ```
    @Controller
    public class HelloController {
    
        @ResponseBody
        @RequestMapping("/hello")
        public String hello(){
            return "hello world";
        }
    }
    ```

* **运行主程序测试**

* **简化部署**

  * ```
    <build>
        <plugins>
            <!--该插件可以将应用打包成一个可执行的jar包-->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
    ```

  * maven-lifecircle-package；在target下找到打包好的jar，复制到桌面；在jar所在路径下启动cmd，执行java -jar jar包名称即可，网页请求可见hello world；jar压缩文件预览查看具体信息;jar中包含tomcat配置信息

## Hello World探究



### pom文件

* **父项目**

  * ```xml
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.2.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    
    它的父项目是
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.2.2.RELEASE</version>
        <relativePath>../../spring-boot-dependencies</relativePath>
    </parent>
    它来真正管理所有spring boot应用内的所有依赖版本
    ```

  * spring boot的版本仲裁中心；以后我们导入依赖默认是不需要写版本的；（没有在dependencies内管理的依赖自然需要申明版本号）

* **启动器**

  * ```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
    ```

  * **spring-boot-starter**-web：帮我们导入了web模块正常运行所依赖的组件

  * spring boot将所有的功能场景都抽取出来，做成一个个的starters（启动器），只需要在项目里面引入这些starter，相关场景的所有依赖都会导入进来，要用什么功能就导入什么场景的启动器



## 主程序类，主入口类

* ```java
  /*@SpringBootApplication来标注一个主程序类，说明这是一个springboot应用*/
  @SpringBootApplication
  public class HelloWorldMainApplication {
      public static void main(String[] args) {
          //spring应用启动起来
          SpringApplication.run(HelloWorldMainApplication.class,args);
      }
  }
  ```

* **@SpringBootApplication**:Spring Boot应用标注在某个类上，说明该类是SpringBoot的主配置类，SpringBoot就应该运行该类的main方法来启动SpringBoot应用；



* ```java
  @Target({ElementType.TYPE})
  @Retention(RetentionPolicy.RUNTIME)
  @Documented
  @Inherited
  @SpringBootConfiguration
  @EnableAutoConfiguration
  @ComponentScan(
      excludeFilters = {@Filter(
      type = FilterType.CUSTOM,
      classes = {TypeExcludeFilter.class}
  ), @Filter(
      type = FilterType.CUSTOM,
      classes = {AutoConfigurationExcludeFilter.class}
  )}
  )
  public @interface SpringBootApplication {
  ```

* **@SpringBootConfiguration**：Spring Boot的配置类；标注在某个类上，表示这是Spring Boot的配置类
  * **@Configuration**：配置类上来标注这个注解；配置类——配置文件；配置类也是容器中的一个组件@Component
  * **@EnableAutoConfiguration**：开启自动配置功能；以前我们需要配置的东西，Spring Boot帮我们自动配置；



* ```java
  @AutoConfigurationPackage
  @Import({AutoConfigurationImportSelector.class})
  public @interface EnableAutoConfiguration {
  ```

* **@AutoConfigurationPackage**：自动配置包

  * ```java
    @Import({Registrar.class})spring的底层注解@import，给容器中导入一个组件；导入的组件由Registrar.clas
    ```

