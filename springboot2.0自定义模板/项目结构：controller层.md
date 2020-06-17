# 文件结构：controller层



@Pathvariable和@RequestBody可以一起使用，@PathVariable Long id指定id, @RequestBody User user指定user数据，表示用user对象数据更新指定id字段

## controller

```java
//返回json字符串
@Controller
@RequestMapping("/animal")
public class AnimalController {

    @GetMapping("/hello")
    public @ResponseBody Animal method(){
        return new Animal();//以json形式将对象传回前端
    }
}
//同上面等价
@RestController
@RequestMapping("/animal")
public class AnimalController {

    @GetMapping("/hello")
    public Animal method(){
        return new Animal();
    }
}
```

## 参数接收

```java
@GetMapping("/test/{id}")//(get)api/test/11?param=one
public void method(@PathVariable(name="id",required=false) Integer id,@RequestParam(name="param") String param,@RequestBody UserVo userVo ){}
/* 
三种接收参数方式，通过注解@PathVariable @RequestParam @RequestBody获取，可以同时起作用
分别对应路径参数，请求携带参数，请求体包含参数

注意：
当参数是“非必要”(required=false)时，参数类型不能是基本类型，若是基本类型，需要改为封装类；因为当没有接收到参数时，参数会赋为null，而基本类型不能是null值
当使用@PathVariable和@RequestBody注解时，需要指定name值，否则接收不到参数

*/
```

### @PathVariable

@PathVariable(required=false)时，配合@GetMapping(value={"/test/{id}","/test"});若不设置多个URL，当不传入参数id时，URL改变了，请求进不来

### @RequestParam

请求携带参数可以是以get方法显示的写在路径上，也可以不写在路径上（因为通过request.getParam()一样可以获取）

### @RequestBody

匹配RequestBody携带的json数据的键和接受参数中属性名相同，不需要键和属性的数量和名称完全对应，也可以接收到参数

配合插件jsonFormat使用，灵活接收参数



## restful api和参数接收

灵活使用参数接收方式去实现restful api的格式

```java
/*
例如：(post)api/users/{id} 表示更新user表某id的记录
*/
@PostMapping("/users/{id}")//请求体携带需要更新的记录user
public void method(@PathVariable("id") int id,@RequestBody User user){}
```





## 方法返回

### 返回json数据

@RestController或@ResponseBody，将返回对象以json格式返回

### 跳转到页面

方法返回String，String表示页面名称

