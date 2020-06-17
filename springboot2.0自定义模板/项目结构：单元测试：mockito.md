# 项目结构：junit4 mockito



参考推荐：https://blog.csdn.net/wslyk606/article/details/81772010

## mock intro

### 概念

- 假如，对象b依赖对象a，a未写完是一个空类空方法，可以用Mock来模拟一个假的a完成测试（前提是要对a类完全了解）

### 应用场景

- 真实行为是不确定的（如当前时间和当前温度）
- 真实对象很难搭建
- 真实对象行为很难触发（如网络错误）
- 真实对象速度很慢（如一个完整的数据库，在测试之前需要初始化）
- 真实对象时用户界面，或包括用户界面在内
- 真实对象使用回调机制
- 真实对象可能还不存在
- 真实对象可能包含不能做测试（而不是实际工作）的信息和方法

### postman测试和mock测试的区别

- postman是客户端测试，真实的网络请求方式实现
- mock是服务端测试，客户端行为是模拟的，编码方式实现



## mockito intro

Mockito 是一个针对 Java 的单元测试模拟框架，是为了简化单元测试过程中测试上下文 ( 或者称之为测试驱动函数以及桩函数 ) 的搭建而开发的工具

### 依赖(非springboot2.0配置)

```xml
<!-- mockito -->
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-all</artifactId>
    <version>1.9.5</version>
    <scope>test</scope>
</dependency>
<!-- junit4 -->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.11</version>
    <scope>test</scope>
</dependency>
```



### mockito常用方法

- perform：执行一个RequestBuilder请求，会自动执行springmvc的流程，并映射到相应的控制器执行处理
- andExpect：添加RequestMatcher验证规则，验证控制器执行完后结果是否正确
- addDo：添加RequestHandler结果处理器，比如调用时打印结果到控制台
- addReturn：最后返回相应的MvcResult，然后进行自定义验证，进行下一步的异步处理

### mockMvc.perform()

- 模拟GET请求：mockMvc.perform(MockMvcRequestBuilders.get("/user/{id}",userId));

- 模拟Post请求：mockMvc.perform(MockMvcRequestBuilders.post("uri",parameters));

- 模拟文件上传：mockMvc.perform(MockMvcRequestBuilders.multipart("uri").file("fileName","file".getBytes()));

- 模拟session和cookie：

  - mockMvc.perform(MockMvcRequestBuilders.get("uri").sessionAttr("name","value"));
  - mockMvc.perform(MockMvcRequestBuilders.get("uri").cookie(new Cookie("name","value")))

- 设置HTTP Header：

  - mockMvc.perform(MockMvcRequestBuilders

    ​						.get("uri")，parameters)

    ​						.contenType("application/x-www-form-urlencode")

    ​						.accept("application/json")

    ​						.header("",""));



## springboot中使用mockito测试

### 依赖

```xml
<!-- springboot中自带mockito依赖，可以不引入 -->
<!-- mockito -->
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-core</artifactId>
    <version>1.8.5</version>
    <scope>test</scope>
</dependency>
```



## mockMvc



## controller测试

```java
public class UserControllerTest {

    private MockMvc mockMvc;

    @Mock
    private IUserService userService;
    @InjectMocks
    private UserController userController;

    @Before
    public void setup(){
        MockitoAnnotations.initMocks(this);//初始化
        mockMvc = MockMvcBuilders.standaloneSetup(userController).build();//构建mvc环境
    }

    @Test
    public void login() {

        //mock方法
        Mockito.when(userService.login("weezy","lilwayne")).thenReturn(true);

        //模拟接口调用
        try {
            this.mockMvc.perform(MockMvcRequestBuilders.get("/users/").param("username","weezy").param("password","lilwayne")).andExpect(MockMvcResultMatchers.content().string("true"));
        } catch (Exception e) {
            System.out.println("异常--------------------------");
        }//期望为true，则测试通过；这里故意将期望设置为false，检测测试是否生效
    }

}
```