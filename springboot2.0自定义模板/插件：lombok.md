# 插件：Lombok



## 依赖

```xml
<dependency>
	<groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <optional>true</optional>
</dependency>
```



## 使用

- 注释
  - @AllArgsConstructor：代替有参构造方法
  - @NoArgsConstrutor：代替无参构造方法
  - @Data：代替get/set方法，equals方法，toString方法等，全父类Object包含的方法
  - @Builder：增加builder方法；即对象使用了该注解后，就会默认拥有builder方法
    - 使用：对象.builder().id(2).author("莫言").builder()
  - @Slf4j：