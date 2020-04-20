# springboot web开发4：MVC自动配置



参考：<https://docs.spring.io/spring-boot/docs/1.5.10.RELEASE/reference/htmlsingle/#boot-features-logging>；"Spring MVC auto-configuration"

- Inclusion of `ContentNegotiatingViewResolver` and `BeanNameViewResolver` beans.
  - 配置ViewResolver（视图解析器：根据方法的返回值得到视图对象View，视图决定如何渲染，即转发，重定向）
  - ContentNegotiatingViewResolver：组合所有的视图解析器的
  - 如何定制：我们自己在容器中添加一个视图解析器，自动将其组合进来
- Support for serving static resources, including support for WebJars (see below).静态文件夹路径，webjars
- Automatic registration of `Converter`, `GenericConverter`, `Formatter` beans.
  - Converter：转换器，public String hello(User user)：类型转换使用Converter
  - Formatter：格式化器，也可以添加自己的格式化器，放入容器即可
- Support for `HttpMessageConverters` (see below).消息转换器
  - HttpMessageConverter：SpringMVC用来转换Http请求和响应的；User->json
  - HttpMessageConverters：是从容器中确定，同样可以自己添加一个放在容器中
- Automatic registration of `MessageCodesResolver` (see below).定义错误代码生成规则
- Static `index.html` support.静态首页访问
- Custom `Favicon` support (see below).Favicon.ico
- Automatic use of a `ConfigurableWebBindingInitializer` bean (see below).
  - 初始化WebDateBinder
  - 请求数据====javaBean
- org.springframework.boot.autoconfigure.web：web的所有自动场景
- If you want to keep Spring Boot MVC features, and you just want to add additional [MVC configuration](https://docs.spring.io/spring/docs/4.3.14.RELEASE/spring-framework-reference/htmlsingle#mvc) (interceptors, formatters, view controllers etc.) you can add your own `@Configuration` class of type `WebMvcConfigurerAdapter`, but **without** `@EnableWebMvc`. If you wish to provide custom instances of `RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter` or `ExceptionHandlerExceptionResolver` you can declare a `WebMvcRegistrationsAdapter` instance providing such components.



## 扩展SpringMvc

* ```xml
  <!-- 原本的配置 -->
  <mvc:view-controller path="/hello" view-name=""success/>
  <mvc:interceptors>
      <mvc:interceptor>
          <mvc:mapping path="/hello"/>
          <bean></bean>
      </mvc:interceptor>
  </mvc:interceptors>
  ```

* 编写一个配置类（@Configuration），是WebMvcConfigurer类型；且不能标注@EnableWebMvc；既保留类所有的自动配置，也能用我们扩展的配置

  * ```java
    //使用WebMvcConfigurer（WebMvcConfigurerAdapter以失效）可以来扩展SpringMvc的功能
    @Configuration
    public class MyMvcConfig extends WebMvcConfigurer {
    
        @Override
        public void addViewControllers(ViewControllerRegistry registry) {
            // super.addViewController(registry);
            //浏览器发送 /spring 请求，映射到 success页面
            registry.addViewController("/spring").setViewName("success");
        }
    }
    ```

* 原理

  * WebMvcAutoConfiguration是SpringMVC的自动配置类

  * 在做其他自动配置时会导入；@Import(EnableWebMvcConfiguration.class)

  * ```java
    @Configuration(proxyBeanMethods = false)
    public static class EnableWebMvcConfiguration extends DelegatingWebMvcConfiguration implements ResourceLoaderAware {
    
    //从容器中获取所有的WebMvcConfigurer
    @Autowired(required = false)
    public void setConfigurers(List<WebMvcConfigurer> configurers) {
        if (!CollectionUtils.isEmpty(configurers)) {
            this.configurers.addWebMvcConfigurers(configurers);
        }
    }
    ```

  * 容器中所有的WebMvcConfigurer都会一起起作用

  * 我们的配置类也会被调用

  * 效果SpringMVC的自动配置和我们的扩展配置都会起作用



## 全面接管SpringMVC

* Springboot对SpringMVC的自动配置不需要了，所有都是自己配，所有SpringMVC的自动配置都失效

* 只需要在配置类添加@EnableWebMvc

* ```java
  //使用WebMvcConfigurer（WebMvcConfigurerAdapter以失效）可以来扩展SpringMvc的功能
  @EnableWebMvc
  @Configuration
  public class MyMvcConfig extends WebMvcConfigurer {
  
      @Override
      public void addViewControllers(ViewControllerRegistry registry) {
          // super.addViewController(registry);
          //浏览器发送 /spring 请求，映射到 success页面
          registry.addViewController("/spring").setViewName("success");
      }
  }
  ```

* 原理：为什么添加#ENableWebMvc自动配置就失效了

  * ```java
    核心是导入了DelegatingWebMvcConfiguration类
    @Import({DelegatingWebMvcConfiguration.class})
    public @interface EnableWebMvc {
    }
    
    @Configuration(proxyBeanMethods = false)
    public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {
        
    @Configuration(proxyBeanMethods = false)
    @ConditionalOnWebApplication(type = Type.SERVLET)
    @ConditionalOnClass({Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class})
    //容器中没有该组件时，这个自动配置类才会生效
    @ConditionalOnMissingBean({WebMvcConfigurationSupport.class})
    @AutoConfigureOrder(-2147483638)
    @AutoConfigureAfter({DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class, ValidationAutoConfiguration.class})
        
    @EnableWebMvc将WebMvcConfigurationSupport组件导入进来
    导入的WebMvcConfigurationSupport只是SpringMVC是最近本的功能
    ```

  * 

## 如何修改springboot的默认配置

* 模式
  * springboot在自动配置大多数组件时，采用先使用（如果有的话）用户添加进容器的组件；如果没有，才自动配置；如果有些组件可以有多个（ViewResolver）将用户配置的和自己默认的组合使用
  * 在Springboot中会有非常多的xxxConfigurer帮我们进行扩展配置