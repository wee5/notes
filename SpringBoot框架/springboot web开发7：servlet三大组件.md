# springboot web开发7：servlet三大组件



* 项目路径结构
  * ![avatar](图片引用\Snipaste_2020-04-23_04-30-07.png)



* **MyServerConfig：配置和服务器相关的组件**

  * ```java
    @Configuration
    public class MyServerConfig {
    
        @Bean
        public ConfigurableServletWebServerFactory configurableServletWebServerFactory(){
            TomcatServletWebServerFactory factory = new TomcatServletWebServerFactory();
            factory.setPort(8081);
            return factory;
        }
    
        @Bean
        public ServletRegistrationBean myServlet(){
            ServletRegistrationBean servletRegistrationBean = new ServletRegistrationBean(new MyServlet(), "/myServlet");
            return servletRegistrationBean;
        }
    
        @Bean
        public FilterRegistrationBean myFilter(){
            FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean();
            filterRegistrationBean.setFilter(new MyFilter());
            filterRegistrationBean.setUrlPatterns(Arrays.asList("/success","/myServlet"));
            return filterRegistrationBean;
        }
    
        @Bean
        public ServletListenerRegistrationBean myListener(){
            ServletListenerRegistrationBean<MyListener> myListenerServletListenerRegistrationBean = new ServletListenerRegistrationBean<>(new MyListener());
            return myListenerServletListenerRegistrationBean;
        }
    }
    ```

* **自定义组件**

  * ```java
    /* 自定义的Servlet */
    public class MyServlet extends HttpServlet {
    
        @Override/*处理doGet请求*/
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            doPost(req,resp);
        }
    
        @Override
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            resp.getWriter().write("Hello MyServlet");
        }
    }
    
    /* 自定义的Filter */
    public class MyFilter implements Filter {
        @Override
        public void init(FilterConfig filterConfig) throws ServletException {
    
        }
    
        @Override
        public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
            System.out.println("MyFilter processing...");
        }
    
        @Override
        public void destroy() {
    
        }
    }
    
    /* 自定义的Listener */
    public class MyListener implements ServletContextListener {
        @Override
        public void contextInitialized(ServletContextEvent sce) {
            System.out.println("contextInitialized...web应用启动");
        }
    
        @Override
        public void contextDestroyed(ServletContextEvent sce) {
            System.out.println("contextDestroyed...当前项目销毁");
        }
    }
    ```

* 补充：**springboot帮我们自动注册springmvc时，自动的注册springmvc前端控制器：DispatcherServlet**

  * ```java
    @ConditionalOnBean(value = {DispatcherServlet.class},name = {"dispatcherServlet"})
    public DispatcherServletRegistrationBean dispatcherServletRegistration(DispatcherServlet dispatcherServlet, WebMvcProperties webMvcProperties, ObjectProvider<MultipartConfigElement> multipartConfig) {
        DispatcherServletRegistrationBean registration = new DispatcherServletRegistrationBean(dispatcherServlet, webMvcProperties.getServlet().getPath());
        //默认拦截：/ 所有请求，包括静态资源，但是不拦截jsp请求；而 /* 会拦截jsp
        //可以通过server.servletPath来修改springmvc前端控制器默认拦截的请求路径
        registration.setName("dispatcherServlet");
        registration.setLoadOnStartup(webMvcProperties.getServlet().getLoadOnStartup());
        multipartConfig.ifAvailable(registration::setMultipartConfig);
        return registration;
    }
    ```