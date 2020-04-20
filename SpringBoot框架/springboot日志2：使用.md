# springboot日志2：使用



## 默认配置（application.properties)

* springboot默认帮我们配置了日志

* 在配置文件中手动配置

  * ```properties
    logging.level.com.company=trace
    
    #在指定文件中生成日志，没有文件则生成；默认当前项目路径
    logging.file.name=spring.log
    #指定路径（中的日志文件中输出日志），日志文件默认为spring.log
    logging.file.path=H:/spring/log
    #这两个不同时生效，且name优先级高于path
    
    #控制台输出日志的方式
    logging.pattern.console=%d{yyyy-mm-dd HH:mm:ss.SSSS} [%thread] %-5level %logger{50} - %msg%n
    #文件中输出日志的方式
    logging.pattern.file=%d{yyyy-mm-dd HH:mm:ss}_[%thread] %-5level %logger{50} 打印：%-5msg%n
    
    #日志输出格式
    #	%d：日期时间
    #	%thread：线程名
    #	%-5level：级别从左显示5个字符宽度
    #	logger{50}：表示logger名字最长50个字符，否则按句点分隔
    #	%msg：日志消息
    #	%n：换行符
    #	{}：给出前面格式的元数据
    ```

  

## 指定配置

* 类路径下放日志框架对应的配置文件即可；springboot会忽略自己的默认配置

  * ![avatar](图片引用\Snipaste_2020-04-18_11-20-15.png)

* springboot推荐日志框架的配置文件名带上“-spring”后缀

  * 可以使用springboot高级Profile功能

  * 有springboot解析，日志框架不会加载带后缀的配置文件

  * ```xml
    <springProfile name="staging">
    	<!-- configutation to be enabled when the "staging" profile is actice -->
        可以指定某段配置只在某环境下生效
    </springProfile>
    ```



## 切换日志框架

* **由slf4j+logback切换为slf4j+log4j**

* ```xml
  <!-- pom.xml中，排除原来的日志框架依赖，和其中间包，删除对应配置文件（不删除也不会再被识别）
   	添加新的日志框架，和其中间包，并添加对应配置文件-->
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
      <exclusions>
          <exclusion>
              <artifactId>logback-classic</artifactId>
              <groupId>ch.qos.logback</groupId>
          </exclusion>
      </exclusions>
  </dependency>
  
  <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-log4j12</artifactId>
   </dependency>
  ```



* **切换为log4j2**

* ```xml
  <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-web</artifactId>
          <exclusions>
              <exclusion>
                  <groupId>org.springframework.boot</groupId>
                  <artifactId>spring-boot-starter-logging</artifactId>
              </exclusion>
          </exclusions>
      </dependency>
  
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-log4j2</artifactId>
      </dependency>
  ```

