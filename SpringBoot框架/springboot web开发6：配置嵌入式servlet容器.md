# Springboot web开发6：配置嵌入式servlet容器



* springboot默认使用tomcat作为嵌入式的servlet容器



## 定制和修改servlet容器相关配置

* 依赖关系：
  * ![avatar](图片引用\Snipaste_2020-04-23_02-28-41.png)





* **配置文件**

  * ```properties
    server.port=8081
    server.context-path=/请求访问的跟路径
    server.tomcat.uri-encoding=utf-8
    
    //通用的servlet容器设置
    server.xxx
    //tomcat的设置
    server.tomcat.xxx
    ```



* 编写一个**ConfigurableServletWebServerFactory**；嵌入式servlet容器定制器，来修改servlet容器的配置

  * ```java
    @Bean<!-- @Bean在类上，将类配置成bean放在spring容器中；在方法上，将返回值配置成bean -->
    public ConfigurableServletWebServerFactory configurableServletWebServerFactory(){
        TomcatServletWebServerFactory factory = new TomcatServletWebServerFactory();
        factory.setPort(8585);
        return factory;
    }
    ```




## 切换其他Servlet容器