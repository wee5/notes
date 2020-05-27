# 项目结构：dao



## 依赖

```xml
<!-- springboot-mybatis -->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>1.3.2</version>
</dependency>
```

## 配置文件

```yml
mybatis:
  mapper-locations: classpath:mapper/*.xml     # mapper映射文件
  configuration:
    mapUnderscoreToCamelCase: true
```



## 启动类

```java
@SpringBootApplication
@MapperScan(basePackages="com.company.spring.dao")
@ServletComponentScan
public class AnimalAidSystemApplication {
```

## 其它类

dao.java

mapper.xml

entity.java

**better-mybatis-generator自动生成上述文件**



## 使用

### xxxExample的使用

```java
userExample.createCriteria().xxx();//包含了各种相等，范围，非空等条件
userExample.setOrderByClause("id desc");//按字段升降排序，带上desc词汇；按多个字段排序待查证
userExample.setLimit(int i);//前i条记录
userExample.setOffset(int i);//越过i条记录，从i+1条记录开始
userExample.setDistinct(Boolean boolean);//是否去掉重复的记录
userExample.or().xxx();//
userExample.or(Criteria criteria);//或
```



### xxxDao的使用

#### 增

```java
userDao.insert(User user);//插入一条记录user
userDao.insertSelective(User user);//插入一条记录user的非null值
```

#### 删

```java
userDao.deleteByExample(UserExample userExample);//删除符合条件的记录
```

#### 改

```java
userDao.updateExample(User user,UserExample userExample);//将符合条件的记录修改为user的值
userDao.updateExampleSelective(User user,UserExample userExample);//将符合条件的记录修改为user的非null值
```

#### 查

```java
userDao.selectByExample(UserExample userExample);//查询符合条件的记录
//查询所有：userExample给null值
```

#### 自定义dao方法

userDao增加方法,userMapper编写对应sql语句即可



### 分页

PageHelper

