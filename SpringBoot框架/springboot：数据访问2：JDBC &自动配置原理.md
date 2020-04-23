# springboot：数据访问2：JDBC &自动配置原理



* dataSource（数据源）：可以理解为 数据库需要的数据的来源



## 实例

* 依赖：

  * ```xml
    <!-- jdbc（数据源） -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jdbc</artifactId>
    </dependency>
    <!-- mysql驱动 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.47</version> <!-- spring 2.0以上默认连接mysql 8.0；因为本机上是mysql 5； 所以会报错“时区错误”；覆盖默认mysql版本就行了  -->
        <scope>runtime</scope>
    </dependency>
    ```



* yml全局配置（这里没有指定数据源，使用默认的jdbc数据源）

  * ```yml
    spring:
      datasource:
        username: root
        password: 123456
        url: jdbc:mysql://localhost:3306/jdbc
        driver-class-name: com.mysql.jdbc.Driver
    ```



* 测试连接

  * ```java
    <!-- 主配置类对应的测试类 -->
    @SpringBootTest
    class DataJdbcApplicationTests {
    
        @Autowired
        DataSource dataSource;
    
        @Test
        void contextLoads() throws SQLException {
            System.out.println(dataSource.getClass());
    
            Connection connection=dataSource.getConnection();
            System.out.println(connection);
            connection.close();
        }
    
    }
    
    <!--
        测试结果：输出
        class com.zaxxer.hikari.HikariDataSource
        HikariProxyConnection@1537371824 wrapping com.mysql.jdbc.JDBC4Connection@435e60ff
    -->
    ```



## 原理

* 自动配置所在的包：**org.springframework.boot.autoconfigure.jdbc**

  * DataSourceConfiguration：给容器添加各种数据源（dataSource）；默认是org.apache.tomcat.jdbc.pool.Database，即不配置/指定也有这个spring.dataSource.Type指定自定义的数据源类型

    * 指定数据源：

      * ```yml
        spring：
        	dataSource：
        		Type： com.alibaba.druid.pool.DruidDataSource
        ```

  * DataSourceInitializer：

    * runSchemaScripts()：运行建表语句；

    * runDataScripts()：运行插入数据的sql语句

    * 默认文件名：schema.sql或schema-all.sql

    * 指定位置：

      * ```yml
        schema:
        	- classpath:department.sql
        ```