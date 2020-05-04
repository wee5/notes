# spring-mybatis.xml



* **配置多个注解扫描路径**：

  * ```xml
    /* 逗号隔开;否则会报错：注入失败 */
    <context:component-scan base-package="com.company.service,com.company.redis"/>
    ```

  * 