# 文件结构：单元测试1：dao层测试



mybatis框架

## pom

```xml
<!-- JDBC驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
<!-- springboot-mybatis -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.2</version>
</dependency>

<build>
    <resources>
        <!-- mapper.xml文件在resources目录下-->
        <resource>
            <directory>src/main/resources</directory>
        </resource>
    </resources>
</build>
```

## 配置文件

```yml
spring:
  jackson:
    date-format: yyyy-MM-dd HH:mm:ss
    time-zone: GMT+8
  datasource:
    url: jdbc:mysql://localhost:3306/animal_aid?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver

mybatis:
  mapper-locations: classpath:mapper/*.xml     # mapper映射文件
```

## 配置类

## 启动类

```yml
@SpringBootApplication
@MapperScan(basePackages="com.company.spring.dao")
public class AnimalAidSystemApplication {
```

## 相关类

## 使用

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = AnimalAidSystemApplication.class)
public class AnimalDaoTest {

    @Resource
    private AnimalDao animalDao;

    @Test
    public void method(){
        Animal animal = animalDao.selectByPrimaryKey(1);
        System.out.println(animal);
    }
}
```