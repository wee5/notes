# 常用：dozer



## 依赖

```xml
<!-- Dozer -->
<dependency>
    <groupId>net.sf.dozer</groupId>
    <artifactId>dozer</artifactId>
    <version>5.4.0</version>
</dependency>
```



## 配置类

```java
@Configuration
public class DozerBeanMapperConfigure {
    @Bean
    public DozerBeanMapper mapper() {
        DozerBeanMapper mapper = new DozerBeanMapper();
        return mapper;
    }
}
```



## 工具类

```java
//用来转换list，需要就写，不需要可以不写（而且这个代码报错，有点问题）
public class DozerUtils {

    static DozerBeanMapper dozerBeanMapper = new DozerBeanMapper();

    public static <T> List<T> mapList(Collection sourceList, Class<T> destinationClass){
        List destinationList = Lists.newArrayList();
        for (Iterator i$ = sourceList.iterator(); i$.hasNext();){
            Object sourceObject = i$.next();
            Object destinationObject = dozerBeanMapper.map(sourceObject, destinationClass);
            destinationList.add(destinationObject);
        }
        return destinationList;
    }
}
```



## 使用

```java
/* 默认按照属性名匹配，只匹配属性名相同的；即不需要两个对象的属性名称和数量完全相同，可多可少 */
@AutoWire
protected Mapper dozerMapper;
EntiryVo entityVo=dozerMapper.map(entity,entityVo)

//工具类使用
DozerUtils.mapList(需要转换的list，集合中元素转换后的类型)
    
//@Mapping("")双向映射，即只需要在entity和vo对应的属性上注解一个。
public class Animal implements Serializable {

    @Mapping("animal")
    private String name;
```

