# 常用：PageHelper



## 依赖

```xml
<!-- pageHelper分页 -->
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.2.10</version>
</dependency>
```



## 使用

```java
//会将后面第一条sql语句进行分页，不需要连续
PageHelper.startPage(int pageNum,int pageSize);
```



