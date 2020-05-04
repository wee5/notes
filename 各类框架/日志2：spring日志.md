# 日志2：spring日志



* spring默认使用日志是commons-logging；logging是spring中唯一强制的外部依赖，spring-core引入类该依赖，不过不需要任何配置，因为commons-logging是日志门面；
* 若没有提供`log4j`、`log4j2`之类的日志实现，那么`commons-logging`就会默认使用`jdk`自带的`java.util.logging`



## 使用log4j（已停更）

* 依赖

  * ```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>4.3.24.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
    </dependencies>
    ```

* 配置文件

  * ```properties
    #一个简单的log4j.xml配置文件
    log4j.rootCategory=INFO, stdout
    
    log4j.appender.stdout=org.apache.log4j.ConsoleAppender
    log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
    log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %t %c{2}:%L - %m%n
    
    log4j.category.org.springframework.beans.factory=DEBUG
    ```

## 使用log4j2

* 依赖

  * ```xml
    <dependencies>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.6.2</version>
        </dependency>
        <dependency>	<!-- log4j和commons-logging的中间包 -->
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-jcl</artifactId>
            <version>2.6.2</version>
        </dependency>
    </dependencies>
    
    <!-- 若需要使用slf4j作为日志门面，还需要添加下面依赖；否则不用添加 -->
    <dependencies>
      <dependency>	<!-- 这个jar好像是错的 -->
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-slf4j-impl</artifactId>
        <version>2.6.2</version>
      </dependency>
    </dependencies>
    
    ```

* 配置文件

  * ```xml
    <!-- log4j2.xml简单配置文件 -->
    <?xml version="1.0" encoding="UTF-8"?>
    <Configuration status="WARN">
      <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
          <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
      </Appenders>
      <Loggers>
        <Logger name="org.springframework.beans.factory" level="DEBUG"/>
        <Root level="error">
          <AppenderRef ref="Console"/>
        </Root>
      </Loggers>
    </Configuration>
    ```



## 使用slf4j

* `slf4j`巧妙的将诸如`spring-core`中用到的`commons-logging`的类或接口，在`jcl-over-slf4j`中写了一份同名的，但在具体的方法或接口等实现类中，再去具体找其他日志实现框架，它起到了一个日志门面的作用

* 依赖

  * ```xml
    <!-- 从spring-core中排除掉 -->
    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>4.3.24.RELEASE</version>
            <exclusions>
                <exclusion>	<!-- 排除jul依赖 -->
                    <groupId>commons-logging</groupId>
                    <artifactId>commons-logging</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>	<!-- commons-logging和slf4j的桥接包 -->
            <groupId>org.slf4j</groupId>
            <artifactId>jcl-over-slf4j</artifactId>
            <version>1.7.21</version>
        </dependency>
        <dependency>	<!-- slf4j和log4j的中间包 -->
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.21</version>
        </dependency>
        <dependency>	<!-- log4j实现包 -->
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
    </dependencies>
    ```

* **logback配合slf4j，不需要中间包**；因为logback出现晚，它实现了slf4j

  * ```xml
    <dependencies>
        <dependency>	<!-- commons-logging和slf4j的门面桥接包 -->
            <artifactId>jcl-over-slf4j</artifactId>
            <version>1.7.21</version>
        </dependency>
        <dependency>	<!-- logback实现包 -->
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.1.7</version>
        </dependency>
    </dependencies>
    ```



* 参考：http://www.mamicode.com/info-detail-2727716.html

























