# springboot web开发2：静态资源的映射规则



## 映射（都是默认配置）

- **/webjars/xx，映射到classpath:/META-INF/resource/webjars/xx**：

  - ```java
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        if (!this.resourceProperties.isAddMappings()) {
            logger.debug("Default resource handling disabled");
        } else {
            Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
            CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
            /* 请求映射 */
            if (!registry.hasMappingForPattern("/webjars/**")) {
                this.customizeResourceHandlerRegistration(registry.addResourceHandler(new String[]{"/webjars/**"}).addResourceLocations(new String[]{"classpath:/META-INF/resources/webjars/"}).setCachePeriod(this.getSeconds(cachePeriod)).setCacheControl(cacheControl));
            }
    
            String staticPathPattern = this.mvcProperties.getStaticPathPattern();
            if (!registry.hasMappingForPattern(staticPathPattern)) {
                this.customizeResourceHandlerRegistration(registry.addResourceHandler(new String[]{staticPathPattern}).addResourceLocations(WebMvcAutoConfiguration.getResourceLocations(this.resourceProperties.getStaticLocations())).setCachePeriod(this.getSeconds(cachePeriod)).setCacheControl(cacheControl));
            }
    
        }
    }
    ```

  - webjars：以jar包的方式引入静态资源；将前端框架以依赖的形式引入；

  - 作用：**前端页面请求前端框架**

  - 参考http://www.webjars.org/

- 示例：（以jquery为例）

  - ```xml
        <!-- 引入jquery依賴jar -->
        <dependency>
            <groupId>org.webjars</groupId>
            <artifactId>jquery</artifactId>
            <version>3.3.1</version>
        </dependency>
    ```

  - ![avatar](H:/markdown%E7%AC%94%E8%AE%B0/SpringBoot%E6%A1%86%E6%9E%B6/%E5%9B%BE%E7%89%87%E5%BC%95%E7%94%A8/Snipaste_2020-04-19_00-52-12.png)

  - 如localhost:8080**/webjars/**jquery/3.3.1/jquery.js请求，映射到**classpath:/META-INF/resource/webjars/**jquery/3.3.1/jquery.js，即图中jquery.js文件



- **/xx映射到**，（访问当前项目的任何资源；静态资源的文件夹）

  - ```java
    "classpath:/META-INF/resources"
    "classpath:/resources/"
    "classpath:/static/"
    "classpath:/public/"
    "/":当前项目的根路径
    ```

  - ![avatar](H:/markdown%E7%AC%94%E8%AE%B0/SpringBoot%E6%A1%86%E6%9E%B6/%E5%9B%BE%E7%89%87%E5%BC%95%E7%94%A8/Snipaste_2020-04-19_09-44-20.png)

  - 如localhost:8080/abc/jquery.js请求，映射到**classpath：/MATA-INF/static/**abc/jquery.js静态资源

- 欢迎页：静态资源文件夹下的所有index.html页面，被**/**映射

  - 如localhost:8080/映射到index页面

- 所有的**/favicon.ico 都是在静态资源文件下找：



* **欢迎页的映射**

  * ```java
    //配置首页映射
    @Bean
    public WelcomePageHandlerMapping welcomePageHandlerMapping(ApplicationContext applicationContext, FormattingConversionService mvcConversionService, ResourceUrlProvider mvcResourceUrlProvider) {
        WelcomePageHandlerMapping welcomePageHandlerMapping = new WelcomePageHandlerMapping(new TemplateAvailabilityProviders(applicationContext), applicationContext, this.getWelcomePage(), this.mvcProperties.getStaticPathPattern());
        welcomePageHandlerMapping.setInterceptors(this.getInterceptors(mvcConversionService, mvcResourceUrlProvider));
        return welcomePageHandlerMapping;
    }
    ```

  * **/xx映射到静态资源文件夹下的所有index.html**



* **应用图标映射**：springboot2.2版本后不支持



* **设置和静态资源有关的参数；缓存时间等**

  * ```java
    @ConfigurationProperties(prefix = "spring.resources",ignoreUnknownFields = false)
    public class ResourceProperties {
    ```

  * 