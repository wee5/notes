# java：异常3：总结



基于@ControllerAdvice和@ExceHandler注解的全局异常处理，该处理方式最为常用；这里也是以该方式为例

其他方式：实现SimpleMappingExceptionResolverBean接口，并封装成bean注入，实现HandlerExceptionResolver，并重写方法



## 代码

### 自定义异常

```java
@ResponseStatus(value = HttpStatus.CONFLICT, reason = "its AException")
public class AException extends RuntimeException {
    private static final long serialVersionUID = 1L;
    public AException(){
        super("这是A异常");
    }

    public AException(String message) {
        super(message);
    }
}
```

### 定义一个ResponseVo

```java
/* 这个并不规范，一般要包含code，message，data */
public class ResponseVo {
    String s;

    public ResponseVo(String s) {
        this.s = s;
    }

    public ResponseVo() {
    }

    public String getS() {
        return s;
    }

    public void setS(String s) {
        this.s = s;
    }
}
```

### 针对controller中的异常进行处理

```java
@ResponseStatus
@ExceptionHandler(AException.class)
public @ResponseBody String method11(){
    return "处理A异常的页面";
}//在controller中添加该方法
```

### 全局异常处理

```java
@ControllerAdvice(assignableTypes = {com.company.exceptiontest.controller1.Controller1.class})
public class ExceptionConfig {

    @ExceptionHandler(ArrayIndexOutOfBoundsException.class)
    public @ResponseBody String method1(ArrayIndexOutOfBoundsException e){
        return "处理异常页面哈哈abc";
    }

    @ExceptionHandler(NoClassDefFoundError.class)
    public  @ResponseBody ResponseVo method2(NoClassDefFoundError e){
        return new ResponseVo("这是找不到类异常");

    }

    @ExceptionHandler(AException.class)
    public ModelAndView method3(){
        ModelAndView modelAndView = new ModelAndView();
        modelAndView.setViewName("myerror.html");
        return modelAndView;
    }
}
```



## 分析

异常处理时，先找该controller中的异常处理方法，再找全局异常处理类

@ExceptionHandler注解指定对某种异常进行处理

@ControllerAdvice对指定位置的异常进行处理，如指定包名等（不指定时，默认对所有异常进行处理）



## 出现的问题

全局异常处理类中：
	输出流字符串是问号乱码：不使用@EnableWebMvc注解
	返回对象进入响应体，提示找不到解析器：返回对象需要写get/set方法，并在返回值前使用@ResponseBody注解（原本是在方法上使用的@ResponseBody注解）
	返回modelandview，提示404：暂未解决

