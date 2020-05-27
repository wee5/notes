# 日志5：slf4j+logback



## 实现

* 无任何其他框架，仅实现slf4j+logback

* 依赖

  * ```xml
    <!-- logback经典包，包含logback实现，slf4j门面，logback-core -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.3</version>
    </dependency>
    ```

* 测试

  * ```java
    public class Test {
        public static void main(String[] args) {
            Logger logger = LoggerFactory.getLogger(Test.class);
            System.out.println(logger.getName());
            logger.info("hello world");
            logger.debug("hi weezy");
            logger.error("hi hov");
            logger.warn("hi breezy");
            logger.trace("hi fit");
        }
    }
    ```

  * 