# springboot配置4：Profile



## Profile

- 多Profile文件

  - 文件名可以是：application-{profile}.properties/yaml；默认使用application.properties全局配置文件

- yaml文档块

  - ```yaml
    server:
    	port: 8081
    spring:
    	profiles:
    		active: prod
    ---
    server:
    	port: 8083
    spring:
    	profiles: dev
    ---
    server:
    	port: 8084
    spring:
    	profiles: prod
    ```

    

- 激活指定profile

  - ```properties
    spring.profiles.active=dev /* 即{profile}中的单词，dev是开发环境 */
    ```

  - 命令行：--spring.profiles.active=dev

    - 在edit configurations-springboot-configApplication-confguration-program arguments中添加；
    - 在打包后的jar包路径下启动cmd，执行命令：java -jar spring-boot-02-config-0.0.1-SNAPSHOT.jar --spring.profiles.active=dev
    - **命令行优先于spring.profiles.active**

  - 虚拟机参数：-Dspring.profiles.active=dev

    - 在edit configurations-springboot-configApplication-confguration-VM options中添加；