# springboot配置2：配置文件注入



- 导入配置文件处理器：配置文件会有代码提示

- ```xml
  <!-- 配置文件处理器 -->
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-configuration-processor</artifactId>
          </dependency>
  ```



- 配置文件

- ```yaml
  # application.yaml文件
  person:
      lastName: zhangsan
      age: 18
      boss: false
      birth: 2017/12/12
      maps: {k1: v1,k2: v2}
      lists:
        - lisi
        - zhaoliu
      dog:
        name: 小狗
        age: 2
  ```

- ```properties
  # application.properties文件
  #idea的 .properties文件默认utf-8，在setting-file encoding中设置
  
  #项目端口号
  server.port=8081
  #项目的访问路径
  server.context.path=/springboot
  
  #配置person的值
  person.last-name=张三
  person.age=18
  person.birth=2017.12.15
  person.boss=false
  person.maps.k1=v1
  person.maps.k2=14
  person.lists=a,b,c
  person.dog.name=dog
  person.dog.age=15
  ```



- 注入到类中

- ```java
  /*	方法一
  * 将配置文件中配置的每一个属性值，映射到该组件中
  * @ConfigurationProperties：告诉springboot将本类中所有属性和配置文件中相关的配置进行绑定
      prefix="person",配置文件中以person开头的，其下所有属性进行映射
      *
      * 只有该组件是容器中的组件，才能用容器提供的ConfigurationProperties功能
   */
  @Component
  @ConfigurationProperties(prefix = "person")
  public class Person {
      private String lastName;
      private Integer age;
      private Boolean boss;
      private Date birth;
  
      private Map<String,Object> maps;
      private List<Object> lists;
      private Dog dog;	/* 包含的复合类型不再需要任何配置 */
      /* get/set方法 */
      
  /* 方法二 */    
  @Validated
  @Component
  public class Person {
      
      @Email	//数据校验 类注解@Validated配合属性注解@Email等
      @Value("${person.last-name}")
      private String lastName;
      
      @Value(#{11*2})
      private Integer age;
      
      @Value("true")
      private Boolean boss;
      
      private Date birth;
  
      private Map<String,Object> maps;
      private List<Object> lists;
      private Dog dog;
  ```



## 配置文件占位符

- 随机数

  - ```java
    ${random.value}		${random.int}		${random.long}
    ${random.int(10)}	${random.int[1024,65536]}
    ```

- 前面配置的值

  - ```properties
    person.last-name=张三${random.uuid}
    person.age=${random.int}
    person.dog.name=${person.hello:张三}_dog
    ```

