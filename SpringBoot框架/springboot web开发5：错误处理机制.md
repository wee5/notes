# springboot web开发5：错误处理机制



## springboot默认错误处理

* 默认效果

  * 浏览器访问，返回一个错误页面
    * ![avatar](图片引用\Snipaste_2020-04-20_00-43-34.png)
  * 浏览器发送请求的请求头
    * ![avatar](图片引用\Snipaste_2020-04-20_01-16-42.png)
  * 若是其他客户端（手机等）访问，默认响应一个json数据
    * ![avatar](图片引用\Snipaste_2020-04-20_00-47-23.png)
    * ![avatar](图片引用\Snipaste_2020-04-20_01-25-09.png)

* 原理：参考ErrorMvcAutoConfiguration，错误处理自动配置

  * 给容器中添加一下组件

    * ```java
      //DefaultErrorAttributes{
              
      //BasicErrorController，处理默认/error请求
      	@Controller
          @RequestMapping({"${server.error.path:${error.path:/error}}"})
          public class BasicErrorController extends AbstractErrorController {
              
      	@RequestMapping(produces = {"text/html"})//产生html类型的数据，处理浏览器请求
          public ModelAndView errorHtml(HttpServletRequest request, HttpServletResponse response) {
              HttpStatus status = this.getStatus(request);
              Map<String, Object> model = Collections.unmodifiableMap(this.getErrorAttributes(request, this.isIncludeStackTrace(request, MediaType.TEXT_HTML)));
              response.setStatus(status.value());
              //去哪个页面作为错误页面
              ModelAndView modelAndView = this.resolveErrorView(request, response, status, model);
              return modelAndView != null ? modelAndView : new ModelAndView("error", model);
          }
              
          @RequestMapping//处理其他客户端请求
          //@ResponseBody  原本用这个标签注释，用来产生json数据；当前版本springboot没有用它注释
          public ResponseEntity<Map<String, Object>> error(HttpServletRequest request) {
              HttpStatus status = this.getStatus(request);
              if (status == HttpStatus.NO_CONTENT) {
                  return new ResponseEntity(status);
              } else {
                  Map<String, Object> body = this.getErrorAttributes(request, this.isIncludeStackTrace(request, MediaType.ALL));
                  return new ResponseEntity(body, status);
              }
          }
      
      //ErrorPageCustomizer
      	@Value("${error.path:/error}")
          private String path = "/error";//系统出现错误页面之后，来到error请求进行处理；（类似web.xml注册的错误页面）
          
      //DefaultErrorViewResolver
          public ModelAndView resolveErrorView(HttpServletRequest request, HttpStatus status, Map<String, Object> model) {
              ModelAndView modelAndView = this.resolve(String.valueOf(status.value()), model);
              if (modelAndView == null && SERIES_VIEWS.containsKey(status.series())) {
                  modelAndView = this.resolve((String)SERIES_VIEWS.get(status.series()), model);
              }
      
              return modelAndView;
          }
      
          private ModelAndView resolve(String viewName, Map<String, Object> model) {
              //默认Springboot可以去找到一个页面，error/404
              String errorViewName = "error/" + viewName;
              //模板引擎可以解析这个页面地址就用模板引擎解析
              TemplateAvailabilityProvider provider = this.templateAvailabilityProviders.getProvider(errorViewName, this.applicationContext);
              //模板引擎不可用时，在静态资源文件夹下找errorViewName对应的页面，error/404.html
              return provider != null ? new ModelAndView(errorViewName, model) : this.resolveResource(errorViewName, model);
          }
      ```

  * 步骤：

    * 一旦系统出现4xx或者5xx之类的错误，ErrorPageCustomizer就会生效（定制错误的响应规则）
    * 就会来到/error请求，就会被BasicErrorController处理
      * 响应页面





## 定制错误响应

* **定制错误页面**
  * **有模板引擎时**，模板文件夹下error/状态码
  * 可以使用4xx和5xx作为错误页面文件名；次级匹配
  * 页面能获取的信息：
    * timestamp：时间戳
    * status：状态码
    * error：错误提示
    * exception：异常对象
    * message：异常消息
    * errors：JSR303数据校验的错误都在这里
  * **没有模板引擎**，模板引擎找不到这个错误页面，静态资源文件夹下找（可以找到页面，但不能解析thymeleaf语法）
  * 以上都错误页面，会来到springboot的默认错误页面
* **定制错误json数据**