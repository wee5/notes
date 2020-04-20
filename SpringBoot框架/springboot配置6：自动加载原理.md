# springboot配置6：自动加载原理



* 配置文件可写的属性：参考https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#common.application.properties；搜索Appendix A. Common application properties



## 自动配置原理

* springboot启动时，加载主配置类；便开启自动配置功能@EnableAutoConfiguration

* @EnableAutoConfiguration作用：

  * 利用EnableAutoConfigurationImportSelector给容器中导入一些组件

  * 可以插入selectimports()方法的内容：

  * ```java
    List<String> configurations=getCandidateConfigurations(annotationMetadata,attributes)获取候选的配置
    ```

    

