# springboot配置3：常用注解



## @Value和@ConfigurationProperties获取值比较

|                                                        | @ConfigurationProperties | @Value       |
| ------------------------------------------------------ | ------------------------ | ------------ |
| 功能                                                   | 批量注入配置文件中的属性 | 一个一个指定 |
| 松散绑定（松散语法：用-）                              | 支持                     | 不支持       |
| spEL                                                   | 不支持                   | 支持         |
| JSR303数据校验（@Validated类注解，结合@Email属性注解） | 支持                     | 不支持       |
| 复杂类型                                               | 支持                     | 不支持       |

- 若只需要在配置文件中获取某个值，使用@Value
- 若专门编写了javaBean和配置文件进行映射，使用@ConfigurationProperties



## @PropertySource和@ImportResource

- **@PropertySource**：加载指定配置文件

  - ```java
    指定配置文件，作为值注入到Person中
    @ImportResource（locations=（"classpath：person.properties"})
    @Componont
    @ConfigurationProperties(prefix="person")
    public class Person{
    	...
    }
    ```

- **@ImportResource**：导入Spring的配置文件，让配置文件的内容生效



- springboot内没有spring的配置文件，我们自己编写的配置文件，也不能自动识别；让spring配置文件生效，加载进来：@ImportResource标注在一个类上

  - ```java
    导入spring配置文件，让其生效
    @ImportResource（locations=（"classpath：beans.xml"})
    ```



- springboot**推荐**给容器加组件的方式：**全注解方式**

  - 配置类，spring配置文件

  - 使用@Bean给容器中添加组件

  - ```java
    /*
    * @Configuration：指明当前类是一个配置类，用来替代之前的spring配置文件
    *
    * 之前：配置文件中用<bean></bean>标签添加组件
    *
    * */
    @Configuration
    public class MyAppConfig {
    
        /* 将方法的返回值添加到容器中；容器中该组件默认的id就是方法名 */
        @Bean
        public HelloService hellService(){
            System.out.println("配置类@bean给容器中添加组件");
            return new HelloService();
        }
    }
    
    ```