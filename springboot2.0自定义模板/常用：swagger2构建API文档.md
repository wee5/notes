# 常用：swagger2构建API文档



参考：https://blog.csdn.net/zhangbijun1230/article/details/102148754

## 依赖

```xml
<!--swagger2构建api文档 -->
<dependency>
    <groupId>com.spring4all</groupId>
    <artifactId>swagger-spring-boot-starter</artifactId>
    <version>1.9.0.RELEASE</version>
</dependency>
```



## 配置文件

```yml
swagger:
  title: spring-boot-starter-swagger #标题
  description: Starter for swagger 2.x #描述
  version: 1.4.0.RELEASE #版本
  license: Apache License, Version 2.0 #许可证
  licenseUrl: https://www.apache.org/licenses/LICENSE-2.0.html #许可证URL
  termsOfServiceUrl: https://github.com/dyc87112/spring-boot-starter-swagger #服务条款URL
  base-package: com.company.spring #扫描接口的包，默认全扫描
  base-path: /** #匹配需要构建文档的接口的规则，默认/**
  contact: #维护人
    name: wee5
    url: https://github.com/wee5
    email: 416383444@qq.com
```



## 启动类

```java
@SpringBootApplication
@EnableSwagger2Doc
public class AnimalAidSystemApplication {
```



## 使用

启动项目；访问localhost:8080/swagger-ui.html

以上文档构建完成，还需要添加文档内容



## 添加文档内容

### controller

```java
@Api(tags = "用户管理")
@RestController
@RequestMapping(value = "/users")     // 通过这里配置使下面的映射都在/users下
public class UserController {

    // 创建线程安全的Map，模拟users信息的存储
    static Map<Long, User> users = Collections.synchronizedMap(new HashMap<>());

    @GetMapping("/")
    @ApiOperation(value = "获取用户列表")
    public List<User> getUserList() {
        List<User> r = new ArrayList<>(users.values());
        return r;
    }

    @PostMapping("/")
    @ApiOperation(value = "创建用户", notes = "根据User对象创建用户")
    public String postUser(@RequestBody User user) {
        users.put(user.getId(), user);
        return "success";
    }

    @GetMapping("/{id}")
    @ApiOperation(value = "获取用户详细信息", notes = "根据url的id来获取用户详细信息")
    public User getUser(@PathVariable Long id) {
        return users.get(id);
    }

    @PutMapping("/{id}")
    @ApiImplicitParam(paramType = "path", dataType = "Long", name = "id", value = "用户编号", required = true, example = "1")
    @ApiOperation(value = "更新用户详细信息", notes = "根据url的id来指定更新对象，并根据传过来的user信息来更新用户详细信息")
    public String putUser(@PathVariable Long id, @RequestBody User user) {
        User u = users.get(id);
        u.setName(user.getName());
        u.setAge(user.getAge());
        users.put(id, u);
        return "success";
    }

    @DeleteMapping("/{id}")
    @ApiOperation(value = "删除用户", notes = "根据url的id来指定删除对象")
    public String deleteUser(@PathVariable Long id) {
        users.remove(id);
        return "success";
    }

}

@Data
@ApiModel(description="用户实体")
public class User {

    @ApiModelProperty("用户编号")
    private Long id;
    @ApiModelProperty("用户姓名")
    private String name;
    @ApiModelProperty("用户年龄")
    private Integer age;

}
```

### api文档

![avatar](图片引入\aHR0cDovL2ltZy5kaWRpc3BhY2UuY29tL0ZveHd6SWdka0lJeDZaNV9VOERacTVNcVZRZl8.jpg)

![avatar](图片引入\aHR0cDovL2ltZy5kaWRpc3BhY2UuY29tL0ZqYzl5dmdZaG5RQ3JNOS0yVmFRaUd3SzB2Nk0.jpg)



## 常用注释

@Api：类上；表示对类的说明；

​	tags=“说明该类的作用，可以在ui界面看到注解”

​	value=“该参数没有什么意义，在ui界面上可以看到，所以不需要配置”

@ApiOperation：请求方法上，说明方法用途作用

​	value=“说明方法用途作用

​	notes=“方法的备注说明

@ApiImplicitParams：请求方法上，表示一组参数说明

​	@ApiImplicitParam：用在@ApiImplicitParams注解中，指定一个请求参数的各个方面信息

​		name：参数名

​		value：参数汉子说明解释

​		required：参数是否必须传

​		paramType：参数方法哪个地方

@ApiResponses：请求方法上，表示一组响应

​	@ApiResponse：用在@ApiResponses注解中，一般用于表达一个错误的响应信息

​	code：数字，如200，400，500

​	message：信息，如请求参数没填好

​	response：抛出异常的类

@ApiModel：响应类上，表示一个返回相应数据的信息（一般用在post创建时，使用@RequestBody的场景，请求参数无法使用@ApiImplicitParam注解时

@ApiModelProperty：用在属性上，描述响应类的属性



## restful接口和参数接收

参考：http://blog.didispace.com/spring-boot-learning-21-2-1/

